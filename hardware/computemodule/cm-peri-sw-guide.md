# Handbuch zum Anschließen und Aktivieren von Computermodulen

** Beachten Sie, dass diese Anweisungen, sofern nicht ausdrücklich anders angegeben, auf Compute Module und Compute Module 3 Module+IO Board(s) identisch funktionieren. **

## Einführung

Dieses Handbuch soll Entwicklern, die das Compute Module (und Compute Module 3) verwenden, dabei helfen, sich mit der Verdrahtung von Peripheriegeräten mit den Compute Module-Pins vertraut zu machen und Änderungen an der Software vorzunehmen, damit diese Peripheriegeräte ordnungsgemäß funktionieren.

Das Compute Module (CM) und Compute Module 3 (CM3) enthalten das Raspberry Pi BCM2835 (oder BCM2837 für CM3) System auf einem Chip (SoC) oder 'Prozessor', Speicher und eMMC. Die eMMC ähnelt einer SD-Karte, ist aber auf die Platine gelötet. Im Gegensatz zu SD-Karten wurde die eMMC speziell für die Verwendung als Datenträger entwickelt und verfügt über zusätzliche Funktionen, die sie in diesem Anwendungsfall zuverlässiger machen. Die meisten Pins des SoCs (GPIO, zwei CSI-Kamera-Schnittstellen, zwei DSI-Display-Schnittstellen, HDMI etc.) sind frei verfügbar und können nach Belieben verdrahtet werden (oder können, wenn sie nicht verwendet werden, in der Regel unbeschaltet bleiben). Das Rechenmodul ist ein DDR2-SODIMM-Formfaktor-kompatibles Modul, daher sollte jeder DDR2-SODIMM-Sockel verwendet werden können (beachten Sie, dass die Pinbelegung NICHT mit einem tatsächlichen SODIMM-Speichermodul übereinstimmt).

Um das Rechenmodul verwenden zu können, muss ein Benutzer ein (relativ einfaches) „Motherboard“ entwerfen, das das Rechenmodul mit Strom versorgen kann (mindestens 3,3 V und 1,8 V) und die Pins mit den erforderlichen Peripheriegeräten für die Anwendung des Benutzers verbindet .

Raspberry Pi bietet ein minimales Motherboard für das Compute Module (als Compute Module IO Board oder CMIO Board bezeichnet), das das Modul mit Strom versorgt, den GPIO zu den Stiftleisten bringt und die Kamera- und Displayschnittstellen zu FFC-Anschlüssen bringt. Es bietet auch HDMI, USB und eine 'ACT'-LED sowie die Möglichkeit, die eMMC eines Moduls über USB von einem PC oder Raspberry Pi aus zu programmieren.

In diesem Handbuch wird zunächst der Boot-Prozess erklärt und wie der Gerätebaum verwendet wird, um angeschlossene Hardware zu beschreiben; Dies sind wesentliche Dinge, die beim Entwerfen mit dem Compute Module zu verstehen sind. Es bietet dann ein funktionierendes Beispiel für das Anschließen eines I2C- und eines SPI-Peripheriegeräts an ein CMIO (oder CMIO V3 für CM3) Board und das Erstellen der Gerätebaumdateien, die erforderlich sind, damit beide Peripheriegeräte unter Linux funktionieren, beginnend mit einem Vanilla Raspberry Pi OS-Image.

## BCM283x GPIOs

BCM283x verfügt über drei Bänke mit General-Purpose Input/Output (GPIO)-Pins: 28 Pins auf Bank 0, 18 Pins auf Bank 1 und 8 Pins auf Bank 2, was insgesamt 54 Pins ergibt. Diese Pins können als echte GPIO-Pins verwendet werden, d.h. Software kann sie als Ein- oder Ausgänge setzen, den Zustand lesen und/oder setzen und sie als Interrupts verwenden. Sie können auch auf "alternative Funktionen" wie I2C, SPI, I2S, UART, SD-Karte und andere eingestellt werden.

