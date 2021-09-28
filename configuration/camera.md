# Kamerakonfiguration

## Einrichten der Kamera-Hardware

**Warnung**: Kameras sind empfindlich gegenüber statischer Aufladung. Erden Sie sich vor dem Umgang mit der Leiterplatte. Ein Spültischhahn oder ähnliches sollte ausreichen, wenn Sie kein Erdungsband haben.

Die Kameraplatine wird über ein 15-poliges Flachbandkabel mit dem Raspberry Pi verbunden. Es müssen nur zwei Verbindungen hergestellt werden: Das Flachbandkabel muss an der Kameraplatine und am Raspberry Pi selbst befestigt werden. Sie müssen das Kabel richtig herum verlegen, sonst funktioniert die Kamera nicht. Auf der Kameraplatine sollte die blaue Rückseite des Kabels von der Platine weg zeigen und auf dem Raspberry Pi sollte sie zum Ethernet-Anschluss zeigen (oder dort, wo sich der Ethernet-Anschluss befinden würde, wenn Sie ein Modell A verwenden).

Obwohl sich die Anschlüsse auf der Platine und dem Pi unterscheiden, funktionieren sie ähnlich. Ziehen Sie auf dem Raspberry Pi selbst die Laschen an jedem Ende des Steckers nach oben. Es sollte leicht nach oben gleiten und sich leicht drehen können. Führen Sie das Flachbandkabel vollständig in den Schlitz ein und stellen Sie sicher, dass es gerade sitzt, und drücken Sie dann die Laschen vorsichtig nach unten, um es einzurasten. Beim PCB-Anschluss der Kamera müssen Sie außerdem die Laschen von der Platine wegziehen, das Kabel vorsichtig einführen und dann die Laschen zurückdrücken. Der PCB-Anschluss kann etwas umständlicher sein als der am Pi selbst.

## Einrichten der Kamerasoftware

Führen Sie die folgenden Anweisungen in der Befehlszeile aus, um den neuesten Kernel, die GPU-Firmware und die Anwendungen herunterzuladen und zu installieren. Damit dies korrekt funktioniert, benötigen Sie eine Internetverbindung.

```bash
sudo apt-Update
sudo apt Voll-Upgrade
```

Jetzt müssen Sie die Kameraunterstützung mit dem Programm `raspi-config` aktivieren, das Sie bei der ersten Einrichtung Ihres Raspberry Pi verwendet haben.

```bash
sudo raspi-config
```

Verwenden Sie die Cursortasten, um *Schnittstellenoptionen* auszuwählen und zu öffnen, und wählen Sie dann *Kamera* und folgen Sie den Anweisungen, um die Kamera zu aktivieren.

Beim Beenden von `raspi-config` fordert es zum Neustart auf. Die Enable-Option stellt sicher, dass beim Neustart die richtige GPU-Firmware mit dem Kameratreiber und der Einstellung ausgeführt wird und die GPU-Speicheraufteilung ausreicht, damit die Kamera genügend Speicher für eine korrekte Ausführung erhält.

Um zu testen, ob das System installiert ist und funktioniert, versuchen Sie den folgenden Befehl:

```bash
Raspistille -v -o test.jpg
```

Das Display sollte eine fünfsekündige Vorschau der Kamera anzeigen und dann ein Bild aufnehmen, das in der Datei `test.jpg` gespeichert wird, während verschiedene Informationsmeldungen angezeigt werden.

## Mehr Informationen

Siehe [Kamerasoftware](../raspbian/applications/camera.md).