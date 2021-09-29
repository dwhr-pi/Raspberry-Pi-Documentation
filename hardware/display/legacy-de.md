# Unterstützung für ältere Raspberry Pi 1-Displays

### Anbringen an Modell A/B Boards

Der DSI-Anschluss auf den Modell-A/B-Platinen verfügt nicht über die erforderlichen I2C-Anschlüsse, um mit dem Touchscreen-Controller und dem DSI-Controller zu kommunizieren. Sie können dies umgehen, indem Sie den mit dem Display-Kit mitgelieferten zusätzlichen Satz Jumperkabel verwenden, um den I2C-Bus an den GPIO-Pins mit der Display-Controller-Platine zu verbinden.

Verbinden Sie mit den Überbrückungskabeln SCL/SDA am GPIO-Header mit den mit SCL/SDA gekennzeichneten horizontalen Pins auf der Anzeigeplatine. Wir empfehlen außerdem, das Model A/B über die GPIO-Pins mit den Jumperkabeln mit Strom zu versorgen.

Für die GPIO-Header-Pinbelegung siehe [dieses Diagramm](http://pinout.xyz/).

Die automatische DSI-Anzeigenerkennung ist auf diesen Boards standardmäßig deaktiviert. Um die Erkennung zu aktivieren, fügen Sie die folgende Zeile zu `/boot/config.txt` hinzu:

`ignore_lcd=0`

Versorgen Sie das Setup über den Micro-USB-Anschluss „PWR IN“ auf der Anzeigeplatine. Versorgen Sie das Setup nicht über den Micro-USB-Port des Pi: Der maximale Nennstrom der Eingangspolysicherung wird überschritten, da das Display etwa 400 mA verbraucht.

Hinweis: Wenn das Display an die GPIO I2C-Pins angeschlossen ist, übernimmt die GPU die Kontrolle über den jeweiligen I2C-Bus. Das Host-Betriebssystem sollte nicht auf diesen I2C-Bus zugreifen, da die gleichzeitige Nutzung des Busses durch GPU und Linux zu sporadischen Abstürzen führt.