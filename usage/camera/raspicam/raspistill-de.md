# Raspistille

`raspistill` ist das Kommandozeilentool zum Aufnehmen von Standbildern mit einem Raspberry Pi Kameramodul.

## Grundlegende Verwendung von Raspistill

Geben Sie bei einem Kameramodul [verbunden und aktiviert](../README.md) den folgenden Befehl in das Terminal ein, um ein Bild aufzunehmen:

```bash
raspistill -o cam.jpg
```

![Umgedrehtes Foto](images/cam.jpg)

In diesem Beispiel wurde die Kamera verkehrt herum positioniert. Wenn die Kamera in dieser Position platziert wird, muss das Bild gespiegelt werden, um richtig nach oben zu erscheinen.

### Vertikaler Flip und horizontaler Flip

Bei auf den Kopf gestellter Kamera muss das Bild um 180° gedreht werden, damit es richtig angezeigt wird. Um dies zu korrigieren, wenden Sie sowohl einen vertikalen als auch einen horizontalen Flip an, indem Sie die Flags `-vf` und `-hf` übergeben:

```bash
raspistill -vf -hf -o cam2.jpg
```

![Vertikal und horizontal gespiegeltes Foto](images/cam2.jpg)

Jetzt wurde das Foto richtig aufgenommen.

### Auflösung

Das Kameramodul nimmt Bilder mit einer Auflösung von `2592 x 1944` auf, was 5.038.848 Pixel oder 5 Megapixel entspricht.

### Dateigröße

Ein mit dem Kameramodul aufgenommenes Foto wird etwa 2,4 MB groß sein. Das sind etwa 425 Fotos pro GB.

Die Aufnahme von 1 Foto pro Minute würde 1 GB in etwa 7 Stunden in Anspruch nehmen. Dies entspricht einer Rate von etwa 144 MB pro Stunde oder 3,3 GB pro Tag.

### Bash-Skript

Sie können ein Bash-Skript erstellen, das mit der Kamera ein Bild aufnimmt. Um ein Skript zu erstellen, öffnen Sie den Editor Ihrer Wahl und schreiben Sie den folgenden Beispielcode:

```bash
#!/bin/bash

DATE=$(date +"%Y-%m-%d_%H%M")

raspistill -vf -hf -o /home/pi/camera/$DATE.jpg
```

Dieses Skript nimmt ein Bild auf und benennt die Datei mit einem Zeitstempel.

Sie müssen auch sicherstellen, dass der Pfad existiert, indem Sie den Ordner `camera` erstellen:

```bash
mkdir camera
```

Angenommen, wir haben es als `camera.sh` gespeichert, wir würden die Datei zuerst ausführbar machen:

```bash
chmod +x camera.sh
```

Dann lauf mit:

```bash
./camera.sh
```

### Mehr Optionen

Um eine vollständige Liste der möglichen Optionen zu erhalten, führen Sie `raspistill` ohne Argumente aus. Um zu scrollen, leiten Sie stderr nach stdout um und leiten Sie die Ausgabe an `less` weiter:

```bash
raspistill 2>&1 | less
```

Verwenden Sie die Pfeiltasten, um zu scrollen, und geben Sie 'q' ein, um das Menü zu verlassen.

## Vollständige Dokumentation

Die vollständige Dokumentation der Kamera finden Sie unter [hardware/camera](../../../hardware/camera/README.md).
