# BCM2837B0

Dies ist der Broadcom-Chip, der im Raspberry Pi 3B+ und 3A+ verwendet wird. Die zugrunde liegende Architektur des BCM2837B0 ist identisch mit dem BCM2837A0-Chip, der in anderen Versionen des Pi verwendet wird. Die ARM-Core-Hardware ist die gleiche, nur die Frequenz wird höher bewertet.

Die ARM-Kerne können mit bis zu 1,4 GHz laufen, wodurch der 3B+/3A+ etwa 17 % schneller ist als der ursprüngliche Raspberry Pi 3. Der VideoCore IV läuft mit 400 MHz. Der ARM-Kern ist 64-Bit, während der VideoCore IV 32-Bit ist.

Der BCM2837B0-Chip ist etwas anders verpackt als der BCM2837A0 und enthält vor allem einen Wärmeverteiler für eine bessere Thermik. Diese ermöglichen höhere Taktfrequenzen (oder den Betrieb mit niedrigeren Spannungen, um den Stromverbrauch zu reduzieren) und eine genauere Überwachung und Steuerung der Temperatur des Chips.

[Dieser Beitrag im Raspberry Pi-Blog](https://www.raspberrypi.org/blog/raspberry-pi-3-model-bplus-sale-now-35/) geht näher auf den BCM2837B0-Chip ein.

Informationen zu den bisherigen Raspberry Pi-Chips finden Sie auch in den folgenden Dokumenten:

* Raspberry Pi 3 Chip [BCM2837](../bcm2837/README.md)
* Raspberry Pi 2 Chip [BCM2836](../bcm2836/README.md)
* Raspberry Pi 1-Chip [BCM2835](../bcm2835/README.md)