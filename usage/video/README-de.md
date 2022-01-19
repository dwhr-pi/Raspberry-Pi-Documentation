# Video auf dem Raspberry Pi abspielen

Auf dem Pi 3 und früheren Modellen ist die einfachste Art, Videos abzuspielen, die Verwendung der OMXPlayer-Anwendung, die ausführlicher [in diesem Dokumentationsabschnitt](../../raspbian/applications/omxplayer.md) beschrieben wird.

Um ein Video abzuspielen, navigieren Sie mit „cd“ zum Speicherort Ihrer Videodatei im Terminal und geben Sie dann den folgenden Befehl ein:

```bash
omxplayer example.mp4
```

Dadurch wird `example.mp4` im Vollbildmodus abgespielt. Drücken Sie "Strg + C", um zu beenden.

Auf dem Pi 4 wurde die Hardwareunterstützung für MPEG2- und VC-1-Codecs entfernt, daher empfehlen wir die Verwendung der VLC-Anwendung, die diese Formate in Software unterstützt. Darüber hinaus verfügt VLC über Hardwareunterstützung für H264 und den neuen HEVC-Codec.

## Beispiel-Videobeispiel: Big Buck Bunny

Ein Videobeispiel des Animationsfilms *Big Buck Bunny* ist auf dem Pi verfügbar. Um es auf einem Pi 3 oder früheren Modellen abzuspielen, geben Sie den folgenden Befehl in ein Terminalfenster ein:

```bash
omxplayer /opt/vc/src/hello_pi/hello_video/test.h264
```

Verwenden Sie auf einem Pi 4 den folgenden Befehl für H264-Dateien:

```bash
omxplayer /opt/vc/src/hello_pi/hello_video/test.h264
```
oder für H264, VC1 oder MPEG2
```bash
vlc /opt/vc/src/hello_pi/hello_video/test.h264
```

Bei der Verwendung von VLC können Sie die Wiedergabeleistung verbessern, indem Sie den rohen H264-Stream kapseln, beispielsweise vom Raspberry Pi Camera Module. Das geht ganz einfach mit `ffmpeg`. Die Wiedergabe wird auch verbessert, wenn VLC im Vollbildmodus ausgeführt wird; Wählen Sie entweder Vollbild auf der Benutzeroberfläche oder fügen Sie die `--fullscreen`-Optionen zur `vlc`-Befehlszeile hinzu.

Dieser Beispielbefehl konvertiert `video.h264` in eine containerisierte `video.mp4` mit 30 fps:

`ffmpeg -r 30 -i video.h264 -c:v copy video.mp4`