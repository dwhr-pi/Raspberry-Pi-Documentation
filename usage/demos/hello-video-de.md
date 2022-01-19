# DIESE SEITE IST JETZT VERALTET

# Hallo-Video

Dadurch wird ein 15 Sekunden langer Videoclip in Full HD 1080p ohne Ton abgespielt. Die Absicht hier ist es, die Video-Decodierungs- und -Wiedergabefähigkeit zu demonstrieren. Sie werden sehen, dass das Video sehr flüssig ist!

![Big Buck Bunny-Screenshot](images/bbb.jpg)
 
Geben Sie die folgenden Befehle ein, um zum Ordner „hello_video“ zu navigieren und die Dateien aufzulisten:

```bash
cd ..
cd hello_video
ls
```

Sie werden wieder die Datei „.bin“ bemerken. Dieser Demo muss jedoch mitgeteilt werden, welcher Videoclip abgespielt werden soll, wenn wir sie ausführen, also muss dies die Datei „test.h264“ sein (h264 ist eine Art Video-Codec).

Sie benötigen das `./`, um das aktuelle Verzeichnis erneut anzugeben:

```bash
./hello_video.bin test.h264
```

Sie sollten jetzt den Videoclip abspielen sehen. Es stammt aus dem Open-Source-Film [Big Buck Bunny](https://en.wikipedia.org/wiki/Big_Buck_Bunny).
