# Raspberry Pi Kameramodul

Dieses Dokument beschreibt die Verwendung der vier Raspberry Pi Kameraanwendungen, Stand 30. April 2020.

Es stehen vier Anwendungen zur Verfügung: `raspistill`, `raspivid`, `raspiyuv` und `raspividyuv`. `raspistill` und `raspiyuv` sind sehr ähnlich und für die Aufnahme von Bildern gedacht; `raspivid` und `raspvidyuv` dienen zum Aufnehmen von Videos.

Alle Anwendungen werden über die Befehlszeile gesteuert und so geschrieben, dass sie die MMAL-API nutzen, die über OpenMAX läuft. Die MMAL-API bietet ein einfacher zu verwendendes System als das von OpenMAX vorgestellte. Beachten Sie, dass MMAL eine Broadcom-spezifische API ist, die nur auf VideoCore 4-Systemen verwendet wird.

Die Anwendungen verwenden bis zu vier OpenMAX (MMAL)-Komponenten: Kamera, Vorschau, Encoder und null_sink. Alle Anwendungen verwenden die Kamerakomponente; `raspistill` verwendet die Image Encode-Komponente; `raspivid` verwendet die Video Encode-Komponente; und `raspiyuv` und `raspividyuv` verwenden keinen Encoder und senden ihre YUV- oder RGB-Ausgabe direkt von der Kamerakomponente in eine Datei.

Die Vorschauanzeige ist optional, kann aber im Vollbildmodus verwendet oder auf einen bestimmten rechteckigen Bereich auf dem Display gerichtet werden. Wenn die Vorschau deaktiviert ist, wird die null_sink-Komponente verwendet, um die Vorschau-Frames zu „absorbieren“. Die Kamera muss Vorschaubilder erzeugen, auch wenn diese nicht für die Anzeige benötigt werden, da sie für die Berechnung der Belichtungs- und Weißabgleicheinstellungen verwendet werden.

Außerdem ist es möglich, die Dateiname-Option wegzulassen (in diesem Fall wird die Vorschau angezeigt, aber keine Datei geschrieben) oder die gesamte Ausgabe auf stdout umzuleiten.

Die Befehlszeilenhilfe ist verfügbar, indem Sie nur den Anwendungsnamen in die Befehlszeile eingeben.

## Einrichten

Siehe [Camera Setup](../../configuration/camera.md).

## Fehlerbehebung

Wenn das Kameramodul nicht richtig funktioniert, können Sie Folgendes versuchen:

- Ist das Flachbandkabel an der seriellen Kameraschnittstelle (CSI) angeschlossen, nicht an der seriellen Anzeigeschnittstelle (DSI)? Der Flachbandstecker passt in beide Ports. Der Kameraanschluss befindet sich in der Nähe des HDMI-Anschlusses.

- Sitzen die Flachbandstecker alle fest und richtig herum? Sie müssen gerade in ihren Fassungen sein.

- Ist der Anschluss des Kameramoduls zwischen dem kleineren schwarzen Kameramodul selbst und der Platine fest angeschlossen? Manchmal kann sich diese Verbindung während des Transports oder beim Verstauen des Kameramoduls in einem Koffer lösen. Klappen Sie den Stecker auf der Platine mit einem Fingernagel hoch und schließen Sie ihn mit leichtem Druck wieder an. Es rastet mit einem ganz leichten Klick ein. Erzwinge es nicht; Wenn es nicht einrastet, ist es wahrscheinlich etwas falsch ausgerichtet.

- Wurden `sudo apt update` und `sudo apt full-upgrade` ausgeführt?

- Wurde `raspi-config` ausgeführt und das Kameramodul aktiviert?

- Ist Ihre Stromversorgung ausreichend? Das Kameramodul erhöht den Strombedarf Ihres Raspberry Pi um etwa 200-250mA.

Wenn die Dinge immer noch nicht funktionieren, versuchen Sie Folgendes:

- `Fehler: raspistill/raspivid-Befehl nicht gefunden`. Dies bedeutet wahrscheinlich, dass Ihr Update/Upgrade in irgendeiner Weise fehlgeschlagen ist. Versuchen Sie es erneut.

- `Fehler: ENOMEM`. Das Kameramodul startet nicht. Überprüfen Sie erneut alle Verbindungen.

- `Fehler: ENOSPC`. Dem Kameramodul geht wahrscheinlich der GPU-Speicher aus. Überprüfen Sie `config.txt` im /boot/-Ordner. Die Option `gpu_mem` sollte mindestens 128 sein. Alternativ können Sie die Option Memory Split im Abschnitt Advanced von `raspi-config` verwenden, um dies einzustellen.

- Wenn Sie alle oben genannten Probleme überprüft haben und das Kameramodul immer noch nicht funktioniert, versuchen Sie, in unseren Foren zu posten, um weitere Hilfe zu erhalten.

## Allgemeine Befehlszeilenoptionen

### Vorschaufenster

```
	--preview,	-p		Vorschaufenstereinstellungen <'x,y,w,h'>
```

Ermöglicht dem Benutzer, die Größe des Vorschaufensters und seine Position auf dem Bildschirm zu definieren. Beachten Sie, dass dies über alle anderen Fenster/Grafiken gelegt wird.

```
	--fullscreen,	-f		Vollbild-Vorschaumodus
```

Erzwingt, dass das Vorschaufenster den gesamten Bildschirm verwendet. Beachten Sie, dass das Seitenverhältnis des eingehenden Bildes beibehalten wird, sodass an einigen Kanten Balken vorhanden sein können.

```
	--nopreview,	-n		Kein Vorschaufenster anzeigen
```

Deaktiviert das Vorschaufenster vollständig. Beachten Sie, dass die Kamera auch bei deaktivierter Vorschau Frames produziert, also Strom verbraucht.

```
	--opacity,	-op		Legt die Deckkraft des Vorschaufensters fest
```

Legt die Deckkraft der Vorschaufenster fest. 0 = unsichtbar, 255 = vollständig undurchsichtig.

### Optionen zur Kamerasteuerung

```
	--sharpness,	-sh		Bildschärfe einstellen (-100 - 100)
```

Stellt die Schärfe des Bildes ein. 0 ist die Standardeinstellung.

```
	--contrast,	-co		Bildkontrast einstellen (-100 - 100)
```

Stellt den Kontrast des Bildes ein. 0 ist die Standardeinstellung.

```
	--brightness,	-br		Bildhelligkeit einstellen (0 - 100)
```

Stellt die Helligkeit des Bildes ein. 50 ist die Standardeinstellung. 0 ist schwarz, 100 ist weiß.

```
	--saturation,	-sa		Bildsättigung einstellen (-100 - 100)
```

