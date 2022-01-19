# DIESE SEITE IST JETZT VERALTET

# Hallo Teekanne

Dies zeigt eine sich drehende Teekanne, auf deren Oberfläche der Videoclip von `hello_video` texturiert ist. Es ist ziemlich beeindruckend! Sie erkennen das Teekannenmodell vielleicht, wenn Sie mit einer Software namens [Blender](https://en.wikipedia.org/wiki/Blender_(software)) vertraut sind. Dies demonstriert OpenGL ES-Rendering und Videodekodierung/-wiedergabe gleichzeitig.

![Teekanne](images/teapot.jpg)

```bash
cd ..
cd hello_teapot
ls
```

Beachten Sie die grüne `.bin`-Datei? Okay, führe es aus!

```bash
./hello_teapot.bin
```

Sie erhalten möglicherweise die folgende Fehlermeldung, wenn Sie versuchen, diese Demo auszuführen: 

```bash
Note: ensure you have sufficient gpu_mem configured
eglCreateImageKHR:  failed to create image for buffer 0x1 target 12465 error 0x300c
eglCreateImageKHR failed.
```

```bash
Hinweis: Stellen Sie sicher, dass ausreichend gpu_mem konfiguriert ist
eglCreateImageKHR: Bild für Puffer 0x1 Ziel 12465 Fehler 0x300c konnte nicht erstellt werden
eglCreateImageKHR fehlgeschlagen.
```

Keine Sorge; Wenn Sie diesen Fehler sehen, müssen Sie nur eine Konfigurationseinstellung ändern, damit er funktioniert.

Der Fehler bedeutet, dass die GPU (Graphics Processing Unit) nicht über genügend Speicher verfügt, um die Demo auszuführen. Es ist die GPU, die beim Zeichnen von 3D-Grafiken auf dem Bildschirm die ganze Arbeit leistet, ähnlich wie die Grafikkarte in einem Gaming-PC. Der Raspberry Pi teilt seinen Speicher/RAM zwischen CPU und GPU und ist standardmäßig so konfiguriert, dass er der GPU nur 64 MB RAM zur Verfügung stellt. Wenn wir dies auf 128 MB erhöhen, sollte das Problem behoben sein.

Dazu müssen Sie den folgenden Befehl eingeben:

```bash
sudo raspi-config
```

Dies öffnet ein Menü auf blauem Hintergrund. Führen Sie die folgenden Aktionen aus:

- Gehen Sie zu Erweiterte Optionen.
- Gehen Sie zu Speicheraufteilung.
- Löschen Sie „64“ und geben Sie stattdessen „128“ ein. Drücken Sie die Eingabetaste.
- Gehen Sie nach unten zum Ende.
- Klicken Sie zum Neustart auf Ja.

Nachdem Sie sich wieder angemeldet haben, geben Sie den folgenden Befehl ein, um zur `hello_teapot`-Demo zurückzukehren:

```bash
cd /opt/vc/src/hello_pi/hello_teapot
```

Versuchen Sie es jetzt erneut und führen Sie es erneut aus, und Sie sollten feststellen, dass es funktioniert.

```bash
./hello_teapot.bin
```

Die Demo läuft für immer, bis Sie sie beenden. Um die Demo zu verlassen, drücken Sie `Strg + C`.
