# NOOBS

**New Out Of Box Software (NOOBS)** ist ein einfacher Betriebssystem-Installationsmanager für den Raspberry Pi.

![NOOBS OS Auswahl](images/noobs.png)

## Wie bekomme ich NOOBS

### Kaufen Sie eine vorinstallierte SD-Karte

SD-Karten mit vorinstalliertem NOOBS sind bei vielen unserer Distributoren und unabhängigen Einzelhändlern erhältlich, darunter [Pimoroni](https://shop.pimoroni.com/products/noobs-8gb-sd-card), [Adafruit](https://www.adafruit.com/products/1583) und [Pi Hut](http://thepihut.com/collections/raspberry-pi-sd-cards-and-adapters/products/noobs-preinstalled-sd-card).

### Herunterladen

Alternativ steht NOOBS zum Download auf der Raspberry Pi-Website zur Verfügung: [raspberrypi.org/downloads](https://www.raspberrypi.org/downloads/)

#### So installieren Sie NOOBS auf einer SD-Karte

Nachdem Sie die NOOBS-Zip-Datei heruntergeladen haben, müssen Sie den Inhalt auf eine formatierte SD-Karte auf Ihrem Computer kopieren.

So richten Sie eine leere SD-Karte mit NOOBS ein:

- Formatieren Sie eine SD-Karte als FAT. Siehe die Anweisungen unten.
  - Ihre SD-Karte muss mindestens 16 GB für das vollständige Raspberry Pi-Betriebssystem oder mindestens 8 GB für alle anderen Installationen haben.
- Laden Sie die Dateien aus der NOOBS-Zip-Datei herunter und extrahieren Sie sie.
- Kopieren Sie die extrahierten Dateien auf die gerade formatierte SD-Karte, sodass sich diese Dateien im Stammverzeichnis der SD-Karte befinden. Bitte beachten Sie, dass die Dateien in einigen Fällen in einen Ordner extrahiert werden können; Wenn dies der Fall ist, kopieren Sie bitte die Dateien aus dem Ordner und nicht aus dem Ordner selbst.
- Beim ersten Booten wird die FAT-Partition "RECOVERY" automatisch auf ein Minimum reduziert und eine Liste der zur Installation verfügbaren Betriebssysteme wird angezeigt.

#### So formatieren Sie eine SD-Karte als FAT

**Hinweis:** Wenn Sie eine SD- (oder Micro-SD-)Karte mit einer Kapazität von mehr als 32 GB (d. h. 64 GB und mehr) formatieren, lesen Sie die separate Anleitung [SDXC-Formatierung](sdxc_formatting.md).

##### Windows

Wenn Sie ein Windows-Benutzer sind, empfehlen wir Ihnen, Ihre SD-Karte mit dem Formatting Tool der SD Association zu formatieren, das von [sdcard.org](https://www.sdcard.org/downloads/formatter_4/) heruntergeladen werden kann. Anweisungen zur Verwendung des Tools finden Sie auf derselben Website.

##### Mac OS

Das [Formatting Tool der SD Association](https://www.sdcard.org/downloads/formatter_4/) ist auch für Mac-Benutzer verfügbar, obwohl das standardmäßige Festplatten-Dienstprogramm von OS X auch in der Lage ist, die gesamte Festplatte zu formatieren. Wählen Sie dazu das SD-Karten-Volume aus und wählen Sie `Erase` im `MS-DOS`-Format.

##### Linux

Für Linux-Benutzer empfehlen wir `gparted` (oder die Kommandozeilen-Version `parted`). Norman Dunbar hat [Anleitungen](http://qdosmsq.dunbar-it.co.uk/blog/2013/06/noobs-for-raspberry-pi/) für Linux-Benutzer geschrieben.

## Was ist in NOOBS enthalten?

Die folgenden Betriebssysteme sind derzeit in NOOBS enthalten:

- [Raspberry Pi OS](https://www.raspberrypi.org)
- [LibreELEC](https://libreelec.tv/)
- [OSMC](https://osmc.tv/)
- [Recalbox](https://www.recalbox.com/)
- [Lakka](http://www.lakka.tv/)
- [RISC OS](https://www.riscosopen.org/wiki/documentation/show/Welcome%20to%20RISC%20OS%20Pi)
- [Screenly OSE](https://www.screenly.io/ose/)
- [Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot)
- [TLXOS](https://thinlinx.com/)

Ab NOOBS v1.3.10 (September 2014) ist in NOOBS standardmäßig nur noch Raspberry Pi OS installiert. Die anderen können mit einer Netzwerkverbindung installiert werden.

## NOOBS und NOOBS Lite

NOOBS ist in zwei Formen verfügbar: Offline- und Netzwerkinstallation oder nur Netzwerkinstallation.

Die Vollversion enthält Raspberry Pi OS, sodass es offline von der SD-Karte installiert werden kann, während die Verwendung von NOOBS Lite oder die Installation eines anderen Betriebssystems eine Internetverbindung erfordert.

Beachten Sie, dass das Betriebssystem-Image der Vollversion veraltet sein kann, wenn eine neue Version des Betriebssystems veröffentlicht wird. Wenn Sie jedoch mit dem Internet verbunden sind, wird Ihnen die Möglichkeit angezeigt, die neueste Version herunterzuladen, wenn eine neuere verfügbar ist.

## NOOBS-Entwicklung

### Neueste NOOBS-Version

Die neueste NOOBS-Version ist **v3.5.0**, veröffentlicht am **15. September 2020**.

(Ab NOOBS v1.4.0 teilt NOOBS Lite nur die ersten beiden Ziffern der Versionsnummer, also v1.4)

### NOOBS-Dokumentation

Eine umfassendere Dokumentation, einschließlich einer erweiterten Konfiguration von NOOBS, ist auf [GitHub](https://github.com/raspberrypi/noobs/blob/master/README.md) verfügbar.

### NOOBS-Quellcode

Siehe den NOOBS-Quellcode auf [GitHub](https://github.com/raspberrypi/noobs).