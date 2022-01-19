# DIESE SEITE IST JETZT VERALTET

# Demoprogramme

Hier sind einige Beispielprogramme, um die Fähigkeiten des Pi zu demonstrieren. Beachten Sie, dass aufgrund der Funktionsweise der neuen 3D-Bibliotheken nicht alle Demoanwendungen auf einem Pi 4 ausgeführt werden können. die nicht funktionierenden melden eine Fehlermeldung, wenn sie auf einem Pi 4 ausgeführt werden.

![Mandelbrot-Fraktal](images/mandelbrot.jpg)

Um diese Programme auszuführen, müssen Sie sich an der Befehlszeile befinden. Ihr Pi kann über die Befehlszeile booten (wobei Sie „startx“ eingeben müssen, um zum Desktop zu gelangen); Wenn ja, gehen Sie geradeaus. Verwenden Sie andernfalls die Start-Schaltfläche, um sich vom Desktop abzumelden.

```bash
pi@raspberrypi ~ $
```

Dies (oben) ist die Eingabeaufforderung. Es sieht schwierig aus, es zu benutzen, aber versuchen Sie, keine Angst davor zu haben! Eine CLI oder Befehlszeilenschnittstelle ist eigentlich eine sehr schnelle und effiziente Möglichkeit, einen Computer zu verwenden.

Navigieren Sie zunächst zum Ordner „hello_pi“, in dem alle Demos gespeichert sind. Geben Sie dazu den folgenden Befehl ein. **TIPP**: Sie können die `TAB`-Taste für die automatische Vervollständigung verwenden, während Sie Befehle eingeben.

```bash
cd /opt/vc/src/hello_pi
```

Die Eingabeaufforderung sollte jetzt wie im folgenden Text aussehen. Der blaue Teil zeigt an, wo Sie sich im Dateisystem des Pi befinden.

```bash
pi@raspberrypi /opt/vc/src/hello_pi $
```

Wenn Sie „ls“ eingeben und die Eingabetaste drücken, sehen Sie eine Liste mit Ordnern; es gibt eine für jede Demo. Bevor Sie sie jedoch ausführen können, müssen sie kompiliert werden. Machen Sie sich keine Sorgen, wenn Sie nicht verstehen, was das ist oder warum Sie es tun müssen; Folgen Sie einfach den Anweisungen für den Moment, und wir werden später mehr darüber erfahren.

Im `hello_pi`-Ordner befindet sich ein kleines Shell-Skript mit dem Namen rebuild.sh, das die Kompilierung für Sie übernimmt. Geben Sie den folgenden Befehl ein, um ihn auszuführen; ignoriere den Kauderwelsch vorerst!

```bash
./rebuild.sh
```

Auf dem Bildschirm wird jetzt viel Text nach oben scrollen, aber für diese Übung können Sie ihn ignorieren. Es ist nur die Ausgabe des Compilers, während er den Democode durcharbeitet. Warten Sie, bis die Eingabeaufforderung zurückkehrt, bevor Sie fortfahren.

Jetzt sind wir endlich bereit, einige Demos auszuführen!

Demoprogramme:

- [Hallo Welt](hello-world.md)
- [Hallo Video](hello-video.md)
- [Hallo Dreieck](hello-triangle.md) (nicht Pi 4)
- [Hallo Fraktal](hello-fractal.md)
- [Hallo Teekanne](hello-teapot.md) (nicht Pi 4)
- [Hallo Audio](hello-audio.md)

Probieren Sie weitere Demos im Ordner „hello_pi“ aus!
