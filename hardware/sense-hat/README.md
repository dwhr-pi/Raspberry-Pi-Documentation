# Sense HAT

## Installation

Um korrekt zu funktionieren, benötigt der Sense HAT einen aktuellen Kernel, aktiviertes I2C und einige Bibliotheken, um loszulegen.

1. Stellen Sie sicher, dass Ihre APT-Paketliste aktuell ist:

    ```bash
    sudo apt update
    ```

1. Als nächstes installieren Sie das Sense-Hat-Paket, das sicherstellt, dass der Kernel auf dem neuesten Stand ist, aktivieren Sie I2C und installieren Sie die erforderlichen Bibliotheken und Programme:

    ```bash
    sudo apt install sense-hat
    ```

1. Schließlich kann ein Neustart erforderlich sein, wenn I2C deaktiviert war oder der Kernel vor der Installation nicht auf dem neuesten Stand war:

    ```bash
    sudo reboot
    ```

## Hardware

Die Schaltpläne finden Sie [hier](images/Sense-HAT-V1_0.pdf).

## Softwareübersicht

Nach der Installation finden Sie Beispielcode unter `/usr/src/sense-hat/examples`.

Diese können durch Ausführen von `cp /usr/src/sense-hat/examples ~/ -a` in das Home-Verzeichnis des Benutzers kopiert werden.

Die C/C++-Beispiele können durch Ausführen von `make` im entsprechenden Verzeichnis kompiliert werden.

Das RTIMULibDrive11-Beispiel wird vorkompiliert geliefert, um sicherzustellen, dass alles wie vorgesehen funktioniert. Es kann durch Ausführen von `RTIMULibDrive11` gestartet und durch Drücken von `Strg+c` geschlossen werden.

### Python sense-hat

`sense-hat` ist die offiziell unterstützte Bibliothek für den Sense HAT; es bietet Zugriff auf alle On-Board-Sensoren und die LED-Matrix.