Legt die Farbsättigung des Bildes fest. 0 ist die Standardeinstellung.

```
	--ISO,	-ISO		Aufnahme-ISO einstellen (100 - 800)
```

Legt die für Aufnahmen zu verwendende ISO fest.

```
	--vstab,	-vs		Videostabilisierung aktivieren
```

Nur im Videomodus: Aktiviert die Videostabilisierung.

```
	--ev,	-ev		EV-Korrektur einstellen (-10 - 10)
```

Stellt die EV-Korrektur des Bildes ein. Standard ist 0.

```
	--exposure,	-ex		Belichtungsmodus einstellen
```

Mögliche Optionen sind:

- auto: Verwenden Sie den automatischen Belichtungsmodus
- night: Wählen Sie die Einstellung für Nachtaufnahmen
- nightpreview: Nachtvorschau
- backlight: Wählen Sie die Einstellung für das Motiv mit Hintergrundbeleuchtung
- spotlight: Scheinwerfer
- sports: Einstellung für Sport auswählen (schneller Verschluss usw.)
- snow: Wählen Sie die Einstellung, die für verschneite Landschaften optimiert ist
- beach: für Strand optimierte Einstellung wählen
- verylong: Einstellung für Langzeitbelichtungen auswählen
- fixedfps: fps auf einen festen Wert beschränken
- antishake: Antishake-Modus
- fireworks: Wählen Sie die für Feuerwerk optimierte Einstellung

Beachten Sie, dass je nach Kameraeinstellung möglicherweise nicht alle diese Einstellungen implementiert werden.

```
	--flicker, -fli		Flimmervermeidungsmodus einstellen
```
Stellen Sie einen Modus ein, um Lichtflimmern bei der Netzfrequenz zu kompensieren, das als dunkles horizontales Band über einem Bild zu sehen ist. Die Flicker-Vermeidung sperrt die Belichtungszeit auf ein Vielfaches der Netzflimmerfrequenz (8,33 ms für 60 Hz oder 10 ms für 50 Hz). Dies bedeutet, dass Bilder stärker verrauscht sein können, da der Steueralgorithmus die Verstärkung anstelle der Belichtungszeit erhöhen muss, wenn er einen Zwischenbelichtungswert wünscht. `auto` kann durch externe Faktoren verwechselt werden, daher ist es vorzuziehen, diese Einstellung deaktiviert zu lassen, es sei denn, sie wird tatsächlich benötigt.

Mögliche Optionen sind:

- off: Flackervermeidung ausschalten
- auto: Netzfrequenz automatisch erkennen
- 50Hz: Vermeidung auf 50Hz einstellen
- 60Hz: Vermeidung auf 60Hz einstellen

```
	--awb,	-awb		Automatischen Weißabgleich (AWB)-Modus einstellen
```
Modi, für die Farbtemperaturbereiche (K) verfügbar sind, haben diese Einstellungen in Klammern.

- off: Weißabgleich-Berechnung deaktivieren
- auto: Automatikmodus (Standard)
- sun: Sonnenmodus (zwischen 5000K und 6500K)
- cloud: Bewölkter Modus (zwischen 6500K und 12000K)
- shade: Schattenmodus
- tungsten: Wolfram-Beleuchtungsmodus (zwischen 2500K und 3500K)
- fluorescent:  fluoreszierender Beleuchtungsmodus (zwischen 2500K und 4500K)
- incandescent: Glühlampen-Beleuchtungsmodus
- flash: Blitzmodus
- horizon: Horizontmodus
- greyworld: Verwenden Sie dies auf der NoIR-Kamera, um falsche AWB-Ergebnisse aufgrund des Fehlens des IR-Filters zu korrigieren.


Beachten Sie, dass je nach Kameratyp möglicherweise nicht alle dieser Einstellungen implementiert werden.

```
	--imxfx,	-ifx		Bildeffekt einstellen
```

Legen Sie einen Effekt fest, der auf das Bild angewendet werden soll:

- none: kein Effekt (Standard)
- negative: invertieren Sie die Bildfarben
- solarise: solarisiere das Bild
- posterise: das Bild posterisieren
- whiteboard: Whiteboard-Effekt
- blackboard: Tafeleffekt
- sketch: Skizzeneffekt
- denoise: das Bild entrauschen
- emboss: das Bild prägen
- oilpaint: Ölfarbeneffekt
- hatch: Schraffurskizzeneffekt
- gpen: Graphitskizzeneffekt
- pastel: Pastelleffekt
- watercolour: Aquarelleffekt
- film: Filmkorneffekt
- blur: das Bild verwischen
- saturation: Farbe sättigt das Bild
- colourswap: Farbwechsel - nicht vollständig implementiert
- washedout: verwaschen - nicht vollständig implementiert
- colourpoint: Farbpunkt - nicht vollständig implementiert
- colourbalance: Farbbalance - nicht vollständig implementiert
- Cartoon: nicht vollständig implementiert

Beachten Sie, dass nicht alle diese Einstellungen unter allen Umständen verfügbar sind.

```
	--colfx,	-cfx		Farbeffekt setzen <U:V>
```

Die gelieferten U- und V-Parameter (Bereich 0 - 255) werden auf die U- und Y-Kanäle des Bildes angewendet. Beispielsweise sollte --colfx 128:128 ein monochromes Bild ergeben.

```
	--metering,	-mm		Messmodus einstellen
```

Geben Sie den Messmodus für die Vorschau und Aufnahme an:

- average: Durchschnitt des gesamten Frames für die Messung
- spot: Spotmessung
- backlit: ein hintergrundbeleuchtetes Bild annehmen
- matrix: Matrixmessung

```
	--rotation,	-rot		Bilddrehung einstellen (0 - 359)
```

Legt die Drehung des Bildes im Sucher und des resultierenden Bildes fest. Dies kann jeden Wert von 0 aufwärts annehmen, aber aufgrund von Hardwarebeschränkungen werden nur Drehungen von 0, 90, 180 und 270 Grad unterstützt.

```
	--hflip,	-hf		Setzt horizontalen Flip
```

Spiegelt die Vorschau und das gespeicherte Bild horizontal.

```
	--vflip,	-vf		Vertikales Spiegeln einstellen
```

Spiegelt die Vorschau und das gespeicherte Bild vertikal.

```
	--roi,	-roi		Sensorregion von Interesse einstellen
```

Ermöglicht die Angabe des Bereichs des Sensors, der als Quelle für die Vorschau und Aufnahme verwendet werden soll. Dies ist als XY für die obere linke Ecke und eine Breite und Höhe mit allen Werten in normalisierten Koordinaten (0,0 - 1,0) definiert. Um also einen ROI auf halber Höhe des Sensors und eine Breite und Höhe von einem Viertel des Sensors festzulegen, verwenden Sie:

