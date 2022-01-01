# Zeitraffer

Um ein Zeitraffer-Video zu erstellen, konfigurieren Sie den Raspberry Pi einfach so, dass er in regelmäßigen Abständen, beispielsweise einmal pro Minute, ein Bild aufnimmt, und verwenden Sie dann eine Anwendung, um die Bilder zu einem Video zusammenzufügen. Dazu gibt es mehrere Möglichkeiten.

## Den eingebauten Zeitraffer-Modus von raspistill verwenden

Die Anwendung `raspistill` hat einen eingebauten Zeitraffermodus, der den Befehlszeilenschalter `--timelapse` (oder `-tl`) verwendet. Der Wert, der dem Schalter folgt, ist die Zeit zwischen den Schüssen in Millisekunden:
```
raspistill -t 30000 -tl 2000 -o image%04d.jpg
```
Beachten Sie das `%04d` im Ausgabedateinamen: Dies gibt die Stelle im Dateinamen an, an der eine Frame-Zählnummer erscheinen soll. Der obige Befehl erzeugt beispielsweise alle zwei Sekunden (2000 ms) über einen Gesamtzeitraum von 30 Sekunden (30000 ms) eine Aufnahme mit den Namen image0001.jpg, image0002.jpg usw. bis hin zu image0015.jpg.

Das `%04d` gibt eine vierstellige Zahl an, wobei führende Nullen hinzugefügt werden, um die erforderliche Anzahl von Stellen zu bilden. So würde beispielsweise `%08d` zu einer achtstelligen Zahl führen. Sie können die `0` auslassen, wenn Sie keine führenden Nullen möchten.

Wenn ein Zeitrafferwert von 0 eingegeben wird, nimmt die Anwendung Bilder so schnell wie möglich auf. Beachten Sie, dass zwischen den Aufnahmen eine erzwungene Mindestpause von etwa 30 Millisekunden eingehalten wird, um sicherzustellen, dass Belichtungsberechnungen durchgeführt werden können.

## Cron verwenden

Eine gute Möglichkeit, ein Bild in regelmäßigen Abständen zu automatisieren, ist die Verwendung von `cron`. Öffnen Sie die Cron-Tabelle zum Bearbeiten:

```
crontab -e
```

Dies wird entweder fragen, welchen Editor Sie verwenden möchten, oder in Ihrem Standardeditor geöffnet. Sobald Sie die Datei in einem Editor geöffnet haben, fügen Sie die folgende Zeile hinzu, um zu planen, jede Minute ein Bild aufzunehmen (beziehen Sie sich auf das Bash-Skript von der [raspistill-Seite] (raspistill.md)):

```
* * * * * /home/pi/camera.sh 2>&1
```

Speichern und beenden Sie und Sie sollten die Meldung sehen:

```
crontab: installing new crontab
```

Stellen Sie sicher, dass Sie z.B. `%04d`, damit `raspistill` jedes Bild in eine neue Datei ausgibt: Wenn nicht, überschreibt `raspistill` jedes Mal, wenn ein Bild ein Bild schreibt, dieselbe Datei.

## Bilder zusammenfügen

Jetzt müssen Sie die Fotos zu einem Video zusammenfügen. Sie können dies auf dem Pi mit `mencoder` tun, aber die Verarbeitung wird langsam sein. Vielleicht möchten Sie die Bilddateien lieber auf Ihren Desktop-Computer oder Laptop übertragen und dort das Video produzieren.

Navigieren Sie zu dem Ordner, der alle Ihre Bilder enthält, und listen Sie die Dateinamen in einer Textdatei auf. Beispielsweise:

```
ls *.jpg > stills.txt
```
### Auf dem Raspberry Pi

Obwohl es langsam sein wird (aufgrund der Codierung in Software und nicht der Raspberry Pi-Hardwarebeschleunigung), können Sie Ihre JPEG-Bilder mit verschiedenen verfügbaren Tools zusammenfügen. Diese Dokumentation verwendet `avconv`, das installiert werden muss.
```
sudo apt install libav-tools
```
Jetzt können Sie die Tools verwenden, um Ihre JPEG-Dateien in eine H264-Videodatei zu konvertieren:
```
avconv -r 10 -i image%04d.jpg -r 10 -vcodec libx264 -vf scale=1280:720 timelapse.mp4
```
Auf einem Raspberry Pi 3 kann dies etwas mehr als einen Frame pro Sekunde kodieren. Die Leistung anderer Pi-Modelle wird variieren. Die verwendeten Parameter sind:

 - -r 10 Angenommen, zehn Bilder pro Sekunde in Eingabe- und Ausgabedateien.
 - -i image%04.jpg Die Spezifikation der Eingabedatei (entsprechend den während der Aufnahme erzeugten Dateien).
 - -vcodec libx264 Verwenden Sie den Software-x264-Encoder.
 - -vf scale=1280:720 Skaliert auf 720p. Sie können auch 1920:1080 oder niedrigere Auflösungen verwenden, je nach Ihren Anforderungen. Beachten Sie, dass der Pi nur Videos bis zu 1080p wiedergeben kann, aber wenn Sie beispielsweise 4K wiedergeben möchten, können Sie dies hier einstellen.
 - timelapse.mp4 Der Name der Ausgabedatei.

`avconv` verfügt über einen umfassenden Parametersatz für verschiedene Kodierungsoptionen und andere Einstellungen. Diese können mit `avconv --help` aufgelistet werden.

### Auf einem anderen Linux-Computer

Sie können die gleiche Anleitung wie für den Raspberry Pi verwenden oder ein alternatives Paket wie `mencoder` verwenden:

```
sudo apt install mencoder
```

Führen Sie nun den folgenden Befehl aus:

```
mencoder -nosound -ovc lavc -lavcopts vcodec=mpeg4:aspect=16/9:vbitrate=8000000 -vf scale=1920:1080 -o timelapse.avi -mf type=jpeg:fps=24 mf://@stills.txt
```

Sobald dies abgeschlossen ist, sollten Sie eine Videodatei namens `timelapse.avi` haben, die einen Zeitraffer Ihrer Bilder enthält.