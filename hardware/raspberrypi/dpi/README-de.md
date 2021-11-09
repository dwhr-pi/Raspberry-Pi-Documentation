# DPI (Parallelanzeigeschnittstelle)

Auf allen Raspberry-Pi-Boards mit dem 40-Wege-Header und den Compute-Modulen steht eine bis zu 24-Bit-parallele RGB-Schnittstelle zur Verfügung. Diese Schnittstelle ermöglicht den Anschluss paralleler RGB-Displays an den Raspberry Pi GPIO entweder in RGB24 (8 Bit für Rot, Grün und Blau) oder RGB666 (6 Bit pro Farbe) oder RGB565 (5 Bit Rot, 6 Bit Grün und 5 Bit Blau).

Diese Schnittstelle wird von der GPU-Firmware gesteuert und kann von einem Benutzer über spezielle config.txt-Parameter und durch Aktivieren des richtigen Linux-Gerätebaum-Overlays programmiert werden.

## GPIO-Pins

Eine der alternativen Funktionen, die auf Bank 0 des Raspberry Pi GPIO auswählbar sind, ist DPI (Display Parallel Interface), eine einfache getaktete parallele Schnittstelle (bis zu 8 Bits von R, G und B; Clock, Enable, hsync und vsync). Diese Schnittstelle ist als alternative Funktion 2 (ALT2) auf GPIO-Bank 0 verfügbar:

![DPI Alternate GPIO-Funktion](dpi-altfn2.png)

Beachten Sie, dass es verschiedene Möglichkeiten gibt, die Farbwerte auf den DPI-Ausgangspins entweder im 565-, 666- oder 24-Bit-Modus darzustellen (siehe die folgende Tabelle und den Teil `output_format` des Parameters `dpi_output_format` unten):

![DPI-Farbausgabe](dpi-packing.png)

## Andere GPIO-Peripheriegeräte deaktivieren

Beachten Sie, dass alle anderen Peripherie-Overlays, die widersprüchliche GPIO-Pins verwenden, deaktiviert werden müssen. Achten Sie in der config.txt darauf, alle dtparams, die I2C oder SPI aktivieren, auszukommentieren oder zu invertieren:

```
dtparam=i2c_arm=off
dtparam=spi=off
```

## Ausgabeformat steuern

Das Ausgabeformat (Takt, Farbformat, Sync-Polarität, Freigabe) kann mit einer magischen Zahl (Ganzzahl ohne Vorzeichen oder Hex-Wert mit dem Präfix 0x) gesteuert werden, die an den Parameter `dpi_output_format` in der config.txt aus den folgenden Feldern erstellt wird:

```
output_format          = (dpi_output_format >>  0) & 0xf;
rgb_order              = (dpi_output_format >>  4) & 0xf;

output_enable_mode     = (dpi_output_format >>  8) & 0x1;
invert_pixel_clock     = (dpi_output_format >>  9) & 0x1;

hsync_disable          = (dpi_output_format >> 12) & 0x1;
vsync_disable          = (dpi_output_format >> 13) & 0x1;
output_enable_disable  = (dpi_output_format >> 14) & 0x1;

hsync_polarity         = (dpi_output_format >> 16) & 0x1;
vsync_polarity         = (dpi_output_format >> 17) & 0x1;
output_enable_polarity = (dpi_output_format >> 18) & 0x1;

hsync_phase            = (dpi_output_format >> 20) & 0x1;
vsync_phase            = (dpi_output_format >> 21) & 0x1;
output_enable_phase    = (dpi_output_format >> 22) & 0x1;

output_format:
   1: DPI_OUTPUT_FORMAT_9BIT_666
   2: DPI_OUTPUT_FORMAT_16BIT_565_CFG1
   3: DPI_OUTPUT_FORMAT_16BIT_565_CFG2
   4: DPI_OUTPUT_FORMAT_16BIT_565_CFG3
   5: DPI_OUTPUT_FORMAT_18BIT_666_CFG1
   6: DPI_OUTPUT_FORMAT_18BIT_666_CFG2
   7: DPI_OUTPUT_FORMAT_24BIT_888

rgb_order:
   1: DPI_RGB_ORDER_RGB
   2: DPI_RGB_ORDER_BGR
   3: DPI_RGB_ORDER_GRB
   4: DPI_RGB_ORDER_BRG

output_enable_mode:
   0: DPI_OUTPUT_ENABLE_MODE_DATA_VALID
   1: DPI_OUTPUT_ENABLE_MODE_COMBINED_SYNCS

invert_pixel_clock:
   0: RGB Data changes on rising edge and is stable at falling edge
   1: RGB Data changes on falling edge and is stable at rising edge.

hsync/vsync/output_enable_polarity:
   0: default for HDMI mode
   1: inverted

hsync/vsync/oe phases:
   0: DPI_PHASE_POSEDGE
   1: DPI_PHASE_NEGEDGE
```

Beachten Sie, dass die einzelnen Bitfelder alle als "Standardverhalten invertieren" fungieren.

## Kontrolle von Timings und Auflösungen

In Firmware vom August 2018 oder höher wurde der config.txt-Eintrag „hdmi_timings“, der zuvor zum Einrichten der DPI-Timings verwendet wurde, durch einen neuen Parameter „dpi_timings“ ersetzt. Wenn der Parameter `dpi_timings` nicht vorhanden ist, wird das System auf die Verwendung des Parameters `hdmi_timings` zurückgreifen, um die Abwärtskompatibilität sicherzustellen. Wenn beide nicht vorhanden sind und ein benutzerdefinierter Modus angefordert wird, wird ein Standardsatz von Parametern für VGAp60 verwendet.

