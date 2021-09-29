# Fehlerbehebung beim Raspberry Pi-Display

## Bevor du anfängst

### Hast du eine gute Stromversorgung?

Haben Sie zeitweilige Probleme oder sehen Sie ein kleines Regenbogenquadrat in der oberen rechten Ecke? Es ist wahrscheinlich, dass Sie eine bessere Stromversorgung benötigen.

Wir empfehlen unseren offiziellen 2,5-A-Adapter, weil wir wissen, dass er funktioniert, aber jede gute 2,5-A-Versorgung sollte funktionieren.

### Haben Sie Raspberry Pi OS aktualisiert?

Wenn nicht, werden viele Probleme gelöst, indem Sie sicherstellen, dass Ihre Software auf dem neuesten Stand ist.

Sie können jede vorherige Verwendung von `rpi-update` rückgängig machen und Ihren Pi durch Verbinden auf die neueste stabile Software zurücksetzen
zu einem Netzwerk und läuft:

```bash
sudo apt-Update
sudo apt install --reinstall libaspberrypi0 libaspberrypi-{bin,dev,doc} raspberrypi-bootloader
sudo neu starten
```

## Häufige Probleme

### Mein Touchscreen funktioniert nicht oder nur zeitweise

- Stellen Sie sicher, dass Sie das Raspberry Pi OS aktualisiert haben (siehe oben für Schritte)
- Überprüfen Sie, ob das kleinere Flachbandkabel richtig sitzt

Wenn Sie sicherstellen möchten, dass Ihr Pi Ihren Touchscreen erkannt hat, versuchen Sie Folgendes auszuführen:

```bash
dmesg | grep -i ft5406
```

Sie sollten ein paar Zeilen sehen, die wie folgt aussehen:

```Text
[ 5.224267] rpi-ft5406 rpi_ft5406: Tastgerät
[ 5.225960] input: FT5406 speicherbasierter Treiber als /devices/virtual/input/input3
```

Ein erkannter Touchscreen bewirkt auch, dass die Parameter `fbheight` und `fbwidth` in `/proc/cmdline` 480 bzw. 800 (die Auflösung des Bildschirms) entsprechen. Sie können dies überprüfen, indem Sie Folgendes ausführen:

```
cat /proc/cmdline | grep bcm2708_fb
```

### Mein Bildschirm steht auf dem Kopf!

Abhängig von Ihrem Display-Ständer können Sie feststellen, dass das LCD-Display standardmäßig auf dem Kopf steht. Sie können dies beheben, indem Sie es mit `/boot/config.txt` drehen.

```bash
sudo nano /boot/config.txt
```

Dann füge hinzu:

```bash
lcd_rotate=2
```

Drücken Sie `STRG+X` und `y` zum Speichern. Und schlussendlich:

```
sudo neu starten
```

### Meine Anzeige blendet zu seltsamen Mustern aus, wenn ich meinen Pi . herunterfahre/neu starte

Keine Panik! Das ist völlig normal.

###Mein Display ist schwarz

* Stellen Sie sicher, dass Sie das Raspberry Pi OS aktualisiert haben (siehe oben für Schritte)
* Überprüfen Sie, ob das Flachbandkabel zwischen Ihrem Pi und dem LCD richtig sitzt
* Stellen Sie sicher, dass Sie eine SD-Karte richtig in Ihren Pi . eingelegt haben

###Mein Display ist weiß

* Überprüfen Sie, ob das größere Flachbandkabel zwischen Display und Treiberplatine richtig sitzt

### Raspberry Pi OS sagt, mein Bildschirm ist 752 x 448. Das ist doch sicher falsch?

Ja, der Bildschirm sollte 800x480 groß sein. Dies ist darauf zurückzuführen, dass Overscan aktiviert ist.

Deaktivieren Sie es, indem Sie raspi-config ausführen:

```bash
sudo raspi-config
```

Navigieren Sie dann zu **Erweiterte Optionen** > **Overscan** und wählen Sie **Deaktivieren**.

