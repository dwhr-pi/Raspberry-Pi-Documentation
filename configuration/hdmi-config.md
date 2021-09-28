## HDMI-Konfiguration

In den allermeisten Fällen führt das einfache Anschließen Ihres mit HDMI ausgestatteten Monitors über ein Standard-HDMI-Kabel an den Raspberry Pi automatisch zum Pi mit der besten Auflösung, die der Monitor unterstützt. Der Raspberry Pi Zero verwendet einen Mini-HDMI-Anschluss, daher benötigen Sie ein Mini-HDMI-zu-Full-Size-HDMI-Kabel oder einen Adapter. Auf dem Raspberry Pi 4 gibt es zwei Micro-HDMI-Anschlüsse, sodass Sie entweder ein oder zwei Micro-HDMI-zu-Full-Size-HDMI-Kabel oder Adapter benötigen, je nachdem, wie viele Displays Sie anschließen möchten. Sie sollten alle HDMI-Kabel anschließen, bevor Sie den Raspberry Pi einschalten.

Der Raspberry Pi 4 kann bis zu zwei Displays mit einer Auflösung von bis zu 1080p bei einer Bildwiederholfrequenz von 60 Hz ansteuern. Wenn Sie bei 4K-Auflösung zwei Displays anschließen, sind Sie auf eine Bildwiederholfrequenz von 30 Hz beschränkt. Sie können auch ein einzelnes Display mit 4K mit einer Bildwiederholfrequenz von 60 Hz betreiben: Dazu muss das Display an den HDMI-Anschluss neben dem USB-C-Stromeingang (mit der Bezeichnung HDMI0) angeschlossen werden. Sie müssen auch die 4Kp60-Ausgabe aktivieren, indem Sie das Flag `hdmi_enable_4kp60=1` in der config.txt setzen. Dieses Flag kann auch mit dem Tool „Raspberry Pi Configuration“ in der Desktop-Umgebung gesetzt werden.

Wenn Sie den 3D-Grafiktreiber (auch FKMS-Treiber genannt) verwenden, finden Sie im Menü Einstellungen eine grafische Anwendung zum Einrichten von Standardanzeigen, einschließlich Multi-Display-Setups. Siehe [Anweisungen zur Verwendung des Tools hier](arandr.md).

Wenn Sie ältere Grafiktreiber verwenden oder sich in Situationen befinden, in denen der Raspberry Pi möglicherweise nicht den besten Modus ermitteln kann, oder Sie speziell eine nicht standardmäßige Auflösung festlegen möchten, kann der Rest dieser Seite hilfreich sein.

Beachten Sie, dass alle Befehle auf dieser Seite vollständig in der Dokumentation config.txt [Video](config-txt/video.md) dokumentiert sind.

### HDMI-Gruppen und -Modus

HDMI hat zwei gemeinsame Gruppen: CEA (Consumer Electronics Association, der typischerweise von Fernsehgeräten verwendete Standard) und DMT (Display Monitor Timings, der typischerweise von Monitoren verwendete Standard). Jede Gruppe bietet einen bestimmten Satz von Modi an, wobei ein Modus die Auflösung, Bildrate, Taktrate und das Seitenverhältnis der Ausgabe beschreibt.

### Welche Modi unterstützt mein Gerät?

Sie können die Anwendung `tvservice` in der Befehlszeile verwenden, um festzustellen, welche Modi von Ihrem Gerät unterstützt werden, zusammen mit anderen nützlichen Daten:

+ `tvservice -s` zeigt den aktuellen HDMI-Status an, inklusive Modus und Auflösung
+ `tvservice -m CEA` listet alle unterstützten CEA-Modi auf
+ `tvservice -m DMT` listet alle unterstützten DMT-Modi auf

Wenn Sie einen Pi 4 mit mehr als einem angeschlossenen Display verwenden, muss `tvservice` mitgeteilt werden, welches Gerät nach Informationen fragen soll. Sie können Anzeige-IDs für alle angeschlossenen Geräte abrufen, indem Sie Folgendes verwenden:

`tvservice -l`

Sie können angeben, welche Anzeige `tvservice` verwendet, indem Sie `-v <display id>` zum `tvservice`-Befehl hinzufügen, z.B.:

+ `tvservice -v 7 -m CEA`, listet alle unterstützten CEA-Modi für Display-ID 7 auf

### Einstellen eines bestimmten HDMI-Modus

Das Einstellen eines bestimmten Modus erfolgt über die config.txt-Einträge `hdmi_group` und `hdmi_mode`. Der Gruppeneintrag wählt zwischen CEA oder DMT und der Modus wählt die Auflösung und Bildrate. Sie finden Moditabellen auf der Seite config.txt [Video Configuration](config-txt/video.md), aber Sie sollten den oben beschriebenen `tvservice`-Befehl verwenden, um genau herauszufinden, welche Modi Ihr Gerät unterstützt.