Die vollständige Dokumentation finden Sie unter [pythonhosted.org/sense-hat](https://pythonhosted.org/sense-hat/).

### RTIMULib

[RTIMULib](https://github.com/RPi-Distro/RTIMULib) ist eine C++- und Python-Bibliothek, die die Verwendung von 9-dof- und 10-dof-IMUs mit eingebetteten Linux-Systemen vereinfacht. Eine vorkalibrierte Einstellungsdatei wird in `/etc/RTIMULib.ini` bereitgestellt, die auch von `sense-hat` kopiert und verwendet wird. Die mitgelieferten Beispiele suchen im aktuellen Arbeitsverzeichnis nach `RTIMULib.ini`, daher können Sie die Datei dorthin kopieren, um genauere Daten zu erhalten.

### Sonstiges

#### LED-Matrix

Die LED-Matrix ist ein RGB565 [framebuffer](https://www.kernel.org/doc/Documentation/fb/framebuffer.txt) mit der ID "RPi-Sense FB". Der entsprechende Geräteknoten kann als Standarddatei oder mmap-ed beschrieben werden. Das mitgelieferte 'snake'-Beispiel zeigt, wie man auf den Framebuffer zugreift.

#### Joystick

Der Joystick erscheint als Eingabeereignisgerät namens "Raspberry Pi Sense HAT Joystick", das den Pfeiltasten und `Enter` zugeordnet ist. Es sollte von jeder Bibliothek unterstützt werden, die Eingaben verarbeiten kann, oder direkt über die [evdev interface](https://www.kernel.org/doc/Documentation/input/input.txt). Geeignete Bibliotheken sind SDL, [pygame](http://www.pygame.org/docs/) und [python-evdev](https://python-evdev.readthedocs.org/en/latest/). Das mitgelieferte 'snake'-Beispiel zeigt, wie Sie direkt auf den Joystick zugreifen.

## Kalibrierung

Aus diesem [Forumsbeitrag](https://www.raspberrypi.org/forums/viewtopic.php?f=104&t=109064&p=750616#p810193).

Installieren Sie die erforderliche Software und führen Sie das Kalibrierprogramm wie folgt aus:

````
sudo apt update
sudo apt install octave -y
cd
cp /usr/share/librtimulib-utils/RTEllipsoidFit ./ -a
cd RTEllipsoidFit
RTIMULibCal
````

Sie sehen dann dieses Menü:  

````
Options are:

  m - calibrate magnetometer with min/max
  e - calibrate magnetometer with ellipsoid (do min/max first)
  a - calibrate accelerometers
  x - exit

Enter option:
````

Drücken Sie das kleine `m`. Die folgende Meldung wird dann angezeigt; Drücken Sie eine beliebige Taste, um zu starten.

````
    Magnetometer min/max calibration
    --------------------------------
    Waggle the IMU chip around, ensuring that all six axes
    (+x, -x, +y, -y and +z, -z) go through their extrema.
    When all extrema have been achieved, enter 's' to save, 'r' to reset
    or 'x' to abort and discard the data.

    Press any key to start...
````

Nach dem Start sehen Sie etwas Ähnliches, wenn Sie den Bildschirm nach oben scrollen:
````
Min x:  51.60  min y:  69.39  min z:  65.91
Max x:  53.15  max y:  70.97  max z:  67.97
````

Konzentrieren Sie sich auf die beiden Zeilen ganz unten auf dem Bildschirm, da dies die zuletzt vom Programm veröffentlichten Messungen sind.
Jetzt müssen Sie den Astro Pi auf jede erdenkliche Weise bewegen. Es hilft, wenn Sie alle nicht notwendigen Kabel abziehen, um Unordnung zu vermeiden.

Versuchen Sie, einen vollständigen Kreis in jeder der Nick-, Roll- und Gierachsen zu erhalten. Achten Sie dabei darauf, dass Sie die SD-Karte nicht versehentlich auswerfen. Verbringen Sie ein paar Minuten damit, den Astro Pi zu bewegen, und hören Sie auf, wenn Sie feststellen, dass sich die Zahlen nicht mehr ändern.

Drücken Sie nun Kleinbuchstaben `s` und dann Kleinbuchstaben `x`, um das Programm zu beenden. Wenn Sie jetzt den Befehl `ls` ausführen, sehen Sie, dass eine neue Datei `RTIMULib.ini` erstellt wurde.

Zusätzlich zu diesen Schritten können Sie auch die Ellipsoidanpassung durchführen, indem Sie die obigen Schritte ausführen, aber 'e' anstelle von 'm' drücken.

Wenn Sie fertig sind, kopieren Sie die resultierende `RTIMULib.ini` nach /etc/ und entfernen Sie die lokale Kopie in `~/.config/sense_hat/`:

    rm ~/.config/sense_hat/RTIMULib.ini
    sudo cp RTIMULib.ini /etc

Sie sind jetzt fertig.

## Aktualisieren der AVR-Firmware

...

## EEPROM-Daten

*Diese Schritte funktionieren möglicherweise nicht auf Raspberry Pi 2 Model B Rev 1.0 und Raspberry Pi 3 Model B Boards. Die Firmware übernimmt die Kontrolle über I2C0, wodurch die ID-Pins als Eingänge konfiguriert werden.*

1. Aktivieren Sie I2C0 und I2C1, indem Sie die folgende Zeile zu `/boot/config.txt` hinzufügen:

    ```
    dtparam=i2c_vc=on
    dtparam=i2c_arm=on
    ```
    
1. Geben Sie zum Neustart den folgenden Befehl ein:

    ```bash
    sudo systemctl reboot
    ```
    
1. Laden Sie das Flash-Tool herunter und erstellen Sie es:

    ```bash
    git clone https://github.com/raspberrypi/hats.git
    cd hats/eepromutils
    make
    ```

### Lektüre

1. EEPROM-Daten können mit folgendem Befehl gelesen werden:

    ```bash
    sudo ./eepflash.sh -f=sense_read.eep -t=24c32 -r
    ```

### Schreiben

*Bitte beachten Sie, dass dieser Vorgang potenziell gefährlich ist und für den normalen Benutzer nicht erforderlich ist. Die folgenden Schritte dienen nur zu Debugging-Zwecken. Wenn ein Fehler auftritt, wird der HAT möglicherweise nicht mehr automatisch erkannt.*

1. Laden Sie die EEPROM-Einstellungen herunter und erstellen Sie die `.eep`-Binärdatei:

    ```bash
    wget https://github.com/raspberrypi/rpi-sense/raw/master/eeprom/eeprom_settings.txt -O sense_eeprom.txt
    ./eepmake sense_eeprom.txt sense.eep /boot/overlays/rpi-sense-overlay.dtb
    ```

1. Schreibschutz deaktivieren:

    ```bash
    i2cset -y -f 1 0x46 0xf3 1
    ```

1. Schreiben Sie die EEPROM-Daten:

    ```bash
    sudo ./eepflash.sh -f=sense.eep -t=24c32 -w

    ```
    
1. Schreibschutz wieder aktivieren:

    ```bash
    i2cset -y -f 1 0x46 0xf3 0
    ```