# SPI

## Seiteninhalt

- [Übersicht](#Übersicht)
- [Hardware](#Hardware)
- [Software](#Software)
- [Linux-Treiber](#Treiber)
- [Fehlerbehebung](#Fehlerbehebung)

<a name="Übersicht"></a>
## Überblick

Die Gerätefamilie Raspberry Pi ist mit einer Reihe von [SPI](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus) Bussen ausgestattet. SPI kann verwendet werden, um eine Vielzahl von Peripheriegeräten anzuschließen - Displays, Netzwerkcontroller (Ethernet, CAN-Bus), UARTs usw. Diese Geräte werden am besten von Kernel-Gerätetreibern unterstützt, aber die `spidev`-API ermöglicht das Schreiben von Userspace-Treibern eine breite Palette von Sprachen.

<a name="Hardware"></a>
##Hardware

Der für alle Raspberry Pi-Geräte gemeinsame BCM2835-Kern verfügt über 3 SPI-Controller:
* SPI0 mit zwei Hardware-Chip-Selects ist im Header aller Pis verfügbar (obwohl es eine alternative Zuordnung gibt, die nur auf einem Compute-Modul verwendet werden kann.
* SPI1 mit drei Hardware-Chip-Selects ist für 40-Pin-Versionen von Pis verfügbar.
* SPI2, ebenfalls mit drei Hardware-Chip-Selects, ist nur auf einem Compute-Modul verwendbar, da die Pins nicht auf den 40-Pin-Header herausgeführt werden.

BCM2711 fügt weitere 4 SPI-Busse hinzu - SPI3 bis SPI6, jeder mit 2 Hardware-Chip-Selects. Alle sind auf dem 40-Pin-Header verfügbar (vorausgesetzt, nichts anderes versucht, dieselben Pins zu verwenden).

Kapitel 10 im Datenblatt [BCM2835 ARM Peripherals](../bcm2835/BCM2835-ARM-Peripherals.pdf) beschreibt den Hauptcontroller. Kapitel 2.3 beschreibt den Zusatzregler.

### Pin/GPIO-Zuordnungen

#### SPI0 (verfügbar für J8/P1-Header auf allen RPi-Versionen)
| SPI-Funktion | Kopfstift | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|---|
| MOSI | 19 | GPIO10 | SPI0_MOSI |
| MISO | 21 | GPIO09 | SPI0_MISO |
| SCLK | 23 | GPIO11 | SPI0_SCLK |
| CE0 | 24 | GPIO08 | SPI0_CE0_N |
| CE1 | 26 | GPIO07 | SPI0_CE1_N |

#### Alternative SPI0-Zuordnung (nur auf Compute-Modulen verfügbar)
| SPI-Funktion | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|
| MOSI | GPIO38 | SPI0_MOSI |
| MISO | GPIO37 | SPI0_MISO |
| SCLK | GPIO39 | SPI0_SCLK |
| CE0 | GPIO36 | SPI0_CE0_N |
| CE1 | GPIO35 | SPI0_CE1_N |

#### SPI1 (nur auf 40-Pin-J8-Stiftleiste verfügbar)
| SPI-Funktion | Kopfstift | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|---|
| MOSI | 38 | GPIO20 | SPI1_MOSI |
| MISO | 35 | GPIO19 | SPI1_MISO |
| SCLK | 40 | GPIO21 | SPI1_SCLK |
| CE0 | 12 | GPIO18 | SPI1_CE0_N |
| CE1 | 11 | GPIO17 | SPI1_CE1_N |
| CE2 | 36 | GPIO16 | SPI1_CE2_N |

#### SPI2 (nur auf Rechenmodulen verfügbar)
| SPI-Funktion | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|
| MOSI | GPIO41 | SPI2_MOSI |
| MISO | GPIO40 | SPI2_MISO |
| SCLK | GPIO42 | SPI2_SCLK |
| CE0 | GPIO43 | SPI2_CE0_N |
| CE1 | GPIO44 | SPI2_CE1_N |
| CE2 | GPIO45 | SPI2_CE2_N |

#### SPI3 (nur BCM2711)
| SPI-Funktion | Kopfstift | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|---|
| MOSI | 03 | GPIO02 | SPI3_MOSI |
| MISO | 28 | GPIO01 | SPI3_MISO |
| SCLK | 05 | GPIO03 | SPI3_SCLK |
| CE0 | 27 | GPIO00 | SPI3_CE0_N |
| CE1 | 18 | GPIO24 | SPI3_CE1_N |

#### SPI4 (nur BCM2711)
| SPI-Funktion | Kopfstift | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|---|
| MOSI | 31 | GPIO06 | SPI4_MOSI |
| MISO | 29 | GPIO05 | SPI4_MISO |
| SCLK | 26 | GPIO07 | SPI4_SCLK |
| CE0 | 07 | GPIO04 | SPI4_CE0_N |
| CE1 | 22 | GPIO25 | SPI4_CE1_N |

#### SPI5 (nur BCM2711)
| SPI-Funktion | Kopfstift | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|---|
| MOSI | 08 | GPIO14 | SPI5_MOSI |
| MISO | 33 | GPIO13 | SPI5_MISO |
| SCLK | 10 | GPIO15 | SPI5_SCLK |
| CE0 | 32 | GPIO12 | SPI5_CE0_N |
| CE1 | 37 | GPIO26 | SPI5_CE1_N |

#### SPI6 (nur BCM2711)
| SPI-Funktion | Kopfstift | Broadcom-Pin-Name | Broadcom-Pin-Funktion |
|---|---|---|---|
| MOSI | 38 | GPIO20 | SPI6_MOSI |
| MISO | 35 | GPIO19 | SPI6_MISO |
| SCLK | 40 | GPIO21 | SPI6_SCLK |
| CE0 | 12 | GPIO18 | SPI6_CE0_N |
| CE1 | 13 | GPIO27 | SPI6_CE1_N |

### Master-Modi

Abkürzungen von Signalnamen

```
SCLK - Serielle UHR
CE - Chip Enable (oft als Chip Select bezeichnet)
MOSI - Master Out Slave In
MISO - Master In Slave Out
MOMI - Master Out Master In
```

#### Standart Modus

Im Standard-SPI-Modus implementiert das Peripheriegerät das standardmäßige serielle 3-Draht-Protokoll (SCLK, MOSI und MISO).

#### Bidirektionaler Modus

Im bidirektionalen SPI-Modus wird der gleiche SPI-Standard implementiert, außer dass ein einzelner Draht für Daten (MOMI) anstelle der beiden im Standardmodus (MISO und MOSI) verwendeten verwendet wird. In diesem Modus dient der MOSI-Pin als MOMI-Pin.

#### LoSSI-Modus (Low Speed ​​Serial Interface)
Der LoSSI-Standard ermöglicht die Ausgabe von Befehlen an Peripheriegeräte (LCD) und die Übertragung von Daten zu und von diesen. LoSSI-Befehle und -Parameter sind 8 Bit lang, aber ein zusätzliches Bit wird verwendet, um anzuzeigen, ob das Byte ein Befehl oder Parameter/Daten ist. Dieses zusätzliche Bit wird für Daten hoch und für einen Befehl niedrig gesetzt. Der resultierende 9-Bit-Wert wird an den Ausgang serialisiert. LoSSI wird häufig mit [MIPI DBI](http://mipi.org/specifications/display-interface) Typ-C-kompatiblen LCD-Controllern verwendet.

**Notiz:**

Einige Befehle lösen ein automatisches Lesen durch den SPI-Controller aus, daher kann dieser Modus nicht als Mehrzweck-9-Bit-SPI verwendet werden.

### Übertragungsmodi

- Abgefragt
- Unterbrechen
- DMA

### Geschwindigkeit

Das CDIV-Feld (Clock Divider) des CLK-Registers legt die SPI-Taktgeschwindigkeit fest:

```
SCLK = Kerntakt / CDIV
Wenn CDIV auf 0 gesetzt ist, ist der Divisor 65536. Der Divisor muss ein Vielfaches von 2 sein, wobei ungerade Zahlen abgerundet werden. Beachten Sie, dass aufgrund von analogen elektrischen Problemen (Anstiegszeiten, Antriebsstärken usw.) nicht alle möglichen Taktraten verwendet werden können.
```

Weitere Informationen finden Sie im Abschnitt [Linux-Treiber](#Treiber).

###Chipauswahl

Die Setup- und Hold-Zeiten in Bezug auf die automatische Aktivierung und Deaktivierung der CS-Leitungen beim Betrieb im **DMA**-Modus sind wie folgt:

- Die CS-Leitung wird mindestens 3 Kerntaktzyklen vor dem msb des ersten Bytes der Übertragung aktiviert.
- Die CS-Leitung wird frühestens 1 Kerntaktzyklus nach der abfallenden Flanke des letzten Taktimpulses deaktiviert.

<a name="Software"></a>
##Software

<a name="Treiber"></a>
### Linux-Treiber

Der Standard-Linux-Treiber ist jetzt der Standard spi-bcm2835.

SPI0 ist standardmäßig deaktiviert. Um es zu aktivieren, verwenden Sie [raspi-config](../../../configuration/raspi-config.md) oder stellen Sie sicher, dass die Zeile `dtparam=spi=on` in `/boot/ nicht auskommentiert ist. config.txt`. Standardmäßig verwendet es 2 Chip-Select-Leitungen, dies kann jedoch mit `dtoverlay=spi0-1cs` auf 1 reduziert werden. `dtoverlay=spi0-2cs` existiert auch und ist ohne Parameter äquivalent zu `dtparam=spi=on`.

Um SPI1 zu aktivieren, können Sie 1, 2 oder 3 Chipauswahlleitungen verwenden und in jedem Fall hinzufügen:
<pre>
dtoverlay=spi1-1cs #1 Chipauswahl
dtoverlay=spi1-2cs #2 Chipauswahl
dtoverlay=spi1-3cs #3 Chipauswahl
</pre>
in die Datei /boot/config.txt. Ähnliche Overlays existieren für SPI2, SPI3, SPI4, SPI5 und SPI6.

Der Treiber verwendet aufgrund einiger Einschränkungen nicht die Hardware-Chip-Select-Leitungen - stattdessen kann er eine beliebige Anzahl von GPIOs als Software-/GPIO-Chip-Select verwenden. Dies bedeutet, dass Sie jedes freie GPIO als CS-Leitung auswählen können, und alle diese SPI-Overlays beinhalten diese Kontrolle - siehe `/boot/overlays/README` für Details, oder führen Sie (zum Beispiel) `dtoverlay -h spi0-2cs . aus ` (`dtoverlay -a | grep spi` könnte hilfreich sein, um sie alle aufzulisten).

#### Geschwindigkeit

Der Treiber unterstützt alle Geschwindigkeiten, die sogar ganzzahlige Teiler des Kerntakts sind, obwohl, wie oben erwähnt, nicht alle diese Geschwindigkeiten aufgrund von Einschränkungen in den GPIOs und in den angeschlossenen Geräten die Datenübertragung unterstützen. Als Faustregel gilt, dass alles über 50 MHz wahrscheinlich nicht funktioniert, aber Ihre Laufleistung kann variieren.

#### Unterstützte Modusbits

```
SPI_CPOL - Taktpolarität
SPI_CPHA - Taktphase
SPI_CS_HIGH - Chipauswahl aktiv hoch
SPI_NO_CS - 1 Gerät pro Bus, kein Chip Select
SPI_3WIRE - Bidirektionaler Modus, Dateneingang und -ausgang gemeinsam genutzt
```

Der bidirektionale oder "3-wire"-Modus wird vom spi-bcm2835-Kernelmodul unterstützt. Bitte beachten Sie, dass in diesem Modus entweder das tx- oder das rx-Feld der spi_transfer-Struktur ein NULL-Zeiger sein muss, da nur Halbduplex-Kommunikation möglich ist. Andernfalls schlägt die Übertragung fehl. Der Quellcode von spidev_test.c berücksichtigt dies nicht richtig und funktioniert daher im 3-Draht-Modus überhaupt nicht.

#### Unterstützte Bits pro Wort

- 8 - Normal
- 9 - Dies wird im LoSSI-Modus unterstützt.

#### Übertragungsmodi

Der Interrupt-Modus wird auf allen SPI-Bussen unterstützt. SPI0 und SPI3-6 unterstützen auch DMA-Übertragungen.

#### SPI-Treiberlatenz

Dieser [Thread](https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=19489) diskutiert Latenzprobleme.

###spidev

spidev präsentiert eine ioctl-basierte Userspace-Schnittstelle zu einzelnen SPI CS-Linien. Der Gerätebaum wird verwendet, um anzuzeigen, ob eine CS-Leitung von einem Kernel-Treibermodul gesteuert oder von spidev im Namen des Benutzers verwaltet wird; es ist nicht möglich, beides gleichzeitig zu tun. Beachten Sie, dass die eigenen Kernel von Raspberry Pi bei der Verwendung von Device Tree zur Aktivierung von Spidev entspannter sind - die Upstream-Kernel geben Warnungen über eine solche Verwendung aus und können sie letztendlich ganz verhindern.

#### Spidev von C . verwenden

Es gibt ein Loopback-Testprogramm in der Linux-Dokumentation, das als Ausgangspunkt verwendet werden kann. Siehe den Abschnitt [Fehlerbehebung](#Fehlerbehebung).

#### Spidev von Python verwenden

Es gibt mehrere Python-Bibliotheken, die Zugriff auf spidev bieten, darunter die fantasievoll benannten `spidev` (`pip install spidev` - siehe https://pypi.org/project/spidev/) und `SPI-Py` (https:// github.com/lthiery/SPI-Py).

#### Verwenden von Spidev aus einer Shell wie bash

```bash
# Schreibe binär 1, 2 und 3
echo -ne "\x01\x02\x03" > /dev/spidev0.0
```

### Andere SPI-Bibliotheken

Es gibt andere Userspace-Bibliotheken, die SPI-Steuerung durch direkte Manipulation der Hardware ermöglichen. Dies wird nicht empfohlen.

<a name="Fehlerbehebung"></a>
## Fehlerbehebung

### Loopback-Test

Dies kann verwendet werden, um das Senden und Empfangen von SPI zu testen. Legen Sie einen Draht zwischen MOSI und MISO. CE0 und CE1 werden nicht getestet.

```bash
wget https://raw.githubusercontent.com/raspberrypi/linux/rpi-3.10.y/Documentation/spi/spidev_test.c
gcc -o spidev_test spidev_test.c
./spidev_test -D /dev/spidev0.0
spi mode: 0
bits per word: 8
max speed: 500000 Hz (500 KHz)

FF FF FF FF FF FF
40 00 00 00 00 95
FF FF FF FF FF FF
FF FF FF FF FF FF
FF FF FF FF FF FF
DE AD BE EF BA AD
F0 0D
```

Einige der obigen Inhalte wurden von [https://elinux.org/RPi_SPI] (der elinux-SPI-Seite) kopiert, die ebenfalls von hier entlehnt sind. Beide sind durch die CC-SA-Lizenz abgedeckt.