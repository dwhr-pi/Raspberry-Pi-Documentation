# Anschließen eines Raspberry Pi-Kameramoduls an die E/A-Platine des Rechenmoduls

##Hinweise vor dem Start

Diese Anweisungen sind für erfahrene Benutzer gedacht. Bei Unklarheiten verwenden Sie bitte die [Raspberry Pi Camera-Foren] (https://www.raspberrypi.org/forums/viewforum.php?f=43) für technische Hilfe.

Sofern nicht ausdrücklich anders angegeben, funktionieren diese Anweisungen sowohl auf dem Rechenmodul als auch auf dem Rechenmodul 3, die an eine Rechenmodul-E/A-Platine angeschlossen sind, identisch.

### Fähigkeiten der Rechenmodulplatine

Das Compute Module verfügt über zwei CSI-2-Kameraschnittstellen. CAM0 ist eine zweispurige Schnittstelle, CAM1 ist eine vierspurige Schnittstelle. Das Compute Module IO-Board stellt diese beiden Schnittstellen zur Verfügung. Beachten Sie, dass die Standard-Raspberry-Pi-Geräte CAM1 verwenden, aber nur zwei Lanes verfügbar machen.

### Aktualisieren Ihres Systems

Die Kamerasoftware wird ständig weiterentwickelt. Bitte stellen Sie sicher, dass Ihr System auf dem neuesten Stand ist, bevor Sie diese Anleitung verwenden.

```
sudo apt-Update
sudp apt Voll-Upgrade
```

###Kryptochip

Bei Verwendung des Compute-Moduls zum Ansteuern von Kameras ist es NICHT erforderlich, den Krypto-Chip zu integrieren, der auf den von Raspberry Pi entwickelten Kameraplatinen verwendet wird, wenn die OM5647-, IMX219- oder HQ-Kameramodule direkt an der Compute-Modul-Trägerplatine angebracht werden. Die Raspberry Pi-Firmware erkennt den CM automatisch und ermöglicht die Kommunikation mit dem Kameramodul, ohne dass der Krypto-Chip vorhanden ist.


## Schnellstart

1. Führen Sie auf dem Rechenmodul `sudo raspi-config` aus und aktivieren Sie die Kamera.
1. Führen Sie als nächstes `sudo wget http://goo.gl/U4t12b -O /boot/dt-blob.bin` . aus
1. Verbinden Sie die RPI-CAMERA-Platine und das Kameramodul mit dem CAM1-Port. Alternativ kann das Pi Zero Kamerakabel verwendet werden.

    ![Anschließen der Adapterplatine](images/CMAIO-Cam-Adapter.jpg)

1. Verbinden Sie die GPIO-Pins wie unten gezeigt miteinander.

    ![GPIO-Verbindung für eine einzelne Kamera](images/CMIO-Cam-GPIO.jpg)

1. (Optional) Um eine zusätzliche Kamera hinzuzufügen, wiederholen Sie Schritt 3 mit CAM0 und verbinden Sie die GPIO-Pins für die zweite Kamera.

    ![GPIO-Verbindung mit zusätzlicher Kamera](images/CMIO-Cam-GPIO2.jpg)

1. Starten Sie abschließend neu, damit die Datei dt-blob.bin gelesen werden kann.

### Software-Unterstützung

Die mitgelieferten Kameraanwendungen `raspivid` und `raspistill` verfügen über die Option -cs (--camselect), um anzugeben, welche Kamera verwendet werden soll.

Wenn Sie Ihre eigene Kameraanwendung basierend auf der MMAL-API schreiben, können Sie den Parameter MMAL_PARAMETER_CAMERA_NUM verwenden, um die aktuelle Kamera einzustellen. Z.B.

```
MMAL_PARAMETER_INT32_T camera_num = {{MMAL_PARAMETER_CAMERA_NUM, sizeof(camera_num)}, CAMERA_NUMBER};
status = mmal_port_parameter_set(camera->control, &camera_num.hdr);
```

## Fortschrittlich

Das Compute Module IO-Board verfügt über einen 22-Wege-0,5-mm-FFC für jeden Kameraport, wobei CAM0 eine zweispurige Schnittstelle und CAM1 die vollständige vierspurige Schnittstelle ist. Der Standard-Raspberry Pi verwendet ein 15-poliges 1-mm-FFC-Kabel, daher benötigen Sie entweder einen Adapter (Teilenummer RPI-CAMERA) oder ein Pi Zero-Kamerakabel.

Auf dem Compute Module IO Board ist es notwendig die vom Raspberry Pi OS benötigten GPIOs und I2C Schnittstelle mit dem CAM1 Anschluss zu überbrücken. Dies geschieht durch Verbinden der GPIOs vom J6 GPIO-Anschluss mit den CD1_SDA/SCL- und CAM1_IO0/1-Pins am J5-Anschluss mithilfe von Überbrückungsdrähten. Darüber hinaus muss eine Datei "dt-blob.bin" bereitgestellt werden, um die Standard-Pin-Zustände zu überschreiben (die Datei dt-blob.bin ist eine Datei, die der GPU mitteilt, welche Pins bei der Steuerung der Kamera verwendet werden sollen. Weitere Informationen hierzu finden Sie unter siehe den entsprechenden Abschnitt in der Anleitung zum Anschließen von Peripheriegeräten an ein Rechenmodul [hier](cm-peri-sw-guide.md)).

*Die unten aufgeführten Pin-Nummern dienen nur als Beispiel. LED- und SHUTDOWN-Pins können bei Bedarf von beiden Kameras gemeinsam genutzt werden.* Die SDA- und SCL-Pins müssen entweder GPIO0 und GPIO1 oder GPIO28 und 29 sein und müssen für jede Kamera individuell sein.

### Schritte zum Anschließen einer Raspberry Pi-Kamera (an CAM1)

1. Befestigen Sie den 0,5 mm 22 W FFC Flexi (im Lieferumfang der RPI-CAMERA-Platine enthalten) am CAM1-Anschluss (Flexkontakte zeigen nach unten). Alternativ kann das Pi Zero Kamerakabel verwendet werden.
1. Befestigen Sie die RPI-CAMERA-Adapterplatine am anderen Ende des 0,5-mm-Flex (Flexkontakte zeigen nach unten).
1. Befestigen Sie eine Raspberry Pi-Kamera an der anderen, größeren 15W 1mm FFC auf der RPI-CAMERA-Adapterplatine (**Kontakte auf dem Raspberry Pi-Kameraflex müssen nach oben zeigen**).
1. Verbinden Sie CD1_SDA (J6 Pin 37) mit GPIO0 (J5 Pin 1).
1. Verbinden Sie CD1_SCL (J6 Pin 39) mit GPIO1 (J5 Pin 3).
1. Verbinden Sie CAM1_IO1 (J6 Pin 41) mit GPIO2 (J5 Pin 5).
1. Verbinden Sie CAM1_IO0 (J6 Pin 43) mit GPIO3 (J5 Pin 7).

Beachten Sie, dass die Zahlen in Klammern konventionelle, physische Pin-Nummern sind, die von links nach rechts und von oben nach unten nummeriert sind. Die Nummern auf dem Siebdruck entsprechen den Broadcom SoC GPIO-Nummern.

### Standard-Pin-Zustände konfigurieren

Die GPIOs, die wir für die Kamera verwenden, verwenden standardmäßig den Eingabemodus des Rechenmoduls. Um [diese Standardeinstellungen zu überschreiben](../../configuration/pin-configuration.md) und dem System auch mitzuteilen, dass dies die Pins sind, die von der Kamera verwendet werden sollen, müssen wir eine `dt-blob.bin . erstellen `, das beim Hochfahren des Systems von der Firmware geladen wird. Diese Datei wird aus einer Quell-dts-Datei erstellt, die die erforderlichen Einstellungen enthält, und auf der Boot-Partition platziert.

Einige [Beispiel-Gerätebaum-Quelldateien](#Beispiel-Gerätebaum-Quelldateien) finden Sie am Ende dieses Dokuments.

Der Abschnitt "pin_config" im Abschnitt "pins_cm { }" (Rechenmodul) oder "pins_cm3 { }" (Rechenmodul3) der Quelle dts benötigt die LED- und Power-Enable-Pins der Kamera auf Ausgänge:

```
pin@p2 { Funktion = "Ausgabe"; Termination = "no_pulling"; };
pin@p3 { Funktion = "Ausgabe"; Termination = "no_pulling"; };
```

Um der Firmware mitzuteilen, welche Pins verwendet und nach wie vielen Kameras gesucht werden soll, fügen Sie dem Abschnitt `pin_defines` Folgendes hinzu:

```
pin_define@CAMERA_0_LED { type = "intern"; Zahl = <2>; };
pin_define@CAMERA_0_SHUTDOWN { type = "intern"; Zahl = <3>; };
pin_define@CAMERA_0_UNICAM_PORT { type = "intern"; Zahl = <1>; };
pin_define@CAMERA_0_I2C_PORT { type = "intern"; Zahl = <0>; };
pin_define@CAMERA_0_SDA_PIN { type = "intern"; Zahl = <0>; };
pin_define@CAMERA_0_SCL_PIN { type = "intern"; Zahl = <1>; };
```

### So befestigen Sie zwei Kameras

Schließen Sie die zweite Kamera wie zuvor an den (CAM0)-Anschluss an.

Schließen Sie die I2C- und GPIO-Leitungen an.

1. Verbinden Sie CD0_SDA (J6 Pin 45) mit GPIO28 (J6 Pin 1).
1. Verbinden Sie CD0_SCL (J6 Pin 47) mit GPIO29 (J6 Pin 3).
1. Verbinden Sie CAM0_IO1 (J6 Pin 49) mit GPIO30 (J6 Pin 5).
1. Verbinden Sie CAM0_IO0 (J6 Pin 51) mit GPIO31 (J6 Pin 7).

Im Abschnitt **pin_config** des Compute-Moduls müssen die LED- und Power-Enable-Pins der zweiten Kamera konfiguriert sein:

```
pin@p30 { Funktion = "Ausgabe"; Termination = "no_pulling"; };
pin@p31 { Funktion = "Ausgabe"; Termination = "no_pulling"; };
```

Ändern Sie im Abschnitt **pin_defines** des Compute-Moduls der dts-Datei den Parameter **NUM_CAMERAS** auf 2 und fügen Sie Folgendes hinzu:

```
pin_define@CAMERA_1_LED { type = "intern"; Zahl = <30>; };
pin_define@CAMERA_1_SHUTDOWN { type = "intern"; Zahl = <31>; };
pin_define@CAMERA_1_UNICAM_PORT { type = "intern"; Zahl = <0>; };
pin_define@CAMERA_1_I2C_PORT { type = "intern"; Zahl = <0>; };
pin_define@CAMERA_1_SDA_PIN { type = "intern"; Zahl = <28>; };
pin_define@CAMERA_1_SCL_PIN { type = "intern"; Zahl = <29>; };
```

<a name="sample-device-tree-source-files"></a>
### Beispiel-Gerätebaum-Quelldateien

[Nur CAM1 aktivieren](dt-blob-cam1.dts)

[Beide Kameras aktivieren](dt-blob-dualcam.dts)

### Kompilieren einer DTS-Datei zu einem Gerätebaum-Blob

Nachdem alle erforderlichen Änderungen an der `dts`-Datei vorgenommen wurden, muss sie kompiliert und auf der Boot-Partition des Geräts abgelegt werden.

Eine Anleitung dazu finden Sie auf der Seite [Pin Configuration](../../configuration/pin-configuration.md).