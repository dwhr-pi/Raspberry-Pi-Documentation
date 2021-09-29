# Videooptionen in config.txt

## Optionen für den Composite-Videomodus

### sdtv_mode

Der Befehl `sdtv_mode` definiert den TV-Standard, der für die Composite-Videoausgabe verwendet wird. Beim ursprünglichen Raspberry Pi wird Composite-Video an der Cinch-Buchse ausgegeben. Auf anderen Raspberry Pis, mit Ausnahme von Pi Zero und Compute Module, wird Composite-Video zusammen mit Ton an der 4-poligen TRRS-Buchse ("Kopfhörer") ausgegeben. Auf dem Pi Zero gibt es einen unbestückten Header mit der Bezeichnung "TV", der Composite-Video ausgibt. Auf dem Compute Module ist Composite-Video über den TVDAC-Pin verfügbar. Der Standardwert von `sdtv_mode` ist `0`.

| sdtv_mode | Ergebnis |
| --- | --- |
| 0 | Normales NTSC |
| 1 | Japanische Version von NTSC – kein Sockel |
| 2 | Normaler PAL |
| 3 | Brasilianische Version von PAL – 525/60 statt 625/50, anderer Unterträger |
| 16 | Progressiver Scan NTSC |
| 18 | Progressive Scan-PAL |

### sdtv_aspekt

Der Befehl `sdtv_aspect` definiert das Seitenverhältnis für die Composite-Videoausgabe. Der Standardwert ist '1'.

| sdtv_aspekt | Ergebnis |
| --- | --- |
| 1 | 4:3 |
| 2 | 14:9 |
| 3 | 16:9 |

### sdtv_disable_colourburst

Das Setzen von `sdtv_disable_colourburst` auf `1` deaktiviert Colorburst bei der Composite-Videoausgabe. Das Bild wird monochrom angezeigt, kann jedoch schärfer erscheinen.

### enable_tvout (nur Pi 4B)

Auf dem Raspberry Pi 4 ist der Composite-Ausgang aufgrund der Art und Weise, wie die internen Uhren miteinander verbunden und zugewiesen sind, standardmäßig deaktiviert. Da Composite-Video eine sehr spezifische Uhr erfordert, bedeutet die Einstellung dieser Uhr auf die erforderliche Geschwindigkeit auf dem Pi 4, dass andere daran angeschlossene Uhren nachteilig beeinflusst werden, was das gesamte System leicht verlangsamt. Da Composite-Video eine weniger häufig verwendete Funktion ist, haben wir uns entschieden, sie standardmäßig zu deaktivieren, um diese Systemverlangsamung zu verhindern.

Um die Composite-Ausgabe zu aktivieren, verwenden Sie die Option `enable_tvout=1`. Wie oben beschrieben, wirkt sich dies in geringem Maße nachteilig auf die Leistung aus.

Bei älteren Pi-Modellen bleibt das zusammengesetzte Verhalten gleich.

## HDMI-Modus-Optionen

**Hinweis für Raspberry Pi4B-Benutzer:** Da der Raspberry Pi 4B über zwei HDMI-Anschlüsse verfügt, können einige HDMI-Befehle auf beide Anschlüsse angewendet werden. Sie können die Syntax `<Befehl>:<Port>` verwenden, wobei Port 0 oder 1 ist, um anzugeben, für welchen Port die Einstellung gelten soll. Wenn kein Port angegeben ist, ist der Standardwert 0. Wenn Sie eine Portnummer in einem Befehl angeben, der keine Portnummer erfordert, wird der Port ignoriert. Weitere Details zur Syntax und alternativen Mechanismen finden Sie im HDMI-Abschnitt auf der [conditionals page](./conditional.md) der Dokumentation.

Um duale 4k-Displays zu unterstützen, verfügt der Raspberrry Pi 4 über eine aktualisierte Videohardware, die den unterstützten Modi geringfügige Einschränkungen auferlegt. Bitte sehen
[hier](./pi4-hdmi.md) für weitere Details.

### hdmi_safe

Das Setzen von `hdmi_safe` auf `1` führt dazu, dass die Einstellungen für den abgesicherten Modus verwendet werden, um zu versuchen, mit maximaler HDMI-Kompatibilität zu booten. Dies entspricht der Einstellung der folgenden Parameter:

```
hdmi_force_hotplug=1
hdmi_ignore_edid=0xa5000080
config_hdmi_boost=4
hdmi_group=2
hdmi_mode=4
disable_overscan=0
overscan_left=24
overscan_right=24
overscan_top=24
overscan_bottom=24
```

### hdmi_ignore_edid

