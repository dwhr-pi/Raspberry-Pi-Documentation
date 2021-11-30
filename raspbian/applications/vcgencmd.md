## vcgencmd

`vcgencmd` ist ein Befehlszeilen-Dienstprogramm, das verschiedene Informationen von der VideoCore-GPU auf dem Raspberry Pi abrufen kann. Viele der verfügbaren Informationen sind nur für Raspberry Pi-Ingenieure von Nutzen, aber für Endbenutzer stehen eine Reihe sehr nützlicher Optionen zur Verfügung, die hier beschrieben werden.

Die Quelle für die Anwendung finden Sie auf unserer Github-Seite [hier](https://github.com/raspberrypi/userland/tree/master/host_applications/linux/apps/gencmd).


### Verwendungszweck

Um eine Liste aller Befehle zu erhalten, die `vcgencmd` unterstützt, geben Sie `vcgencmd commands` ein.

Einige der nützlicheren Befehle werden unten beschrieben.

### Befehle

#### vcos

Der Befehl `vcos` hat eine Reihe von Unterbefehlen.

`version` Zeigt das Erstellungsdatum und die Version der Firmware auf dem VideoCore an.
`log status` Zeigt den Fehlerprotokollstatus der verschiedenen VideoCore-Softwarebereiche an.

#### version

Zeigt das Erstellungsdatum und die Version der Firmware auf dem VideoCore an.

#### get_camera

Zeigt den aktivierten und erkannten Status der offiziellen Kamera an. 1 bedeutet ja, 0 bedeutet nein. Während alle Firmware (außer Cutdown-Versionen) die Kamera unterstützt, muss diese Unterstützung mit [raspi-config](../../configuration/raspi-config.md) aktiviert werden.

#### get_throttled

Gibt den gedrosselten Zustand des Systems zurück. Dies ist ein Bitmuster - ein gesetztes Bit hat folgende Bedeutung:

| Bit | Hex-Wert | Bedeutung |
|:---:|----------|-----------|
| 0 | 1 | Unterspannung erkannt |
| 1 | 2 | Armfrequenzbegrenzung |
| 2 | 4 | Derzeit gedrosselt |
| 3 | 8 | Softtemperaturbegrenzung aktiv |
| 16 | 10000 | Unterspannung ist aufgetreten |
| 17 | 20000 | Es ist eine Arm-Frequency-Capping aufgetreten |
| 18 | 40000 | Drosselung ist aufgetreten |
| 19 | 80000 | Weiche Temperaturgrenze ist aufgetreten |

Ein Wert von Null gibt an, dass keine der obigen Bedingungen zutrifft.

Um herauszufinden, ob eines dieser Bits gesetzt wurde, konvertieren Sie den zurückgegebenen Wert in einen Binärwert und nummerieren Sie dann jedes Bit oben. Sie können dann sehen, welche Bits gesetzt sind. Zum Beispiel:

``0x50000 = 0101 0000 0000 0000 0000``

Wenn wir die Bitnummern oben hinzufügen, erhalten wir:

```text
19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
 0  1  0  1  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
```

Daraus können wir sehen, dass die Bits 18 und 16 gesetzt sind, was darauf hinweist, dass der Pi zuvor aufgrund von Unterspannung gedrosselt wurde, derzeit jedoch aus keinem Grund gedrosselt wird.

Alternativ können die Werte mit den obigen Hex-Werten abgeleitet werden, indem nacheinander der größte Wert abgezogen wird:

``0x50000 = 40000 + 10000``

#### mess_temp

Gibt die vom integrierten Temperatursensor gemessene Temperatur des SoC zurück.

#### measure_clock [Uhr]

Dies gibt die aktuelle Frequenz des angegebenen Takts zurück. Die Optionen sind:

| Uhr   | Beschreibung |
|:-----:|--------------|
| arm   | ARM-Kerne |
| core  | VC4-Skaliererkerne |
| H264  | H264-Block |
| isp   | Bildsignalprozessor |
| v3d   | 3D-Block |
| uart  | UART |
| pwm   | PWM-Block (analoger Audioausgang) |
| emmc  | SD-Kartenschnittstelle |
| pixel | Pixelwert |
| vec   | Analoger Video-Encoder |
| hdmi  | HDMI |
| dpi   | Peripherieschnittstelle anzeigen |

z.B. `vcgencmd measure_clock arm`

#### measure_volts [Block]

Zeigt die aktuellen Spannungen an, die vom spezifischen Block verwendet werden.

| Block   | Beschreibung |
|:-------:|--------------|
| core    | VC4-Kernspannung |
| sdram_c | SDRAM-Kernspannung |
| sdram_i | SDRAM-E/A-Spannung |
| sdram_p | SDRAM Phy-Spannung|

#### otp_dump

Zeigt den Inhalt des One Time Programmable (OTP) Speichers an, der Teil des SoC ist. Dies sind 32-Bit-Werte, indiziert von 8 bis 64. Weitere Informationen finden Sie auf der [OTP-Bits-Seite](../../hardware/raspberrypi/otpbits.md).

#### get_mem type

Berichtet über die Speichermenge, die den ARM-Kernen `vcgencmd get_mem arm` und der VC4 `vcgencmd get_mem gpu` zugewiesen ist.

**Hinweis:** Auf einem Raspberry Pi 4 mit mehr als 1 GB RAM ist die Option "arm" ungenau. Dies liegt daran, dass die GPU-Firmware, die diesen Befehl implementiert, nur das erste Gigabyte RAM auf dem System kennt, sodass die Einstellung "arm" immer 1 GB abzüglich des Speicherwerts "gpu" zurückgibt. Um einen genauen Bericht über die Größe des ARM-Speichers zu erhalten, verwenden Sie einen der Standard-Linux-Befehle wie `free` oder `cat /proc/meminfo`

#### codec_enabled [Typ]

Meldet, ob der angegebene CODEC-Typ aktiviert ist. Mögliche Optionen für den Typ sind AGIF, FLAC, H263, H264, MJPA, MJPB, MJPG, **MPG2**, MPG4, MVC0, PCM, THRA, VORB, VP6, VP8, **WMV9**, **WVC1** . Die hervorgehobenen erfordern derzeit eine kostenpflichtige Lizenz (weitere Informationen finden Sie in den [FAQ](../../faqs/README.md#pi-video), außer auf dem Pi4, wo diese Hardware-Codecs bevorzugt deaktiviert sind Software-Decodierung, für die keine Lizenz erforderlich ist. Beachten Sie, dass, da der H265-HW-Block auf dem Raspberry Pi4 nicht Teil der VideoCore-GPU ist, über diesen Befehl nicht auf seinen Status zugegriffen wird.

#### get_config-Typ | name

Dies gibt alle Konfigurationselemente des angegebenen Typs zurück, die in config.txt festgelegt wurden, oder ein einzelnes Konfigurationselement. Mögliche Werte für den Typparameter sind **int, str** oder verwenden Sie einfach den Namen des Konfigurationselements.

#### get_lcd_info

Zeigt die Auflösung und Farbtiefe eines angeschlossenen Displays an.

#### mem_oom

Zeigt Statistiken zu allen Ereignissen wegen Speichermangels an, die im VC4-Speicherbereich auftreten.

#### mem_reloc_stats

Zeigt Statistiken von der verschiebbaren Speicherzuordnung auf dem VC4 an.

#### read_ring_osc

Gibt die aktuelle Drehzahlspannung und Temperatur des Ringoszillators zurück.

#### hdmi_timings

Zeigt die aktuellen Timings der HDMI-Einstellungen an. Einzelheiten zu den zurückgegebenen Werten finden Sie unter [Video Config](../../configuration/config-txt/video.md).

#### dispmanx_list

Geben Sie eine Liste aller derzeit angezeigten Dispmanx-Elemente aus.

#### display_power [0 | 1 | -1] [Anzeige]

Zeigen Sie den aktuellen Energiestatus des Displays an oder legen Sie den Energiestatus des Displays fest. `vcgencmd display_power 0` schaltet die Stromzufuhr zum aktuellen Display aus. `vcgencmd display_power 1` schaltet das Display ein. Wenn kein Parameter eingestellt ist, wird hier der aktuelle Leistungszustand angezeigt. Der letzte Parameter ist eine optionale Anzeige-ID, wie sie von `tvservice -l` oder aus der folgenden Tabelle zurückgegeben wird und die es ermöglicht, eine bestimmte Anzeige ein- oder auszuschalten.

Beachten Sie, dass dies beim 7" Raspberry Pi Touch Display einfach die Hintergrundbeleuchtung ein- und ausschaltet. Die Touch-Funktionalität funktioniert weiterhin wie gewohnt.

`vcgencmd display_power 0 7` schaltet die Anzeige von ID 7 aus, die HDMI 1 auf einem Raspberry Pi 4 ist.

| Anzeige | ID |
| --- | --- |
|Haupt-LCD | 0 |
|Sekundär-LCD | 1 |
|HDMI 0 | 2 |
|Composite | 3 |
|HDMI 1 | 7 |

Um festzustellen, ob eine bestimmte Display-ID ein- oder ausgeschaltet ist, verwenden Sie -1 als ersten Parameter.

`vcgencmd display_power -1 7` gibt 0 zurück, wenn Display-ID 7 ausgeschaltet ist, 1, wenn Display-ID 7 eingeschaltet ist, oder -1, wenn Display-ID 7 in einem unbekannten Zustand ist, zum Beispiel unentdeckt.