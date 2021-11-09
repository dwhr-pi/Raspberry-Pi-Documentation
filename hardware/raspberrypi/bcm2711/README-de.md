# BCM2711

Dies ist der Broadcom-Chip, der im Raspberry Pi 4 Model B verwendet wird. Die Architektur des BCM2711 ist eine erhebliche Verbesserung gegenüber der von den SoCs in früheren Pi-Modellen verwendeten. Er setzt das Quad-Core-CPU-Design des BCM2837 fort, verwendet aber den leistungsstärkeren ARM-A72-Kern. Es verfügt über einen stark verbesserten GPU-Funktionssatz mit viel schnellerer Ein-/Ausgabe aufgrund der Integration eines PCIe-Links, der die USB 2- und USB 3-Ports verbindet, und eines nativ angeschlossenen Ethernet-Controllers. Es ist auch in der Lage, mehr Speicher zu adressieren als die bisher verwendeten SoCs.

Die ARM-Kerne können mit bis zu 1,5 GHz laufen, wodurch der Pi 4 etwa 50 % schneller ist als der Raspberry Pi 3B+. Die neue VideoCore VI 3D-Einheit läuft jetzt mit bis zu 500 MHz. Die ARM-Kerne sind 64-Bit, und während der VideoCore 32-Bit ist, gibt es eine neue Memory Management Unit, die bedeutet, dass sie auf mehr Speicher zugreifen kann als frühere Versionen.

Der BCM2711-Chip verwendet weiterhin die mit dem BCM2837B0 begonnene Wärmeverteilungstechnologie, die ein besseres Wärmemanagement bietet.

Ein Datenblatt für den BCM2711 finden Sie [hier](https://datasheets.raspberrypi.org/bcm2711/bcm2711-peripherals.pdf).

## Einige technische Details

**Prozessor:** Quad-Core Cortex-A72 (ARM v8) 64-Bit-SoC bei 1,5 GHz. Weitere Informationen finden Sie auf der [Wikipedia-Seite](https://en.wikipedia.org/wiki/ARM_Cortex-A72) auf der A72.

**Speicher:** Zugriff auf bis zu 4GB LPDDR4-2400 SDRAM (je nach Modell)

**Caches:** 32 KB Daten + 48 KB Befehls-L1-Cache pro Kern. 1 MB L2-Cache.

**Multimedia:** H.265 (4Kp60-Dekodierung); H.264 (1080p60-Dekodierung, 1080p30-Kodierung); OpenGL ES, 3.0-Grafik

**I/O:** PCIe-Bus, integrierter Ethernet-Port, 2 × DSI-Ports (nur einer auf Raspberry Pi 4B freigelegt), 2 × CSI-Ports (nur einer auf Raspberry Pi 4B freigelegt), bis zu 6 × I2C, up bis 6 × UART (muxed mit I2C), bis zu 6 × SPI (nur fünf auf Raspberry Pi 4B belichtet), dualer HDMI-Videoausgang, Composite-Videoausgang.


Informationen zu den vorherigen Raspberry Pi-Chips finden Sie in den folgenden Dokumentationsabschnitten:

* Raspberry Pi 3+ Chip [BCM2837B0](../bcm2837b0/README.md)
* Raspberry Pi 3 Chip [BCM2837](../bcm2837/README.md)
* Raspberry Pi 2 Chip [BCM2836](../bcm2836/README.md)
* Raspberry Pi 1-Chip [BCM2835](../bcm2835/README.md)