```
-roi 0.5,0.5,0.25,0.25
```

```
	--shutter,	-ss		Einstellen der Verschlusszeit/-zeit
```

Setzt die Verschlusszeit auf den angegebenen Wert (in Mikrosekunden). Die Verschlusszeitbegrenzungen sind wie folgt:

| Kameraversion  | Max (Mikrosekunden)  |
|----------------|:--------------------:| 
| V1 (OV5647)    | 6000000 (i.e. 6s)    |
| V2 (IMX219)    | 10000000 (i.e. 10s)  |
| HQ (IMX477)    | 200000000 (i.e. 200s)|

Die Verwendung von Werten über diesen Maximalwerten führt zu undefiniertem Verhalten.

```
	--drc,	-drc		Enable/disable - Dynamische Bereichskomprimierung aktivieren/deaktivieren
```

DRC ändert die Bilder, indem es den Bereich der dunklen Bereiche vergrößert und die helleren Bereiche verringert. Dies kann das Bild in Bereichen mit schwachem Licht verbessern.

- off
- low
- med
- high

DRC ist standardmäßig deaktiviert.

```
	--stats,	-st		Standbildaufnahmerahmen für Bildstatistiken verwenden
```

Erzwingen Sie die Neuberechnung von Statistiken für Standbilder-Erfassungsdurchgänge. Digitale Verstärkung und AWB werden auf der Grundlage der tatsächlichen Erfassungsbildstatistik neu berechnet und nicht auf dem vorhergehenden Vorschaubild.

```
	--awbgains,	-awbg
```

Legt Blau- und Rotverstärkungen (als Gleitkommazahlen) fest, die angewendet werden sollen, wenn `-awb off` eingestellt ist, z.B. -awbg 1.5,1.2

```
	--analoggain,	-ag
```

Stellt den analogen Verstärkungswert direkt am Sensor ein (Gleitkommawert von 1,0 bis 8,0 für den OV5647-Sensor am Kameramodul V1 und 1,0 bis 12,0 für den IMX219-Sensor am Kameramodul V2 und den IMX447 an der HQ-Kamera).

```
	--digitalgain,	-dg
```

Legt den vom ISP angewendeten digitalen Verstärkungswert fest (Gleitkommawert von 1,0 bis 64,0, aber Werte über etwa 4,0 führen zu überbelichteten Bildern)

```
	--mode,	-md
```

Stellt einen bestimmten Sensormodus ein und deaktiviert die automatische Auswahl. Mögliche Werte hängen von der Version des verwendeten Kameramoduls ab:

Version 1.x (OV5647)

|Modus| Größe | Seitenverhältnis |Bildraten | Sichtfeld | Binning |
|-----|-------|------------------|----------|-----------|---------|
|0| automatische Auswahl | | | | |
|1|1920x1080|16:9| 1-30fps|Teilweise|Keine|
|2|2592x1944|4:3|1-15fps|Voll|Keine|
|3|2592x1944|4:3|0,1666-1fps|Voll|Keine|
|4|1296x972|4:3|1-42fps|Voll|2x2|
|5|1296x730|16:9|1-49fps|Voll|2x2|
|6|640x480|4:3|42,1-60fps|Full|2x2 plus Überspringen|
|7|640x480|4:3|60,1-90fps|Voll|2x2 plus Überspringen|

Version 2.x (IMX219)

|Modus| Größe | Seitenverhältnis |Bildraten | Sichtfeld | Binning |
|-----|-------|------------------|----------|-----------|---------|
|0| automatische Auswahl | | | | |
|1|1920x1080|16:9| 0.1-30fps|Teilweise|Keine|
|2|3280x2464|4:3|0,1-15fps|Voll|Keine|
|3|3280x2464|4:3|0,1-15fps|Voll|Keine|
|4|1640x1232|4:3|0,1-40fps|Voll|2x2|
|5|1640x922|16:9|0,1-40fps|Voll|2x2|
|6|1280x720|16:9|40-90fps|Teilweise|2x2|
|7|640x480|4:3|40-200fps<sup>1</sup>|Teilweise|2x2|

<sup>1</sup>Bei Bildraten über 120 fps ist es notwendig, die automatische Belichtungs- und Verstärkungsregelung mit `-ex off` zu deaktivieren. Dadurch sollten die höheren Bildraten erreicht werden, aber Belichtungszeit und Verstärkungen müssen auf feste Werte eingestellt werden, die vom Benutzer bereitgestellt werden.

HQ-Kamera

|Modus| Größe | Seitenverhältnis |Bildraten | Sichtfeld | Binning/Skalierung |
|-----|-------|------------------|----------|-----------|--------------------|
|0| automatische Auswahl | | | | |
|1|2028x1080|169:90| 0.1-50fps|Teilweise|2x2 binned|
|2|2028x1520|4:3|0,1-50fps|Voll|2x2 binned|
|3|4056x3040|4:3|0,005-10fps|Voll|Keine|
|4|1012x760|4:3|50,1-120fps|Voll|4x4 skaliert|

```
	--camselect,	-cs
```
Wählt aus, welche Kamera in einem Multikamerasystem verwendet werden soll. Verwenden Sie 0 oder 1.


```
	--annotate,	-a		Enable/set annotate flags or text
```
Fügt dem Bild Text und/oder Metadaten hinzu.

Metadaten werden mit einer Bitmasken-Notation angezeigt, fügen Sie sie also zusammen, um mehrere Parameter anzuzeigen. 12 zeigt beispielsweise time(4) und date(8) an, da 4+8=12.

Text kann Platzhalter für Datum/Uhrzeit enthalten, indem das Zeichen '%' verwendet wird, wie es von <a title="strftime man page" href="http://man7.org/linux/man-pages/man3/strftime.3 verwendet wird. html">strftime</a>.

|Wert| Bedeutung | Beispielausgabe |
|----|-----------|-----------------|
|-a 4|Zeit|20:09:33|
|-a 8|Datum|28.10.15|
|-a 12|4+8=12 Datum(4) und Uhrzeit(8) anzeigen|20:09:33 10/28/15|
|-a 16|Verschlusseinstellungen| |
|-a 32|CAF-Einstellungen| |
|-a 64|Verstärkungseinstellungen| |
|-a 128|Objektiveinstellungen| |
|-a 256|Bewegungseinstellungen| |
|-a 512|Rahmennummer| |
|-a 1024|Schwarzer Hintergrund| |
|-a "ABC %Y-%m-%d %X"|Text anzeigen|ABC %Y-%m-%d %X|
|-a 4 -a "ABC %Y-%m-%d %X"|Benutzerdefinierte <a title="strftime man-Seite" href="http://man7.org/linux/man-pages/man3/ anzeigen strftime.3.html">formatiertes</a> Datum/Uhrzeit|ABC 2015-10-28 20:09:33|
|-a 8 -a "ABC %Y-%m-%d %X"|Benutzerdefinierte <a title="strftime man-Seite" href="http://man7.org/linux/man-pages/man3/ anzeigen strftime.3.html">formatiertes</a> Datum/Uhrzeit|ABC 2015-10-28 20:09:33|

