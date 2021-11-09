# GPIO-Boot-Modus

**GPIO-Boot-Modus ist nur auf dem Raspberry Pi 3A+, 3B, 3B+, Compute Module 3 und 3+ verfügbar**

Der Raspberry Pi kann so konfiguriert werden, dass der Boot-Modus beim Einschalten über Hardware ausgewählt werden kann, die an den GPIO-Anschluss angeschlossen ist: Dies ist der GPIO-Boot-Modus. Dies geschieht durch Setzen von Bits im OTP-Speicher des SoC. Sobald die Bits gesetzt sind, weisen sie 5 GPIOs dauerhaft zu, um diese Auswahl zu ermöglichen. Sobald die OTP-Bits gesetzt sind, können sie nicht mehr deaktiviert werden. Sie sollten daher sorgfältig darüber nachdenken, dies zu aktivieren, da diese 5 GPIO-Leitungen immer das Booten steuern. Obwohl Sie die GPIOs nach dem Booten des Pi für einige andere Funktionen verwenden können, müssen Sie sie so einrichten, dass sie beim Booten des Pi die gewünschten Boot-Modi aktivieren.

Um den GPIO-Boot-Modus zu aktivieren, fügen Sie der Datei [config.txt](../../../configuration/config-txt/README.md) die folgende Zeile hinzu:

```
program_gpio_bootmode=n
```

Wobei n die GPIO-Bank ist, die Sie verwenden möchten. Starten Sie dann den Pi einmal neu, um das OTP mit dieser Einstellung zu programmieren. Bank 1 ist die GPIOs 22-26, Bank 2 ist die GPIOs 39-43. Sofern Sie kein Rechenmodul haben, müssen Sie Bank 1 verwenden: Die GPIOs in Bank 2 sind nur auf dem Rechenmodul verfügbar. Aufgrund der Anordnung der OTP-Bits haben Sie, wenn Sie zuerst den GPIO-Boot-Modus für Bank 1 programmieren, die Möglichkeit, Bank 2 später auszuwählen. Das Umgekehrte gilt nicht: Sobald Bank 2 für den GPIO-Boot-Modus ausgewählt wurde, können Sie Bank 1 nicht mehr auswählen.

Sobald der GPIO-Boot-Modus aktiviert ist, bootet der Raspberry Pi nicht mehr. Sie müssen mindestens einen GPIO-Pin im Bootmodus hochziehen, damit der Pi booten kann.

## Pinbelegung im GPIO-Boot-Modus
### Raspberry Pi 3B und Rechenmodul 3

|Bank 1|Bank 2|Boottyp|
|:----:|:---:|:--------:|
|22 |39 |SD0 |
|23 |40 |SD1 |
|24 |41 |NAND (derzeit keine Linux-Unterstützung) |
|25 |42 |SPI (derzeit keine Linux-Unterstützung) |
|26 |43 |USB |

USB in der obigen Tabelle wählt sowohl den USB-Geräte-Boot-Modus als auch den USB-Host-Boot-Modus aus. Um einen USB-Boot-Modus zu verwenden, muss dieser im OTP-Speicher aktiviert werden. Weitere Informationen finden Sie unter [USB-Gerätestart](device.md) und [USB-Hoststart](host.md).

### Raspberry Pi 3A+, 3B+ und Rechenmodul 3+

|Bank 1|Bank 2|Boottyp|
|:----:|:---:|:--------:|
|20 |37 |SD0 |
|21 |38 |SD1 |
|22 |39 |NAND (derzeit keine Linux-Unterstützung) |
|23 |40 |SPI (derzeit keine Linux-Unterstützung) |
|24 |41 |USB-Gerät |
|25 |42 |USB-Host - Massenspeichergerät |
|26 |43 |USB-Host - Ethernet |

## Startreihenfolge

Die verschiedenen Boot-Modi werden in der numerischen Reihenfolge der GPIO-Zeilen versucht, d. h. SD0, dann SD1, dann NAND und so weiter.

## Startablauf

SD0 ist die Broadcom SD-Karte / MMC-Schnittstelle. Wenn das Boot-ROM im SoC ausgeführt wird, verbindet es SD0 immer mit dem integrierten microSD-Kartensteckplatz. Bei Rechenmodulen mit einem eMMC-Gerät ist SD0 daran angeschlossen; auf dem Compute Module Lite SD0 ist am Edge Connector verfügbar und wird an den microSD-Kartensteckplatz im CMIO-Trägerboard angeschlossen. SD1 ist die Arasan SD-Karte / MMC-Schnittstelle, die auch SDIO-fähig ist. Alle Raspberry Pi-Modelle mit integriertem WLAN verwenden SD1, um sich über SDIO mit dem WLAN-Chip zu verbinden.

Der Standard-Pull-Widerstand auf den GPIO-Leitungen beträgt 50 kOhm, wie auf Seite 102 des [BCM2835 ARM-Peripherie-Datenblatts] dokumentiert (../../hardware/raspberrypi/bcm2835/BCM2835-ARM-Peripherals.pdf). Ein Pull-Widerstand von 5K Ohm wird empfohlen, um eine GPIO-Reihe zu ziehen: Dadurch kann der GPIO funktionieren, aber nicht zu viel Strom verbrauchen.