# raspivid

`raspivid` ist das Befehlszeilentool zum Aufnehmen von Videos mit einem Raspberry Pi-Kameramodul.

## Grundlegende Verwendung von Raspivid

Nehmen Sie mit einem Kameramodul [verbunden und aktiviert](../README.md) ein Video mit dem folgenden Befehl auf:

```bash
raspivid -o vid.h264
```

Denken Sie daran, `-hf` und `-vf` zu verwenden, um das Bild bei Bedarf zu spiegeln, wie bei [raspistill](raspistill.md)

Dadurch wird eine 5-Sekunden-Videodatei im hier angegebenen Pfad als `vid.h264` gespeichert (Standarddauer).

### Länge des Videos angeben

Um die Länge des aufgenommenen Videos anzugeben, übergeben Sie das Flag "-t" mit einer Anzahl von Millisekunden. Beispielsweise:

```bash
raspivid -o video.h264 -t 10000
```

Dadurch werden 10 Sekunden Video aufgezeichnet.

### Mehr Optionen

Um eine vollständige Liste der möglichen Optionen zu erhalten, führen Sie `raspivid` ohne Argumente aus oder leiten Sie diesen Befehl durch `less` und scrollen Sie durch:

```bash
raspivid 2>&1 | less
```

Verwenden Sie die Pfeiltasten, um zu scrollen, und geben Sie 'q' ein, um das Menü zu verlassen.

### MP4-Videoformat

Der Pi erfasst Videos als rohen H264-Videostream. Viele Mediaplayer weigern sich, es abzuspielen, oder spielen es mit einer falschen Geschwindigkeit ab, es sei denn, es ist in ein geeignetes Containerformat wie MP4 "verpackt". Der einfachste Weg, eine MP4-Datei über den Befehl raspivid zu erhalten, ist die Verwendung von MP4Box.

Installieren Sie MP4Box mit diesem Befehl:

```bash
sudo apt install -y gpac
```

Nehmen Sie Ihr Rohvideo mit raspivid auf und packen Sie es wie folgt in einen MP4-Container:

```bash
# 30 Sekunden Rohvideo mit 640 x 480 und einer Bitrate von 150 kB/s in eine pivideo.h264-Datei aufnehmen:
raspivid -t 30000 -w 640 -h 480 -fps 25 -b 1200000 -p 0,0,640,480 -o pivideo.h264 
# Wickeln Sie das Rohvideo mit einem MP4-Container ein: 
MP4Box -add pivideo.h264 pivideo.mp4
# Entfernen Sie die Quell-Rohdatei und lassen Sie die verbleibende pivideo.mp4-Datei zum Abspielen übrig
rm pivideo.h264
```

Alternativ können Sie MP4 wie folgt um Ihre vorhandene Raspivid-Ausgabe wickeln:

```bash
MP4Box -add video.h264 video.mp4
```

## Vollständige Dokumentation

Die vollständige Dokumentation der Kamera finden Sie unter [hardware/camera](../../../hardware/camera/README.md).
