# Pi4 Bootflow

Der Pi4 mit dem BCM2711 SoC verfügt über einen neuen, ausgefeilteren Boot-Prozess. Das Hinzufügen eines EEPROM bedeutet, dass die Datei `bootcode.bin`, die sich in `/boot` befindet, nicht mehr benötigt wird. Details zum EEPROM finden Sie [hier](../booteeprom.md).

Der Bootflow für den Pi4 ist wie folgt:

* BCM2711 SoC fährt hoch
* On-Board-Bootrom sucht nach der Bootloader-Wiederherstellungsdatei (recovery.bin) auf der SD-Karte. Wenn es gefunden wird, führt es es aus, um das EEPROM zu flashen und recovery.bin löst einen Reset aus.
* Andernfalls lädt das Bootrom den Haupt-Bootloader aus dem EEPROM.
* Der Bootloader überprüft das eingebaute BOOT_ORDER-Konfigurationselement, um zu bestimmen, welche Art von Boot ausgeführt werden soll.
  * SD-Karte
  * Netzwerk
  * USB Massenspeicher


## SD-Kartenstart
Der Bootloader lädt die Dateien im [Bootordner](../../../configuration/boot_folder.md) entsprechend den [Bootoptionen](../../../configuration/config-txt/ boot.md) in config.txt

## Netzwerkstart

Details zum Netzwerkbooten finden Sie [hier](../bcm2711_bootloader_config.md)

## USB-Massenspeicher booten

Das Booten über USB befindet sich noch in der Entwicklung.


## STARTREIHENFOLGE

Das Konfigurationselement `BOOT_ORDER` ist in den Bootloader-Code im EEPROM eingebettet. Weitere Informationen zum Ändern der Bootloader-Konfiguration finden Sie auf der Seite [Pi4 Bootloader Configuration](../bcm2711_bootloader_config.md).