### Mein Touchscreen ist nicht richtig ausgerichtet: Meine Taps sind etwas außerhalb

Dies ist wahrscheinlich auch ein Nebeneffekt des aktivierten Overscans, versuchen Sie die Lösung oben.

### Mein Bildschirm funktioniert nicht mit meinem alten Model B oder Model A Pi

Das Model A oder B Pi benötigt ein paar zusätzliche Verbindungen und eine zusätzliche Konfigurationszeile. Siehe [die Supportseite für ältere Displays] (legacy.md).

###Einige Fenster sind am unteren Bildschirmrand abgeschnitten, sodass ich sie nicht verwenden kann

Wenn einige Fenster in X an der Seite/unten des Bildschirms abgeschnitten werden, ist dies leider ein Nebeneffekt der Entwickler, die von einer Bildschirmauflösung von mindestens 1024x768 Pixel ausgehen.

Sie können versteckte Schaltflächen und Felder normalerweise aufdecken, indem Sie;

- Rechtsklick auf den Rand oder oberen Rand des Fensters,
- "bewegen" auswählen
- Verwenden Sie die Pfeiltaste nach oben, um das Fenster vom oberen Bildschirmrand nach oben zu verschieben

Wenn Sie keine Maus haben, sehen Sie sich den Rechtsklick-Fix unten an.

## Tipps

### Wie verwende ich mehrere Monitore?

Im X-Desktop kann man derzeit HDMI und das LCD nicht zusammen verwenden, aber man kann die Ausgabe bestimmter Anwendungen an den einen oder anderen Bildschirm senden.

Omxplayer ist ein Beispiel. Es wurde geändert, um die sekundäre Anzeigeausgabe zu ermöglichen.

Um mit der Anzeige eines Videos auf dem LCD-Display zu beginnen (vorausgesetzt, es handelt sich um das Standarddisplay), geben Sie einfach Folgendes ein:

```bash
omxplayer video.mkv
```

So starten Sie ein zweites Video auf dem HDMI-Typ:

```bash
omxplayer --display=5 video.mkv
```

**Bitte beachten Sie: Möglicherweise müssen Sie den der GPU zugewiesenen Speicher auf 128 MB erhöhen, wenn die Videos 1080P sind. Passen Sie dazu den gpu_mem-Wert in der config.txt an. Die Schlagzeilen des Raspberry Pi sind 1080P30-dekodiert. Wenn Sie also zwei 1080P-Clips verwenden, wird es je nach Komplexität der Videos möglicherweise nicht richtig wiedergegeben.**

Anzeigenummern sind:

* LCD: 4
* TV/HDMI: 5
* Nicht standardmäßige Anzeige automatisch auswählen: 6

### Wie aktiviere ich Rechtsklick?

Sie können einen Rechtsklick mit einer Einstellungsänderung emulieren. Gerade:

```bash
sudo nano /etc/X11/xorg.conf
```

Einfügen:

```
Abschnitt "InputClass"
   Kennung "Kalibrierung"
   Treiber "evdev"
   MatchProduct "FT5406 speicherbasierter Treiber"

   Option "EmulierenThirdButton" "1"
   Option "EmulateThirdButtonTimeout" "750"
   Option "EmulierenThirdButtonMoveThreshold" "30"
EndSection
```

Drücken Sie `STRG+X` und `y` zum Speichern. Dann:

```bash
sudo neu starten
```

Nach der Aktivierung funktioniert der Rechtsklick durch Drücken und Halten des Touchscreens und wird nach einer kurzen Verzögerung aktiviert.

### Wie erhalte ich eine Bildschirmtastatur?

#### Virtuelle Tastatur Florenz

Installieren mit:

```bash
sudo apt installieren florenz
```

#### Virtuelle Matchbox-Tastatur

Installieren Sie wie folgt:

```bash
sudo apt install matchbox-keyboard
```

Und dann in **Zubehör** > **Tastatur** suchen.
