# CSI-2 (Kamera serielle Schnittstelle 2) "Unicam"

Die in der Raspberry Pi-Reihe verwendeten SoCs verfügen alle über zwei Kameraschnittstellen, die entweder CSI-2 D-PHY 1.1- oder CCP2-Quellen (Compact Camera Port 2) unterstützen. Diese Schnittstelle ist unter dem Codenamen "Unicam" bekannt. Die erste Instanz von Unicam unterstützt 2 CSI-2-Datenspuren, während die zweite 4 unterstützt. Jede Spur kann mit bis zu 1 Gbit/s (DDR, die maximale Verbindungsfrequenz beträgt also 500 MHz) laufen.

Die normalen Varianten des Raspberry Pi legen jedoch nur die zweite Instanz frei und leiten *nur* 2 der Datenspuren zum Kameraanschluss. Die Compute Module-Reihe leitet alle Lanes von beiden Peripheriegeräten weiter.

## Software-Schnittstellen

Für die Kommunikation mit dem Unicam-Peripheriegerät stehen 3 unabhängige Softwareschnittstellen zur Verfügung:

### Firmware

Die Closed-Source-GPU-Firmware enthält Treiber für Unicam und drei Kamerasensoren sowie einen Bridge-Chip. Dies sind die Raspberry Pi Camera v1.3 (Omnivision OV5647), Raspberry Pi Camera v2.1 (Sony IMX219), Raspberry Pi HQ Camera (Sony IMX477) und ein nicht unterstützter Treiber für den Toshiba TC358743 HDMI->CSI2 Bridge-Chip.

Dieser Treiber integriert den Quelltreiber, Unicam, ISP und Tuner-Steuerung in einen vollständigen Kamerastapel, der verarbeitete Ausgabebilder liefert. Es kann über MMAL, OpenMAX IL und V4L2 mit dem Kernelmodul bcm2835-v4l2 verwendet werden. Über diese Schnittstelle werden nur Raspberry Pi Kameras unterstützt.

### MMAL Rawcam-Komponente

