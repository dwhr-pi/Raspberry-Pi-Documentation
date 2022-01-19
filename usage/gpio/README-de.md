# GPIO

Ein leistungsstarkes Merkmal des Raspberry Pi ist die Reihe von GPIO-Pins (General Purpose Input/Output) entlang der Oberkante der Platine. Ein 40-poliger GPIO-Header ist auf allen aktuellen Raspberry Pi-Boards zu finden (auf Pi Zero und Pi Zero W unbestückt). Vor dem Pi 1 Model B+ (2014) bestanden Boards aus einem kürzeren 26-Pin-Header.

![GPIO-Pins](images/GPIO-Pinout-Diagram-2.png)

Jeder der GPIO-Pins kann (in der Software) als Eingangs- oder Ausgangspin bezeichnet und für eine Vielzahl von Zwecken verwendet werden.

![GPIO-Layout](images/GPIO.png)

**Hinweis: Die Nummerierung der GPIO-Pins ist nicht in numerischer Reihenfolge; Die GPIO-Pins 0 und 1 sind auf der Platine vorhanden (physische Pins 27 und 28), sind jedoch für die erweiterte Verwendung reserviert (siehe unten).**

## Spannungen

Auf der Platine befinden sich zwei 5V-Pins und zwei 3V3-Pins sowie eine Reihe von Erdungspins (0V), die nicht konfigurierbar sind. Die restlichen Pins sind alle Allzweck-3V3-Pins, was bedeutet, dass die Ausgänge auf 3V3 eingestellt sind und die Eingänge 3V3-tolerant sind.

## Ausgänge

Ein als Ausgangspin bezeichneter GPIO-Pin kann auf High (3V3) oder Low (0V) gesetzt werden.

## Eingänge

Ein als Eingangspin bezeichneter GPIO-Pin kann als High (3V3) oder Low (0V) gelesen werden. Dies wird durch die Verwendung interner Pull-up- oder Pull-down-Widerstände erleichtert. Die Pins GPIO2 und GPIO3 haben feste Pull-up-Widerstände, aber für andere Pins kann dies in der Software konfiguriert werden.

## Mehr

Neben einfachen Ein- und Ausgabegeräten können die GPIO-Pins mit einer Vielzahl alternativer Funktionen verwendet werden, einige sind auf allen Pins verfügbar, andere auf bestimmten Pins.

- PWM (Pulsweitenmodulation)
    - Software-PWM auf allen Pins verfügbar
    - Hardware-PWM verfügbar auf GPIO12, GPIO13, GPIO18, GPIO19
-SPI
    - SPI0: MOSI (GPIO10); MISO (GPIO9); SCLK (GPIO11); CE0 (GPIO8), CE1 (GPIO7)
    - SPI1: MOSI (GPIO20); MISO (GPIO19); SCLK (GPIO21); CE0 (GPIO18); CE1 (GPIO17); CE2 (GPIO16)
- I2C
    - Daten: (GPIO2); Uhr (GPIO3)
    - EEPROM-Daten: (GPIO0); EEPROM-Uhr (GPIO1)
- Seriell
    -TX (GPIO14); Empfang (GPIO15)

## GPIO-Pinbelegung

Es ist wichtig zu wissen, welcher Pin welcher ist. Einige Leute verwenden Pin-Etiketten (wie die Platine [RasPiO Portsplus](http://rasp.io/portsplus/) oder das druckbare [Raspberry Leaf](https://github.com/splitbrain/rpibplusleaf)).

![](images/raspio-portsplus.jpg)

Auf dem Raspberry Pi kann auf eine praktische Referenz zugegriffen werden, indem ein Terminalfenster geöffnet und der Befehl "Pinout" ausgeführt wird. Dieses Tool wird von der [GPIO Zero](https://gpiozero.readthedocs.io/)-Python-Bibliothek bereitgestellt, die standardmäßig auf dem Raspberry Pi OS-Desktop-Image installiert ist, aber nicht auf Raspberry Pi OS Lite.

![](images/gpiozero-pinout.png)

Weitere Einzelheiten zu den erweiterten Fähigkeiten der GPIO-Pins finden Sie im [interaktiven Pinout-Diagramm](http://pinout.xyz/) von Gadgetoid.

## Programmierung mit GPIO

Es ist möglich, GPIO-Pins mit einer Reihe von Programmiersprachen und Tools zu steuern. Sehen Sie sich die folgenden Anleitungen an, um loszulegen:

- [GPIO mit Scratch 1.4](scratch1/README.md)
- [GPIO mit Scratch 2](scratch2/README.md)
- [GPIO mit Python](python/README.md)
- [GPIO mit C/C++ mit Standard-Kernel-Schnittstelle über libgpiod](https://kernel.googlesource.com/pub/scm/libs/libgpiod/libgpiod/+/v0.2.x/README.md)
- [GPIO mit C/C++ unter Verwendung der Fremdbibliothek Pigpio](http://abyz.me.uk/rpi/pigpio/)
- [GPIO mit Processing3](https://processing.org/reference/libraries/io/GPIO.html)

**Warnung: Während das Anschließen einfacher Komponenten an die GPIO-Pins absolut sicher ist, ist es wichtig, bei der Verkabelung vorsichtig zu sein. LEDs sollten Widerstände haben, um den durch sie fließenden Strom zu begrenzen. Verwenden Sie keine 5 V für 3V3-Komponenten. Schließen Sie Motoren nicht direkt an die GPIO-Pins an, sondern verwenden Sie stattdessen eine [H-Brückenschaltung oder eine Motorsteuerplatine](https://projects.raspberrypi.org/en/projects/physical-computing/16).**

## Berechtigungen

Um die GPIO-Ports nutzen zu können, muss Ihr Benutzer Mitglied der Gruppe „gpio“ sein. Der Benutzer „pi“ ist standardmäßig Mitglied, andere Benutzer müssen manuell hinzugefügt werden.

```bash
sudo usermod -a -G gpio <Benutzername>
```