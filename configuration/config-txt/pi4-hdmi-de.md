# Raspberry Pi 4 HDMI-Pipeline

Um Dual-Displays und Modi bis zu 4k60 zu unterstützen, hat der Raspberry Pi 4 die Hardware der HDMI-Kompositionspipeline auf verschiedene Weise aktualisiert. Eine der wichtigsten Änderungen besteht darin, dass für jeden Taktzyklus 2 Ausgabepixel generiert werden.

Jeder HDMI-Modus verfügt über eine Liste von Timings, die alle Parameter rund um die Dauer der Synchronisierungsimpulse steuern. Diese werden typischerweise über einen Pixeltakt und dann eine Anzahl aktiver Pixel, eine vordere Schwarzschulter, einen Synchronisationsimpuls und eine hintere Schwarzschulter für jede der horizontalen und vertikalen Richtungen definiert.

Alles mit 2 Pixeln pro Takt zu betreiben bedeutet, dass der Pi4 kein Timing unterstützen kann, bei dem _beliebige_ der horizontalen Timings nicht durch 2 teilbar sind. Die Firmware und der Linux-Kernel filtern jeden Modus heraus, der diese Kriterien nicht erfüllt.

Es gibt nur einen Modus in den CEA- und DMT-Standards, der in diese Kategorie fällt - DMT-Modus 81, der 1366 x 768 @ 60 Hz beträgt. Dieser Modus hat ungerade Werte für die Timings der horizontalen Synchronisation und der hinteren Schwarzschulter. Es ist auch ein ungewöhnlicher Modus für eine Breite, die nicht durch 8 teilbar ist.

Wenn Ihr Monitor diese Auflösung hat, fällt der Pi4 automatisch in den nächsten Modus, der vom Monitor angekündigt wird; Dies ist normalerweise 1280 x 720.

Bei einigen Monitoren ist es möglich, sie so zu konfigurieren, dass sie 1360x768 @ 60Hz verwenden. Sie geben diesen Modus normalerweise nicht über ihre EDID bekannt, sodass die Auswahl nicht automatisch erfolgen kann, sondern manuell durch Hinzufügen ausgewählt werden kann

```
hdmi_group=2
hdmi_mode=87
hdmi_cvt=1360 768 60
```
in [config.txt](./video.md).

Timings, die manuell über eine `hdmi_timings=`-Zeile in `config.txt` angegeben werden, müssen auch die Einschränkung erfüllen, dass alle horizontalen Timing-Parameter durch 2 teilbar sind.

`dpi_timings=` sind nicht auf die gleiche Weise eingeschränkt, da diese Pipeline immer noch nur mit einem einzigen Pixel pro Taktzyklus läuft.