```
	--annotateex,	-ae		Setzt zusätzliche Anmerkungsparameter
```

Gibt Anmerkungsgröße, Textfarbe und Hintergrundfarbe an. Die Farben sind im Hex-YUV-Format.

Größe reicht von 6 - 160; Der Standardwert ist 32. Wenn Sie nach einer ungültigen Größe fragen, sollten Sie den Standardwert erhalten.

|Beispiel|Erklärung|
|-------|----------|
|-ae 32,0xff,0x808000 -a "Anmerkungstext"|gibt Größe 32 weißen Text auf schwarzem Hintergrund|
|-ae 10,0x00,0x8080FF -a "Anmerkungstext"|gibt schwarzen Text der Größe 10 auf weißem Hintergrund|

```
	--stereo,	-3d
```

Wählen Sie den angegebenen Stereo-Bildgebungsmodus aus; `sbs` wählt den Side-by-Side-Modus, `tb` wählt den Top/Bottom-Modus; `off` schaltet den Stereomodus aus (Standard).

```
	--decimate,	-dec
```
Halbiert Breite und Höhe des Stereobildes.

```
	--3dswap,	-3dswap
```
Vertauscht die bei der stereoskopischen Bildgebung verwendete Kamerareihenfolge; HINWEIS: funktioniert derzeit nicht.

```
	--settings,	-set
```
Ruft die aktuellen Kameraeinstellungen ab und schreibt sie auf stdout.


## Anwendungsspezifische Einstellungen

### Raspistille

```
	--width,	-w		Setzt die Bildbreite <Größe>

	--height,	-h		Bildhöhe einstellen <Größe>

	--quality,	-q		Einstellen der JPEG-Qualität <0 bis 100>
```

Qualität 100 ist fast vollständig unkomprimiert. 75 ist ein guter Allround-Wert.

```
	--raw,          -r      Rohe Bayer-Daten zu JPEG-Metadaten hinzufügen
```

Diese Option fügt die Bayer-Rohdaten von der Kamera in die JPEG-Metadaten ein.

```
	--output,	-o		Ausgabedateiname <Dateiname>
```

Gibt den Ausgabedateinamen an. Wenn nicht angegeben, wird keine Datei gespeichert. Wenn der Dateiname '-' ist, wird die gesamte Ausgabe an stdout gesendet.

```
	--latest,	-l		Letzten Frame mit Dateiname verknüpfen <Dateiname>
```

Erstellt eine Dateisystemverknüpfung unter diesem Namen zum neuesten Frame.

```
	--verbose,	-v		Ausführliche Informationen während der Ausführung ausgeben
```

Gibt während des Programmlaufs Debug-/Informationsmeldungen aus.

```
	--timeout,	-t		Zeit, bevor die Kamera ein Bild aufnimmt und herunterfährt
```

Das Programm wird für die angegebene Dauer in Millisekunden ausgeführt. Es nimmt dann die Aufnahme und speichert sie, wenn eine Ausgabe angegeben wird. Wenn kein Timeout-Wert angegeben ist, wird er auf 5 Sekunden (-t 5000) gesetzt. Beachten Sie, dass niedrige Werte (weniger als 500 ms, obwohl dies von anderen Einstellungen abhängen kann) der Kamera möglicherweise nicht genügend Zeit zum Starten geben und nicht genügend Frames für die automatischen Algorithmen wie AWB und AGC bereitstellen, um genaue Ergebnisse zu liefern.

Wenn auf 0 gesetzt, läuft die Vorschau unbegrenzt, bis sie mit STRG-C gestoppt wird. In diesem Fall erfolgt keine Erfassung.

```
	--timelapse,	-tl		Zeitraffermodus
```

Der spezifische Wert ist die Zeit zwischen den Schüssen in Millisekunden. Beachten Sie, dass Sie `%04d` an der Stelle im Dateinamen angeben sollten, an der eine Frame-Zählnummer erscheinen soll. Der folgende Code erzeugt beispielsweise alle 2 Sekunden über einen Zeitraum von insgesamt 30 Sekunden eine Aufnahme mit den Namen `image0001.jpg`, `image0002.jpg` und so weiter bis hin zu `image0015.jpg`.

```
-t 30000 -tl 2000 -o image%04d.jpg
```

Beachten Sie, dass `%04d` eine 4-stellige Zahl anzeigt, wobei führende Nullen hinzugefügt werden, um die erforderliche Anzahl von Stellen zu bilden. So würde beispielsweise `%08d` zu einer 8-stelligen Zahl führen.

Wenn ein Zeitrafferwert von 0 eingegeben wird, nimmt die Anwendung Bilder so schnell wie möglich auf. Beachten Sie, dass zwischen den Aufnahmen eine erzwungene Mindestpause von 30 ms liegt, um sicherzustellen, dass Belichtungsberechnungen durchgeführt werden können.

```
	--framestart,	-fs
```

Gibt die erste Bildnummer im Zeitraffer an. Nützlich, wenn Sie bereits mehrere Frames gespeichert haben und beim nächsten Frame erneut beginnen möchten.

```
	--datetime,	-dt
```

Anstelle einer einfachen Bildnummer verwenden die Zeitrafferdateinamen einen Datums-/Uhrzeitwert im Format `aabbccddee`, wobei `aa` der Monat ist, `bb` der Tag des Monats, `cc` die Stunde, `dd` ist die Minute und `ee` ist die Sekunde.

```
	--timestamp,	-ts
```

Anstelle einer einfachen Bildnummer verwenden die Zeitrafferdateinamen eine einzelne Zahl, die dem Unix-Zeitstempel entspricht, d. h. die Sekunden seit 1970.

```
	--thumb,	-th		Einstellen der Thumbnail-Parameter (x:y:quality)
```

Ermöglicht die Angabe des Miniaturbilds, das in die JPEG-Datei eingefügt wird. Wenn nicht angegeben, wird standardmäßig eine Größe von 64 x 48 bei Qualität 35 verwendet.

Wenn `--thumb none` angegeben ist, werden keine Thumbnail-Informationen in die Datei eingefügt. Dadurch wird die Dateigröße etwas reduziert.

