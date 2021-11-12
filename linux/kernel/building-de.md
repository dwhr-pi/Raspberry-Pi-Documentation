# Kernel-Building

Die Standard-Compiler und -Linker, die mit einem Betriebssystem geliefert werden, sind so konfiguriert, dass sie ausführbare Dateien erstellen, die auf diesem Betriebssystem ausgeführt werden – sie sind native Tools – aber das muss nicht der Fall sein. Ein Cross-Compiler ist so konfiguriert, dass er Code für ein anderes Ziel erstellt als das, das den Build-Prozess ausführt, und seine Verwendung wird als Cross-Compilierung bezeichnet.

Die Kreuzkompilierung des Raspberry Pi-Kernels ist aus zwei Gründen nützlich:

 * es ermöglicht die Erstellung eines 64-Bit-Kernels mit einem 32-Bit-Betriebssystem und umgekehrt, und
 * Selbst ein bescheidener Laptop kann einen Pi-Kernel deutlich schneller kompilieren als der Pi selbst.

Die folgenden Anweisungen sind in native Builds und Cross-Compilierung unterteilt. Wählen Sie den Abschnitt aus, der für Ihre Situation geeignet ist - obwohl es viele gemeinsame Schritte zwischen den beiden gibt, gibt es auch einige wichtige Unterschiede.

## Local building