Das Setzen von `hdmi_ignore_edid` auf `0xa5000080` ermöglicht das Ignorieren von EDID-/Display-Daten, wenn Ihr Display keine genaue [EDID] hat (https://en.wikipedia.org/wiki/Extended_display_identification_data). Dieser ungewöhnliche Wert ist erforderlich, um sicherzustellen, dass er nicht versehentlich ausgelöst wird.

### hdmi_edid_file

Wenn Sie `hdmi_edid_file` auf `1` setzen, liest die GPU EDID-Daten aus der Datei `edid.dat`, die sich in der Bootpartition befindet, anstatt sie vom Monitor zu lesen. Weitere Informationen finden Sie [hier](https://www.raspberrypi.org/forums/viewtopic.php?p=173430#p173430).

### hdmi_edid_filename

Auf dem Raspberry Pi 4B können Sie mit dem Befehl `hdmi_edid_filename` den Dateinamen der zu verwendenden EDID-Datei angeben und auch angeben, auf welchen Port die Datei angewendet werden soll. Dies erfordert auch `hdmi_edid_file=1`, um EDID-Dateien zu aktivieren.

Zum Beispiel:

```
hdmi_edid_file=1
hdmi_edid_filename:0=FileForPortZero.edid
hdmi_edid_filename:1=FileForPortOne.edid
```

### hdmi_force_edid_audio

Das Setzen von `hdmi_force_edid_audio` auf `1` gibt vor, dass alle Audioformate von der Anzeige unterstützt werden, was den Passthrough von DTS/AC3 ermöglicht, auch wenn dies nicht als unterstützt gemeldet wird.

### hdmi_ignore_edid_audio

Das Setzen von `hdmi_ignore_edid_audio` auf `1` gibt vor, dass alle Audioformate vom Display nicht unterstützt werden. Dies bedeutet, dass ALSA standardmäßig die analoge Audiobuchse (Kopfhörer) verwendet.

### hdmi_force_edid_3d

Das Setzen von `hdmi_force_edid_3d` auf `1` gibt vor, dass alle CEA-Modi 3D unterstützen, auch wenn die EDID keine Unterstützung dafür anzeigt.

### hdmi_ignore_cec_init

Wenn Sie `hdmi_ignore_cec_init` auf `1` setzen, wird die anfängliche aktive Quellnachricht während des Bootens nicht mehr gesendet. Dies verhindert, dass ein CEC-fähiges Fernsehgerät beim Neustart Ihres Raspberry Pi aus dem Standby-Modus und der Kanalumschaltung kommt.

### hdmi_ignore_cec

Das Setzen von `hdmi_ignore_cec` auf `1` gibt vor, dass [CEC](https://en.wikipedia.org/wiki/Consumer_Electronics_Control#CEC) vom Fernseher überhaupt nicht unterstützt wird. Es werden keine CEC-Funktionen unterstützt.

### cec_osd_name

Der Befehl `cec_osd_name` setzt den anfänglichen CEC-Namen des Geräts. Die Standardeinstellung ist Raspberry Pi.

### hdmi_pixel_encoding

Der Befehl `hdmi_pixel_encoding` erzwingt den Pixelkodierungsmodus. Standardmäßig wird der von der EDID angeforderte Modus verwendet, sodass Sie ihn nicht ändern müssen.

| hdmi_pixel_encoding | Ergebnis |
| --- | --- |
| 0 | Standard (RGB begrenzt für CEA, RGB voll für DMT) |
| 1 | RGB begrenzt (16-235) |
| 2 | RGB voll (0-255) |
| 3 | YCbCr begrenzt (16-235) |
| 4 | YCbCr voll (0-255) |

### hdmi_max_pixel_freq

Die Pixelfrequenz wird von der Firmware und KMS verwendet, um HDMI-Modi zu filtern. Beachten Sie, dass dies nicht mit der Bildrate identisch ist. Es gibt die maximale Frequenz an, die ein gültiger Modus haben kann, wodurch Modi mit höherer Frequenz aussortiert werden. Die Frequenzen für alle HDMI-Modi finden Sie auf der Wiki-Seite [hier](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data#CEA_EDID_Timing_Extension_data_format_-_Version_3), Abschnitt "CEA/EIA-861 Standardauflösungen und Timings".

Wenn Sie beispielsweise alle 4K-Modi deaktivieren möchten, können Sie eine maximale Frequenz von 200000000 angeben, da alle 4K-Modi Frequenzen darüber haben.

### hdmi_blanking

Der Befehl `hdmi_blanking` steuert, was passiert, wenn das Betriebssystem das Display mit DPMS in den Standby-Modus versetzt, um Strom zu sparen. Wenn diese Option nicht oder auf 0 gesetzt ist, wird der HDMI-Ausgang ausgeblendet, aber nicht ausgeschaltet. Um das Verhalten anderer Computer nachzuahmen, können Sie den HDMI-Ausgang auch so einstellen, dass er sich ausschaltet, indem Sie diese Option auf 1 setzen: Das angeschlossene Display wechselt in einen Standby-Modus mit geringem Stromverbrauch.

**Auf dem Raspberry Pi 4 führt die Einstellung hdmi_blanking=1 nicht zum Abschalten des HDMI-Ausgangs, da diese Funktion noch nicht implementiert wurde.**

**HINWEIS:** Diese Funktion kann Probleme verursachen, wenn Anwendungen verwendet werden, die den Framebuffer nicht verwenden, wie z. B. omxplayer.

| hdmi_blanking | Ergebnis |
| --- | --- |
| 0 | HDMI-Ausgang wird ausgeblendet |
| 1 | HDMI-Ausgang wird ausgeschaltet und ausgeblendet |

### hdmi_drive

Mit dem Befehl `hdmi_drive` können Sie zwischen HDMI- und DVI-Ausgabemodi wählen.

| hdmi_drive | Ergebnis |
| --- | --- |
| 1 | Normaler DVI-Modus (kein Ton) |
| 2 | Normaler HDMI-Modus (Ton wird gesendet, wenn unterstützt und aktiviert) |

### config_hdmi_boost

Konfiguriert die Signalstärke der HDMI-Schnittstelle. Der Mindestwert ist „0“ und der Höchstwert „11“.

Der Standardwert für die Originalmodelle B und A ist „2“. Der Standardwert für das Modell B+ und alle späteren Modelle ist „5“.

Wenn HDMI-Probleme (Sprenkeln, Interferenzen) auftreten, versuchen Sie es mit '7'. Bei sehr langen HDMI-Kabeln können bis zu '11' erforderlich sein, aber so hohe Werte sollten nicht verwendet werden, es sei denn, es ist unbedingt erforderlich.

Diese Option wird auf dem Raspberry Pi 4 ignoriert.

### hdmi_group

Der Befehl `hdmi_group` definiert die HDMI-Ausgangsgruppe entweder als CEA (Consumer Electronics Association, der typischerweise von Fernsehgeräten verwendete Standard) oder DMT (Display Monitor Timings, der typischerweise von Monitoren verwendete Standard). Diese Einstellung sollte in Verbindung mit `hdmi_mode` verwendet werden.

| hdmi_gruppe | Ergebnis |
| --- | --- |
| 0 | Automatische Erkennung von EDID |
| 1 | CEA |
| 2 | DMT |

### hdmi_mode

Zusammen mit `hdmi_group` definiert `hdmi_mode` das HDMI-Ausgabeformat. Formatmodusnummern werden aus der CTA-Spezifikation abgeleitet, die [hier] gefunden wurde (https://web.archive.org/web/20171201033424/https://standards.cta.tech/kwspub/published_docs/CTA-861-G_FINAL_revised_2017.pdf)

Um einen hier nicht aufgeführten benutzerdefinierten Anzeigemodus festzulegen, siehe [diesen Thread](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=24679).

Beachten Sie, dass nicht alle Modi bei allen Modellen verfügbar sind.

Diese Werte sind gültig, wenn `hdmi_group=1` (CEA):

| hdmi_mode | Auflösung | Frequenz | Bildschirmseitenverhältnis | Anmerkungen |
| --------- | --------- | ----------| :--------: |------ |
| 1 | VGA (640x480) | 60Hz | 4:3 | |
| 2 | 480p | 60Hz | 4:3 | |
| 3 | 480p | 60Hz | 16:9 | |
| 4 | 720p | 60Hz | 16:9 | |
| 5 | 1080i | 60Hz | 16:9 | |
| 6 | 480i | 60Hz | 4:3 | |
| 7 | 480i | 60Hz | 16:9 | |
| 8 | 240p | 60Hz | 4:3 | |
| 9 | 240p | 60Hz | 16:9 | |
| 10 | 480i | 60Hz | 4:3 | Pixelvervierfachung |
| 11 | 480i | 60Hz | 16:9 | Pixelvervierfachung |
| 12 | 240p | 60Hz | 4:3 | Pixelvervierfachung |
| 13 | 240p | 60Hz | 16:9 |Pixelvervierfachung|
| 14 | 480p | 60Hz | 4:3 | Pixelverdopplung |
| 15 | 480p | 60Hz | 16:9 | Pixelverdopplung |
| 16 | 1080p | 60Hz | 16:9 | |
| 17 | 576p | 50Hz | 4:3 | |
| 18 | 576p | 50Hz | 16:9 | |
| 19 | 720p | 50Hz | 16:9 | |
| 20 | 1080i | 50Hz | 16:9 | |
| 21 | 576i | 50Hz | 4:3 | |
| 22 | 576i | 50Hz | 16:9 | |
| 23 | 288p | 50Hz | 4:3 | |
| 24 | 288p | 50Hz | 16:9 | |
| 25 | 576i | 50Hz | 4:3 | Pixelvervierfachung |
| 26 | 576i | 50Hz | 16:9 | Pixelvervierfachung |
| 27 | 288p | 50Hz | 4:3 | Pixelvervierfachung |
| 28 | 288p | 50Hz | 16:9 | Pixelvervierfachung |
| 29 | 576p | 50Hz | 4:3 | Pixelverdopplung |
| 30 | 576p | 50Hz | 16:9 | Pixelverdopplung |
| 31 | 1080p | 50Hz | 16:9 | |
| 32 | 1080p | 24Hz | 16:9 | |
| 33 | 1080p | 25Hz | 16:9 | |
| 34 | 1080p | 30Hz | 16:9 | |
| 35 | 480p | 60Hz | 4:3 | Pixelvervierfachung |
| 36 | 480p | 60Hz | 16:9 | Pixelvervierfachung |
| 37 | 576p | 50Hz | 4:3 | Pixelvervierfachung |
| 38 | 576p | 50Hz | 16:9 | Pixelvervierfachung |
| 39 | 1080i | 50Hz | 16:9 | reduzierte Ausblendung |
| 40 | 1080i | 100Hz | 16:9 | |
| 41 | 720p | 100Hz | 16:9 | |
| 42 | 576p | 100Hz | 4:3 | |
| 43 | 576p | 100Hz | 16:9 | |
| 44 | 576i | 100Hz | 4:3 | |
| 45 | 576i | 100Hz | 16:9 | |
| 46 | 1080i | 120Hz | 16:9 | |
| 47 | 720p | 120Hz | 16:9 | |
| 48 | 480p | 120Hz | 4:3 | |
| 49 | 480p | 120Hz | 16:9 | |
| 50 | 480i | 120Hz | 4:3 | |
| 51 | 480i | 120Hz | 16:9 | |
| 52 | 576p | 200Hz | 4:3 | |
| 53 | 576p | 200Hz | 16:9 | |
| 54 | 576i | 200Hz | 4:3 | |
| 55 | 576i | 200Hz | 16:9 | |
| 56 | 480p | 240Hz | 4:3 | |
| 57 | 480p | 240Hz | 16:9 | |
| 58 | 480i | 240Hz | 4:3 | |
| 59 | 480i | 240Hz | 16:9 | |
| 60 | 720p | 24Hz | 16:9 | |
| 61 | 720p | 25Hz | 16:9 | |
| 62 | 720p | 30Hz | 16:9 | |
| 63 | 1080p | 120Hz | 16:9 | |
| 64 | 1080p | 100Hz | 16:9 | |
| 65 | Benutzerdefiniert | | | |
| 66 | 720p | 25Hz | 64:27 | Pi 4|
| 67 | 720p | 30Hz | 64:27 | Pi 4 |
| 68 | 720p | 50Hz | 64:27 | Pi 4 |
| 69 | 720p | 60Hz | 64:27 | Pi 4 |
| 70 | 720p | 100Hz | 64:27 | Pi 4 |
| 71 | 720p | 120Hz | 64:27 | Pi 4 |
| 72 | 1080p | 24Hz | 64:27 | Pi 4 |
| 73 | 1080p | 25Hz | 64:27 | Pi 4 |
| 74 | 1080p | 30Hz | 64:27 | Pi 4 |
| 75 | 1080p | 50Hz | 64:27 | Pi 4 |
| 76 | 1080p | 60Hz | 64:27 | Pi 4 |
| 77 | 1080p | 100Hz | 64:27 | Pi 4 |
| 78 | 1080p | 120Hz | 64:27 | Pi 4 |
| 79 | 1680x720 | 24Hz | 64:27 | Pi 4 |
| 80 | 1680x720 | 25z | 64:27 | Pi 4 |
| 81 | 1680x720 | 30Hz | 64:27 | Pi 4 |
| 82 | 1680x720 | 50Hz | 64:27 | Pi 4 |
| 83 | 1680x720 | 60Hz | 64:27 | Pi 4 |
| 84 | 1680x720 | 100Hz | 64:27 | Pi 4 |
| 85 | 1680x720 | 120Hz | 64:27 | Pi 4 |
| 86 | 2560x720 | 24Hz | 64:27 | Pi 4 |
| 87 | 2560x720 | 25Hz | 64:27| Pi 4 |
| 88 | 2560x720 | 30Hz | 64:27 | Pi 4 |
| 89 | 2560x720 | 50Hz | 64:27 | Pi 4 |
| 90 | 2560x720 | 60Hz | 64:27 | Pi 4 |
| 91 | 2560x720| 100Hz | 64:27 | Pi 4 |
| 92 | 2560x720 | 120Hz | 64:27 | Pi 4 |
| 93 | 2160p | 24Hz | 16:9 | Pi 4 |
| 94 | 2160p | 25Hz | 16:9 | Pi 4 |
| 95 | 2160p | 30Hz | 16:9 | Pi 4 |
| 96 | 2160p | 50Hz | 16:9 | Pi 4|
| 97 | 2160p | 60Hz | 16:9 | Pi 4|
| 98 | 4096x2160 | 24Hz | 256:135 | Pi 4 |
| 99 | 4096x2160 | 25Hz | 256:135 | Pi 4 |
| 100 | 4096x2160 | 30Hz | 256:135 | Pi 4 |
| 101 | 4096x2160 | 50Hz | 256:135 | Pi 4 |
| 102 | 4096x2160 | 60Hz | 256:135 | Pi 4 |
| 103 | 2160p | 24Hz | 64:27 | Pi 4 |
| 104 | 2160p | 25Hz | 64:27 | Pi 4 |
| 105 | 2160p | 30Hz | 64:27 | Pi 4 |
| 106 | 2160p | 50Hz | 64:27 | Pi 4 |
| 107 | 2160p | 60Hz | 64:27 | Pi 4 |

Pixelverdopplung und -vervierfachung weist auf eine höhere Taktrate hin, wobei jedes Pixel zwei- bzw. viermal wiederholt wird.

Diese Werte sind gültig, wenn `hdmi_group=2` (DMT):

| hdmi_mode | Auflösung | Frequenz | Bildschirmseitenverhältnis | Anmerkungen |
| --------- | --------- | ----------| :--------: |------ |
| 1 | 640x350 | 85Hz | | |
| 2 | 640x400 | 85Hz | 16:10 | |
| 3 | 720x400 | 85Hz | | |
| 4 | 640x480 | 60Hz | 4:3 | |
| 5 | 640x480 | 72Hz | 4:3 | |
| 6 | 640x480 | 75Hz | 4:3 | |
| 7 | 640x480 | 85Hz | 4:3 | |
| 8 | 800x600 | 56Hz | 4:3 | |
| 9 | 800x600 | 60Hz | 4:3 | |
| 10 | 800x600 | 72Hz | 4:3 | |
| 11 | 800x600 | 75Hz | 4:3 | |
| 12 | 800x600 | 85Hz | 4:3 | |
| 13 | 800x600 | 120Hz | 4:3 | |
| 14 | 848x480 | 60Hz |16:9| |
| 15 | 1024x768 | 43Hz | 4:3 |inkompatibel mit dem Raspberry Pi |
| 16 | 1024x768 | 60Hz | 4:3 | |
| 17 | 1024x768 | 70Hz | 4:3 | |
| 18 | 1024x768 | 75Hz | 4:3 | |
| 19 | 1024x768 | 85Hz | 4:3 | |
| 20 | 1024x768 | 120Hz | 4:3 | |
| 21 | 1152x864 | 75Hz | 4:3 | |
| 22 | 1280x768 | 60Hz| 15:9 | reduzierte Ausblendung |
| 23 | 1280x768 | 60Hz | 15:9 | |
| 24 | 1280x768 | 75Hz | 15:9 | |
| 25 | 1280x768 | 85Hz | 15:9 | |
| 26 | 1280x768 | 120Hz | 15:9 | reduzierte Ausblendung |
| 27 | 1280x800 | 60 | 16:10 | reduzierte Ausblendung |
| 28 | 1280x800 | 60Hz | 16:10 | |
| 29 | 1280x800 | 75Hz | 16:10 | |
| 30 | 1280x800 | 85Hz | 16:10 | |
| 31 | 1280x800 | 120Hz | 16:10 |reduzierte Ausblendung |
| 32 | 1280x960 | 60Hz | 4:3 | |
| 33 | 1280x960 | 85Hz | 4:3 | |
| 34 | 1280x960 | 120Hz | 4:3 |reduzierte Ausblendung |
| 35 | 1280x1024 | 60Hz | 5:4 | |
| 36 | 1280x1024 | 75Hz | 5:4 | |
| 37 | 1280x1024 | 85Hz | 5:4 | |
| 38 | 1280x1024 | 120Hz | 5:4 | reduzierte Ausblendung |
| 39 | 1360x768 | 60Hz | 16:9 | |
| 40 | 1360x768 | 120Hz | 16:9 | reduzierte Ausblendung |
| 41 | 1400x1050 | 60Hz| 4:3 | reduzierte Ausblendung |
| 42 | 1400x1050 | 60Hz | 4:3 | |
| 43 | 1400x1050 | 75Hz | 4:3 | |
| 44 | 1400x1050 | 85Hz | 4:3 | |
| 45 | 1400x1050 | 120Hz | 4:3 | reduzierte Ausblendung |
| 46 | 1440x900 | 60Hz | 16:10 | reduzierte Ausblendung |
| 47 | 1440x900 | 60Hz | 16:10 | |
| 48 | 1440x900 | 75Hz | 16:10 | |
| 49 | 1440x900 | 85Hz | 16:10 | |
| 50 | 1440x900 | 120Hz | 16:10 |reduzierte Ausblendung |
| 51 | 1600x1200 | 60Hz | 4:3 | |
| 52 | 1600x1200 | 65Hz | 4:3 | |
| 53 | 1600x1200 | 70Hz | 4:3 | |
| 54 | 1600x1200 | 75Hz | 4:3 | |
| 55 | 1600x1200 | 85Hz | 4:3 | |
| 56 | 1600x1200 | 120Hz | 4:3 | reduzierte Ausblendung |
| 57 | 1680x1050 | 60Hz | 16:10 | reduzierte Ausblendung |
| 58 | 1680x1050 | 60Hz | 16:10 | |
| 59 | 1680x1050 | 75Hz | 16:10 | |
| 60 | 1680x1050 | 85Hz | 16:10 | |
| 61 | 1680x1050 | 120Hz | 16:10 | reduzierte Ausblendung |
| 62 | 1792x1344 | 60Hz | 4:3 | |
| 63 | 1792x1344 | 75Hz | 4:3 | |
| 64 | 1792x1344 | 120Hz | 4:3 | reduzierte Ausblendung |
| 65 | 1856x1392 | 60Hz | 4:3 | |
| 66 | 1856x1392 | 75Hz | 4:3 | |
| 67 | 1856x1392 | 120Hz | 4:3 | reduzierte Ausblendung |
| 68 | 1920x1200 | 60Hz | 16:10 |reduzierte Ausblendung |
| 69 | 1920x1200 | 60Hz | 16:10 | |
| 70 | 1920x1200 | 75Hz | 16:10 | |
| 71 | 1920x1200 | 85Hz | 16:10 | |
| 72 | 1920x1200 | 120Hz | 16:10 |reduzierte Ausblendung |
| 73 | 1920x1440 | 60Hz | 4:3 | |
| 74 | 1920x1440 | 75Hz | 4:3 | |
| 75 | 1920x1440 | 120Hz | 4:3 | reduzierte Ausblendung |
| 76 | 2560x1600 | 60Hz| 16:10 | reduzierte Ausblendung |
| 77 | 2560x1600 | 60Hz | 16:10 | |
| 78 | 2560x1600 | 75Hz | 16:10 | |
| 79 | 2560x1600 | 85Hz | 16:10 | |
| 80 | 2560x1600 | 120Hz | 16:10 | reduzierte Ausblendung |
| 81 | 1366x768 | 60Hz | 16:9 | [NICHT auf Pi4](./pi4-hdmi.md) |
| 82 | 1920x1080 | 60Hz | 16:9 | 1080p |
| 83 | 1600x900 | 60Hz | 16:9 | reduzierte Ausblendung |
| 84 | 2048x1152 | 60Hz | 16:9 | reduzierte Ausblendung |
| 85 | 1280x720 | 60Hz | 16:9 | 720p |
| 86 | 1366x768 | 60Hz | 16:9 | reduzierte Ausblendung |

Beachten Sie, dass es ein [Pixeltaktlimit] gibt (https://www.raspberrypi.org/forums/viewtopic.php?f=26&t=20155&p=195443#p195443). Der höchste unterstützte Modus bei Modellen vor dem Raspberry Pi 4 ist 1920x1200 bei 60Hz mit reduziertem Blanking, während der Raspberry Pi 4 bis zu 4096x2160 (bekannt als 4k) bei 60Hz unterstützt. Beachten Sie auch, dass Sie, wenn Sie beide HDMI-Anschlüsse des Raspberry Pi 4 für die 4k-Ausgabe verwenden, auf beiden auf 30 Hz begrenzt sind.

### hdmi_timings

Dies ermöglicht die Einstellung von rohen HDMI-Timing-Werten für einen benutzerdefinierten Modus, der mit `hdmi_group=2` und `hdmi_mode=87` ausgewählt wird.

```
hdmi_timings=<h_active_pixels> <h_sync_polarity> <h_front_porch> <h_sync_pulse> <h_back_porch> <v_active_lines> <v_sync_polarity> <v_front_porch> <v_sync_pulse> <v_back_interch> <v_sync_offset_plac> <v_sync_offset_aoff> <v_sync_offset_aoff> <Seitenverhältnis>
```

```
<h_active_pixels> = horizontale Pixel (Breite)
<h_sync_polarity> = hsync-Polarität invertieren
<h_front_porch> = horizontale Vorwärtspolsterung von DE aktiver Kante
<h_sync_pulse> = hsync-Pulsbreite in Pixeltakten
<h_back_porch> = vertikale Rückenpolsterung von DE aktiver Kante
<v_active_lines> = vertikale Pixelhöhe (Linien)
<v_sync_polarity> = vsync-Polarität invertieren
<v_front_porch> = vertikales Vorwärtspolstern von DE aktiver Kante
<v_sync_pulse> = vsync-Pulsbreite in Pixeltakten
<v_back_porch> = vertikale Rückenpolsterung von DE aktiver Kante
<v_sync_offset_a> = bei Null belassen
<v_sync_offset_b> = bei Null belassen
<pixel_rep> = bei Null belassen
<frame_rate> = Bildschirmaktualisierungsrate in Hz
<interlaced> = bei Null belassen
<pixel_freq> = Taktfrequenz (Breite*Höhe*Bildrate)
<Seitenverhältnis> = *
```

`*` Das Seitenverhältnis kann auf einen von acht Werten eingestellt werden (wählen Sie den für Ihren Bildschirm am besten geeigneten):

```
HDMI_ASPECT_4_3 = 1
HDMI_ASPECT_14_9 = 2
HDMI_ASPECT_16_9 = 3
HDMI_ASPECT_5_4 = 4
HDMI_ASPECT_16_10 = 5
HDMI_ASPECT_15_9 = 6
HDMI_ASPECT_21_9 = 7
HDMI_ASPECT_64_27 = 8
```

### hdmi_force_mode

Die Einstellung auf `1` entfernt alle anderen Modi außer den durch `hdmi_mode` und `hdmi_group` spezifizierten aus der internen Liste, was bedeutet, dass sie in keiner aufgezählten Liste von Modi erscheinen. Diese Option kann hilfreich sein, wenn ein Display die Einstellungen von `hdmi_mode` und `hdmi_group` zu ignorieren scheint.

###edid_content_type

Erzwingt den EDID-Inhaltstyp auf einen bestimmten Wert.

Die Optionen sind:
 - `0` = `EDID_ContentType_NODATA`, Inhaltstyp keiner.
 - `1` = `EDID_ContentType_Graphics`, Inhaltstyp Grafiken, ITC muss auf 1 gesetzt werden
 - `2` = `EDID_ContentType_Photo`, Inhaltstyp Foto
 - `3` = `EDID_ContentType_Cinema`, Inhaltstyp Kino
 - `4` = `EDID_ContentType_Game`, Inhaltstyp Spiel
 
### hdmi_enable_4kp60 (nur Pi 4B)

Standardmäßig wählt der Raspberry Pi 4B beim Anschluss an einen 4K-Monitor eine Bildwiederholfrequenz von 30 Hz. Verwenden Sie diese Option, um die Auswahl von 60-Hz-Bildwiederholfrequenzen zu ermöglichen. Beachten Sie, dass dies den Stromverbrauch und die Temperatur des Raspberry Pi erhöht. Es ist nicht möglich, 4Kp60 gleichzeitig an beiden Micro-HDMI-Ports auszugeben.
 
## Welche Werte gelten für meinen Monitor?

Ihr HDMI-Monitor unterstützt möglicherweise nur eine begrenzte Anzahl von Formaten. Um herauszufinden, welche Formate unterstützt werden, verwenden Sie die folgende Methode:

  1. Stellen Sie das Ausgabeformat auf VGA 60Hz (`hdmi_group=1` und `hdmi_mode=1`) und booten Sie Ihren Raspberry Pi
  1. Geben Sie den folgenden Befehl ein, um eine Liste der von CEA unterstützten Modi anzuzeigen: `/opt/vc/bin/tvservice -m CEA`
  1. Geben Sie den folgenden Befehl ein, um eine Liste der von DMT unterstützten Modi anzuzeigen: `/opt/vc/bin/tvservice -m DMT`
  1. Geben Sie den folgenden Befehl ein, um Ihren aktuellen Status anzuzeigen: `/opt/vc/bin/tvservice -s`
  1. Geben Sie die folgenden Befehle ein, um detailliertere Informationen von Ihrem Monitor auszugeben: `/opt/vc/bin/tvservice -dedid.dat; /opt/vc/bin/edidparser edid.dat`

Die `edid.dat` sollte auch bereitgestellt werden, wenn Probleme mit dem Standard-HDMI-Modus behoben werden.

## Benutzerdefinierter Modus

Wenn Ihr Monitor einen Modus erfordert, der nicht in einer der obigen Tabellen enthalten ist, können Sie stattdessen einen benutzerdefinierten [CVT](https://en.wikipedia.org/wiki/Coordinated_Video_Timings)-Modus dafür definieren:

```
hdmi_cvt=<Breite> <Höhe> <Framerate> <Seitenverhältnis> <Ränder> <Interlace> <rb>
```

| Wert | Standard | Beschreibung |
| --- | --- | --- |
| Breite | (erforderlich) | Breite in Pixel |
| Höhe | (erforderlich) | Höhe in Pixel |
| Bildrate | (erforderlich) | Bildrate in Hz |
| Aspekt | 3 | Seitenverhältnis 1=4:3, 2=14:9, 3=16:9, 4=5:4, 5=16:10, 6=15:9 |
| Ränder | 0 | 0=Ränder deaktiviert, 1=Ränder aktiviert |
| verschachteln | 0 | 0=progressiv, 1=interlaced |
| rb | 0 | 0=normal, 1=reduzierte Ausblendung |

Felder am Ende können weggelassen werden, um die Standardwerte zu verwenden.

Beachten Sie, dass dies einfach den Modus **erstellt** (Gruppe 2 Modus 87). Damit der Pi dies standardmäßig verwendet, müssen Sie einige zusätzliche Einstellungen hinzufügen. Im Folgenden wird beispielsweise eine Auflösung von 800 × 480 ausgewählt und das Audiolaufwerk aktiviert:

```
hdmi_cvt=800 480 60 6
hdmi_group=2
hdmi_mode=87
hdmi_drive=2
```

Dies funktioniert möglicherweise nicht, wenn Ihr Monitor die Standard-CVT-Timings nicht unterstützt.

## LCD-Display/Touchscreen-Optionen

###ignor_lcd

Standardmäßig wird das Raspberry Pi LCD-Display verwendet, wenn es am I2C-Bus erkannt wird. `ignore_lcd=1` überspringt diese Erkennungsphase und daher wird das LCD-Display nicht verwendet.

### display_default_lcd

Wenn ein Raspberry Pi DSI LCD erkannt wird, wird es als Standardanzeige verwendet und zeigt den Framebuffer an. Die Einstellung `display_default_lcd=0` stellt sicher, dass das LCD nicht das Standarddisplay ist, was normalerweise bedeutet, dass der HDMI-Ausgang der Standard ist. Das LCD kann weiterhin verwendet werden, indem seine Anzeigenummer aus unterstützten Anwendungen ausgewählt wird, z. B. omxplayer.

### lcd_framerate

Geben Sie die Framerate des Raspberry Pi LCD-Displays in Hertz/fps an. Standardmäßig auf 60 Hz eingestellt.

### lcd_rotate

Dadurch wird das Display mithilfe der integrierten Flip-Funktionalität des LCDs umgedreht, was eine kostengünstigere Operation ist als die Verwendung des GPU-basierten Rotationsvorgangs.

Zum Beispiel kompensiert `lcd_rotate=2` eine auf dem Kopf stehende Anzeige.

###disable_touchscreen

Aktivieren/Deaktivieren des Touchscreens.

`disable_touchscreen=1` deaktiviert den Touchscreen auf dem offiziellen Raspberry Pi LCD-Display.

### enable_dpi_lcd

Aktivieren Sie LCD-Anzeigen, die an die DPI-GPIOs angeschlossen sind. Dies soll die Verwendung von LCD-Displays von Drittanbietern über die parallele Display-Schnittstelle ermöglichen.

### dpi_group, dpi_mode, dpi_output_format

Die Parameter 'dpi_group' und 'dpi_mode' config.txt werden verwendet, um einen der vordefinierten Modi einzustellen (DMT- oder CEA-Modi, wie sie von HDMI oben verwendet werden). Ein Benutzer kann benutzerdefinierte Modi ähnlich wie für HDMI generieren (siehe Abschnitt `dpi_timings`).

`dpi_output_format` ist eine Bitmaske, die verschiedene Parameter angibt, die zum Einrichten des Anzeigeformats verwendet werden.

Weitere Details zur Verwendung der DPI-Modi und des Ausgabeformats finden Sie [hier](../../hardware/raspberrypi/dpi/README.md).

### dpi_timings

Dies ermöglicht die Einstellung von Roh-DPI-Timing-Werten für einen benutzerdefinierten Modus, ausgewählt mit `dpi_group=2` und `dpi_mode=87`.

```
dpi_timings=<h_active_pixels> <h_sync_polarity> <h_front_porch> <h_sync_pulse> <h_back_porch> <v_active_lines> <v_sync_polarity> <v_front_porch> <v_sync_pulse> <v_plac_rate> <v_back_porch> <v_back_porch> <v_sync_offset_a> <v_sync_offset_a> <v_sync_bset_a> <v_sync_offset_a> <Seitenverhältnis>
```

```
<h_active_pixels> = horizontale Pixel (Breite)
<h_sync_polarity> = hsync-Polarität invertieren
<h_front_porch> = horizontale Vorwärtspolsterung von DE aktiver Kante
<h_sync_pulse> = hsync-Pulsbreite in Pixeltakten
<h_back_porch> = vertikale Rückenpolsterung von DE aktiver Kante
<v_active_lines> = vertikale Pixelhöhe (Linien)
<v_sync_polarity> = vsync-Polarität invertieren
<v_front_porch> = vertikale Vorwärtspolsterung von DE aktiver Kante
<v_sync_pulse> = vsync-Pulsbreite in Pixeltakten
<v_back_porch> = vertikale Rückenpolsterung von DE aktiver Kante
<v_sync_offset_a> = bei Null belassen
<v_sync_offset_b> = bei Null belassen
<pixel_rep> = bei Null belassen
<frame_rate> = Bildschirmaktualisierungsrate in Hz
<interlaced> = bei Null belassen
<pixel_freq> = Taktfrequenz (Breite*Höhe*Bildrate)
<Seitenverhältnis> = *
```

`*` Das Seitenverhältnis kann auf einen von acht Werten eingestellt werden (wählen Sie den für Ihren Bildschirm am besten geeigneten):

```
HDMI_ASPECT_4_3 = 1
HDMI_ASPECT_14_9 = 2
HDMI_ASPECT_16_9 = 3
HDMI_ASPECT_5_4 = 4
HDMI_ASPECT_16_10 = 5
HDMI_ASPECT_15_9 = 6
HDMI_ASPECT_21_9 = 7
HDMI_ASPECT_64_27 = 8
```

## Generische Anzeigeoptionen

### hdmi_force_hotplug

Das Setzen von `hdmi_force_hotplug` auf `1` gibt vor, dass das HDMI-Hotplug-Signal bestätigt wird, sodass es den Anschein hat, dass ein HDMI-Display angeschlossen ist. Mit anderen Worten, der HDMI-Ausgabemodus wird verwendet, auch wenn kein HDMI-Monitor erkannt wird.

### hdmi_ignore_hotplug

Das Setzen von `hdmi_ignore_hotplug` auf `1` gibt vor, dass das HDMI-Hotplug-Signal nicht bestätigt wird, sodass es den Anschein hat, als sei kein HDMI-Display angeschlossen. Mit anderen Worten, der Composite-Ausgabemodus wird verwendet, auch wenn ein HDMI-Monitor erkannt wird.

###disable_overscan

Setzen Sie `disable_overscan` auf `1`, um die von der Firmware gesetzten Standardwerte von [overscan](../raspi-config.md#overscan) zu deaktivieren. Der Standardwert für Overscan für den linken, rechten, oberen und unteren Rand ist '48' für HD-CEA-Modi, '32' für SD-CEA-Modi und '0' für DMT-Modi. Der Standardwert für `disable_overscan` ist `0`.

**HINWEIS:** Alle weiteren zusätzlichen Overscan-Optionen wie `overscan_scale` oder Overscan-Kanten können auch nach dieser Option angewendet werden.

###overscan_left

Der Befehl `overscan_left` gibt die Anzahl der Pixel an, die zum Firmware-Standardwert von Overscan am linken Bildschirmrand hinzugefügt werden sollen. Der Standardwert ist `0`.

Erhöhen Sie diesen Wert, wenn der Text über den linken Bildschirmrand fließt; verringern Sie ihn, wenn zwischen dem linken Bildschirmrand und dem Text ein schwarzer Rand vorhanden ist.

###overscan_right

Der Befehl `overscan_right` gibt die Anzahl der Pixel an, die zum Firmware-Standardwert von Overscan am rechten Bildschirmrand hinzugefügt werden sollen. Der Standardwert ist `0`.

Erhöhen Sie diesen Wert, wenn der Text über den rechten Bildschirmrand fließt; verringern Sie ihn, wenn zwischen dem rechten Bildschirmrand und dem Text ein schwarzer Rand vorhanden ist.

###overscan_top

Der Befehl `overscan_top` gibt die Anzahl der Pixel an, die zum Firmware-Standardwert von Overscan am oberen Bildschirmrand hinzugefügt werden sollen. Der Standardwert ist `0`.

Erhöhen Sie diesen Wert, wenn der Text über den oberen Bildschirmrand fließt; verringern Sie ihn, wenn zwischen dem oberen Bildschirmrand und dem Text ein schwarzer Rand vorhanden ist.

###overscan_bottom

Der Befehl `overscan_bottom` gibt die Anzahl der Pixel an, die zum Firmware-Standardwert von Overscan am unteren Bildschirmrand hinzugefügt werden sollen. Der Standardwert ist `0`.

Erhöhen Sie diesen Wert, wenn der Text über den unteren Bildschirmrand fließt; verringern Sie ihn, wenn zwischen dem unteren Bildschirmrand und dem Text ein schwarzer Rand vorhanden ist.

### overscan_scale

Setzen Sie `overscan_scale` auf `1`, um zu erzwingen, dass alle Nicht-Framebuffer-Ebenen den Overscan-Einstellungen entsprechen. Der Standardwert ist `0`.

**HINWEIS:** Diese Funktion wird im Allgemeinen nicht empfohlen: Sie kann die Bildqualität beeinträchtigen, da alle Ebenen auf dem Display von der GPU skaliert werden. Das Deaktivieren von Overscan auf dem Display selbst ist die empfohlene Option, um zu vermeiden, dass Bilder doppelt skaliert werden (durch die GPU und das Display).

### framebuffer_width

Der Befehl `framebuffer_width` gibt die Framebuffer-Breite der Konsole in Pixeln an. Der Standardwert ist die Anzeigebreite abzüglich des gesamten horizontalen Overscans.

### framebuffer_height

Der Befehl `framebuffer_height` gibt die Framebuffer-Höhe der Konsole in Pixeln an. Der Standardwert ist die Anzeigehöhe abzüglich des gesamten vertikalen Overscans.
C4
### max_framebuffer_height, max_framebuffer_width

Gibt die maximalen Abmessungen an, die der interne Bildpuffer haben darf.

###framebuffer_depth

Verwenden Sie `framebuffer_depth`, um die Framebuffer-Tiefe der Konsole in Bits pro Pixel anzugeben. Der Standardwert ist "16".

| framebuffer_depth | Ergebnis | Notizen |
| --- | --- | --- |
| 8 | 8bit Framebuffer | Standard-RGB-Palette macht den Bildschirm unlesbar |
| 16 | 16bit Framebuffer | |
| 24 | 24bit Framebuffer | Kann zu einer beschädigten Anzeige führen |
| 32 | 32bit Framebuffer | Muss möglicherweise in Verbindung mit `framebuffer_ignore_alpha=1` verwendet werden |

###framebuffer_ignore_alpha

Setzen Sie `framebuffer_ignore_alpha` auf `1`, um den Alphakanal zu deaktivieren. Kann bei der Anzeige eines 32bit `framebuffer_depth` helfen.

### framebuffer_priority

In einem System mit mehreren Displays, das den Legacy-Grafiktreiber (vor KMS) verwendet, zwingt dies ein bestimmtes internes Anzeigegerät, der erste Linux-Framebuffer (d. h. /dev/fb0) zu sein.

Folgende Optionen können eingestellt werden:

| Anzeige | ID |
| --- | --- |
|Haupt-LCD | 0 |
|Sekundär-LCD | 1 |
|HDMI 0 | 2 |
|Verbundwerkstoff | 3 |
|HDMI 1 | 7 |

###max_framebuffers

Dieser Konfigurationseintrag legt die maximale Anzahl von Firmware-Framebuffern fest, die erstellt werden können. Gültige Optionen sind 0,1 und 2. Standardmäßig ist dies bei Geräten vor dem Pi4 auf 1 eingestellt, muss also auf 2 erhöht werden, wenn mehr als ein Display verwendet wird, zum Beispiel HDMI und ein DSI- oder DPI-Display. Die Raspberry Pi4-Konfiguration setzt dies standardmäßig auf 2, da sie über zwei HDMI-Anschlüsse verfügt.

Im Allgemeinen ist es in den meisten Fällen sicher, dies auf 2 zu setzen, da Framebuffer nur erstellt werden, wenn ein angeschlossenes Gerät tatsächlich erkannt wird.

Wenn Sie diesen Wert auf 0 setzen, können Sie den Speicherbedarf im Headless-Modus reduzieren, da die Zuweisung von Framebuffern verhindert wird.

### Testmodus

Der Befehl `test_mode` zeigt während des Bootvorgangs (nur über die Composite-Video- und analogen Audioausgänge) ein Testbild und einen Testton für die angegebene Anzahl von Sekunden an, bevor das Betriebssystem wie gewohnt gestartet wird. Dies wird als Herstellungstest verwendet; der Standardwert ist `0`.

###display_hdmi_rotate

Verwenden Sie `display_hdmi_rotate`, um die Ausrichtung der HDMI-Anzeige zu drehen oder umzukehren. Der Standardwert ist `0`.

| display_hdmi_rotate | Ergebnis |
| --- | --- |
| 0 | keine Drehung |
| 1 | 90 Grad im Uhrzeigersinn drehen |
| 2 | 180 Grad im Uhrzeigersinn drehen |
| 3 | 270 Grad im Uhrzeigersinn drehen |
| 0x10000 | horizontaler Flip |
| 0x20000 | vertikaler Flip |

Beachten Sie, dass die 90- und 270-Grad-Rotationsoptionen zusätzlichen Speicher auf der GPU erfordern, sodass diese nicht mit der 16-MB-GPU-Aufteilung funktionieren.

Wenn Sie den VC4 FKMS V3D-Treiber verwenden (dies ist die Standardeinstellung auf dem Raspberry Pi 4), werden 90- und 270-Grad-Drehungen nicht unterstützt. Das Dienstprogramm zur Bildschirmkonfiguration bietet Anzeigedrehungen für diesen Treiber. Weitere Informationen finden Sie auf dieser [page](../display_rotation.md).

###display_lcd_rotate

Verwenden Sie für den Legacy-Grafiktreiber (Standard bei Modellen vor dem Pi4) `display_lcd_rotate`, um die LCD-Ausrichtung zu drehen oder zu spiegeln. Die Parameter sind dieselben wie bei `display_hdmi_rotate`. Siehe auch `lcd_rotate`.

### display_rotate

`display_rotate` ist in der neuesten Firmware veraltet, wurde aber aus Gründen der Abwärtskompatibilität beibehalten. Bitte verwenden Sie stattdessen `display_lcd_rotate` und `display_hdmi_rotate`.

Verwenden Sie `display_rotate`, um die Bildschirmausrichtung zu drehen oder zu spiegeln. Die Parameter sind dieselben wie bei `display_hdmi_rotate`.

###disable_fw_kms_setup

Standardmäßig analysiert die Firmware die EDID jedes über HDMI angeschlossenen Displays, wählt einen geeigneten Videomodus aus und übergibt dann die Auflösung und Bildrate des Modus zusammen mit den Overscan-Parametern über die Einstellungen in der Kernel-Befehlszeile an den Linux-Kernel. In seltenen Fällen kann dies dazu führen, dass ein Modus ausgewählt wird, der nicht in der EDID enthalten ist und möglicherweise nicht mit dem Gerät kompatibel ist. Sie können `disable_fw_kms_setup=1` verwenden, um die Übergabe dieser Parameter zu deaktivieren und dieses Problem zu vermeiden. Das Linux-Videomodussystem (KMS) analysiert dann die EDID selbst und wählt einen geeigneten Modus aus.

## Andere Optionen

### dispmanx_offline

Erzwingt die Offline-Komposition von dispmanx in zwei Offscreen-Framebuffern. Dadurch können mehr Dispmanx-Elemente zusammengesetzt werden, ist jedoch langsamer und kann die Bildschirm-Framerate auf typischerweise 30 fps begrenzen.

*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses .) geteilt wird /bis-sa/3.0/)*