# Startvorgang

**Die folgende Bootsequenz gilt nur für die BCM2837- und BCM2837B0-basierten Modelle des Raspberry Pi. Bei früheren Modellen versucht der Pi [SD-Kartenstart](sdcard.md), gefolgt von [USB-Gerätemodusstart](device.md). Die Startsequenz des Raspberry Pi4 finden Sie auf [dieser](bootflow_2711.md) Seite**

Die USB-Boot-Standardeinstellungen auf dem Raspberry Pi 3 hängen davon ab, welche Version verwendet wird. Auf dieser [Seite](./msd.md) finden Sie Informationen zum Aktivieren von USB-Startmodi, wenn diese nicht standardmäßig aktiviert sind.

Beim Booten des BCM2837 verwendet er zwei verschiedene Quellen, um zu bestimmen, welche Boot-Modi aktiviert werden sollen. Zuerst wird der OTP-Speicherblock (einmalig programmierbar) überprüft, um zu sehen, welche Boot-Modi aktiviert sind. Wenn die GPIO-Boot-Modus-Einstellung aktiviert ist, werden die relevanten GPIO-Leitungen getestet, um auszuwählen, welcher der OTP-aktivierten Boot-Modi versucht werden soll. Beachten Sie, dass der GPIO-Boot-Modus nur verwendet werden kann, um Boot-Modi auszuwählen, die bereits im OTP aktiviert sind. Weitere Informationen zum Konfigurieren des GPIO-Startmodus finden Sie unter [GPIO-Startmodus](gpio.md). Der GPIO-Boot-Modus ist standardmäßig deaktiviert.

Als nächstes überprüft das Boot-ROM jede der Boot-Quellen auf eine Datei namens bootcode.bin; wenn es erfolgreich ist, lädt es den Code in den lokalen 128K-Cache und springt dorthin. Der gesamte Boot-Modus-Prozess ist wie folgt:

* BCM2837 boots
* OTP lesen, um zu bestimmen, welche Boot-Modi aktiviert werden sollen
* Wenn der GPIO-Boot-Modus aktiviert ist, verwenden Sie den GPIO-Boot-Modus, um die Liste der aktivierten Boot-Modi zu verfeinern
* Wenn aktiviert: Überprüfen Sie die primäre SD auf bootcode.bin auf GPIO 48-53
    * Success - Boot
    * Fail - timeout (fünf Sekunden)
* Wenn aktiviert: Überprüfen Sie die sekundäre SD
    * Success - Boot
    * Fail - timeout (fünf Sekunden)
* Wenn aktiviert: Überprüfen Sie NAND
* Falls aktiviert: SPI prüfen
* Falls aktiviert: USB prüfen
    * Wenn OTG-Pin == 0
        * USB aktivieren, auf gültige USB 2.0-Geräte warten (zwei Sekunden)
            * Gerät gefunden:
                * Wenn Gerätetyp == Hub
                    * Rekursion für jeden Port
                * Wenn Gerätetyp == (Massenspeicher oder LAN951x)
                    * In Geräteliste speichern
        * Rekurs durch jedes MSD
            * Wenn bootcode.bin Boot gefunden hat
        * Rekursion durch jedes LAN951x
            * DHCP / TFTP-Boot
    * sonst (Gerätemodus booten)
        * Aktivieren Sie den Gerätemodus und warten Sie, bis der Host-PC aufgezählt hat
        * Wir antworten dem PC mit VID: 0a5c PID: 0x2763 (Pi 1 oder Pi 2) oder 0x2764 (Pi 3)

ANMERKUNGEN:

* Wenn keine SD-Karte eingelegt ist, dauert es fünf Sekunden, bis der SD-Boot-Modus fehlschlägt. Um dies zu reduzieren und schneller auf USB zurückzugreifen, können Sie entweder eine SD-Karte ohne darauf befindliche SD-Karte einlegen oder die oben beschriebene GPIO-Bootmode-OTP-Einstellung verwenden, um nur USB zu aktivieren.
* Der Standard-Pull für die GPIOs ist auf Seite 102 des [ARM Peripherals datasheet](../bcm2835/BCM2835-ARM-Peripherals.pdf) definiert. Wenn der Wert zum Startzeitpunkt nicht dem Standard-Pull entspricht, wird dieser Startmodus aktiviert.
* USB-Enumeration ermöglicht die Stromversorgung der Downstream-Geräte an einem Hub und wartet dann darauf, dass das Gerät die Leitungen D+ und D- zieht, um anzuzeigen, ob es sich um USB 1 oder USB 2 handelt. Dies kann einige Zeit dauern: bei einigen Geräten Es kann bis zu drei Sekunden dauern, bis eine Festplatte hochgefahren ist und den Aufzählungsvorgang startet. Da nur so erkannt werden kann, dass die Hardware angeschlossen ist, müssen wir eine Mindestzeit (zwei Sekunden) abwarten. Wenn das Gerät nach diesem maximalen Timeout nicht reagiert, kann das Timeout mit `program_usb_boot_timeout=1` in `config.txt` auf fünf Sekunden erhöht werden.
* MSD-Boot hat Vorrang vor Ethernet-Boot.
* Es ist nicht mehr erforderlich, dass die erste Partition die FAT-Partition ist, da der MSD-Boot weiterhin nach einer FAT-Partition über die erste hinaus sucht.
* Das Boot-ROM unterstützt jetzt auch die GUID-Partitionierung und wurde mit Festplatten getestet, die unter Mac, Windows und Linux partitioniert wurden.
* Das LAN951x wird anhand der Hersteller-ID 0x0424 und der Produkt-ID 0xec00 erkannt: Dies unterscheidet sich vom eigenständigen LAN9500-Gerät mit der Produkt-ID 0x9500 oder 0x9e00. Um das eigenständige LAN9500 ​​zu verwenden, müsste ein I2C-EEPROM hinzugefügt werden, um diese IDs so zu ändern, dass sie mit dem LAN951x übereinstimmen.

Der primäre Bootmodus der SD-Karte ist standardmäßig auf GPIOs 49-53 eingestellt. Es ist möglich, von der sekundären SD-Karte auf einem zweiten Satz von Pins zu booten, d. h. eine sekundäre SD-Karte zu den GPIO-Pins hinzuzufügen. Diese Fähigkeit haben wir jedoch noch nicht aktiviert.

NAND-Boot- und SPI-Boot-Modi funktionieren, obwohl sie noch keine volle GPU-Unterstützung bieten.

Der Bootmodus des USB-Geräts ist zum Zeitpunkt der Herstellung standardmäßig aktiviert, aber der Bootmodus des USB-Hosts ist nur mit `program_usb_boot_mode=1` aktiviert. Nach der Aktivierung verwendet der Prozessor den Wert des OTGID-Pins am Prozessor, um zwischen den beiden Modi zu entscheiden. Auf einem Raspberry Pi Model B wird der OTGID-Pin auf '0' getrieben und bootet daher nur über den Host-Modus, sobald er aktiviert ist (es ist nicht möglich, über den Gerätemodus zu booten, da das LAN9515-Gerät im Weg ist).

Der USB bootet als USB-Gerät auf dem Pi Zero oder Compute Module, wenn der OTGID-Pin schwebend gelassen wird (zum Beispiel beim Anschließen an einen PC), sodass Sie die bootcode.bin in das Gerät "spritzen" können. Der Code dafür ist [usbboot](https://github.com/raspberrypi/usbboot).