```
	--demo,	-d		Führe einen Demo-Modus aus <Millisekunden>
```

Diese Option durchläuft den Bereich der Kameraoptionen. Es wird keine Erfassung durchgeführt und die Demo endet am Ende des Timeout-Zeitraums, unabhängig davon, ob alle Optionen durchlaufen wurden. Die Zeit zwischen den Zyklen sollte als Millisekundenwert angegeben werden.

```
	--encoding,	-e		Codierung für die Ausgabedatei
```

Gültige Optionen sind `jpg`, `bmp`, `gif` und `png`. Beachten Sie, dass das Speichern von nicht beschleunigten Bildtypen (GIF, PNG, BMP) viel länger dauert als das Speichern von jpg, das hardwarebeschleunigt ist. Beachten Sie auch, dass das Dateinamensuffix bei der Entscheidung über die Kodierung einer Datei vollständig ignoriert wird.

```
	--restart,	-rs
```

Setzt das JPEG-Neustartmarkierungsintervall auf einen bestimmten Wert. Kann für verlustbehaftete Transportströme nützlich sein, da eine beschädigte JPEG-Datei immer noch teilweise angezeigt werden kann.

```
	--exif,	-x		EXIF-Tag zum Anwenden auf Captures (Format als 'key=value')
```

Ermöglicht das Einfügen bestimmter EXIF-Tags in das JPEG-Bild. Sie können bis zu 32 EXIF-Tag-Einträge haben. Dies ist nützlich für Aufgaben wie das Hinzufügen von GPS-Metadaten. So stellen Sie beispielsweise den Längengrad ein:

```
--exif GPS.GPSLongitude=5/1,10/1,15/1
```

würde den Längengrad auf 5 Grad, 10 Minuten, 15 Sekunden einstellen. Weitere Informationen zu den verfügbaren Tags finden Sie in der EXIF-Dokumentation; die unterstützten Tags sind wie folgt:

```
IFD0.<   oder
IFD1.<
ImageWidth, ImageLength, BitsPerSample, Compression, PhotometricInterpretation, ImageDescription, Make, Model, StripOffsets, Orientation, SamplesPerPixel, RowsPerString, StripByteCounts, XResolution, YResolution, PlanarConfiguration, ResolutionUnit, TransferFunction, Software, DateTime, Artist, JPEG,PrimaryChromatic YCbCrKoeffizienten, YCbCrSubSampling, YCbCrPositioning, ReferenceBlackWhite, Copyright>

EXIF.<
ExposureTime, FNumber, ExposureProgram, SpectralSensitivity, ISOSpeedRatings, OECF, ExifVersion, DateTimeOriginal, DateTimeDigitized, ComponentsConfiguration, CompressedBitsPerPixel, ShutterSpeedValue, ApertureValue, BrightnessValue, ExposureBiasValue, MaxApertureValue, SubjectDistance,ThTimerNoteS Light SubSecTimeOriginal, SubSecTimeDigitized, FlashpixVersion, ColorSpace, PixelXDimension, PixelYDimension, RelatedSoundFile, FlashEnergy, SpatialFrequencyResponse, FocalPlaneXResolution, FocalPlaneYResolution, FocalPlaneResolutionUnit, SubjectLocation,ExposureIndex,Scene,SceneMode,Inmm White,FocalPlaneResolution,FocalPlaneResolution,FocalPlaneResolutionUnit Kontrast, Sättigung, Schärfe, DeviceSettingDescription, SubjectDistanceRange, ImageUniqueID>

GPS.<
GPSVersionID, GPSLatitudeRef, GPSLatitude, GPSLongitudeRef, GPSLongitude, GPSAltitudeRef, GPSAltitude, GPSTimeStamp, GPSSatellites, GPSStatus, GPSMeasureMode, GPSDOP, GPSSpeedRef, GPSSpeed, GPSTrackRef, GPSTrack, GPSImgDirectionRef, DDLest, GPSImgDirection, GPSTimeStamp, GPSDestLitude, GPSDest, GPSDestDistanceRef, GPSDestDistance, GPSProcessingMethod, GPSAreaInformation, GPSDateStamp, GPSDifferential>

EINT.<
InteroperabilityIndex, InteroperabilityVersion, RelatedImageFileFormat, RelatedImageWidth, RelatedImageLength>
```

Beachten Sie, dass eine kleine Teilmenge dieser Tags automatisch vom Kamerasystem gesetzt wird, aber von allen EXIF-Optionen in der Befehlszeile überschrieben wird.

Die Einstellung `--exif none` verhindert, dass EXIF-Informationen in der Datei gespeichert werden. Dadurch wird die Dateigröße etwas reduziert.

```
	--gpsdexif,	-gps
```

Wendet Echtzeit-EXIF-Informationen von jedem angeschlossenen GPS-Dongle (mit GSPD) auf das Bild an; erfordert die Installation von `libgps.so`.

```
	--fullpreview,	-fp		Voller Vorschaumodus
```
Dadurch wird das Vorschaufenster im Aufnahmemodus mit voller Auflösung ausgeführt. Die maximale Anzahl Bilder pro Sekunde in diesem Modus beträgt 15 fps, und die Vorschau hat das gleiche Sichtfeld wie die Aufnahme. Captures sollten schneller erfolgen, da keine Modusänderung erforderlich sein sollte. Diese Funktion befindet sich derzeit in der Entwicklung.

```
	--keypress,	-k		Tastendruckmodus
```

Die Kamera wird für die angeforderte Zeit (`-t`) betrieben und während dieser Zeit kann durch Drücken der Eingabetaste eine Aufnahme gestartet werden. Wenn Sie X und dann die Eingabetaste drücken, wird die Anwendung beendet, bevor das Zeitlimit erreicht ist. Wenn das Timeout auf 0 gesetzt ist, läuft die Kamera auf unbestimmte Zeit, bis der Benutzer X und dann die Eingabetaste drückt. Die Verwendung der ausführlichen Option (`-v`) zeigt eine Eingabeaufforderung an, die nach Benutzereingaben fragt, andernfalls wird keine Eingabeaufforderung angezeigt.

```
	--signal,	-s		Signalmodus
```

Die Kamera wird für die angeforderte Zeit ('-t') betrieben und während dieser Zeit kann eine Aufnahme gestartet werden, indem ein 'USR1'-Signal an den Kameraprozess gesendet wird. Dies kann mit dem Befehl `kill` erfolgen. Sie finden die Kameraprozess-ID mit dem Befehl `pgrep raspistill`.

`kill -USR1 <Prozess-ID von Raspistill>`

```
	--burst,	-bm
```

