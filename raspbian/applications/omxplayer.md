# OMXPlayer: Ein beschleunigter Kommandozeilen-Mediaplayer

Auf dem Raspberry Pi OS ist ein Kommandozeilen-Mediaplayer namens OMXPlayer installiert. Dies ist HW-beschleunigt und kann viele gängige Audio- und Videodateiformate wiedergeben.

OMXPlayer wurde von Edgar Hucek des Kodi-Projekts entwickelt.

OMXPlayer verwendet die OpenMAX (omx) Hardware Acceleration Interface (API), die offiziell unterstützte Medien-API auf dem Raspberry Pi.

## Grundlegende Nutzung

Die einfachste Befehlszeile ist `omxplayer <Name der Mediendatei>`. Die Mediendatei kann Audio oder Video oder beides sein. Für die folgenden Beispiele haben wir eine H264-Videodatei verwendet, die in der standardmäßigen Raspberry Pi OS-Installation enthalten ist.

```
omxplayer /opt/vc/src/hello_pi/hello_video/test.h264
```

Standardmäßig wird das Audio an den analogen Port gesendet. Wenn Sie ein HDMI-fähiges Anzeigegerät mit Lautsprechern verwenden, müssen Sie omxplayer anweisen, das Audiosignal über die HDMI-Verbindung zu senden.

```
omxplayer --adev hdmi /opt/vc/src/hello_pi/hello_video/test.h264
```

Bei der Videoanzeige wird die gesamte Anzeige als Ausgabe verwendet. Mit der Fensteroption können Sie festlegen, auf welchem ​​Teil der Anzeige das Video angezeigt werden soll.

```
omxplayer --win 0,0,640,480 /opt/vc/src/hello_pi/hello_video/test.h264
```

Sie können auch angeben, welcher Teil des Videos angezeigt werden soll: Dies wird als Zuschneidefenster bezeichnet. Dieser Teil des Videos wird an die Anzeige angepasst, es sei denn, Sie verwenden auch die Fensteroption.

```
omxplayer --crop 100,100,300,300 /opt/vc/src/hello_pi/hello_video/test.h264
```
Wenn Sie das Touchscreen-Display der Raspberry Pi Foundation verwenden und es für die Videoausgabe verwenden möchten, verwenden Sie die Anzeigeoption, um anzugeben, welches Display verwendet werden soll. `n` ist 5 für HDMI, 4 für den Touchscreen. Beim Raspberry Pi 4 haben Sie zwei Möglichkeiten für die HDMI-Ausgabe. `n` ist 2 für HDMI0 und 7 für HDMI1.

```
omxplayer --display n /opt/vc/src/hello_pi/hello_video/test.h264
```
## Während der Wiedergabe verfügbare Optionen

Während der Wiedergabe stehen eine Reihe von Optionen zur Verfügung, die durch Drücken der entsprechenden Taste ausgeführt werden. Nicht alle Optionen sind für alle Dateien verfügbar. Die Liste der Tastenbelegungen kann mit `omxplayer --keys` angezeigt werden:

```
    1                    decrease speed - Geschwindigkeit verringern
    2                    increase speed - Geschwindigkeit erhöhen
    <                    rewind - zurückspulen
    >                    fast forward - schneller Vorlauf
    z                    show info - Infos anzeigen
    j                    previous audio stream - vorheriger Audiostream
    k                    next audio stream - nächster Audiostream
    i                    previous chapter - vorheriges Kapitel
    o                    next chapter - nächstes Kapitel
    n                    previous subtitle stream - vorheriger Untertitelstream
    m                    next subtitle stream - nächster Untertitelstream
    s                    toggle subtitles - Untertitel umschalten
    w                    show subtitles - Untertitel anzeigen
    x                    hide subtitles - Untertitel ausblenden
    d                    decrease subtitle delay (- 250 ms) - Untertitelverzögerung verringern (- 250 ms)
    f                    increase subtitle delay (+ 250 ms) - Untertitelverzögerung erhöhen (+ 250 ms)
    q                    exit omxplayer - omxplayer beenden
    p / Leertaste        pause/resume - Pause/Fortsetzen
    -                    decrease volume - Lautstärke verringern
    + / =                increase volume - Lautstärke erhöhen
    Pfeil nach links     suchen -30 Sekunden
    Pfeil nach rechts    suchen +30 Sekunden
    Pfeil nach unten     suchen -600 Sekunden
    Pfeil nach oben      suchen +600 Sekunden

```

## Alle Befehlszeilenoptionen

Dies ist eine vollständige Liste der Optionen, die im Build vom 23. September 2016 verfügbar sind und mit `omxplayer --help` angezeigt werden:

