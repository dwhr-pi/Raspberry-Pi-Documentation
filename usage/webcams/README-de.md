# Verwendung einer Standard-USB-Webcam

Anstatt das Raspberry Pi [Kameramodul] (../camera/README.md) zu verwenden, können Sie eine Standard-USB-Webcam verwenden, um Bilder und Videos auf dem Raspberry Pi aufzunehmen.

Beachten Sie, dass die Qualität und Konfigurierbarkeit des Kameramoduls einer Standard-USB-Webcam weit überlegen ist.

## fswebcam installieren

Installieren Sie zuerst das Paket „fswebcam“:

```bash
sudo apt install fswebcam
```

## Fügen Sie Ihren Benutzer zur Gruppe „Video“ hinzu

Wenn Sie nicht das standardmäßige `pi`-Benutzerkonto verwenden, müssen Sie Ihren Benutzernamen zur `video`-Gruppe hinzufügen, andernfalls werden die Fehlermeldungen 'Zugriff verweigert' angezeigt.

```bash
sudo usermod -a -G video <Benutzername>
```

Um zu überprüfen, ob der Benutzer korrekt zur Gruppe hinzugefügt wurde, verwenden Sie den Befehl „groups“.

## Grundlegende Verwendung

Geben Sie den Befehl „fswebcam“ gefolgt von einem Dateinamen ein, und ein Bild wird mit der Webcam aufgenommen und unter dem angegebenen Dateinamen gespeichert:

```bash
fswebcam image.jpg
```

Dieser Befehl zeigt die folgenden Informationen:

```
--- Öffne /dev/video0...
Quellmodul v4l2 ausprobieren ...
/dev/video0 geöffnet.
Es wurde keine Eingabe angegeben, wobei die erste verwendet wurde.
Auflösung von 384x288 auf 352x288 anpassen.
--- Bild wird aufgenommen...
Beschädigte JPEG-Daten: 2 irrelevante Bytes vor Markierung 0xd4
Erfasster Frame in 0,00 Sekunden.
--- Aufgenommenes Bild wird verarbeitet...
JPEG-Bild in „image.jpg“ schreiben.
```

```
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 384x288 to 352x288.
--- Capturing frame...
Corrupt JPEG data: 2 extraneous bytes before marker 0xd4
Captured frame in 0.00 seconds.
--- Processing captured image...
Writing JPEG image to 'image.jpg'.
```

![Einfache Bilderfassung](images/image.jpg)

Beachten Sie die verwendete kleine Standardauflösung und das Vorhandensein eines Banners mit dem Zeitstempel.

### Auflösung angeben

Die in diesem Beispiel verwendete Webcam hat eine Auflösung von „1280 x 720“. Um also die Auflösung anzugeben, mit der das Bild aufgenommen werden soll, verwenden Sie das „-r“-Flag:

```bash
fswebcam -r 1280x720 image2.jpg
```

Dieser Befehl zeigt die folgenden Informationen:

```
--- Öffne /dev/video0...
Quellmodul v4l2 ausprobieren ...
/dev/video0 geöffnet.
Es wurde keine Eingabe angegeben, wobei die erste verwendet wurde.
--- Bild wird aufgenommen...
Beschädigte JPEG-Daten: 1 irrelevantes Byte vor Markierung 0xd5
Erfasster Frame in 0,00 Sekunden.
--- Aufgenommenes Bild wird verarbeitet...
JPEG-Bild in „image2.jpg“ schreiben.
```

![Bild in voller Auflösung](images/image2.jpg)

Das Bild wurde jetzt mit der vollen Auflösung der Webcam aufgenommen, wobei das Banner vorhanden ist.

### Kein Banner angeben

Fügen Sie nun das `--no-banner`-Flag hinzu:

```bash
fswebcam -r 1280x720 --no-banner image3.jpg
```

die folgende Informationen anzeigt:

```
--- Öffne /dev/video0...
Quellmodul v4l2 ausprobieren ...
/dev/video0 geöffnet.
Es wurde keine Eingabe angegeben, wobei die erste verwendet wurde.
--- Bild wird aufgenommen...
Beschädigte JPEG-Daten: 2 irrelevante Bytes vor Markierung 0xd6
Erfasster Frame in 0,00 Sekunden.
--- Aufgenommenes Bild wird verarbeitet...
Banner deaktivieren.
JPEG-Bild in „image3.jpg“ schreiben.
```