Stellt den Burst-Capture-Modus ein. Dadurch wird verhindert, dass die Kamera zwischen den Aufnahmen in den Vorschaumodus zurückkehrt, sodass Aufnahmen näher beieinander aufgenommen werden können.

### raspiyuv

Viele der Optionen für `raspiyuv` sind die gleichen wie für `raspistill`. Dieser Abschnitt zeigt die Unterschiede.

Nicht unterstützte Optionen:

```
--exif, --encoding, --thumb, --raw, --quality
```

Zusätzliche Optionen:

```
	--rgb,	-rgb		Unkomprimierte Daten als RGB888 speichern
```
Diese Option erzwingt, dass das Bild als RGB-Daten mit 8 Bit pro Kanal gespeichert wird, anstatt als YUV420.

Beachten Sie, dass die in `raspiyuv` gespeicherten Bildpuffer auf eine durch 32 teilbare horizontale Größe aufgefüllt werden, sodass am Ende jeder Zeile möglicherweise ungenutzte Bytes stehen. Puffer werden auch vertikal aufgefüllt, um durch 16 teilbar zu sein, und im YUV-Modus wird jede Ebene von Y, U, V auf diese Weise aufgefüllt.

```
	--luma,	-y
```

Gibt nur den Luma (Y)-Kanal des YUV-Bildes aus. Dies ist effektiv der Schwarzweiß- oder Intensitätsteil des Bildes.

```
	--bgr,	-bgr
```

Speichert die Bilddaten als BGR-Daten statt als YUV.


### raspivid

```
	--width,	-w		Setzt die Bildbreite <Größe>
```
Breite des resultierenden Videos. Dieser sollte zwischen 64 und 1920 liegen.

```
	--height,	-h		Bildhöhe einstellen <Größe>
```
Höhe des resultierenden Videos. Dieser sollte zwischen 64 und 1080 liegen.

```
	--bitrate,	-b		Bitrate einstellen
```
Verwenden Sie Bits pro Sekunde, also wären 10 Mbit/s `-b 10000000`. Für H264, 1080p30 wäre eine hochwertige Bitrate 15 Mbit/s oder mehr. Die maximale Bitrate beträgt 25 Mbit/s (`-b 25000000`), aber viel mehr als 17 Mbit/s zeigen keine merkliche Verbesserung bei 1080p30.

```
	--output,	-o		Ausgabedateiname <Dateiname>
```
Geben Sie den Ausgabedateinamen an. Wenn nicht angegeben, wird keine Datei gespeichert. Wenn der Dateiname '-' ist, wird die gesamte Ausgabe an stdout gesendet.

Um eine Verbindung zu einem entfernten IPv4-Host herzustellen, verwenden Sie `tcp` oder `udp` gefolgt von der erforderlichen IP-Adresse. z.B. `tcp://192.168.1.2:1234` oder `udp://192.168.1.2:1234`.

Um einen TCP-Port (IPv4) abzuhören und auf eine eingehende Verbindung zu warten, verwenden Sie die Option `--listen (-l)`, z.B. `raspivid -l -o tcp://0.0.0.0:3333` bindet an alle Netzwerkschnittstellen, `raspivid -l -o tcp://192.168.1.1:3333` bindet an ein lokales IPv4.

```
	--listen,	-l
```

Wenn eine Netzwerkverbindung als Datensenke verwendet wird, lässt diese Option das System auf eine Verbindung vom Remote-System warten, bevor Daten gesendet werden.

```
	--verbose,	-v		Ausführliche Informationen während der Ausführung ausgeben
```
Gibt während des Programmlaufs Debug-/Informationsmeldungen aus.

```
	--timeout,	-t		Zeit, bevor die Kamera ein Bild aufnimmt und herunterfährt
```
Die Gesamtdauer, für die das Programm ausgeführt wird. Wenn nicht angegeben, ist der Standardwert 5000 ms (5 Sekunden). Wenn auf 0 gesetzt, wird die Anwendung auf unbestimmte Zeit ausgeführt, bis sie mit Strg-C gestoppt wird.

```
	--demo,	-d		Führe einen Demo-Modus aus <Millisekunden>
```

Diese Option durchläuft den Bereich der Kameraoptionen. Es erfolgt keine Aufnahme und die Demo endet am Ende des Timeout-Zeitraums, unabhängig davon, ob alle Optionen durchlaufen wurden. Die Zeit zwischen den Zyklen sollte als Millisekundenwert angegeben werden.

```
	--framerate,	-fps		Geben Sie die Bilder pro Sekunde an, die aufgezeichnet werden sollen
```
Derzeit beträgt die zulässige Mindestbildrate 2 fps und die maximale 30 fps. Dies wird sich voraussichtlich in Zukunft ändern.

```
	--penc,	-e		Vorschaubild nach der Kodierung anzeigen
```
Schalten Sie eine Option ein, um die Vorschau nach der Komprimierung anzuzeigen. Dadurch werden alle Komprimierungsartefakte im Vorschaufenster angezeigt. Im Normalbetrieb zeigt die Vorschau die Kameraausgabe vor der Komprimierung. Es kann nicht garantiert werden, dass diese Option in zukünftigen Versionen funktioniert.

```
	--intra,	-g		Geben Sie die Intra-Refresh-Periode an (Schlüsselbildrate/GoP)
```
Legt die Intra-Refresh-Periode (GoP)-Rate für das aufgezeichnete Video fest. H264-Video verwendet in jeder Intra-Refresh-Periode einen vollständigen Frame (I-Frame), auf dem nachfolgende Frames basieren. Diese Option gibt die Anzahl der Frames zwischen jedem I-Frame an. Größere Zahlen hier verringern die Größe des resultierenden Videos, und kleinere Zahlen machen den Stream weniger fehleranfällig.

```
	--qp,	-qp		Quantisierungsparameter einstellen
```

Legt den anfänglichen Quantisierungsparameter für den Stream fest. Variiert von ca. 10 bis 40 und beeinflusst die Qualität der Aufnahme stark. Höhere Werte verringern die Qualität und verringern die Dateigröße. Kombinieren Sie diese Einstellung mit einer Bitrate von 0, um eine vollständig variable Bitrate einzustellen.

```
	--profile,	-pf		H264-Profil angeben, das für die Kodierung verwendet werden soll
```

Legt das H264-Profil fest, das für die Kodierung verwendet werden soll. Optionen sind:

- baseline
- main
- high

```
	--level,	-lev
```

Gibt die H264-Encoder-Ebene an, die für die Codierung verwendet werden soll. Die Optionen sind „4“, „4.1“ und „4.2“.

```
	--irefresh,	-if
```

Legt den H264-Intra-Refresh-Typ fest. Mögliche Optionen sind `zyklisch`, `adaptiv`, `beide` und `zyklische Zeilen`.

