# Flashen des Compute Module eMMC

Das Rechenmodul verfügt über ein integriertes eMMC-Gerät, das an die primäre SD-Kartenschnittstelle angeschlossen ist. In dieser Anleitung wird erläutert, wie Sie Daten mithilfe eines Compute Module IO-Boards in den eMMC-Speicher schreiben.

Bitte lesen Sie auch den Abschnitt in den [Datenblätter des Rechenmoduls](datasheet.md)

## Schritte zum Flashen der eMMC auf einem Rechenmodul

Um das Compute Module eMMC zu flashen, benötigen Sie entweder ein Linux-System (ein Raspberry Pi wird empfohlen, oder Ubuntu auf einem PC) oder ein Windows-System (Windows 10 wird empfohlen). Für BCM2837 (CM3) wurde ein Fehler behoben, der den Mac betraf, so dass dies auch funktioniert.

**Hinweis** Es gibt einen Fehler im Bootloader BCM2835 (CM1), der ein leicht falsches USB-Paket an den Host zurückgibt. Die meisten USB-Hosts scheinen diesen gutartigen Fehler zu ignorieren und funktionieren gut; Wir sehen jedoch einige USB-Anschlüsse, die aufgrund dieses Fehlers nicht funktionieren. Wir verstehen nicht ganz, warum einige Ports ausfallen, da es anscheinend nicht damit zu tun hat, ob es sich um USB2 oder USB3 handelt (wir haben gesehen, dass beide Typen funktionieren), aber es ist wahrscheinlich spezifisch für den Host-Controller und -Treiber. Dieser Fehler wurde in BCM2837 behoben.

**Für Windows-Benutzer**

Unter Windows steht ein Installer zur Verfügung, um die erforderlichen Treiber und das Boot-Tool automatisch zu installieren. Alternativ kann ein Benutzer es mit Cygwin kompilieren und ausführen und/oder die Treiber manuell installieren.

### Windows Installer

Für diejenigen, die das Compute Module eMMC nur als Massenspeichergerät unter Windows aktivieren möchten, ist das eigenständige Installationsprogramm die empfohlene Option. Dieses Installationsprogramm wurde unter Windows 10 32-Bit und 64-Bit und Windows XP 32-Bit getestet.

Bitte stellen Sie sicher, dass Sie nicht auf USB-Geräte schreiben, während das Installationsprogramm ausgeführt wird.

