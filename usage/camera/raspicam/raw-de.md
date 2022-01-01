# Verwenden von RAW auf den Raspberry Pi-Kameras

Die Definition von Rohbildern kann variieren. Die übliche Bedeutung sind Bayer-Rohdaten direkt vom Sensor, obwohl einige ein unkomprimiertes Bild, das den ISP durchlaufen hat (und daher verarbeitet wurde), als Rohdaten betrachten.

Beide Optionen sind von den Raspberry Pi-Kameras verfügbar.

## Verarbeitete, verlustfreie Bilder

Die übliche Ausgabe von `raspistill` ist eine komprimierte JPEG-Datei, die alle Stufen der Bildverarbeitung durchlaufen hat, um ein qualitativ hochwertiges Bild zu erzeugen. JPEG, da es sich um ein verlustbehaftetes Format handelt, wirft jedoch einige Informationen weg, die der Benutzer möglicherweise haben möchte.

`raspistill` hat eine `encoding`-Option, mit der Sie das Ausgabeformat festlegen können: entweder `jpg`, `gif`, `bmp` oder `png`. Die beiden letztgenannten sind verlustfrei, sodass keine Daten weggeworfen werden, um die Komprimierung zu verbessern, sondern eine Konvertierung vom ursprünglichen YUV erforderlich ist, und da diese Formate keine Hardwareunterstützung bieten, produzieren sie Bilder etwas langsamer als JPEG.

z.B.

`raspistill --encoding png -o fred.png`

Eine andere Möglichkeit besteht darin, die Anwendung [`raspiyuv`](./raspiyuv.md) zu verwenden. Dies vermeidet jede letzte Formatierungsstufe und schreibt YUV420- oder RGB888-Rohdaten in die angeforderte Datei. YUV420 ist das Format, das in vielen ISPs verwendet wird, daher kann dies als ein Dump der verarbeiteten Bilddaten am Ende der ISP-Verarbeitung betrachtet werden.

## Unbearbeitete Bilder

Für einige Anwendungen, wie beispielsweise Astrofotografie, kann es nützlich sein, die Bayer-Rohdaten direkt vom Sensor zu haben. Diese Daten müssen nachbearbeitet werden, um ein brauchbares Bild zu erzeugen.

`raspistill` hat eine raw-Option, die diese Bayer-Rohdaten am Ende der JPEG-Ausgabedatei anhängt.

`raspistill --raw -o fred.jpg`

Die Rohdaten müssen aus der `JPEG`-Datei extrahiert werden. Informationen dazu finden Sie [hier](https://www.raspberrypi.org/blog/processing-raw-image-files-from-a-raspberry-pi-high-quality-camera/)