```
	--inline,	-ih		PPS-, SPS-Header einfügen
```
Erzwingt, dass der Stream PPS- und SPS-Header in jedem I-Frame enthält. Wird für bestimmte Streaming-Fälle benötigt, z.B. Apple HLS. Diese Header sind klein, also erhöhen Sie die Dateigröße nicht stark.

```
	--spstimings,	-stm
```

Fügen Sie Timing-Informationen in den SPS-Block ein.

```
	--timed,	-td		Schaltet zeitgesteuert zwischen Aufnahme und Pause um
```
Mit dieser Option kann die Videoaufnahme in bestimmten Zeitintervallen angehalten und neu gestartet werden. Zwei Werte sind erforderlich: die Einschaltzeit und die Ausschaltzeit. Die Einschaltzeit ist die Zeit, während der das Video aufgenommen wird, und die Ausschaltzeit ist die Zeit, während der das Video pausiert wird. Die Gesamtzeit der Aufnahme wird durch die Option `timeout` definiert. Beachten Sie, dass die Aufnahme je nach Ein- und Ausschaltzeiten etwas länger dauern kann als die Timeout-Einstellung.

Zum Beispiel:

```
raspivid -o test.h264 -t 25000 -timed 2500,5000
```

wird 25 Sekunden lang aufnehmen. Die Aufzeichnung erfolgt über einen Zeitrahmen bestehend aus 2500 ms (2,5 s) Segmenten mit 5000 ms (5 s) Lücken, die sich über die 20 s wiederholen. Die gesamte Aufnahme wird also tatsächlich nur 10s lang sein, da 4 Segmente von 2,5s = 10s durch 5s Lücken getrennt sind. So:

2.5 Aufnahme – 5 Pause – 2.5 Aufnahme – 5 Pause – 2.5 Aufnahme – 5 Pause – 2.5 Aufnahme

ergibt eine Gesamtaufnahmedauer von 25 Sekunden, aber nur 10 Sekunden des tatsächlich aufgenommenen Filmmaterials.

```
	--keypress,	-k		Umschalten zwischen Aufnahme und Pause beim Drücken der Eingabetaste
```
Bei jedem Drücken der Eingabetaste wird die Aufnahme angehalten oder neu gestartet. Wenn Sie X und dann die Eingabetaste drücken, wird die Aufnahme gestoppt und die Anwendung geschlossen. Beachten Sie, dass der Timeout-Wert verwendet wird, um das Ende der Aufnahme zu signalisieren, aber nur nach jedem Drücken der Eingabetaste überprüft wird; Wenn das System also auf einen Tastendruck wartet, wartet es vor dem Beenden immer noch auf den Tastendruck, auch wenn die Zeitüberschreitung abgelaufen ist.

```
	--signal,	-s		Umschalten zwischen Aufnahme und Pause gemäß SIGUSR1
```

Das Senden eines `USR1`-Signals an den `raspivid`-Prozess schaltet zwischen Aufnahme und Pause um. Dies kann mit dem Befehl `kill` erfolgen, wie unten beschrieben. Die Prozess-ID von `raspivid` finden Sie mit `pgrep raspivid`.

`kill -USR1 <Prozess-ID von Raspivid>`

Beachten Sie, dass der Timeout-Wert verwendet wird, um das Ende der Aufzeichnung anzuzeigen, aber nur nach jedem Empfang des `SIGUSR1`-Signals überprüft wird; Wenn das System also auf ein Signal wartet, wartet es auch nach Ablauf des Timeouts vor dem Beenden auf das Signal.

```
	--split,	-sp
```
Im Signal- oder Tastendruckmodus wird bei jedem Neustart der Aufnahme eine neue Datei erstellt.

```
	--circular,	-c
```

Wählen Sie den Ringpuffermodus. Alle codierten Daten werden in einem Ringpuffer gespeichert, bis ein Trigger aktiviert wird, dann wird der Puffer gespeichert.

```
	--vectors,	-x
```

Schaltet die Ausgabe von Bewegungsvektoren vom H264-Encoder an den angegebenen Dateinamen ein.

```
	--flush,	-fl
```

Erzwingt eine Leerung der Ausgabedatenpuffer, sobald Videodaten geschrieben werden. Dadurch wird das Caching geschriebener Daten durch das Betriebssystem umgangen und die Latenz kann verringert werden.

```
	--save-pts,	-pts
```

Speichert Zeitstempelinformationen in der angegebenen Datei. Nützlich als Eingabedatei für `mkvmerge`.

```
	--codec,	-cd
```

Gibt den zu verwendenden Encoder-Codec an. Optionen sind `H264` und `MJPEG`. H264 kann bis zu 1080p codieren, während MJPEG bis zur Sensorgröße codieren kann, jedoch aufgrund der höheren Verarbeitungs- und Speicheranforderungen mit verringerten Frameraten.

```
	--initial,	-i		Definiert den Anfangszustand beim Start
```

Legen Sie fest, ob die Kamera pausiert oder sofort mit der Aufnahme beginnt. Die Optionen sind "Aufzeichnen" oder "Pause". Beachten Sie, dass, wenn Sie ein einfaches Timeout verwenden und `initial` auf `pause` gesetzt ist, keine Ausgabe aufgezeichnet wird.

```
	--segment,	-sg		Segmente den Stream in mehrere Dateien
```

Anstatt eine einzelne Datei zu erstellen, wird die Datei in Segmente von ungefähr der angegebenen Anzahl von Millisekunden aufgeteilt. Um unterschiedliche Dateinamen bereitzustellen, sollten Sie `%04d` oder ähnliches an der Stelle im Dateinamen hinzufügen, an der eine Segmentanzahl erscheinen soll, z.B.:

```
--segment 3000 -o video%04d.h264
```

produziert Videoclips mit einer Länge von ca. 3000 ms (3s) mit den Namen `video0001.h264`, `video0002.h264` usw. Die Clips sollten nahtlos sein (keine Frame-Drops zwischen den Clips), aber die Genauigkeit jeder Cliplänge hängt von der Intraframe-Periode, da die Segmente immer auf einem I-Frame beginnen. Sie sind daher immer gleich oder länger als der angegebene Zeitraum.

Die neueste Version von Raspivid ermöglicht auch, dass der Dateiname zeitbasiert ist, anstatt eine Segmentnummer zu verwenden. Zum Beispiel:
```
--segment 3000 -o video_%c.h264
```
erzeugt Dateinamen, die wie folgt formatiert sind: `video_Fri 20 Jul 16:23:48 2018.h264`

