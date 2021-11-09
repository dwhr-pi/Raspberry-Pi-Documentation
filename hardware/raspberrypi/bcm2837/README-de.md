# BCM2837

Dies ist der Broadcom-Chip, der im Raspberry Pi 3 und in späteren Modellen des Raspberry Pi 2 verwendet wird. Die zugrunde liegende Architektur des BCM2837 ist identisch mit der des BCM2836. Der einzige signifikante Unterschied ist der Ersatz des ARMv7-Quad-Core-Clusters durch einen Quad-Core-ARM-Cortex-A53-Cluster (ARMv8).

Die ARM-Kerne laufen mit 1,2 GHz, wodurch das Gerät etwa 50% schneller ist als der Raspberry Pi 2. Der VideoCore IV läuft mit 400 MHz.

Einzelheiten zur ARM-Peripheriespezifikation, die auch für den BCM2837 gilt, entnehmen Sie bitte dem folgenden BCM2836-Dokument.

- [BCM2836 ARM-lokale Peripheriegeräte](../bcm2836/QA7_rev3.4.pdf)

Siehe auch den Chip des Raspberry Pi 2 [BCM2836](../bcm2836/README.md) und den Chip des Raspberry Pi 1 [BCM2835](../bcm2835/README.md).