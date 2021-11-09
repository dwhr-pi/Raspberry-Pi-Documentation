# Raspberry Pi Boot-Modi

## Einführung

Der Raspberry Pi hat eine Reihe von verschiedenen Phasen des Bootens. In diesem Dokument wird erläutert, wie die Bootmodi funktionieren und welche für das Booten von Linux unterstützt werden.

[Boot-Sequenz](bootflow.md)

[SD-Kartenstart](sdcard.md)

[USB boot](usb.md) umfasst die folgenden zwei Modi:
* [Gerätestart](device.md): Booten als Massenspeichergerät
* [Host boot](host.md): Booten als USB-Host mit einer der folgenden Möglichkeiten:
  * [Massenspeicherboot](msd.md): Booten vom Massenspeichergerät
  * [Netzwerkboot](net.md): Booten über Ethernet
  
[GPIO-Boot-Modus](gpio.md)
  
## Spezieller bootcode.bin-only Boot-Modus
USB-Host- und Ethernet-Boot können von BCM2837-basierten Raspberry Pis ausgeführt werden, d. Darüber hinaus können alle Raspberry Pi-Modelle **außer Pi 4B** eine neue bootcode.bin-only-Methode verwenden, um den USB-Host-Boot zu aktivieren.

**Hinweis:** Der Raspberry Pi 4B verwendet nicht die Datei bootcode.bin - stattdessen befindet sich der Bootloader in einem On-Board-EEPROM-Chip. Der Pi 4B Bootloader unterstützt derzeit nur das Booten von einer SD-Karte. Die Unterstützung für USB-Host-Modus-Boot und Ethernet-Boot wird durch ein zukünftiges Software-Update hinzugefügt. Siehe [Pi4 Bootflow](./bootflow_2711.md) und [SPI Boot EEPROM](../booteeprom.md).

Formatieren Sie eine SD-Karte als FAT32 und kopieren Sie sie auf die neueste [bootcode.bin](https://github.com/raspberrypi/firmware/raw/master/boot/bootcode.bin). Die SD-Karte muss im Pi vorhanden sein, damit er booten kann. Sobald bootcode.bin von der SD-Karte geladen wurde, fährt der Pi mit dem Booten im USB-Host-Modus fort.

Dies ist nützlich für die Raspberry Pi 1, 2 und Zero-Modelle, die auf den BCM2835- und BCM2836-Chips basieren, und in Situationen, in denen ein Pi 3 nicht booten kann (die neueste bootcode.bin enthält zusätzliche Bugfixes für den Pi 3B, im Vergleich auf den in den BCM2837A eingebrannten Bootcode.

Wenn Sie ein Problem mit einem Massenspeichergerät haben, das trotz dieser bootcode.bin immer noch nicht funktioniert, dann fügen Sie bitte eine neue Datei 'timeout' auf der SD-Karte hinzu. Dadurch wird die Zeit, die auf die Initialisierung des Massenspeichergeräts gewartet wird, auf sechs Sekunden verlängert.

## bootcode.bin UART aktivieren (Pre Raspberry Pi 4B)

Informationen zum Aktivieren des UART auf dem Pi4-Bootloader finden Sie auf [dieser Seite](../bcm2711_bootloader_config.md).

Es ist möglich, einen UART in einem frühen Stadium zu aktivieren, um Bootprobleme zu beheben (nützlich mit dem obigen Bootmodus bootcode.bin only). Stellen Sie dazu sicher, dass Sie über eine aktuelle Version der Firmware (einschließlich bootcode.bin) verfügen. So überprüfen Sie, ob UART in Ihrer aktuellen Firmware unterstützt wird:

```
$ strings bootcode.bin | grep BOOT_UART
BOOT_UART=0
```

Um UART aus bootcode.bin zu aktivieren, verwenden Sie:

```
sed -i -e "s/BOOT_UART=0/BOOT_UART=1/" bootcode.bin
```

Als nächstes schließen Sie ein geeignetes serielles USB-Kabel an Ihren Host-Computer an (ein Raspberry Pi funktioniert, obwohl ich finde, dass der einfachste Weg darin besteht, ein serielles USB-Kabel zu verwenden, da es ohne lästige config.txt-Einstellungen funktioniert). Verwenden Sie die Standard-Pins 6, 8 und 10 (GND, GPIO14, GPIO15) auf einem Pi- oder CM-Board.

Verwenden Sie dann "screen" unter Linux oder einem Mac oder "putty" unter Windows, um eine Verbindung zur seriellen Schnittstelle herzustellen.

Richten Sie Ihre Seriennummer so ein, dass sie bei 115200-8-N-1 empfängt, und starten Sie dann Ihr Pi / Compute-Modul. Sie sollten sofort eine serielle Ausgabe vom Gerät erhalten, wenn bootcode.bin ausgeführt wird.