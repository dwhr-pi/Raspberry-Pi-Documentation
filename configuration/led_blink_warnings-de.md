## LED-Warn-Blinkcodes

Wenn ein Pi aus irgendeinem Grund nicht bootet oder herunterfahren muss, blinkt in vielen Fällen eine LED eine bestimmte Anzahl von Malen, um anzuzeigen, was passiert ist. Die LED blinkt für eine Reihe von langen Blinkzeichen (0 oder mehr) und dann kurzes Blinken, um den genauen Status anzuzeigen. In den meisten Fällen wiederholt sich das Muster nach einer Lücke von 2 Sekunden.

| Langes Blinken | Kurzes Blinken | Status |
|:------------:|:------------:|--------|
| 0 | 3 | Allgemeiner Fehler beim Booten |
| 0 | 4 | start*.elf nicht gefunden |
| 0 | 7 | Kernel-Image nicht gefunden |
| 0 | 8 | SDRAM-Fehler |
| 0 | 9 | Nicht genügend SDRAM |
| 0 | 10 | Im HALT-Zustand |
| 2 | 1 | Partition nicht FAT |
| 2 | 2 | Fehler beim Lesen von Partition |
| 2 | 3 | Erweiterte Partition nicht FAT |
| 2 | 4 | Dateisignatur/Hash-Nichtübereinstimmung - Pi 4 |
| 3 | 1 | SPI EEPROM-Fehler - Pi 4 |
| 3 | 2 | SPI EEPROM ist schreibgeschützt - Pi 4 |
| 4 | 4 | Nicht unterstützter Board-Typ |
| 4 | 5 | Schwerwiegender Firmware-Fehler |
| 4 | 6 | Stromausfall Typ A |
| 4 | 7 | Stromausfall Typ B |