Dies war eine Übergangsoption, bevor der V4L2-Treiber verfügbar war. Die MMAL-Komponente `vc.ril.rawcam` ermöglicht den Empfang der CSI2-Rohdaten auf die gleiche Weise wie der V4L2-Treiber, aber die gesamte Quellkonfiguration muss vom Benutzerland über eine beliebige Schnittstelle durchgeführt werden, die die Quelle benötigt. Die Raspiraw-Anwendung ist auf [github] (https://github.com/raspberrypi/raspiraw) verfügbar. Es verwendet diese Komponente und die Standard-I2C-Registersätze für OV5647, IMX219 und ADV7282M, um Streaming zu unterstützen.


###V4L2

Für den Unicam-Block ist ein vollständig quelloffener Kernel-Treiber verfügbar; Dies ist ein Kernelmodul namens bcm2835-unicam. Dies ist eine Schnittstelle zu V4L2-Subdevice-Treibern für die Quelle, um die Rohframes zu liefern. Dieser bcm2835-unicam-Treiber steuert den Sensor und konfiguriert den CSI-2-Empfänger so, dass das Peripheriegerät die Rohframes (nach Debayer) in SDRAM für V4L2 schreibt, um sie an Anwendungen zu liefern. Abgesehen von dieser Möglichkeit, die CSI-2 Bayer-Formate auf 16 Bit/Pixel zu entpacken, findet keine Bildverarbeitung zwischen der Bildquelle (z.
```
|------------------------|
|     bcm2835-unicam     |
|------------------------|
     ^             |
     |      |-------------|
 img |      |  Subdevice  |
     |      |-------------|
     v   -SW/HW-   |
|---------|   |-------------|
| Unicam  |   | I2C oder SPI|
|---------|   |-------------|
csi2/ ^             |
ccp2  |             |
    |-----------------|
    |     Sensor      |
    |-----------------|
```

Mainline Linux verfügt über eine Reihe vorhandener Treiber. Der Raspberry Pi-Kernelbaum verfügt über einige zusätzliche Treiber und Gerätebaum-Overlays, um sie zu konfigurieren, die alle getestet und als funktionierend bestätigt wurden. Sie beinhalten:

| Gerät | Typ | Anmerkungen |
| :----- | :--- | :---- |
| Omnivision OV5647 | 5MP-Kamera | Original Raspberry Pi Kamera |
| Sony IMX219 | 8MP Kamera | Revision 2 Raspberry Pi Kamera |
| Sony IMX477 | 12MP Kamera | Raspberry Pi HQ-Kamera |
| Toshiba TC358743 | HDMI-zu-CSI-2-Brücke | |
| Analog Devices ADV728x-M | Analoges Video zu CSI-2-Brücke| Keine Interlaced-Unterstützung |
| Infineon IRS1125 | Laufzeit-Tiefensensor| Unterstützt von einem Dritten |

Da der Subdevice-Treiber auch ein Kernel-Treiber mit einer standardisierten API ist, können Drittanbieter ihre eigenen für jede Quelle ihrer Wahl schreiben.

## Entwicklung eines Drittanbieter-Treibers für bcm2835-unicam

Dies ist der empfohlene Ansatz für die Verbindung über Unicam.

Wenn Sie einen Treiber für ein neues Gerät entwickeln, das mit dem bcm2835-unicam-Modul verwendet werden soll, benötigen Sie den Treiber und die entsprechenden Gerätebaum-Overlays. Idealerweise sollte der Treiber an die Mailingliste [linux-media](http://vger.kernel.org/vger-lists.html#linux-media) zur Codeüberprüfung und zum Einfügen in die Mainline gesendet und dann in die [Raspberry Pi-Kernel-Baum](https://github.com/raspberrypi/linux), aber es können Ausnahmen gemacht werden, damit der Treiber überprüft und direkt mit dem Raspberry Pi-Kernel zusammengeführt wird.

Bitte beachten Sie, dass alle Kernel-Treiber unter der GPLv2-Lizenz lizenziert sind, daher **MUSS** Quellcode verfügbar sein. Nur der Versand von Binärmodulen stellt einen Verstoß gegen die GPLv2-Lizenz dar, unter der der Linux-Kernel lizenziert ist.

Die bcm2835-unicam wurde geschrieben, um alle Arten von CSI-2-Quelltreibern, wie sie derzeit im Mainline-Linux-Kernel zu finden sind, unterzubringen. Grob lassen sich diese in Kamerasensoren und Brückenchips unterteilen. Bridge-Chips ermöglichen die Konvertierung zwischen einem anderen Format und CSI-2.

### Kamerasensoren

Der Sensortreiber für einen Kamerasensor ist für die gesamte Konfiguration des Geräts verantwortlich, normalerweise über I2C oder SPI. Anstatt einen Treiber von Grund auf neu zu schreiben, ist es oft einfacher, einen vorhandenen Treiber als Grundlage zu nehmen und ihn entsprechend zu modifizieren.

Der IMX219-Treiber ist ein guter Ausgangspunkt, die im 5.4-Kernel gefundene Version findet sich [hier](https://github.com/raspberrypi/linux/blob/rpi-5.4.y/drivers/media/i2c/imx219 .C). Dieser Treiber unterstützt sowohl die 8-Bit- als auch die 10-Bit-Bayer-Auslesung, so dass das Aufzählen von Frame-Formaten und Frame-Größen etwas komplizierter ist.

Sensoren unterstützen im Allgemeinen V4L2-Benutzerkontrollen, die [hier] (https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/control.html) dokumentiert sind. Nicht alle diese Steuerelemente müssen in einem Treiber implementiert werden. Der IMX219-Treiber implementiert nur eine kleine Untermenge, die unten aufgeführt ist und deren Implementierung von der Funktion `imx219_set_ctrl` übernommen wird.

- `V4L2_CID_PIXEL_RATE` / `V4L2_CID_VBLANK` / `V4L2_CID_HBLANK`: ermöglicht der Anwendung, die Bildrate einzustellen.
- `V4L2_CID_EXPOSURE`: setzt die Belichtungszeit in Zeilen. Die Anwendung muss `V4L2_CID_PIXEL_RATE`, `V4L2_CID_HBLANK` und die Framebreite verwenden, um die Zeilenzeit zu berechnen.
- `V4L2_CID_ANALOGUE_GAIN`: analoge Verstärkung in sensorspezifischen Einheiten.
- `V4L2_CID_DIGITAL_GAIN`: optionale digitale Verstärkung in sensorspezifischen Einheiten.
- `V4L2_CID_HFLIP / V4L2_CID_VFLIP`: Spiegelt das Bild entweder horizontal oder vertikal. Beachten Sie, dass dieser Vorgang die Bayer-Reihenfolge der Daten im Frame ändern kann, wie dies beim imx219 der Fall ist.
- `V4L2_CID_TEST_PATTERN` / `V4L2_CID_TEST_PATTERN_*`: Ermöglicht die Ausgabe verschiedener Testmuster vom Sensor. Nützlich zum Debuggen.

Im Fall des IMX219 werden viele dieser Steuerelemente direkt auf Registerschreibvorgänge des Sensors selbst abgebildet.

Der Gerätebaum wird verwendet, um den Sensortreiber auszuwählen und zu konfigurieren
Parameter wie Anzahl CSI-2 Lanes, Continuous Clock Lane
Betrieb und Verbindungsfrequenz (oft wird nur eine unterstützt). Das IMX219-Gerätebaum-Overlay für den 5.4-Kernel finden Sie [hier](https://github.com/raspberrypi/linux/blob/rpi-5.4.y/arch/arm/boot/dts/overlays/imx219-overlay. dt)

### Brückenchips

Dies sind Geräte, die einen eingehenden Videostream, zum Beispiel HDMI oder Composite, in einen CSI-2-Stream umwandeln, der vom Raspberry Pi CSI-2-Empfänger akzeptiert werden kann.

Der Umgang mit Bridge-Chips ist komplizierter, da sie im Gegensatz zu Kamerasensoren auf das eingehende Signal reagieren und dies an die Anwendung melden müssen.

Die Mechanismen zur Handhabung von Brückenchips können grob in analog oder digital unterteilt werden.

Bei der Verwendung von `ioctls` in den folgenden Abschnitten bedeutet ein `_S_` im `ioctl`-Namen, dass es sich um eine Set-Funktion handelt, während `_G_` eine Get-Funktion ist und `_ENUM` eine Menge zulässiger Werte aufzählt.

#### Analoge Videoquellen

Analoge Videoquellen verwenden den Standard `ioctls` zum Erkennen und Einstellen von Videostandards. :[`VIDIOC_G_STD`](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/vidioc-g-std.html), [`VIDIOC_S_STD`](https:// www.kernel.org/doc/html/latest/userspace-api/media/v4l/vidioc-g-std.html), [`VIDIOC_ENUMSTD`](https://www.kernel.org/doc/html/latest /userspace-api/media/v4l/vidioc-enumstd.html) und [`VIDIOC_QUERYSTD`](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/vidioc-querystd .html)

 Die Auswahl des falschen Standards führt im Allgemeinen zu beschädigten Bildern. Durch das Festlegen des Standards wird normalerweise auch die Auflösung in der V4L2 CAPTURE-Warteschlange festgelegt. Er kann nicht über `VIDIOC_S_FMT` eingestellt werden. Generell ist es sinnvoll, den erkannten Standard über `VIDIOC_QUERYSTD` anzufordern und dann vor dem Streamen mit `VIDIOC_S_STD` zu setzen.

#### Digitale Videoquellen

Für digitale Videoquellen wie HDMI gibt es einen alternativen Satz von Aufrufen, mit denen alle digitalen Timing-Parameter angegeben werden können ([`VIDIOC_G_DV_TIMINGS`](https://www.kernel.org/doc/html/latest/userspace- api/media/v4l/vidioc-g-dv-timings.html), [`VIDIOC_S_DV_TIMINGS`](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/vidioc-g -dv-timings.html), [`VIDIOC_ENUM_DV_TIMINGS`](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/vidioc-enum-dv-timings.html) und [`VIDIOC_QUERY_DV_TIMINGS`](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/vidioc-query-dv-timings.html)).

Wie bei analogen Bridges fixieren die Timings normalerweise die V4L2 CAPTURE-Warteschlangenauflösung und das Aufrufen von `VIDIOC_S_DV_TIMINGS` mit dem Ergebnis von `VIDIOC_QUERY_DV_TIMINGS` vor dem Streaming sollte sicherstellen, dass das Format korrekt ist.

Je nach Bridge-Chip und Treiber können Änderungen der Eingangsquelle über `VIDIOC_SUBSCRIBE_EVENT` und `V4L2_EVENT_SOURCE_CHANGE` an die Applikation gemeldet werden.

#### Derzeit unterstützte Geräte

Es gibt 2 Bridge-Chips, die derzeit vom Rasberry Pi Linux-Kernel unterstützt werden, den Analog Devices ADV728x-M für analoge Videoquellen und den Toshiba TC358743 für HDMI-Quellen.


*Analoge Geräte ADV728x(A)-M Analoges Video zu CSI2-Brücke*

Diese Chips konvertieren Composite-, S-Video (Y/C) oder Component (YPrPb) Video in eine einspurige CSI-2-Schnittstelle und werden vom [ADV7180 Kernel-Treiber] (https://github.com/raspberrypi/ linux/blob/rpi-5.4.y/drivers/media/i2c/adv7180.c).

Produktdetails zu den verschiedenen Versionen dieses Chips finden Sie auf der Website von Analog Devices.

[ADV7280A](https://www.analog.com/en/products/adv7280a.html), [ADV7281A](https://www.analog.com/en/products/adv7281a.html), [ADV7282A]( https://www.analog.com/en/products/adv7282a.html)

Aufgrund von fehlendem Code in der aktuellen Core-V4L2-Implementierung schlägt die Auswahl der Quelle fehl, sodass die Raspberry Pi-Kernelversion einen Kernelmodulparameter namens `dbg_input` zum ADV7180-Kerneltreiber hinzufügt, der die Eingabequelle jedes Mal festlegt, wenn VIDIOC_S_STD aufgerufen wird. Irgendwann wird Mainstream das zugrunde liegende Problem beheben (eine Trennung zwischen dem Kernel-API-Aufruf s_routing und dem Userspace-Aufruf `VIDIOC_S_INPUT`) und diese Modifikation wird entfernt.

Bitte beachten Sie, dass der Empfang von Interlaced-Video nicht unterstützt wird, daher ist die ADV7281(A)-M-Version des Chips von begrenztem Nutzen, da sie nicht über den erforderlichen I2P-Deinterlacing-Block verfügt. Stellen Sie außerdem sicher, dass Sie bei der Auswahl eines Geräts die Option -M angeben. Andernfalls erhalten Sie einen parallelen Ausgangsbus, der nicht an den Raspberry Pi angeschlossen werden kann.

Es sind keine kommerziell erhältlichen Boards bekannt, die diese Chips verwenden, aber dieser Treiber wurde über das Analog Devices [EVAL-ADV7282-M Evaluation Board] (https://www.analog.com/en/design-center/evaluation-hardware .) getestet -und-software/evaluation-boards-kits/EVAL-ADV7282A-M.html)

Dieser Treiber kann mit dem `config.txt` dtoverlay `adv7282m` geladen werden, wenn Sie die Chipvariante `ADV7282-M` verwenden; oder `adv728x-m` mit einem Parameter von entweder `adv7280m=1`, `adv7281m=1` oder `adv7281ma=1`, wenn Sie eine andere Variante verwenden. z.B.
```
dtoverlay=adv728x-m,adv7280m=1
```

*Toshiba TC358743 HDMI-zu-CSI2-Brücke*

Dies ist ein HDMI-zu-CSI-2-Brückenchip, der Videodaten mit bis zu 1080p60 konvertieren kann.

Informationen zu diesem Bridge-Chip finden Sie auf der [Toshiba-Website](https://toshiba.semicon-storage.com/ap-en/semiconductor/product/interface-bridge-ics-for-mobile-peripheral-devices/hdmir -interface-bridge-ics/detail.TC358743XBG.html)

Der TC358743 verbindet HDMI mit CSI-2- und I2S-Ausgängen. Es wird vom [TC358743 Kernelmodul](https://github.com/raspberrypi/linux/blob/rpi-5.4.y/drivers/media/i2c/tc358743.c) unterstützt.

Der Chip unterstützt eingehende HDMI-Signale als RGB888, YUV444 oder YUV422 mit bis zu 1080p60. Es kann RGB888 weiterleiten oder in YUV444 oder YUV422 konvertieren und in beide Richtungen zwischen YUV444 und YUV422 konvertieren. Nur RGB888- und YUV422-Unterstützung wurde getestet. Bei Verwendung von 2 CSI-2-Lanes sind die maximal unterstützten Raten 1080p30 als RGB888 oder 1080p50 als YUV422. Bei Verwendung von 4 Lanes auf einem Compute-Modul kann 1080p60 in beiden Formaten empfangen werden.

HDMI handelt die Auflösung durch ein empfangendes Gerät aus, das eine [EDID](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data) aller unterstützten Modi ankündigt. Der Kernel-Treiber kennt keine Auflösungen, Bildraten oder Formate, die Sie erhalten möchten, daher liegt es am Benutzer, eine geeignete Datei bereitzustellen.
Dies geschieht über VIDIOC_S_EDID ioctl oder einfacher mit `v4l2-ctl --fix-edid-checksums --set-edid=file=filename.txt` (das Hinzufügen der Option --fix-edid-checksums bedeutet, dass Sie 'müssen die Prüfsummenwerte in der Quelldatei nicht korrekt erhalten). Das Generieren der erforderlichen EDID-Datei (ein textueller Hexdump einer binären EDID-Datei) ist nicht allzu mühsam, und es stehen Tools für deren Generierung zur Verfügung, die jedoch den Rahmen dieser Seite sprengen würden.

Verwenden Sie wie oben beschrieben die ioctls `DV_TIMINGS`, um den Treiber so zu konfigurieren, dass er dem eingehenden Video entspricht. Am einfachsten geht das mit dem Befehl `v4l2-ctl --set-dv-bt-timings query`. Der Treiber unterstützt das Generieren der SOURCE_CHANGED-Ereignisse, wenn Sie eine Anwendung schreiben möchten, die eine sich ändernde Quelle verarbeitet. Das Ändern des Ausgabepixelformats wird durch Einstellen über VIDIOC_S_FMT erreicht, jedoch wird nur das Pixelformatfeld aktualisiert, da die Auflösung durch die dv-Timings konfiguriert wird.

Es gibt ein paar im Handel erhältliche Boards, die diesen Chip mit dem Raspberry Pi verbinden. Die Auvidea B101 und B102 sind die am weitesten verbreiteten, aber auch andere gleichwertige Boards sind erhältlich.

Dieser Treiber wird mit dem `config.txt` dtoverlay `tc358743` geladen.

Der Chip unterstützt auch die Aufnahme von Stereo-HDMI-Audio über I2S. Die Auvidea-Boards brechen die relevanten Signale auf einen Header auf, der mit dem 40-Pin-Header des Pi verbunden werden kann. Die erforderliche Verkabelung ist:

| Signal   | B101-Header | Pi 40 Stiftleiste | BCM-GPIO |
|----------|:-----------:|:-----------------:|:--------:|
| LRCK/WFS |     7       |       35          |    19    |
| BCK/SCK  |     6       |       12          |    18    |
| DATA/SD  |     5       |       38          |    20    |
| GND      |     8       |       39          |    N/A   |

Das Overlay `tc358743-audio` wird *zusätzlich* zum Overlay `tc358743` benötigt. Dies sollte ein ALSA-Aufnahmegerät für das HDMI-Audio erstellen.
Bitte beachten Sie, dass es kein Resampling des Audios gibt. Das Vorhandensein von Audio wird im V4L2-Regler TC358743_CID_AUDIO_PRESENT / "audio-present" widergespiegelt und die Abtastrate des eingehenden Audios wird im V4L2-Regler TC358743_CID_AUDIO_SAMPLING_RATE / "Audio sampling-frequency" widergespiegelt. Bei einer Aufnahme ohne Ton werden Warnungen ausgegeben, ebenso wie bei einer Aufnahme mit einer anderen Abtastrate als der gemeldeten.