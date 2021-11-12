# _libcamera_ Installation für Raspberry Pi

## Vorbereitung Ihres Pi .s

Auf Ihrem Raspberry Pi sollte die neueste Version des Raspberry Pi-Betriebssystems (_Buster_ zum Zeitpunkt des Schreibens) ausgeführt werden und die Kamera- und I2C-Schnittstellen müssen beide aktiviert sein (überprüfen Sie die Registerkarte _Interfaces_ des Tools _Raspberry Pi Configuration_ aus dem Menü _Preferences_). . Stellen Sie zunächst sicher, dass Ihr System, Ihre Firmware und alle seine Anwendungen und Repositorys auf dem neuesten Stand sind, indem Sie die folgenden Befehle in ein Terminalfenster eingeben.

```bash
sudo apt update
sudo apt full-upgrade
```

Derzeit (Mai 2020) ist die notwendige _libcamera_-Unterstützung noch nicht in die Standardversion des Raspberry Pi OS eingebunden, daher ist es notwendig, den neuesten Release Candidate zu installieren. Starten Sie dazu zuerst Ihren Pi neu und verwenden Sie dann

```bash
sudo rpi-update
```

**WARNUNG**: Beachten Sie, dass der Release Candidate nicht so gründlich getestet wurde wie ein offizieller Release. Wenn Ihr Raspberry Pi wichtige oder kritische Daten enthält, empfehlen wir dringend, dass Sie zuerst ein Backup erstellen oder eine neue SD-Karte verwenden, um _libcamera_ auszuprobieren.

Als nächstes muss die Datei `/boot/config.txt` aktualisiert werden, um den Kameratreiber zu laden und zu verwenden, indem die folgenden Zeilen am Ende hinzugefügt werden. Derzeit müssen wir auch die `core_freq_min` der GPU aktualisieren, was jedoch nach weiteren Updates zu gegebener Zeit überflüssig wird.

```bash
dtoverlay=imx219
core_freq_min=250
```

Wenn Sie einen anderen Sensor als den `imx219` verwenden, müssen Sie hier den alternativen Namen angeben (z.

**HINWEIS**: Nach dem Neustart wird die Steuerung des Kamerasystems an die ARM-Kerne übergeben und Firmware-basierte Kamerafunktionen (wie Raspistill usw.) funktionieren nicht mehr. Das Zurücksetzen von `/boot/config.txt` und ein Neustart stellt das vorherige Verhalten wieder her.

## Softwareabhängigkeiten

Das Build-System und die Laufzeitumgebung von _libcamera_ haben eine Reihe von Abhängigkeiten. Sie können mit den folgenden Befehlen installiert werden.

```bash
sudo apt install libboost-dev
sudo apt install libgnutls28-dev openssl libtiff5-dev
sudo apt install meson
sudo apt install qtbase5-dev libqt5core5a libqt5gui5 libqt5widgets5
sudo pip3 install pyyaml
```

## _libcamera_ und _qcam_ aufbauen

Wir können nun den Code auschecken und den Build für den Raspberry Pi wie folgt konfigurieren.

```bash
git clone git://linuxtv.org/libcamera.git
cd libcamera
```

und um den Build zu konfigurieren (immer noch im selben _libcamera_-Verzeichnis):

```bash
meson build
cd build
meson configure -Dpipelines=raspberrypi -Dtest=false
cd ..
```

Schließlich sind wir bereit, den Quellcode zu erstellen.

```bash
sudo ninja -C build install
```

## Ein Bild aufnehmen

Bilder können mit der Anwendung _qcam_ aufgenommen werden, die aus dem Verzeichnis _libcamera_ gestartet werden kann, indem Sie Folgendes eingeben:

```bash
build/src/qcam/qcam
```

##Weitere Dokumentation

Weitere Informationen finden Sie im _Raspberry Pi Camera Algorithm and Tuning Guide_, [hier](rpi_SOFT_libcamera_1p0.pdf).

Informationen zum Schreiben eigener Kernel-Module zur Unterstützung neuer CSI-2-Kameras und Bridge-Chips finden Sie [hier](./csi-2-usage.md).