Auf einem Compute-Modul können sowohl Bank 0 als auch Bank 1 kostenlos verwendet werden. Bank 2 wird für eMMC- und HDMI-Hot-Plug-Erkennung und ACT-LED / USB-Boot-Steuerung verwendet.

Auf einem laufenden System ist es nützlich, sich den Zustand jedes der GPIO-Pins anzusehen (auf welche Funktion sie eingestellt sind und den Spannungspegel am Pin), damit Sie sehen können, ob das System wie erwartet eingerichtet ist. Dies ist besonders hilfreich, wenn Sie sehen möchten, ob ein Gerätebaum wie erwartet funktioniert, oder um einen Blick auf die Pin-Zustände während des Hardware-Debuggings zu werfen.

Raspberry Pi bietet das Paket "raspi-gpio", das ein Tool zum Hacken und Debuggen von GPIO ist (HINWEIS: Sie müssen es als Root ausführen).
So installieren Sie `raspi-gpio`:
```
sudo apt install raspi-gpio
```

Wenn `apt` das `raspi-gpio`-Paket nicht finden kann, müssen Sie zuerst ein Update durchführen:
```
sudo apt-Update
```

Um Hilfe zu `raspi-gpio` zu erhalten, führen Sie es mit dem `help`-Argument aus:
```
sudo raspi-gpio hilfe
```

Um beispielsweise die aktuelle Funktion und den Pegel aller GPIO-Pins anzuzeigen, verwenden Sie:
```
sudo raspi-gpio get
```

Beachten Sie, dass `raspi-gpio` mit dem `funcs`-Argument verwendet werden kann, um eine Liste aller unterstützten GPIO-Funktionen pro Pin zu erhalten. Es druckt eine Tabelle im CSV-Format aus. Die Idee ist, die Tabelle in eine `.csv`-Datei zu leiten und diese Datei dann z.B. Excel:
```
sudo raspi-gpio funcs > gpio-funcs.csv
```

## BCM283x Boot-Prozess

BCM283x-Geräte bestehen aus einer VideoCore-GPU und ARM-CPU-Kernen. Die GPU ist in der Tat ein System, das aus einem DSP-Prozessor und Hardwarebeschleunigern für Imaging, Videokodierung und -dekodierung, 3D-Grafik und Bildzusammenstellung besteht.

Bei BCM283x-Geräten ist es der DSP-Kern in der GPU, der zuerst bootet. Es ist für die allgemeine Einrichtung und Verwaltung vor dem Hochfahren des/der Haupt-ARM-Prozessor(s) verantwortlich.

Die BCM283x-Geräte, wie sie auf Raspberry Pi- und Compute-Module-Boards verwendet werden, haben einen dreistufigen Boot-Prozess:

1. Der GPU-DSP wird zurückgesetzt und führt Code aus einem kleinen internen ROM (dem Boot-ROM) aus. Der einzige Zweck dieses Codes besteht darin, einen Bootloader der zweiten Stufe über eine der externen Schnittstellen zu laden. Auf einem Raspberry Pi oder Compute Module sucht dieser Code zuerst nach einem Bootloader der zweiten Stufe auf der SD-Karte (eMMC); es erwartet, dass diese `bootcode.bin` heißt und sich auf der ersten Partition befindet (die FAT32 sein muss). Wenn keine SD-Karte gefunden wird oder `bootcode.bin` nicht gefunden wird, wartet das Boot-ROM im 'USB-Boot'-Modus und wartet darauf, dass ein Host ihm über die USB-Schnittstelle einen Bootloader der zweiten Stufe zur Verfügung stellt.

2. Der Bootloader der zweiten Stufe (`bootcode.bin` auf der SD-Karte oder `usbbootcode.bin` für USB-Boot) ist für das Einrichten der LPDDR2-SDRAM-Schnittstelle und verschiedene andere kritische Systemfunktionen sowie das Laden und Ausführen der Haupt-GPU-Firmware verantwortlich (genannt `start.elf`, wieder auf der primären SD-Kartenpartition).