```
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
--- Capturing frame...
Corrupt JPEG data: 1 extraneous bytes before marker 0xd5
Captured frame in 0.00 seconds.
--- Processing captured image...
Writing JPEG image to 'image2.jpg'.
```

![Bild in voller Auflösung ohne Banner](images/image3.jpg)

Jetzt wird das Bild in voller Auflösung ohne Banner aufgenommen.

## Schlechte Bilder

Mit einer USB-Webcam können Bilder in schlechter Qualität auftreten, wie z. B. dieses zufällig künstlerische Stück:

![Schlechtes Webcam-Bild](images/jack.jpg)

Einige Webcams sind zuverlässiger als andere, aber diese Art von Problem kann bei Webcams mit schlechter Qualität auftreten. Wenn das Problem weiterhin besteht, stellen Sie sicher, dass Ihr System [auf dem neuesten Stand](../../raspbian/updating.md) ist. Probieren Sie auch andere Webcams aus, aber die beste Leistung erzielen Sie mit dem Raspberry Pi [Kameramodul](https://www.raspberrypi.org/help/camera-module-setup/).

## Bash-Skript

Sie können ein Bash-Skript schreiben, das ein Bild mit der Webcam aufnimmt. Das folgende Skript speichert die Bilder im Verzeichnis „/home/pi/webcam“, erstellen Sie also zuerst das Unterverzeichnis „webcam“ mit:

```bash
mkdir webcam
```

Öffnen Sie zum Erstellen eines Skripts den Editor Ihrer Wahl und schreiben Sie den folgenden Beispielcode:

```bash
#!/bin/bash

DATE=$(date +"%Y-%m-%d_%H%M")

fswebcam -r 1280x720 --no-banner /home/pi/webcam/$DATE.jpg
```

Dieses Skript nimmt ein Bild auf und benennt die Datei mit einem Zeitstempel. Sagen wir, wir haben es als `webcam.sh` gespeichert, wir würden die Datei zuerst ausführbar machen:

```bash
chmod +x webcam.sh
```

Dann laufen mit:

```bash
./webcam.sh
```

Was die Befehle in der Datei ausführen und die übliche Ausgabe liefern würde:

```
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
--- Capturing frame...
Corrupt JPEG data: 2 extraneous bytes before marker 0xd6
Captured frame in 0.00 seconds.
--- Processing captured image...
Disabling banner.
Writing JPEG image to '/home/pi/webcam/2013-06-07_2338.jpg'.
```

## Zeitraffer mit Cron

Sie können „cron“ verwenden, um das Aufnehmen eines Bildes in einem bestimmten Intervall zu planen, z. B. jede Minute, um einen Zeitraffer aufzunehmen.

Öffnen Sie zuerst die Cron-Tabelle zum Bearbeiten:

```
crontab -e
```

Dadurch werden Sie entweder gefragt, welchen Editor Sie verwenden möchten, oder in Ihrem Standard-Editor öffnen. Sobald Sie die Datei in einem Editor geöffnet haben, fügen Sie die folgende Zeile hinzu, um jede Minute ein Foto aufzunehmen (bezogen auf das Bash-Skript von oben):

```bash
* * * * * /home/pi/webcam.sh 2>&1
```

Speichern und beenden Sie und Sie sollten die Meldung sehen:

```bash
crontab: installing new crontab
```

Stellen Sie sicher, dass Ihr Skript nicht jedes aufgenommene Bild mit demselben Dateinamen speichert. Dadurch wird das Bild jedes Mal überschrieben.

## Andere nützliche Tools

Es stehen weitere Tools zur Verfügung, die sich bei der Verwendung der Kamera oder einer Webcam als nützlich erweisen können:

- [SSH](../../remote-access/ssh/README.md)
    - Verwenden Sie SSH, um über Ihr lokales Netzwerk remote auf den Raspberry Pi zuzugreifen
- [SCP](../../remote-access/ssh/scp.md)
    - Kopieren Sie Dateien über SSH, um Kopien von Bildern zu erhalten, die auf dem Pi auf Ihrem Hauptcomputer aufgenommen wurden
- [rsync](../../remote-access/ssh/rsync.md)
    - Verwenden Sie "rsync", um den Ordner mit Bildern, die in einem Ordner zwischen Ihrem Pi aufgenommen wurden, mit Ihrem Computer zu synchronisieren
- [cron](../../linux/usage/cron.md)
    - Verwenden Sie "cron", um die Aufnahme eines Bildes in einem bestimmten Intervall zu planen, z. B. jede Minute, um einen Zeitraffer aufzunehmen