Die Parameter 'dpi_group' und 'dpi_mode' config.txt werden verwendet, um entweder vorgegebene Modi (DMT- oder CEA-Modi wie von HDMI verwendet) einzustellen oder ein Benutzer kann benutzerdefinierte Modi generieren.

Um einen benutzerdefinierten DPI-Modus zu generieren, starten Sie [hier](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=24679).

Wenn Sie einen benutzerdefinierten DPI-Modus einrichten, verwenden Sie in config.txt:
```
dpi_group=2
dpi_mode=87
```

Dies weist den Treiber an, die benutzerdefinierten `dpi_timings`-Timings (ältere Firmware verwendet `hdmi_timings`) für das DPI-Panel zu verwenden.

Die `dpi_timings`-Parameter werden als durch Leerzeichen getrennter Parametersatz angegeben:

```
dpi_timings=<h_active_pixels> <h_sync_polarity> <h_front_porch> <h_sync_pulse> <h_back_porch> <v_active_lines> <v_sync_polarity> <v_front_porch> <v_sync_pulse> <v_back_porch> <v_sync_offset_a> <v_sync_offset_b> <pixel_rep> <frame_rate> <interlaced> <pixel_freq> <aspect_ratio>

<h_active_pixels> = horizontal pixels (width)
<h_sync_polarity> = invert hsync polarity
<h_front_porch>   = horizontal forward padding from DE acitve edge
<h_sync_pulse>    = hsync pulse width in pixel clocks
<h_back_porch>    = vertical back padding from DE active edge
<v_active_lines>  = vertical pixels height (lines)
<v_sync_polarity> = invert vsync polarity
<v_front_porch>   = vertical forward padding from DE active edge
<v_sync_pulse>    = vsync pulse width in pixel clocks
<v_back_porch>    = vertical back padding from DE active edge
<v_sync_offset_a> = leave at zero
<v_sync_offset_b> = leave at zero
<pixel_rep>       = leave at zero
<frame_rate>      = screen refresh rate in Hz
<interlaced>      = leave at zero
<pixel_freq>      = clock frequency (width*height*framerate)
<aspect_ratio>    = *

* The aspect ratio can be set to one of eight values (choose closest for your screen):

HDMI_ASPECT_4_3 = 1
HDMI_ASPECT_14_9 = 2
HDMI_ASPECT_16_9 = 3
HDMI_ASPECT_5_4 = 4
HDMI_ASPECT_16_10 = 5
HDMI_ASPECT_15_9 = 6
HDMI_ASPECT_21_9 = 7
HDMI_ASPECT_64_27 = 8
```

## Überlagerungen

Ein Linux-Gerätebaum-Overlay wird verwendet, um die GPIO-Pins in den richtigen Modus zu schalten (alt-Funktion 2). Wie bereits erwähnt, ist die GPU für die Ansteuerung der DPI-Anzeige verantwortlich. Daher gibt es keinen Linux-Treiber; das Overlay stellt einfach die GPIO-Alt-Funktionen richtig ein.

Ein "full fat" DPI-Overlay (dpi24.dtb) wird bereitgestellt, das alle 28 GPIOs in den ALT2-Modus versetzt und die vollen 24 Bit des Farbbusses sowie die h- und v-Sync-, Enable- und Pixel-Clock bereitstellt. Beachten Sie, dass ** alle ** der GPIO-Pins der Bank 0 verwendet werden.

Ein zweites Overlay (vga666.dtb) ist für die Ansteuerung von VGA-Monitorsignalen im 666-Modus vorgesehen, die die Clock- und DE-Pins (GPIO 0 und 1) nicht benötigen und nur die GPIOs 4-21 für Farbe benötigen (mit Modus 5).

Diese Überlagerungen sind ziemlich trivial und ein Benutzer kann sie bearbeiten, um eine benutzerdefinierte Überlagerung zu erstellen, um nur die Pins zu aktivieren, die für ihren spezifischen Anwendungsfall erforderlich sind. Wenn man beispielsweise eine DPI-Anzeige mit vsync, hsync, pclk und de verwendet, aber im RGB565-Modus (Modus 2), dann könnte das dpi24.dtb-Overlay so bearbeitet werden, dass die GPIOs 20-27 nicht in den DPI-Modus geschaltet werden und könnte daher für andere Zwecke verwendet werden.

## Beispiel für config.txt-Einstellungen

### Gert VGA666-Adapter

Dieses Setup ist für den [Gert VGA-Adapter](https://github.com/fenlogic/vga666).

Beachten Sie, dass die Anweisungen in der Dokumentation im obigen GitHub-Link etwas veraltet sind. Verwenden Sie daher bitte die folgenden Einstellungen.

```
dtoverlay=vga666
enable_dpi_lcd=1
display_default_lcd=1
dpi_group=2
dpi_mode=82
```

### 800x480 LCD-Panel

Hinweis: Dies wurde mit [DPI-Add-On-Board und 800x480-LCD-Panel von Adafruit](https://www.adafruit.com/products/2453) getestet.

```
dtoverlay=dpi24
overscan_left=0
overscan_right=0
overscan_top=0
overscan_bottom=0
framebuffer_width=800
framebuffer_height=480
enable_dpi_lcd=1
display_default_lcd=1
dpi_group=2
dpi_mode=87
dpi_output_format=0x6f005
dpi_timings=800 0 40 48 88 480 0 13 3 32 0 0 0 60 0 32000000 6
```