3. `start.elf` übernimmt und ist verantwortlich für das weitere System-Setup und das Booten des ARM-Prozessor-Subsystems und enthält die Firmware, die auf den verschiedenen Teilen der GPU läuft. Es liest zuerst 'dt-blob.bin', um die anfänglichen GPIO-Pin-Zustände und GPU-spezifische Schnittstellen und Takte zu bestimmen, und analysiert dann 'config.txt'. Es lädt dann eine ARM-Gerätebaumdatei (z. B. `bcm2708-rpi-cm.dtb` für ein Rechenmodul) und alle in `config.txt` angegebenen Gerätebaum-Overlays, bevor das ARM-Subsystem gestartet und die Gerätebaumdaten an das Booten übergeben werden Linux Kernel.

## Gerätebaum

[Device Tree](http://www.devicetree.org/) ist eine spezielle Methode zur Codierung aller Informationen über die an ein System angeschlossene Hardware (und folglich erforderliche Treiber).

Auf einem Pi- oder Compute-Modul gibt es mehrere Dateien in der ersten FAT-Partition des SD/eMMC, die binäre 'Device Tree'-Dateien sind. Diese Binärdateien (normalerweise mit der Erweiterung `.dtb`) werden vom Gerätebaum-Compiler aus für Menschen lesbaren Textbeschreibungen (normalerweise Dateien mit der Erweiterung `.dts`) kompiliert.

Auf einem standardmäßigen Raspberry Pi OS-Image in der ersten (FAT) Partition finden Sie zwei verschiedene Arten von Gerätebaumdateien, eine wird nur von der GPU verwendet und der Rest sind Standard-ARM-Gerätebaumdateien für jedes der BCM283x-basierten Pi-Produkte:

* `dt-blob.bin` (wird von der GPU verwendet)
* `bcm2708-rpi-b.dtb` (verwendet für Pi-Modell A und B)
* `bcm2708-rpi-b-plus.dtb` (verwendet für Pi-Modell B+ und A+)
* `bcm2709-rpi-2-b.dtb` (verwendet für Pi 2 Modell B)
* `bcm2710-rpi-3-b.dtb` (verwendet für Pi 3 Modell B)
* `bcm2708-rpi-cm.dtb` (verwendet für Pi Compute Module)
* `bcm2710-rpi-cm3.dtb` (verwendet für Pi Compute Module 3)

HINWEIS `dt-blob.bin` existiert standardmäßig nicht, da in `start.elf` eine `default`-Version kompiliert ist, aber für Compute Module-Projekte ist es oft notwendig, eine `dt-blob.bin` ( die die standardmäßig eingebaute Datei überschreibt).

Beachten Sie, dass `dt-blob.bin` im kompilierten Gerätebaumformat vorliegt, aber nur von der GPU-Firmware gelesen wird, um Funktionen exklusiv für die GPU einzurichten - siehe unten.

Eine Anleitung zum Erstellen von dt-blob.bin finden Sie [hier](../../configuration/pin-configuration.md).
Eine umfassende Anleitung zum Linux-Gerätebaum für Raspberry Pi finden Sie [hier](../../configuration/device-tree.md).

Während des Bootens kann der Benutzer über den Parameter `device_tree` in `config.txt` einen bestimmten ARM-Gerätebaum spezifizieren, indem er beispielsweise die Zeile `device_tree=mydt.dtb` zu `config.txt` hinzufügt, wobei `mydt.dtb ` ist die zu ladende dtb-Datei anstelle einer der standardmäßigen ARM-dtb-Dateien. Ein Benutzer kann zwar einen vollständigen Gerätebaum für sein Compute Module-Produkt erstellen, die empfohlene Methode zum Hinzufügen von Hardware besteht jedoch in der Verwendung von Overlays (siehe nächster Abschnitt).

Zusätzlich zum Laden einer ARM-dtb unterstützt `start.elf` das Laden zusätzlicher Device Tree 'Overlays' über den `dtoverlay` Parameter in `config.txt`, zum Beispiel das Hinzufügen von so vielen `dtoverlay=myoverlay` Zeilen wie nötig wie Overlays zu `config.txt`, wobei darauf hingewiesen wird, dass Overlays in `/overlays` leben und das Suffix `-overlay.dtb` haben, zB `/overlays/myoverlay-overlay.dtb`. Overlays werden mit der Basis-dtb-Datei zusammengeführt, bevor die Daten beim Start an den Linux-Kernel übergeben werden.

Overlays werden verwendet, um dem Basis-dtb Daten hinzuzufügen, die (nominell) nicht boardspezifische Hardware beschreiben. Dazu gehören die verwendeten GPIO-Pins und deren Funktion sowie das/die angeschlossene(n) Gerät(e), damit die richtigen Treiber geladen werden können. Die Konvention ist, dass auf einem Raspberry Pi alle an die Bank0-GPIOs (den GPIO-Header) angeschlossene Hardware mit einem Overlay beschrieben werden soll. Auf einem Rechenmodul sollte die gesamte an die Bank0- und Bank1-GPIOs angeschlossene Hardware in einer Overlay-Datei beschrieben werden. Sie müssen diese Konventionen nicht befolgen: Sie können alle Informationen in eine einzige dtb-Datei packen, wie zuvor beschrieben, und `bcm2708-rpi-cm.dtb` ersetzen. Das Befolgen der Konventionen bedeutet jedoch, dass Sie ein "Standard" Raspberry Pi OS-Release verwenden können, mit seinem Standard-Basis-dtb und allen produktspezifischen Informationen, die in einem separaten Overlay enthalten sind. Gelegentlich kann sich der Basis-DTB ändern - normalerweise so, dass Overlays nicht zerstört werden -, weshalb die Verwendung eines Overlays empfohlen wird.

## dt-blob.bin

Wenn `start.elf` ausgeführt wird, liest es zuerst etwas namens `dt-blob.bin`. Dies ist eine spezielle Form des Gerätebaum-Blobs, der der GPU mitteilt, wie sie (zunächst) die GPIO-Pin-Zustände einrichtet, sowie alle Informationen über GPIOs/Peripheriegeräte, die von der GPU gesteuert (im Besitz) werden, anstatt über Linux verwendet zu werden der Arm. Zum Beispiel wird das Peripheriegerät Raspberry Pi Camera von der GPU verwaltet, und die GPU benötigt exklusiven Zugriff auf eine I2C-Schnittstelle, um mit ihr zu kommunizieren, sowie ein paar Steuerpins. I2C0 auf den meisten Pi-Boards und Compute-Modulen ist nominell für die exklusive GPU-Nutzung reserviert. Die Informationen, welche GPIO-Pins die GPU für I2C0 und zur Steuerung der Kamerafunktionen verwenden soll, stammen aus `dt-blob.bin`.

HINWEIS: Die `start.elf`-Firmware hat eine `eingebaute` Standard-`dt-blob.bin`, die verwendet wird, wenn keine `dt-blob.bin` im Stammverzeichnis der ersten FAT-Partition gefunden wird. Die meisten Compute-Modul-Projekte möchten ihre eigene benutzerdefinierte `dt-blob.bin` bereitstellen. Beachten Sie, dass `dt-blob.bin` angibt, welcher Pin für die HDMI-Hot-Plug-Erkennung ist, obwohl sich dies auf dem Compute-Modul nie ändern sollte. Es kann auch verwendet werden, um einen GPIO als GPCLK-Ausgang einzurichten und eine ACT-LED anzugeben, die die GPU beim Booten verwenden kann. Weitere Funktionen können in Zukunft hinzugefügt werden. Informationen zu `dt-blob.bin` finden Sie [hier](../../configuration/pin-configuration.md).

[minimal-cm-dt-blob.dts](minimal-cm-dt-blob.dts) ist ein Beispiel für eine `.dts`-Gerätebaumdatei, die die HDMI-Hot-Plug-Erkennung und die ACT-LED einrichtet und alle anderen GPIOs auf Eingänge mit Standard-Pulls.

Um `minimal-cm-dt-blob.dts` nach `dt-blob.bin` zu kompilieren, verwenden Sie den Device Tree Compiler `dtc`:
```
dtc -I dts -O dtb -o dt-blob.bin minimal-cm-dt-blob.dts
```

## ARM Linux-Gerätebaum

Nachdem `start.elf` `dt-blob.bin` gelesen und die anfänglichen Pin-Zustände und Takte eingerichtet hat, liest es `config.txt`, die viele andere Optionen für die Systemeinrichtung enthält (siehe [hier](../. ./configuration/config-txt/README.md) für eine umfassende Anleitung).

Nach dem Lesen von `config.txt` wird eine weitere Gerätebaumdatei gelesen, die für das Board spezifisch ist, auf dem die Hardware läuft: dies ist `bcm2708-rpi-cm.dtb` für ein Compute Module oder `bcm2710-rpi-cm.dtb` für CM3. Diese Datei ist eine standardmäßige ARM-Linux-Gerätebaumdatei, die detailliert beschreibt, wie die Hardware an den Prozessor angeschlossen ist: Welche Peripheriegeräte sind im SoC vorhanden und wo, welche GPIOs werden verwendet, welche Funktionen haben diese GPIOs und welche physischen Geräte sind angeschlossen. Diese Datei richtet die GPIOs entsprechend ein und überschreibt den in `dt-blob.bin` eingerichteten Pin-Status, wenn er anders ist. Es wird auch versuchen, Treiber für das/die spezifische(n) Gerät(e) zu laden.

Obwohl die Datei `bcm2708-rpi-cm.dtb` zum Laden aller angeschlossenen Geräte verwendet werden kann, wird für Benutzer von Compute Module empfohlen, diese Datei in Ruhe zu lassen. Verwenden Sie stattdessen das im standardmäßigen Raspberry Pi OS-Software-Image enthaltene, und fügen Sie Geräte mit einer benutzerdefinierten "Overlay"-Datei wie zuvor beschrieben hinzu. Die Datei `bcm2708-rpi-cm.dtb` enthält (deaktivierte) Einträge für die verschiedenen Peripheriegeräte (I2C, SPI, I2S usw.) und keine GPIO-Pin-Definitionen, außer dem eMMC/SD-Karten-Peripheriegerät, das GPIO-Defs hat und aktiviert ist , weil es immer auf den gleichen Pins liegt. Die Idee ist, dass die separate Overlay-Datei die erforderlichen Schnittstellen aktiviert, die verwendeten Pins beschreibt und auch die erforderlichen Treiber beschreibt. Die `start.elf`-Firmware liest die `bcm2708-rpi-cm.dtb` und führt sie mit den Overlay-Daten zusammen, bevor sie den zusammengeführten Gerätebaum beim Booten an den Linux-Kernel weitergibt.

## Gerätebaumquelle und Kompilierung

Das Raspberry Pi OS-Image stellt kompilierte dtb-Dateien bereit, aber wo sind die Quell-dts-Dateien? Sie leben im Raspberry Pi Linux-Kernel-Zweig auf [GitHub](https://github.com/raspberrypi/linux). Schauen Sie im Ordner `arch/arm/boot/dts` nach.

Einige Standard-Overlay-dts-Dateien befinden sich in `arch/arm/boot/dts/overlays`. Entsprechende Overlays für Standard-Hardware, die an einen **Raspberry Pi** im Raspberry Pi OS-Image angehängt werden können, befinden sich auf der FAT-Partition im Verzeichnis `/overlays`. Beachten Sie, dass diese bestimmte Pins auf BANK0 annehmen, da sie für die Verwendung auf einem Raspberry Pi vorgesehen sind. Verwenden Sie im Allgemeinen die Quelle dieser Standard-Overlays als Leitfaden für die Erstellung eigener Overlays, es sei denn, Sie verwenden dieselben GPIO-Pins, die Sie verwenden würden, wenn die Hardware an den GPIO-Header eines Raspberry Pi angeschlossen wäre.

Das Kompilieren dieser dts-Dateien in dtb-Dateien erfordert eine aktuelle Version des Device Tree-Compilers `dtc`. Weitere Informationen finden Sie [hier](../../configuration/device-tree.md), aber der einfachste Weg, eine entsprechende Version auf einem Pi zu installieren, besteht darin, Folgendes auszuführen:
```
sudo apt install device-tree-compiler
```

Wenn Sie Ihren eigenen Kernel bauen, erhält der Build-Host auch eine Version in `scripts/dtc`. Sie können dafür sorgen, dass Ihre Overlays automatisch erstellt werden, indem Sie sie zu `Makefile` in `arch/arm/boot/dts/overlays` hinzufügen und das make-Ziel 'dtbs' verwenden.

## Gerätebaum-Debugging

Wenn der Linux-Kernel auf den ARM-Kernen gebootet wird, stellt die GPU ihm einen vollständig zusammengestellten Gerätebaum zur Verfügung, der aus den Basis-dts und allen Overlays zusammengesetzt ist. Dieser vollständige Baum ist über die Linux-Proc-Schnittstelle in `/proc/device-tree` verfügbar, wo Knoten zu Verzeichnissen und Eigenschaften zu Dateien werden.

Sie können `dtc` verwenden, um dies als menschenlesbare dts-Datei zum Debuggen auszuschreiben. Sie können den fertig aufgebauten Gerätebaum sehen, der oft sehr nützlich ist:
```
dtc -I fs -O dts -o proc-dt.dts /proc/device-tree
```

Wie bereits im GPIO-Abschnitt erklärt, ist es auch sehr nützlich, `raspi-gpio` zu verwenden, um sich das Setup der GPIO-Pins anzusehen, um zu überprüfen, ob sie so sind, wie Sie es erwarten:
```
raspi-gpio bekommen
```

Wenn etwas schief zu laufen scheint, können Sie auch nützliche Informationen finden, indem Sie die GPU-Log-Meldungen ausgeben:
```
sudo vcdbg log msg
```

Sie können weitere Diagnosen in die Ausgabe einfügen, indem Sie `dtdebug=1` zu `config.txt` hinzufügen.

## Hilfe bekommen

Bitte verwenden Sie das [Device Tree subforum](https://www.raspberrypi.org/forums/viewforum.php?f=107) in den Raspberry Pi-Foren, um Fragen zum Device Tree zu stellen.

## Beispiele

Für diese einfachen Beispiele habe ich ein CMIO-Board mit Peripheriegeräten verwendet, die über Jumper-Drähte angeschlossen sind.

Für jedes der Beispiele gehen wir von einem CM+CMIO- oder CM3+CMIO3-Board mit einer Neuinstallation der neuesten Raspberry Pi OS Lite-Version auf dem CM aus. Siehe Anleitung [hier](cm-emmc-flashing.md).

Die Beispiele hier erfordern eine Internetverbindung, daher wird ein USB-Hub plus Tastatur plus WLAN- oder Ethernet-Dongle empfohlen, der an den CMIO-USB-Port angeschlossen ist.

Bitte poste alle Probleme, Bugs oder Fragen im Raspberry Pi [Device Tree Subforum](https://www.raspberrypi.org/forums/viewforum.php?f=107).

## Beispiel 1 - Anschließen einer I2C-RTC an BANK1-Pins

In diesem einfachen Beispiel verdrahten wir eine NXP PCF8523 Echtzeituhr (RTC) mit den GPIO-Pins der CMIO-Platine BANK1: 3V3, GND, I2C1_SDA auf GPIO44 und I2C1_SCL auf GPIO45.

Laden Sie [minimal-cm-dt-blob.dts](minimal-cm-dt-blob.dts) herunter und kopieren Sie es auf die FAT-Partition der SD-Karte, die sich in `/boot` befindet, wenn der CM gebootet hat.

Bearbeiten Sie `minimal-cm-dt-blob.dts` und ändern Sie die Pin-Zustände von GPIO44 und 45 mit Pullups auf I2C1:
```
sudo nano /boot/minimal-cm-dt-blob.dts
```

Zeilen ändern:
```
pin@p44 { Funktion = "Eingabe"; Beendigung = "pull_down"; }; // STANDARDZUSTAND WAR INPUT NO PULL
pin@p45 { Funktion = "Eingang"; Beendigung = "pull_down"; }; // STANDARDZUSTAND WAR INPUT NO PULL
```

zu:
```
pin@p44 { Funktion = "i2c1"; Beendigung = "pull_up"; }; // SDA1
pin@p45 { Funktion = "i2c1"; Beendigung = "pull_up"; }; // SCL1
```

HINWEIS: Wir könnten diese `dt-blob.dts` ohne Änderungen verwenden. blob.dts`. Ich konfiguriere `dt-blob.dts` gerne so, wie ich es für die endgültigen GPIOs erwarte, da sie dann während der GPU-Boot-Phase so schnell wie möglich in ihren endgültigen Zustand versetzt werden, aber dies ist nicht unbedingt erforderlich. In einigen Fällen müssen Sie möglicherweise Pins beim Booten der GPU konfigurieren, sodass sie sich beim Laden von Linux-Treibern in einem bestimmten Zustand befinden. Beispielsweise muss eine Rücksetzleitung möglicherweise in der richtigen Ausrichtung gehalten werden.

Kompilieren Sie `dt-blob.bin`:
```
sudo dtc -I dts -O dtb -o /boot/dt-blob.bin /boot/minimal-cm-dt-blob.dts
```

Schnappen Sie sich [example1-overlay.dts](example1-overlay.dts) und legen Sie es in `/boot` ab und kompilieren Sie es:
```
sudo dtc -@ -I dts -O dtb -o /boot/overlays/example1.dtbo /boot/example1-overlay.dts
```
Beachten Sie das '-@' in der `dtc`-Befehlszeile. Dies ist notwendig, wenn Sie dts-Dateien mit externen Referenzen kompilieren, wie dies bei Overlays der Fall ist.

Bearbeiten Sie `/boot/config.txt` und fügen Sie die Zeile hinzu:
```
dtoverlay=Beispiel1
```

Jetzt speichern und neu starten.

Nach dem Neustart sollten Sie einen rtc0-Eintrag in /dev sehen. Laufen:
```
sudo hwclock
```

wird mit der Hardware-Uhrzeit zurückkehren und kein Fehler.

## Beispiel 2 - Anschließen eines ENC28J60 SPI-Ethernet-Controllers an BANK0

In diesem Beispiel verwenden wir eines der bereits verfügbaren Overlays in /boot/overlays, um BANK0 einen ENC28J60 SPI-Ethernet-Controller hinzuzufügen. Der Ethernet-Controller ist mit den SPI-Pins CE0, MISO, MOSI und SCLK (jeweils GPIO8-11) sowie GPIO25 für einen fallenden Flanken-Interrupt und natürlich GND und 3V3 verbunden.

In diesem Beispiel werden wir `dt-blob.bin` nicht ändern, obwohl Sie dies natürlich tun können, wenn Sie es wünschen. Wir sollten sehen, dass der Linux-Gerätebaum die Pins korrekt einrichtet.

Bearbeiten Sie `/boot/config.txt` und fügen Sie die Zeile hinzu:
```
dtoverlay=enc28j60
```

Jetzt speichern und neu starten.

Nach dem Neustart sollten Sie wie zuvor einen rtc0-Eintrag in /dev sehen. Laufen:
```
sudo hwclock
```

wird mit der Hardware-Uhrzeit zurückkehren und kein Fehler.

Sie sollten auch über eine Ethernet-Konnektivität verfügen:
```
ping 8.8.8.8
```

sollte arbeiten.

endlich läuft:
```
sudo raspi-gpio get
```

sollte zeigen, dass GPIO8-11 auf ALT0 (SPI)-Funktionen geändert wurde.

## Anbringen einer Kamera oder Kameras
Um eine Kamera oder Kameras anzuschließen, siehe die Dokumentation [hier](cmio-camera.md)