```
 -h --help                  Diese Hilfe drucken
 -v --version               Versionsinfo drucken
 -k --keys                  Tastenbelegung drucken
 -n --aidx                  index Audiostream-Index: z.B. 1
 -o --adev                  Gerät Audioausgabegerät : z.B. HDMI/lokal/beide/alsa[:Gerät]
 -i --info                  Streamformat ausgeben und beenden
 -I --with-info             Dump-Stream-Format vor der Wiedergabe
 -s --stats                 Punkte und Pufferstatistiken
 -p --passthrough           Audio-Passthrough
 -d --deinterlace           Deinterlacing erzwingen
     --nodeinterlace        Kein Deinterlacing erzwingen
     --nativedeinterlace    lässt das Display mit Interlace umgehen
     --anaglyph type        konvertiert 3d in anaglyph
     --advanced[=0]         Aktiviert/deaktiviert das erweiterte Deinterlace für HD-Videos (standardmäßig aktiviert)
 -w --hw                    Hw-Audio-Dekodierung
 -3 -3d mode                Fernseher in den 3D-Modus schalten (z. B. SBS/TB)
 -M --allow-mvc             Erlaube die Dekodierung beider Ansichten des MVC-Stereostreams
 -y --hdmiclocksync         Zeigt die Bildwiederholfrequenz an, um dem Video zu entsprechen (Standard)
 -z --nohdmiclocksync       Passt die Bildwiederholfrequenz nicht an das Video an
 -t --sid index             Untertitel mit Index anzeigen
 -r --refresh               Passt die Bildrate/Auflösung an das Video an
 -g --genlog                Protokolldatei erstellen
 -l --pos n                 Startposition (hh:mm:ss)
 -b --blank[=0xAARRGGBB]    Setzt die Videohintergrundfarbe auf Schwarz (oder optionalen ARGB-Wert)
     --loop                 Schleifendatei. Ignoriert, wenn Datei nicht durchsuchbar
     --no-boost-on-downmix  Erhöhen Sie die Lautstärke beim Downmixen nicht
     --vol n                setzt das Anfangsvolumen in Millibel (Standard 0)
     --amp n                setzt die anfängliche Verstärkung in Millibel (Standard 0)
     --no-osd               Keine Statusinformationen auf dem Bildschirm anzeigen
     --no-keys              Tastatureingabe deaktivieren (verhindert das Hängenbleiben bei bestimmten TTYs)
     --subtitles path       Externe Untertitel im UTF-8 srt-Format
     --font path            Standard Pfad: /usr/share/fonts/truetype/freefont/FreeSans.ttf
     --italic-font path     Standard Pfad: /usr/share/fonts/truetype/freefont/FreeSansOblique.ttf
     --font-size size       Schriftgröße in 1/1000 Bildschirmhöhe (Standard: 55)
     --align left/center    Untertitel-Ausrichtung (Standard: links)
     --no-ghost-box         Keine halbtransparenten Kästchen hinter Untertiteln
     --lines n              Anzahl der Zeilen im Untertitelpuffer (Standard: 3)
     --win 'x1 y1 x2 y2'    Position des Videofensters setzen
 --win x1,y1,x2,y2          Position des Videofensters setzen
 --crop 'x1 y1 x2 y2'       Zuschneidebereich für Eingangsvideo festlegen
 --crop x1,y1,x2,y2         Zuschneidebereich für Eingangsvideo festlegen
 --aspect-mode type         Letterbox, Füllen, Strecken. Standard ist Stretch, wenn win angegeben ist, sonst Letterbox
 --audio_fifo n             Größe des Audioausgabe-Fifo in Sekunden
 --video_fifo n             Größe des Videoausgabe-Fifo in MB
 --audio_queue n            Größe der Audioeingangswarteschlange in MB
 --video_queue n            Größe der Videoeingangswarteschlange in MB
 --threshold n              Menge der gepufferten Daten, die erforderlich ist, um die Pufferung abzuschließen [s]
 --timeout n                Timeout für angehaltene Datei-/Netzwerkvorgänge (Standard 10s)
 --orientation n            Ausrichtung des Videos festlegen (0, 90, 180 oder 270)
 --fps n                    Setzt fps des Videos, wenn keine Zeitstempel vorhanden sind
 --live                     Auf Live-TV oder Vod-Stream eingestellt
 --layout                   Legt das Layout der Ausgangslautsprecher fest (z. B. 5.1)
 --dbus_name name           Standard: org.mpris.MediaPlayer2.omxplayer
 --key-config <file>        Verwendet Tastenzuordnungen in <Datei> anstelle der Standardeinstellung
 --alpha                    Videotransparenz einstellen (0..255)
 --layer n                  Legt die Nummer des Video-Render-Layers fest (höhere Zahlen stehen oben)
 --display n                Anzeige auf Ausgabe setzen auf
 --cookie 'cookie'          Angegebenes Cookie als Teil von HTTP-Anfragen senden
 --user-agent 'ua'          Angegebenen User-Agent als Teil von HTTP-Anfragen senden
 --lavfdopts 'opts'         An libavformat übergebene Optionen, z.B. 'Sondengröße:250000,...'
 --avdict 'opts'            An Demuxer übergebene Optionen, z. B. 'rtsp_transport:tcp,...'

```