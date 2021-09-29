# Raspberry Pi Touch-Display

## Einführung

Das Raspberry Pi Touch Display ist ein LCD-Display, das über den DSI-Anschluss mit dem Raspberry Pi verbunden wird. In einigen Situationen ermöglicht es die gleichzeitige Verwendung von HDMI- und LCD-Displays (dafür ist Softwareunterstützung erforderlich).

## Board-Unterstützung

Das DSI-Display ist so konzipiert, dass es mit allen Raspberry Pi-Modellen funktioniert, jedoch benötigen frühe Modelle, die keine Befestigungslöcher haben (das Raspberry Pi 1 Modell A und B), zusätzliche Montageteile, um die HAT-abgemessenen Abstandshalter auf dem Display zu montieren Leiterplatte.

## Physische Installation

Das folgende Bild zeigt, wie Sie den Raspberry Pi an der Rückseite des Touch-Displays anbringen (falls erforderlich) und wie Sie sowohl die Daten (Flachbandkabel) als auch die Stromversorgung (rote / schwarze Drähte) vom Raspberry Pi an das Display anschließen. Wenn Sie den Raspberry Pi nicht an der Rückseite des Displays befestigen, achten Sie beim Anbringen des Flachbandkabels besonders auf die richtige Ausrichtung. Die schwarzen und roten Stromkabel sollten jeweils an die GND- und 5V-Pins angeschlossen werden.

![DSI-Display-Verbindungen](GPIO_power-500x333.jpg "DSI-Display-Verbindungen")

[Legacy-Unterstützung für Raspberry Pi 1 Modell A/B](legacy.md)


## Bildschirmausrichtung

LCD-Displays haben einen optimalen Betrachtungswinkel, und je nachdem, wie der Bildschirm montiert ist, kann es erforderlich sein, die Ausrichtung des Displays zu ändern, um die besten Ergebnisse zu erzielen. Standardmäßig sind das Raspberry Pi Touch Display und der Raspberry Pi so eingestellt, dass sie am besten funktionieren, wenn sie von leicht oben betrachtet werden, zum Beispiel auf einem Desktop. Wenn Sie von unten betrachten, können Sie das Display physisch drehen und dann die Systemsoftware anweisen, dies zu kompensieren, indem Sie den Bildschirm auf den Kopf stellen.

### FKMS-Modus

Auf dem Raspberry Pi 4B wird standardmäßig der FKMS-Modus verwendet. FKMS verwendet die DRM/MESA-Bibliotheken, um Grafiken und 3D-Beschleunigung bereitzustellen.

Um die Bildschirmausrichtung bei der Ausführung des grafischen Desktops einzustellen, wählen Sie die Option „Bildschirmkonfiguration“ aus dem Menü „Einstellungen“. Klicken Sie im Layout-Editor mit der rechten Maustaste auf das DSI-Anzeigerechteck, wählen Sie Ausrichtung und dann die gewünschte Option.

Um die Bildschirmausrichtung im Konsolenmodus festzulegen, müssen Sie die Kernel-Befehlszeile bearbeiten, um die erforderliche Ausrichtung an das System zu übergeben.
```bash
sudo nano /boot/cmdline.txt
```
Um um 90 Grad im Uhrzeigersinn zu drehen, fügen Sie Folgendes zur cmdline hinzu, stellen Sie sicher, dass sich alles in derselben Zeile befindet, und fügen Sie keine Wagenrückläufe hinzu. Mögliche Rotationswerte sind 0, 90, 180 und 270.
```
Video=DSI-1:800x480@60,drehen=90
```
HINWEIS: Im Konsolenmodus ist es nicht möglich, die DSI-Anzeige separat zur HDMI-Anzeige zu drehen. Wenn Sie also beide angeschlossen haben, müssen beide auf den gleichen Wert eingestellt werden.

###Alter Grafikmodus