Auf dem Pi 4 fügen Sie zur Angabe des HDMI-Ports eine Indexkennung zum Eintrag „hdmi_group“ oder „hdmi_mode“ in der config.txt hinzu, z. `hdmi_mode:0` oder `hdmi_group:1`.

### Einstellen eines benutzerdefinierten HDMI-Modus

Es gibt zwei Optionen zum Einstellen eines benutzerdefinierten Modus: `hdmi_cvt` und `hdmi_timings`.

`hdmi_cvt` setzt einen benutzerdefinierten koordinierten Video-Timing-Eintrag, der hier vollständig beschrieben wird: [Video Configuration](config-txt/video.md#Custom%20Mode)

In seltenen Fällen kann es erforderlich sein, die genauen Taktanforderungen des HDMI-Signals zu definieren. Dies ist ein vollständig benutzerdefinierter Modus, der durch das Setzen von `hdmi_group=2` und `hdmi_mode=87` aktiviert wird. Sie können dann den config.txt-Befehl `hdmi_timings` verwenden, um die spezifischen Parameter für Ihre Anzeige einzustellen.
`hdmi_timings` gibt alle Timings an, die ein HDMI-Signal verwenden muss. Diese Timings finden Sie normalerweise im Datenblatt des verwendeten Displays.

`hdmi_timings=<h_active_pixels> <h_sync_polarity> <h_front_porch> <h_sync_pulse> <h_back_porch> <v_active_pixels> <h_sync_polarity> <h_front_porch> <h_sync_pulse> <h_back_porch> <v_ch_active_porch> <v_ch_active_porch> <v_ch_active_porch> <v_ch_active_porch> <v_ch_active_porch> <v_v_active_porch> <v_sync_offset_b> <pixel_rep> <frame_rate> <interlaced> <pixel_freq> <aspect_ratio>`

| Timing | Zweck |
| ------------- | ------------- |
| `h_active_pixels` | Die horizontale Auflösung |
| `h_sync_polarity` | 0 oder 1, um die horizontale Sync-Polarität zu definieren |
| `h_front_portal` | Anzahl der horizontalen Pixel der Veranda |
| `h_sync_pulse` | Breite des horizontalen Synchronisationsimpulses |
| `h_back_porch` | Anzahl der horizontalen Pixel der hinteren Veranda |
| `v_active_lines` | Die vertikale Auflösung |
| `v_sync_polarity` | 0 oder 1, um die vertikale Sync-Polarität zu definieren |
| `v_front_portal` | Anzahl der vertikalen Pixel der Veranda |
| `v_sync_pulse` | Breite des vertikalen Synchronisationsimpulses |
| `v_back_portal` | Anzahl der vertikalen Pixel der hinteren Veranda |
| `v_sync_offset_a` | Bei 0 verlassen |
| `v_sync_offset_b` | Bei 0 verlassen |
| `pixel_rep` | Bei 0 verlassen |
| `Bildrate` | Bildrate des Modus |
| `interlaced` | 0 für non-interlaced, 1 für interlaced |
| `pixel_freq` | Die Moduspixelfrequenz |
| `Seitenverhältnis` | Das erforderliche Seitenverhältnis |

`aspect_ratio` sollte einer der folgenden sein:

| Verhältnis | `aspekt_verhältnis` ID |
|------|----|
| `4:3` | 1 |
|`14:9` | 2 |
|`16:9` | 3 |
|`5:4` | 4 |
|`16:10`| 5 |
|`15:9` | 6 |
|`21:9` | 7 |
|`64:27`| 8 |

Für den Pi4 können Sie zur Angabe des HDMI-Ports eine Indexkennung zur config.txt hinzufügen. z.B. `hdmi_cvt:0=...` oder `hdmi_timings:1=...`. Wenn keine Portkennung angegeben ist, werden die Einstellungen auf Port 0 angewendet.

###Anzeigendrehung

Weitere Informationen finden Sie auf der [Display-Rotation-Seite](./display_rotation.md).

### HDMI funktioniert nicht richtig?

In seltenen Fällen müssen Sie möglicherweise die HDMI-Laufwerksstärke erhöhen, z. B. wenn das Display gesprenkelt ist oder wenn Sie sehr lange Kabel verwenden. Dafür gibt es ein config.txt-Element, `config_hdmi_boost`, das auf der [config.txt-Videoseite](config-txt/video.md) dokumentiert ist.

Der Raspberry Pi 4B unterstützt `config_hdmi_boost` noch nicht, die Unterstützung für diese Option wird in einem zukünftigen Software-Update hinzugefügt.