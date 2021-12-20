# Audiowiedergabe auf dem Raspberry Pi

Die einfachste Art, Audio abzuspielen, ist die Anwendung OMXPlayer, die [hier] genauer beschrieben wird (../../raspbian/applications/omxplayer.md).

Um eine MP3-Datei abzuspielen, navigieren Sie mit „cd“ zum Speicherort der `.mp3`-Datei im Terminal und geben Sie den folgenden Befehl ein:

```bash
omxplayer example.mp3
```
    
Dadurch wird die Audiodatei `example.mp3` entweder über die eingebauten Lautsprecher Ihres Monitors oder über Ihre Kopfhörer, die über die Kopfhörerbuchse angeschlossen sind, abgespielt.

Wenn Sie eine Beispieldatei benötigen, können Sie diese mit dem folgenden Befehl herunterladen:

```bash
wget http://rpf.io/lamp3 -O example.mp3 --no-check-certificate
```

Wenn Sie nichts hören, vergewissern Sie sich, dass Ihre Kopfhörer oder Lautsprecher richtig angeschlossen sind. Beachten Sie, dass omxplayer ALSA nicht verwendet und daher die von `raspi-config` oder `amixer` gesetzte [audio configuration](../../configuration/audio-config.md) ignoriert.

Wenn die automatische Erkennung des richtigen Audioausgabegeräts durch omxplayer fehlschlägt, können Sie die Ausgabe über HDMI erzwingen mit:

```bash
omxplayer -o hdmi example.mp3
```

Alternativ können Sie die Ausgabe über die Kopfhörerbuchse erzwingen mit:

```bash
omxplayer -o local example.mp3
```

Sie können sogar die Ausgabe über die Kopfhörerbuchse und HDMI erzwingen mit:

```bash
omxplayer -o both example.mp3
```
## omxplayer als Hintergrundjob ausführen

`omxplayer` wird sofort geschlossen, wenn es im Hintergrund ohne tty (Benutzereingabe) ausgeführt wird. Um erfolgreich zu laufen, müssen Sie `omxplayer` mit der Option `--no-keys` mitteilen, dass keine Benutzereingaben erforderlich sind.

```bash
omxplayer --no-keys example.mp3 &
```

Durch Hinzufügen des `&` am Ende des Befehls wird der Job im Hintergrund ausgeführt. Anschließend können Sie den Status dieses Hintergrundjobs mit dem Befehl `jobs` überprüfen. Standardmäßig wird der Job abgeschlossen, wenn `omxplayer` die Wiedergabe beendet, aber Sie können ihn bei Bedarf jederzeit mit dem `kill`-Befehl stoppen.

```bash
$ Jobs
$ jobs
[1]-  Running             omxplayer --no-keys example.mp3 &
$ kill %1
$
[1]-  Terminated          omxplayer --no-keys example.mp3 &
```