Der Legacy-Grafikmodus wird standardmäßig auf allen Raspberry Pi-Modellen vor dem Raspberry Pi 4B verwendet und kann bei Bedarf auch auf dem Raspberry Pi 4B verwendet werden, indem der FKMS-Modus deaktiviert wird, indem die FKMS-Zeile in `config.txt` auskommentiert wird. Hinweis: Der Legacy-Modus auf dem Raspberry Pi 4B hat keine 3D-Beschleunigung, daher sollte er nur verwendet werden, wenn Sie einen bestimmten Grund dafür haben.

Um das Display umzudrehen, fügen Sie der Datei `/boot/config.txt` die folgende Zeile hinzu:

`lcd_rotate=2`

Dadurch werden das LCD und der Touchscreen vertikal umgedreht und die physische Ausrichtung des Displays ausgeglichen.

Sie können die Anzeige auch drehen, indem Sie Folgendes zur Datei `config.txt` hinzufügen.

- `display_lcd_rotate=x`, wobei `x` eines der folgenden sein kann:

| display_lcd_rotate | Ergebnis |
| --- | --- |
| 0 | keine Drehung |
| 1 | 90 Grad im Uhrzeigersinn drehen |
| 2 | 180 Grad im Uhrzeigersinn drehen |
| 3 | 270 Grad im Uhrzeigersinn drehen |
| 0x10000 | horizontaler Flip |
| 0x20000 | vertikaler Flip |

Beachten Sie, dass die 90- und 270-Grad-Rotationsoptionen zusätzlichen Speicher auf der GPU erfordern, sodass diese nicht mit der 16-MB-GPU-Aufteilung funktionieren.

## Touchscreen-Ausrichtung

Darüber hinaus haben Sie die Möglichkeit, die Drehung des Touchscreens unabhängig vom Display selbst zu ändern, indem Sie eine `dtoverlay`-Anweisung in der `config.txt` hinzufügen, zum Beispiel:

`dtoverlay=rpi-ft5406,Touchscreen-Swapped-x-y=1,Touchscreen-invertiert-x=1`

Die Optionen für den Touchscreen sind:

| DT-Parameter | Aktion |
|-----------------------|------------------------- --------|
|Touchscreen-Größe-x | Legt die X-Auflösung fest (Standard 800) |
|Touchscreen-Größe-y | Legt die Y-Auflösung fest (Standard 600) |
|Touchscreen-invertiert-x | X-Koordinaten invertieren |
|Touchscreen-invertiert-y | Y-Koordinaten invertieren |
|Touchscreen-getauscht-x-y| X- und Y-Koordinaten vertauschen |

## Fehlerbehebung

Lesen Sie hier unsere Schritte, Tipps und Tricks zur Fehlerbehebung: [Fehlerbehebung für das Raspberry Pi Touch Display](troubleshooting.md).

## Spezifikationen

- 800×480 RGB-LCD-Display
- 24-Bit-Farbe
- Industriequalität: 140-Grad-Betrachtungswinkel horizontal, 130-Grad-Betrachtungswinkel vertikal
- 10-Punkt-Multitouch-Touchscreen
- PWM-Hintergrundbeleuchtungssteuerung und Leistungssteuerung über I2C-Schnittstelle
- Rückseite mit Metallrahmen mit Befestigungspunkten für Raspberry Pi Display Conversion Board und Raspberry Pi
- Lebensdauer der Hintergrundbeleuchtung: 20000 Stunden
- Betriebstemperatur: -20 bis +70 Grad Celsius
- Lagertemperatur: -30 bis +80 Grad Celsius
- Kontrastverhältnis: 500
* Durchschnittliche Helligkeit: 250 cd/m<sup>2</sup>
* Betrachtungswinkel (Grad):
  * Oben - 50
  * Unten - 70
  * Links - 70
  * Rechts - 70

## Mechanische Spezifikation des Moduls

* Außenmaße: 192,96 × 110,76 mm
* Sichtbarer Bereich: 154,08 × 85,92 mm
* [Mechanische Zeichnung herunterladen (PDF, 592kb)](7InchDisplayDrawing-14092015.pdf)
* [Zusätzliche Zeichnung mit Radius und Glasdicke](radius.png)