# Kameramodul

Die Raspberry Pi Kameramodule sind offizielle Produkte der Raspberry Pi Foundation. Das ursprüngliche 5-Megapixel-Modell wurde 2013 [veröffentlicht](https://www.raspberrypi.org/blog/camera-board-available-for-sale/) und ein 8-Megapixel-[Camera Module v2](https://www.raspberrypi.org/products/camera-module-v2/) wurde [released](https://www.raspberrypi.org/blog/new-8-megapixel-camera-board-sale-25/) in 2016. Für beide Iterationen gibt es Versionen mit sichtbarem Licht und Infrarot. Eine 12 Megapixel [Hochwertige Kamera](https://www.raspberrypi.org/products/raspberry-pi-high-quality-camera/) wurde [veröffentlicht](https://www.raspberrypi.org/blog/new-product-raspberry-pi-high-quality-camera-on-sale-now-at-50/) im Jahr 2020. Es gibt keine Infrarot-Version der HQ-Kamera, jedoch der [IR-Filter kann entfernt werden](hqcam_filter_removal.md) bei Bedarf.

## Hardware-Spezifikation

| | Kameramodul v1 | Kameramodul v2 | HQ-Kamera |
| --- | --- | --- | --- |
| Nettopreis | $25 | $25 | $50 |
| Größe | ca. 25 × 24 × 9 mm | | 38 x 38 x 18,4 mm (ohne Objektiv) |
| Gewicht | 3g | 3g | |
| Standbildauflösung | 5 Megapixel | 8 Megapixel | 12,3 Megapixel |
| Videomodi | 1080p30, 720p60 und 640 × 480p60/90 | 1080p30, 720p60 und 640 × 480p60/90 | 1080p30, 720p60 und 640 × 480p60/90 |
| Linux-Integration | V4L2-Treiber verfügbar | V4L2-Treiber verfügbar | V4L2-Treiber verfügbar |
| C-Programmier-API | OpenMAX IL und andere verfügbar | OpenMAX IL und andere verfügbar | |
| Sensor | OmniVision OV5647 | Sony IMX219 | [Sony IMX477](https://www.sony-semicon.co.jp/products/common/pdf/IMX477-AACK_Flyer.pdf) |
| Sensorauflösung | 2592 × 1944 Pixel | 3280 × 2464 Pixel | 4056 x 3040 Pixel|
| Sensorbildbereich | 3,76 × 2,74 mm | 3,68 x 2,76 mm (4,6 mm Diagonale) | 6,287 mm x 4,712 mm (7,9 mm Diagonale) |
| Pixelgröße | 1,4 µm × 1,4 µm | 1,12 µm x 1,12 µm | 1,55 µm x 1,55 µm |
| Optische Größe | 1/4" | 1/4" | |
| Vollformat-SLR-Objektiv-Äquivalent | 35 mm | | |
| S/N-Verhältnis | 36 dB | | |
| Dynamikbereich | 67 dB @ 8x Verstärkung | | |
| Empfindlichkeit | 680 mV/lux-sek | | |
| Dunkelstrom | 16 mV/s bei 60 °C | | |
| Brunnenkapazität | 4.3 Ke- | | |
| Fester Fokus | 1 m bis unendlich | | Nicht zutreffend |
| Brennweite | 3,60 mm +/- 0,01 | 3,04 mm | Abhängig vom Objektiv |
| Horizontales Sichtfeld | 53,50 +/- 0,13 Grad | 62,2 Grad | Abhängig vom Objektiv|
| Vertikales Sichtfeld | 41,41 +/- 0,11 Grad | 48,8 Grad | Abhängig vom Objektiv |
| Brennweite (F-Stop) | 2.9 | 2,0 | Abhängig vom Objektiv |

## Hardwarefunktionen

| Verfügbar | Implementiert |
| --- | --- |
| Hauptstrahlwinkelkorrektur | Ja |
| Global und Rolling Shutter | Rolling Shutter |
| Automatische Belichtungssteuerung (AEC) | Nein - wird stattdessen vom ISP durchgeführt |
| Automatischer Weißabgleich (AWB) | Nein - wird stattdessen vom ISP durchgeführt |
| Automatische Schwarzwertkalibrierung (ABLC) | Nein - wird stattdessen vom ISP durchgeführt |
| Automatische 50/60 Hz Luminanzerkennung | Nein - wird stattdessen vom ISP durchgeführt |
| Bildrate bis zu 120 fps | Maximal 90fps. Einschränkungen der Bildgröße für die höheren Bildraten (VGA nur für über 47 fps) |
| AEC/AGC 16-Zonen-Größen-/Positions-/Gewichtskontrolle | Nein - wird stattdessen vom ISP durchgeführt |
| Spiegeln und umdrehen | Ja |
| Zuschneiden | Nein - wird stattdessen vom ISP durchgeführt (außer 1080p-Modus) |
| Objektivkorrektur | Nein - wird stattdessen vom ISP durchgeführt |
| Fehlerhafte Pixelunterdrückung | Nein - wird stattdessen vom ISP durchgeführt |
| 10-Bit-RAW-RGB-Daten | Ja - Formatkonvertierungen über GPU verfügbar |
| Unterstützung für LED- und Blitz-Strobe-Modus | LED-Blitz |
| Unterstützung für interne und externe Bildsynchronisation für den Bildbelichtungsmodus | Nein |
| Unterstützung für 2 × 2 Binning für besseres SNR bei schlechten Lichtverhältnissen | Alles, was unter 1296 x 976 ausgegeben wird, verwendet den 2 x 2 Binned-Modus |
| Unterstützung für horizontales und vertikales Subsampling | Ja, über Binning und Überspringen |
| On-Chip-Phasenregelschleife (PLL) | Ja |
| Standard serielle SCCB-Schnittstelle | Ja |
| Digitaler Videoanschluss (DVP) parallele Ausgangsschnittstelle | Nein |
| MIPI-Schnittstelle (zwei Spuren) | Ja |
| 32 Byte eingebetteter, einmalig programmierbarer (OTP) Speicher | Nein |
| Eingebetteter 1,5-V-Regler für Kernleistung | Ja |

## Software-Features

Die vollständige Dokumentation zur Kamerasoftware finden Sie [hier](../../raspbian/applications/camera.md).

| | |
| --- | --- |
| Bildformate | JPEG (beschleunigt), JPEG + RAW, GIF, BMP, PNG, YUV420, RGB888 |
| Videoformate | raw h.264 (beschleunigt) |
| Effekte | Negativ, Solarisieren, Posterisieren, Whiteboard, Tafel, Skizze, Entrauschen, Prägen, Ölfarbe, Schraffur, Gpen, Pastell, Aquarell, Film, Unschärfe, Sättigung |
| Belichtungsmodi |Auto, Nacht, Nachtvorschau, Hintergrundbeleuchtung, Scheinwerfer, Sport, Schnee, Strand, sehr lang, feste Bilder pro Sekunde, Anti-Shake, Feuerwerk |
| Messmodi | Durchschnitt, Spot, Hintergrundbeleuchtung, Matrix |
| Automatischer Weißabgleich | Aus, Auto, Sonne, Wolken, Schatten, Kunstlicht, Leuchtstoffröhren, Glühlampen, Blitz, Horizont |
| Auslöser | Tastendruck, UNIX-Signal, Zeitüberschreitung |
| Zusätzliche Modi | Demo, Explosion/Zeitraffer, Ringspeicher, Video mit Bewegungsvektoren, segmentiertes Video, Live-Vorschau auf 3D-Modellen |

## Informationen zur Übertragung des HQ-Kamera-IR-Filters

Die HQ-Kamera verwendet einen Hoya CM500 Infrarotfilter. Seine Übertragungseigenschaften sind in der folgenden Grafik dargestellt.

![CM500 Transmissionsdiagramm](./hoyacm500.png)

## Technische Zeichnung

- Kameramodul v2 [PDF](mechanisch/rpi_MECH_Camera2_2p1.pdf)
- HQ-Kameramodul [PDF](mechanisch/rpi_MECH_HQcamera_1p0.pdf)
- HQ Camera Module Objektivfassung[PDF](mechanisch/rpi_MECH_HQcamera_lensmount_1p0.pdf)
  
## Schema

- Kameramodul v2 [PDF](schematics/rpi_SCH_Camera2_2p1.pdf)
- HQ-Kameramodul [PDF](schematics/rpi_SCH_HQcamera_1p0.pdf)