Installieren Sie auf einem Raspberry Pi zuerst die neueste Version von [Raspberry Pi OS] (https://www.raspberrypi.org/downloads/). Starten Sie dann Ihren Pi, schließen Sie Ethernet an, um Zugriff auf die Quellen zu erhalten, und melden Sie sich an.

Installieren Sie zuerst Git und die Build-Abhängigkeiten:

```bash
sudo apt install git bc bison flex libssl-dev make
```

Als nächstes holen Sie sich die Quellen, was einige Zeit in Anspruch nehmen wird:

```bash
git clone --depth=1 https://github.com/raspberrypi/linux
```

<a name="choosing_sources"></a>

### Quellen auswählen

Der obige Befehl `git clone` lädt den aktuellen aktiven Zweig (denjenigen, aus dem wir Raspberry Pi OS-Images erstellen) ohne Verlauf herunter. Wenn Sie `--depth=1` weglassen, wird das gesamte Repository heruntergeladen, einschließlich des vollständigen Verlaufs aller Zweige, aber dies dauert viel länger und belegt viel mehr Speicherplatz.

Um einen anderen Zweig herunterzuladen (wieder ohne Verlauf), verwenden Sie die Option `--branch`:

```bash
git clone --depth=1 --branch <branch> https://github.com/raspberrypi/linux
```

wobei `<branch>` der Name des Branchs ist, den Sie herunterladen möchten.

Informationen zu den verfügbaren Zweigen finden Sie im [originalES GitHub-repository](https://github.com/raspberrypi/linux).

### Kernel-Konfiguration

Konfigurieren Sie den Kernel; Neben der Standardkonfiguration möchten Sie möglicherweise [Ihren Kernel detaillierter konfigurieren](configuring.md) oder [Patches aus einer anderen Quelle anwenden](patching.md), um erforderliche Funktionen hinzuzufügen oder zu entfernen.

<a name="default_configuration"></a>
#### Übernehmen der Standardkonfiguration

Bereiten Sie zunächst die Standardkonfiguration vor, indem Sie je nach Raspberry Pi-Version die folgenden Befehle ausführen:

##### Standard-Build-Konfiguration für Raspberry Pi 1, Pi Zero, Pi Zero W und Compute Module

```bash
cd linux
KERNEL=kernel
make bcmrpi_defconfig
```

##### Raspberry Pi 2, Pi 3, Pi 3+ und Compute Module 3 Standard-Build-Konfiguration

```bash
cd linux
KERNEL=kernel7
make bcm2709_defconfig
```

##### Raspberry Pi 4 Standard-Build-Konfiguration

```bash
cd linux
KERNEL=kernel7l
make bcm2711_defconfig
```

#### Anpassen der Kernel-Version mit LOCALVERSION

Zusätzlich zu Ihren Kernel-Konfigurationsänderungen möchten Sie vielleicht die `LOCALVERSION` anpassen, um sicherzustellen, dass Ihr neuer Kernel nicht denselben Versionsstring wie der Upstream-Kernel erhält. Dies stellt sowohl klar, dass Sie Ihren eigenen Kernel in der Ausgabe von `uname` ausführen, und stellt sicher, dass vorhandene Module in `/lib/modules` nicht überschrieben werden.

Ändern Sie dazu die folgende Zeile in `.config`:
```
CONFIG_LOCALVERSION="-v7l-MY_CUSTOM_KERNEL"
```
Sie können diese Einstellung auch grafisch ändern, wie in [den Anweisungen zur Kernel-Konfiguration] (configuring.md) gezeigt. Es befindet sich unter "Allgemeines Setup" => "Lokale Version - an Kernel-Release anhängen".

### Building

Erstellen und installieren Sie den Kernel, die Module und die Gerätebaum-Blobs; dieser Schritt kann je nach verwendetem Pi-Modell **lange** Zeit in Anspruch nehmen:

```bash
make -j4 zImage modules dtbs
sudo make modules_install
sudo cp arch/arm/boot/dts/*.dtb /boot/
sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/
sudo cp arch/arm/boot/zImage /boot/$KERNEL.img
```

**Hinweis**: Auf einem Raspberry Pi 2/3/4 teilt das Flag "-j4" die Arbeit auf alle vier Kerne auf, wodurch die Kompilierung erheblich beschleunigt wird.

## Cross-Compiling

Zunächst benötigen Sie einen geeigneten Linux-Cross-Compilation-Host. Wir neigen dazu, Ubuntu zu verwenden; seit Raspberry Pi OS ist
auch eine Debian-Distribution, bedeutet dies, dass viele Aspekte ähnlich sind, wie zum Beispiel die Befehlszeilen.

Sie können dies entweder mit VirtualBox (oder VMWare) unter Windows tun oder direkt auf Ihrem Computer installieren. Als Referenz können Sie den Anweisungen online [bei Wikihow](http://www.wikihow.com/Install-Ubuntu-on-VirtualBox) folgen.

### Erforderliche Abhängigkeiten und Toolchain installieren

Um die Quellen für die Kreuzkompilierung zu erstellen, stellen Sie sicher, dass Sie die erforderlichen Abhängigkeiten auf Ihrem Computer haben, indem Sie Folgendes ausführen:
```bash
sudo apt install git bc bison flex libssl-dev make libc6-dev libncurses5-dev
```

Wenn Sie feststellen, dass Sie andere Dinge benötigen, senden Sie bitte einen Pull-Request, um die Dokumentation zu ändern.

#### Installieren Sie die 32-Bit-Toolchain für einen 32-Bit-Kernel
```bash
sudo apt install crossbuild-essential-armhf
```

#### Oder installieren Sie die 64-Bit-Toolchain für einen 64-Bit-Kernel
```bash
sudo apt install crossbuild-essential-arm64
```

### Quellen abrufen

Um den minimalen Quellbaum für den aktuellen Zweig herunterzuladen, führen Sie Folgendes aus:

```bash
git clone --depth=1 https://github.com/raspberrypi/linux
```

Anweisungen zur Auswahl eines anderen Zweigs finden Sie oben unter [**Quellen auswählen**](#choosing_sources).

### Build erstellen

Geben Sie die folgenden Befehle ein, um die Build- und Gerätebaumdateien zu erstellen:

#### 32-Bit-Konfigurationen
Für Pi 1, Pi Zero, Pi Zero W oder Compute Module:

```bash
cd linux
KERNEL=kernel
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcmrpi_defconfig
```

Für Pi 2, Pi 3, Pi 3+ oder Compute-Modul 3:

```bash
cd linux
KERNEL=kernel7
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2709_defconfig
```

Für Raspberry Pi 4:

```bash
cd linux
KERNEL=kernel7l
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2711_defconfig
```

#### 64-Bit-Konfigurationen
Für Pi 3, Pi 3+ oder Compute Module 3:
```bash
cd linux
KERNEL=kernel8
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcmrpi3_defconfig
```

Für Raspberry Pi 4:
```bash
cd linux
KERNEL=kernel8
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcm2711_defconfig
```

#### Mit Konfigurationen erstellen

**Hinweis**: Um die Kompilierung auf Mehrprozessorsystemen zu beschleunigen und einige Verbesserungen auf Einzelprozessorsystemen zu erzielen, verwenden Sie `-j n`, wobei n die Anzahl der Prozessoren * 1,5 ist. Alternativ können Sie auch experimentieren und sehen, was funktioniert!

##### Für alle 32-Bit-Builds
```bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs
```

##### Für alle 64-Bit-Builds
**Hinweis**: Beachten Sie den Unterschied zwischen dem Image-Ziel zwischen 32 und 64-Bit.
```bash
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image modules dtbs
```

### Direkt auf die SD-Karte installieren

Nachdem Sie den Kernel erstellt haben, müssen Sie ihn auf Ihren Raspberry Pi kopieren und die Module installieren. Dies geschieht am besten direkt mit einem SD-Kartenleser.

Verwenden Sie zuerst `lsblk` vor und nach dem Einstecken Ihrer SD-Karte, um sie zu identifizieren. Sie sollten mit so etwas enden:

```
sdb
   sdb1
   sdb2
```

wobei `sdb1` die FAT (Boot) Partition ist und `sdb2` die ext4 Dateisystem (Root) Partition.

Wenn es sich um eine NOOBS-Karte handelt, sollten Sie etwa Folgendes sehen:

```
sdb
  sdb1
  sdb2
  sdb5
  sdb6
  sdb7
```

wobei `sdb6` die FAT (Boot) Partition ist und `sdb7` die ext4 Dateisystem (Root) Partition.

Mounten Sie diese zuerst und passen Sie die Partitionsnummern für NOOBS-Karten an (falls erforderlich):

```bash
mkdir mnt
mkdir mnt/fat32
mkdir mnt/ext4
sudo mount /dev/sdb6 mnt/fat32
sudo mount /dev/sdb7 mnt/ext4
```


Als nächstes installieren Sie die Kernel-Module auf der SD-Karte:

#### Für 32-Bit
```bash
sudo env PATH=$PATH make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=mnt/ext4 modules_install
```

#### Für 64-Bit
```bash
sudo env PATH=$PATH make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_PATH=mnt/ext4 modules_install
```

Kopieren Sie abschließend die Kernel- und Gerätebaum-Blobs auf die SD-Karte und stellen Sie sicher, dass Sie Ihren alten Kernel sichern:

#### Für 32-Bit

```bash
sudo cp mnt/fat32/$KERNEL.img mnt/fat32/$KERNEL-backup.img
sudo cp arch/arm/boot/zImage mnt/fat32/$KERNEL.img
sudo cp arch/arm/boot/dts/*.dtb mnt/fat32/
sudo cp arch/arm/boot/dts/overlays/*.dtb* mnt/fat32/overlays/
sudo cp arch/arm/boot/dts/overlays/README mnt/fat32/overlays/
sudo umount mnt/fat32
sudo umount mnt/ext4
```

#### Für 64-Bit

```bash
sudo cp mnt/fat32/$KERNEL.img mnt/fat32/$KERNEL-backup.img
sudo cp arch/arm64/boot/Image mnt/fat32/$KERNEL.img
sudo cp arch/arm64/boot/dts/broadcom/*.dtb mnt/fat32/
sudo cp arch/arm64/boot/dts/overlays/*.dtb* mnt/fat32/overlays/
sudo cp arch/arm64/boot/dts/overlays/README mnt/fat32/overlays/
sudo umount mnt/fat32
sudo umount mnt/ext4
```

Eine andere Möglichkeit besteht darin, den Kernel an dieselbe Stelle zu kopieren, jedoch mit einem anderen Dateinamen - zum Beispiel kernel-myconfig.img - anstatt die Datei kernel.img zu überschreiben. Sie können dann die Datei config.txt bearbeiten, um den Kernel auszuwählen, in den der Pi booten soll:

```
kernel=kernel-myconfig.img
```

Dies hat den Vorteil, dass Ihr Kernel getrennt von dem vom System und allen automatischen Update-Tools verwalteten Kernel-Image bleibt und Sie problemlos auf einen Standard-Kernel zurückgreifen können, falls Ihr Kernel nicht booten kann.

Schließlich stecken Sie die Karte in den Pi und booten ihn!