1. Laden Sie das [Windows-Installationsprogramm](https://github.com/raspberrypi/usbboot/raw/master/win32/rpiboot_setup.exe) herunter und führen Sie es aus, um die Treiber und das Boot-Tool zu installieren.
1. Stecken Sie den USB-Anschluss Ihres Host-PCs in den CMIO USB SLAVE-Anschluss und stellen Sie sicher, dass J4 auf die Position EN eingestellt ist.
1. Schalten Sie die CMIO-Platine ein; Windows sollte nun die Hardware finden und den Treiber installieren.
1. Führen Sie nach Abschluss der Treiberinstallation das zuvor installierte Tool „RPiBoot.exe“ aus.
1. Nach einigen Sekunden erscheint das Compute Module eMMC unter Windows als Diskette (USB-Massenspeichergerät).

### Einrichten der E/A-Platine des Rechenmoduls

### Rechenmodul 4
Stellen Sie sicher, dass das Rechenmodul korrekt auf der E/A-Platine installiert ist. Es sollte flach auf dem IO-Board aufliegen.

* Stellen Sie sicher, dass `nRPI_BOOT`, das J2 (`disable eMMC Boot`) auf der IO-Platine ist, auf die Position 'EN' gesetzt ist.
* Verwenden Sie ein Micro-USB-Kabel, um den Micro-USB-Slave-Port J11 auf der IO-Platine mit dem Hostgerät zu verbinden.
* Noch nicht einschalten.

### Rechenmodul 1..3
Stellen Sie sicher, dass das Rechenmodul selbst korrekt auf der E/A-Platine installiert ist. Es sollte parallel zur Platine liegen, wobei die Rastklammern eingerastet sind.

* Stellen Sie sicher, dass J4 (USB SLAVE BOOT ENABLE) auf die Position 'EN' eingestellt ist.
* Verwenden Sie ein Micro-USB-Kabel, um den Micro-USB-Slave-Port J15 auf der IO-Platine mit dem Hostgerät zu verbinden.
* Noch nicht einschalten.

### rpiboot auf Ihrem Hostsystem erstellen (Cygwin/Linux)

Wir werden Git verwenden, um den rpiboot-Quellcode zu erhalten, also stellen Sie sicher, dass Git installiert ist. Verwenden Sie in Cygwin das Cygwin-Installationsprogramm. Verwenden Sie auf einem Pi oder einem anderen Debian-basierten Linux-Computer den folgenden Befehl:

```bash
sudo apt installieren git
```

Git kann einen Fehler erzeugen, wenn das Datum nicht richtig eingestellt ist. Geben Sie auf einem Raspberry Pi Folgendes ein, um dies zu korrigieren:

```bash
sudo Datum MMDDhhmm
```

Dabei ist 'MM' der Monat, 'DD' das Datum und 'hh' und 'mm' sind Stunden bzw. Minuten.

Klonen Sie das `usbboot`-Tool-Repository:

```bash
git clone --depth=1 https://github.com/raspberrypi/usbboot
cd usbboot
```

`libusb` muss installiert sein. Wenn Sie Cygwin verwenden, stellen Sie bitte sicher, dass `libusb` wie zuvor beschrieben installiert ist. Geben Sie unter Raspberry Pi OS oder einem anderen Debian-basierten Linux den folgenden Befehl ein:

```bash
sudo apt install libusb-1.0-0-dev
```

Erstellen und installieren Sie nun das `usbboot`-Tool:

```bash
machen
```

Führen Sie das `usbboot`-Tool aus und es wird auf eine Verbindung warten:

```bash
sudo ./rpiboot
```

Stecken Sie nun den Host-Rechner in den USB-Slave-Port des Compute Module IO-Boards und schalten Sie das CMIO-Board ein. Das `rpiboot`-Tool erkennt das Compute-Modul und sendet Boot-Code, um den Zugriff auf die eMMC zu ermöglichen.

Für weitere Informationen laufen
```
./rpiboot -h
```

### Schreiben in die eMMC - Windows

Nachdem `rpiboot` abgeschlossen ist, wird ein neues USB-Massenspeicherlaufwerk in Windows angezeigt. Wir empfehlen, dieser [Anleitung] (../../installation/installing-images/windows.md) zu folgen und Win32DiskImager zu verwenden, um Bilder auf das Laufwerk zu schreiben, anstatt zu versuchen, `/dev/sda` usw. von Cygwin zu verwenden.

Stellen Sie sicher, dass J4 (USB SLAVE BOOT ENABLE) / J2 (nRPI_BOOT) deaktiviert ist und/oder nichts an den USB-Slave-Port angeschlossen ist. Das Aus- und Wiedereinschalten der IO-Platine sollte nun dazu führen, dass das Compute Module von eMMC bootet.

### Schreiben an die eMMC - Linux

Nachdem `rpiboot` abgeschlossen ist, wird ein neues Gerät angezeigt; Dies ist normalerweise `/dev/sda` auf einem Pi, aber es könnte ein anderer Ort sein wie `/dev/sdb`, also checken Sie `/dev/` ein oder führen Sie `lsblk` aus, bevor Sie `rpiboot` ausführen, damit Sie sehen können, was Änderungen.

Sie müssen nun ein rohes OS-Image (z. B. [Raspberry Pi OS](https://www.raspberrypi.org/downloads/raspbian/)) auf das Gerät schreiben. Beachten Sie, dass die Ausführung des folgenden Befehls je nach Größe des Images einige Zeit in Anspruch nehmen kann: (Ändern Sie `/dev/sdX` in das entsprechende Gerät.)

```bash
sudo dd if=raw_os_image_of_your_choice.img of=/dev/sdX bs=4MiB
```

Nachdem das Bild geschrieben wurde, ziehen Sie den USB-Stecker ab und wieder ein. Sie sollten zwei Partitionen (für Raspberry Pi OS) in `/dev` sehen. Insgesamt sollten Sie etwas Ähnliches sehen:

```bash
/dev/sdX <- Gerät
/dev/sdX1 <- Erste Partition (FAT)
/dev/sdX2 <- Zweite Partition (Linux-Dateisystem)
```

Die Partitionen `/dev/sdX1` und `/dev/sdX2` können nun normal gemountet werden.

Stellen Sie sicher, dass J4 (USB SLAVE BOOT ENABLE) / J2 (nRPI_BOOT) deaktiviert ist und/oder nichts an den USB-Slave-Port angeschlossen ist. Das Aus- und Wiedereinschalten der IO-Platine sollte nun dazu führen, dass das Compute Module von eMMC bootet.

## Bootloader EEPROM flashen - Compute Module 4
Das `rpiboot`-Tool ist die empfohlene Methode zum Aktualisieren des Bootloader-EEPROM auf Compute Module 4. Nachdem Sie die anfänglichen EMMC-Flashing-Setup-Schritte ausgeführt haben, führen Sie den folgenden Befehl aus, um das `recovery`-Image anstelle des EMMC-Image auszuführen.

```bash
./rpiboot -d Wiederherstellung
```

Das `recovery`-Verzeichnis des `rpiboot`-Tools enthält ein standardmäßiges `pieeprom.bin`-Bootloader-EEPROM-Image. Weitere Informationen zum Ändern der eingebetteten Konfigurationsdatei finden Sie auf den Seiten [boot EEPROM](../raspberrypi/booteeprom.md) und [bootloader configuration](../raspberrypi/bcm2711_bootloader_config.md).

Die SHA256-Prüfsummendatei muss mit dem Image `pieeprom.bin` übereinstimmen. Zum Generieren der `.sig`-Datei ausführen

```bash
sha256sum pieeprom.bin | awk '{print $2}' > pieeprom.sig
````

Das Bootloader-Image im Verzeichnis `recovery` ist das neueste Hersteller-Image mit Standardeinstellungen. Es ist für die Verwendung auf einem [Compute Module 4 IO Board](https://www.raspberrypi.org/products/compute-module-4-io-board) mit Raspberry Pi OS, das von SD/EMMC als Compute Module bootet, vorgesehen 4 Entwicklungsplattform.


## Fehlerbehebung

Für einen kleinen Prozentsatz von Raspberry Pi Compute Module 3s wurden Bootprobleme gemeldet. Wir haben diese auf die Methode zurückgeführt, mit der die FAT32-Partition erstellt wurde. Wir glauben, dass das Problem auf einen Unterschied im Timing zwischen dem BCM2835/6/7 und den neueren eMMC-Geräten zurückzuführen ist. Die folgende Methode zum Erstellen der Partition ist in unseren Händen eine zuverlässige Lösung.

```bash
$ sudo geteilt /dev/<Gerät>
(getrennt) mkpart primäres Fett32 4MiB 64MiB
(getrennt) q
$ sudo mkfs.vfat -F32 /dev/<Gerät>
$ sudo cp -r <Dateien>/* <Einhängepunkt>
```