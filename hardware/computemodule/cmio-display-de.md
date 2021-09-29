# Anbringen des offiziellen Raspberry Pi 7-Zoll-Displays an die E/A-Platine des Rechenmoduls

**Dieses Dokument ist in Arbeit und richtet sich an fortgeschrittene Benutzer.**

Damit das Display mit dem Compute Module funktioniert, muss die Firmware vom 23. Oktober 2015 oder höher sein (verwenden Sie `vcgencmd version` zum Überprüfen). Damit das Display mit dem Compute Module 3 funktioniert, muss die Firmware von Oktober 2016 oder höher sein.

**Hinweis:** Das Raspberry Pi Zero Kamerakabel kann nicht als Alternative zum RPI-DISPLAY-Adapter verwendet werden, da die Verkabelung anders ist.

## Schnellstart — nur Anzeige

1. Verbinden Sie das Display mit dem DISP1-Port auf der E/A-Platine des Rechenmoduls über den 22-W-zu-15-W-Anzeigeadapter.
1. Verbinden Sie diese Pins mit Überbrückungsdrähten:

```
GPIO0 - CD1_SDA
GPIO1 - CD1_SCL
```

1. Führen Sie auf dem Rechenmodul Folgendes aus:

```sudo wget https://goo.gl/iiVxuA -O /boot/dt-blob.bin```

1. Starten Sie neu, damit die Datei `dt-blob.bin` gelesen werden kann.

## Schnellstart — Display und Kamera(s)
Dadurch werden `disp1` und `cam1` aktiviert, mit der Option, `cam0` zu aktivieren.

1. Verbinden Sie das Display mit dem DISP1-Port auf der E/A-Platine des Rechenmoduls über den 22-W-zu-15-W-Anzeigeadapter, genannt RPI-DISPLAY.
1. Verbinden Sie das Kameramodul mit dem CAM1-Port auf der E/A-Platine des Rechenmoduls über den 22-W-zu-15-W-Adapter namens RPI-CAMERA. Alternativ kann das Kamerakabel Raspberry Pi Zero verwendet werden.
1. (Optional) Verbinden Sie das Kameramodul mit dem CAM0-Port auf der E/A-Platine des Rechenmoduls über den 22-W-zu-15-W-Adapter namens RPI-CAMERA. Alternativ kann das Kamerakabel Raspberry Pi Zero verwendet werden.
1. Verbinden Sie diese Pins mit Überbrückungsdrähten:

```
GPIO0 - CD1_SDA
GPIO1 - CD1_SCL
GPIO4 - CAM1_IO1
GPIO5 - CAM1_IO0
```
Bitte beachten Sie, dass sich die Verkabelung etwas von der auf der Kameraseite unterscheidet, da Sie die GPIO-Pins 4 und 5 anstelle von 2 und 3 verwenden.

1. Fügen Sie für `cam0` Links hinzu:

```
GPIO28 - CD0_SDA
GPIO29 - CD0_SCL
GPIO30 - CAM0_IO1
GPIO31 - CAM0_IO0
```

![GPIO-Verbindung für ein einzelnes Display und zwei Kameramodule](images/CMIO-Cam-Disp-GPIO.jpg)
(Bitte beachten Sie, dass dieses Bild aktualisiert werden muss, um zwei Kameramodule anzuzeigen, oder die zusätzlichen Überbrückungskabel entfernt werden müssen)

1. Führen Sie auf dem Rechenmodul für das Display und ein Kameramodul Folgendes aus:

```sudo wget https://goo.gl/gaqNrO -O /boot/dt-blob.bin```

  Führen Sie für das Display und zwei Kameramodule Folgendes aus:

```sudo wget https://goo.gl/htHv7m -O /boot/dt-blob.bin```

1. Starten Sie neu, damit die Datei `dt-blob.bin` gelesen werden kann.

![Kameravorschau auf dem 7-Zoll-Display](images/CMIO-Cam-Disp-Example.jpg)
(Bitte beachten Sie, dass dieses Bild aktualisiert werden muss, um zwei Kameramodule anzuzeigen, oder die zusätzlichen Überbrückungskabel entfernt werden müssen)

### Software-Unterstützung

Es ist keine zusätzliche Konfiguration erforderlich, um den Touchscreen zu aktivieren. Die Touch-Schnittstelle sollte die Arbeit der Box beenden, sobald der Bildschirm erfolgreich erkannt wurde.


###Quellen
- [dt-blob-disp1-only.dts](dt-blob-disp1-only.dts)
- [dt-blob-disp1-cam1.dts](dt-blob-disp1-cam1.dts)
- [dt-blob-disp1-cam2.dts](dt-blob-disp1-cam2.dts)