Es stehen viele verschiedene Formatierungsoptionen zur Verfügung – siehe [hier](http://man7.org/linux/man-pages/man3/strftime.3.html) für eine vollständige Liste. Beachten Sie, dass die Optionen `%d` und `%u` nicht verfügbar sind, da sie für die Formatierung von Segmentnummern verwendet werden und dass einige Kombinationen ungültige Dateinamen erzeugen können.

```
	--wrap,	-wr		Setzt den Maximalwert für die Segmentnummer
```
Bei der Ausgabe von Segmenten ist dies das Maximum, das die Segmentnummer erreichen kann, bevor sie auf 1 zurückgesetzt wird. Dadurch können Segmente aufgezeichnet, jedoch die ältesten überschrieben werden. Wenn also im obigen Segmentbeispiel auf 4 gesetzt, sind die erzeugten Dateien `video0001.h264`, `video0002.h264`, `video0003.h264` und `video0004.h264`. Sobald `video0004.h264` aufgezeichnet wurde, wird der Zähler auf 1 zurückgesetzt und `video0001.h264` wird überschrieben.

```
	--start,	-sn		Setzt die Anfangssegmentnummer
```
Bei der Ausgabe von Segmenten ist dies die anfängliche Segmentnummer, mit der eine vorherige Aufnahme von einem bestimmten Segment fortgesetzt werden kann. Der Standardwert ist 1.

```
	--raw,	-r
```
Geben Sie den Ausgabedateinamen für alle angeforderten Rohdatendateien an.

```
	--raw-format,	-rf
```
Geben Sie das Rohformat an, das verwendet werden soll, wenn Rohausgabe angefordert wird. Optionen wie `yuv`, `rgb` und `grey`. `grey` speichert einfach den Y-Kanal des YUV-Bildes.


## Beispiele

### Immer noch erfasst

Standardmäßig werden Aufnahmen mit der höchsten vom Sensor unterstützten Auflösung durchgeführt. Dies kann mit den Befehlszeilenoptionen `-w` und `-h` geändert werden.

Nehmen Sie nach 2s (Zeitangaben in Millisekunden) im Sucher ein Standard-Capture auf und speichern Sie es in `image.jpg`:

```bash
raspistill -t 2000 -o image.jpg
```

Nehmen Sie eine Aufnahme mit einer anderen Auflösung auf:

```bash
raspistill -t 2000 -o image.jpg -w 640 -h 480
```

Reduzieren Sie die Qualität erheblich, um die Dateigröße zu reduzieren:

```bash
raspistill -t 2000 -o image.jpg -q 5
```

Erzwingen, dass die Vorschau bei Koordinate 100,100 mit einer Breite von 300 Pixeln und einer Höhe von 200 Pixeln angezeigt wird:

```bash
raspistill -t 2000 -o image.jpg -p 100,100,300,200
```

Vorschau komplett deaktivieren:

```bash
raspistill -t 2000 -o image.jpg -n
```

Speichern Sie das Bild als PNG-Datei (verlustfreie Komprimierung, aber langsamer als JPEG). Beachten Sie, dass das Dateinamensuffix bei der Auswahl der Bildkodierung ignoriert wird:

```bash
raspistill -t 2000 -o image.png –e png
```

Fügen Sie dem JPEG einige EXIF-Informationen hinzu. Dies setzt den Namen des Künstler-Tags auf Boris und die GPS-Höhe auf 123,5 m. Beachten Sie, dass Sie beim Festlegen von GPS-Tags mindestens GPSLatitude, GPSLatitudeRef, GPSLongitude, GPSLongitudeRef, GPSAltitude und GPSAltitudeRef festlegen sollten:

```bash
raspistill -t 2000 -o image.jpg -x IFD0.Artist=Boris -x GPS.GPSAltitude=1235/10
```

Stellen Sie einen Prägebildeffekt ein:

```bash
raspistill -t 2000 -o image.jpg -ifx emboss
```

Stellen Sie die U- und V-Kanäle des YUV-Bildes auf bestimmte Werte ein (128:128 erzeugt ein Graustufenbild):

```bash
raspistill -t 2000 -o image.jpg -cfx 128:128
```

Vorschau für 2 Sekunden ausführen, ohne gespeichertes Bild:

```bash
raspistill -t 2000
```

Nehmen Sie alle 10 Sekunden für 10 Minuten (10 Minuten = 600000 ms) ein Zeitraffer-Bild auf, benennen Sie die Dateien `image_num_001_today.jpg`, `image_num_002_today.jpg` usw., wobei das neueste Bild auch unter dem Namen `latest. jpg`:

```bash
raspistill -t 600000 -tl 10000 -o image_num_%03d_today.jpg -l latest.jpg
```

Machen Sie ein Bild und senden Sie die Bilddaten an stdout:

```bash
raspistill -t 2000 -o -
```

Nehmen Sie ein Bild auf und senden Sie die Bilddaten an eine Datei:

```bash
raspistill -t 2000 -o - > my_file.jpg
```

Lassen Sie die Kamera für immer laufen und machen Sie ein Bild, wenn die Eingabetaste gedrückt wird:

```bash
raspistill -t 0 -k -o my_pics%02d.jpg
```

###Videoaufnahmen

Die Einstellungen für Bildgröße und Vorschau sind dieselben wie bei der Aufnahme von Standbildern. Die Standardgröße für Videoaufnahmen ist 1080p (1920x1080).

Nehmen Sie einen 5s-Clip mit den Standardeinstellungen (1080p30) auf:

```bash
raspivid -t 5000 -o video.h264
```

Nehmen Sie einen 5s-Clip mit einer angegebenen Bitrate (3,5 Mbit/s) auf:

```bash
raspivid -t 5000 -o video.h264 -b 3500000
```

Nehmen Sie einen 5s-Clip mit einer angegebenen Bildrate (5fps) auf:

```bash
raspivid -t 5000 -o video.h264 -f 5
```

Codieren Sie einen 5s Kamerastream und senden Sie die Bilddaten an stdout:

```bash
raspivid -t 5000 -o -
```

Verschlüsseln Sie einen 5s Kamerastream und senden Sie die Bilddaten an eine Datei:

```bash
raspivid -t 5000 -o - > my_file.h264
```

## Shell-Fehlercodes

Die hier beschriebenen Anwendungen geben nach Abschluss einen Standardfehlercode an die Shell zurück. Mögliche Fehlercodes sind:

| C Definiere | Code | Beschreibung |
|------------|------|-------------|
| EX_OK | 0 | Anwendung wurde erfolgreich ausgeführt |
| EX_USAGE | 64 | Ungültiger Befehlszeilenparameter |
| EX_SOFTWARE | 70 | Software- oder Kamerafehler |
| 	| 130 | Anwendung mit Strg-C beendet |