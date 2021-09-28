# UART-Konfiguration

Auf dem Raspberry Pi sind zwei Arten von UART verfügbar - [PL011](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.ddi0183g/index.html) und Mini-UART . Der PL011 ist ein leistungsfähiger, weitgehend 16550-kompatibler UART, während der Mini-UART über einen reduzierten Funktionsumfang verfügt.

Alle UARTs auf dem Raspberry Pi sind nur 3,3 V - Schäden treten auf, wenn sie an 5-V-Systeme angeschlossen werden. Ein Adapter kann zum Anschluss an 5V-Systeme verwendet werden. Alternativ sind von verschiedenen Drittanbietern kostengünstige USB-auf-3,3-V-Seriell-Adapter erhältlich.

## Pi Zero, 1, 2 und 3 - zwei UARTs

Die Raspberry Pi Zero, 1, 2 und 3 enthalten jeweils zwei UARTs wie folgt:

| Name | Typ |
|-----|------|
|UART0 |PL011 |
|UART1 |Mini-UART |

##Pi 4 - sechs UARTS

Der Raspberry Pi 4 verfügt über vier zusätzliche PL011s, die standardmäßig deaktiviert sind. Die vollständige Liste der UARTs auf dem Pi 4 ist:

| Name | Typ |
|-----|------|
|UART0 |PL011 |
|UART1 |Mini-UART |
|UART2 |PL011 |
|UART3 |PL011 |
|UART4 |PL011 |
|UART5 |PL011 |

## Primärer UART

Auf dem Raspberry Pi wird ein UART ausgewählt, der auf GPIO 14 (Senden) und 15 (Empfangen) vorhanden ist - dies ist der primäre UART. Standardmäßig ist dies auch der UART, auf dem eine Linux-Konsole vorhanden sein kann. Beachten Sie, dass GPIO 14 Pin 8 am GPIO-Header ist, während GPIO 15 Pin 10 ist.

## Sekundärer UART

Der sekundäre UART ist normalerweise nicht am GPIO-Anschluss vorhanden. Bei Modellen, die diesen Controller enthalten, ist der sekundäre UART standardmäßig mit der Bluetooth-Seite des kombinierten Wireless LAN/Bluetooth-Controllers verbunden.

## Aufbau

Standardmäßig ist nur UART0 aktiviert. Die folgende Tabelle fasst die Belegung der ersten beiden UARTs zusammen:

| Modell | erster PL011 (UART0)| Mini-UART |
|-------|----------|-------|
| Raspberry Pi Zero | primär | sekundär |
| Raspberry Pi Zero W | sekundär (Bluetooth) | primär |
| Raspberry Pi 1 | primär | sekundär |
| Raspberry Pi 2 | primär | sekundär |
| Raspberry Pi 3 | sekundär (Bluetooth) | primär |
| Raspberry Pi 4 | sekundär (Bluetooth) | primär |

Hinweis: Der Mini-UART ist standardmäßig deaktiviert, unabhängig davon, ob er als primärer oder sekundärer UART bezeichnet wird.

Linux-Geräte auf Raspberry Pi OS:

| Linux-Gerät | Beschreibung |
|----------------|-------------|
|`/dev/ttyS0` |mini-UART |
|`/dev/ttyAMA0`|erster PL011 (UART0) |
|`/dev/serial0` |primärer UART |
|`/dev/serial1` |sekundärer UART |

Hinweis: `/dev/serial0` und `/dev/serial1` sind symbolische Links, die entweder auf `/dev/ttyS0` oder `/dev/ttyAMA0` zeigen.

## Mini-UART- und CPU-Kernfrequenz

Um den Mini-UART zu verwenden, müssen Sie den Raspberry Pi so konfigurieren, dass er eine feste VPU-Kerntaktfrequenz verwendet. Dies liegt daran, dass der Mini-UART-Takt mit dem VPU-Kerntakt verbunden ist, sodass sich bei einer Änderung der Kerntaktfrequenz auch die UART-Baudrate ändert. Die Einstellungen `enable_uart` und `core_freq` können zur `config.txt` hinzugefügt werden, um das Verhalten des Mini-UART zu ändern. Die folgende Tabelle fasst die möglichen Kombinationen zusammen:

| Mini-UART auf | . eingestellt Kernuhr | Ergebnis |
|--------------------|------------|--------|
| primärer UART | variabel | Mini-UART deaktiviert |
| primärer UART | behoben durch Setzen von `enable_uart=1` | Mini-UART aktiviert, Kerntakt auf 250MHz fixiert, oder wenn `force_turbo=1` gesetzt ist, die VPU-Turbofrequenz |
| sekundärer UART | variabel | Mini-UART deaktiviert |
| sekundärer UART | behoben durch Setzen von `core_freq=250` | Mini-UART aktiviert |

Der Standardzustand des Flags `enable_uart` hängt davon ab, welcher UART der primäre UART ist:

| Primärer UART | Standardzustand des enable_uart-Flags |
|----------------|------------------------------------------------ -|
| Mini-UART | 0 |
| erster PL011 (UART0) | 1 |

## serielle Linux-Konsole deaktivieren

Standardmäßig wird der primäre UART der Linux-Konsole zugewiesen. Wenn Sie den primären UART für andere Zwecke verwenden möchten, müssen Sie das Raspberry Pi OS neu konfigurieren. Dies kann mit [raspi-config](raspi-config.md) erfolgen:

1. Starten Sie raspi-config: `sudo raspi-config`.
1. Wählen Sie Option 3 - Schnittstellenoptionen.
1. Wählen Sie Option P6 - Serieller Port.
1. Bei der Eingabeaufforderung `Möchten Sie, dass eine Login-Shell über seriell zugänglich ist?` antworten Sie mit 'Nein'
1. Bei der Eingabeaufforderung `Möchten Sie die Hardware des seriellen Anschlusses aktivieren?` antworten Sie mit 'Ja'
1. Beenden Sie raspi-config und starten Sie den Pi neu, damit die Änderungen wirksam werden.

## Aktivieren der frühen Konsole (earlycon) für Linux

Obwohl der Linux-Kernel die UARTs relativ früh im Boot-Prozess startet, dauert es noch lange, bis einige kritische Teile der Infrastruktur eingerichtet wurden. Ein Fehler in diesen frühen Stadien kann ohne Zugriff auf die Kernel-Log-Meldungen von diesem Zeitpunkt schwer zu diagnostizieren sein. Das ist das Problem, das der "earlycon"-Mechanismus geschaffen hat, um ihn zu umgehen. Konsolen, die die Verwendung von Earlycon unterstützen, stellen eine zusätzliche Schnittstelle zum Kernel dar, die eine einfache, synchrone Ausgabe ermöglicht - printk kehrt erst zurück, wenn die Zeichen an den UART ausgegeben wurden.

Aktivieren Sie Earlycon mit einem Kernel-Befehlszeilenparameter - fügen Sie einen der folgenden Punkte zu `cmdline.txt` hinzu, je nachdem, welcher UART der primäre ist:
```
# Für Pi 4 und Compute Module 4 (BCM2711)
Earlycon=uart8250,mmio32,0xfe215040
Earlycon=pl011,mmio32,0xfe2010000

# Für Pi 2, Pi 3 und Compute Module 3 (BCM2836 & BCM2837)
Earlycon=uart8250,mmio32,0x3f215040
Earlycon=pl011,mmio32,0x3f2010000

# Für Pi 1, Pi Zero und Compute Module (BCM2835)
Earlycon=uart8250,mmio32,0x20215040
Earlycon=pl011,mmio32,0x20201000
```
Die Baudrate ist auf 115200 eingestellt.

Hinweis Die Auswahl der falschen frühen Konsole kann das Booten des Pi verhindern.

## UARTs und Gerätebaum

Verschiedene UART-Gerätebaum-Overlay-Definitionen finden Sie im [Kernel-GitHub-Baum](https://github.com/raspberrypi/linux). Die zwei nützlichsten Overlays sind [`disable-bt`](https://github.com/raspberrypi/linux/blob/rpi-5.4.y/arch/arm/boot/dts/overlays/disable-bt-overlay. dts) und [`miniuart-bt`](https://github.com/raspberrypi/linux/blob/rpi-5.4.y/arch/arm/boot/dts/overlays/miniuart-bt-overlay.dts).

`disable-bt` deaktiviert das Bluetooth-Gerät und macht den ersten PL011 (UART0) zum primären UART. Sie müssen auch den Systemdienst deaktivieren, der das Modem initialisiert, damit es keine Verbindung zum UART herstellt, indem Sie `sudo systemctl disable hciuart` verwenden.

`miniuart-bt` schaltet die Bluetooth-Funktion um, um den Mini-UART zu verwenden, und macht den ersten PL011 (UART0) zum primären UART. Beachten Sie, dass dies die maximal nutzbare Baudrate reduzieren kann (siehe Mini-UART-Einschränkungen unten). Sie müssen auch den VPU-Kerntakt auf eine feste Frequenz einstellen, indem Sie entweder `force_turbo=1` oder `core_freq=250` verwenden.

Die Overlays `uart2`, `uart3`, `uart4` und `uart5` werden verwendet, um die vier zusätzlichen UARTs auf dem Pi 4 zu aktivieren. Im Ordner befinden sich weitere UART-spezifische Overlays. Siehe `/boot/overlays/README` für Details zu Gerätebaum-Overlays oder führen Sie `dtoverlay -h overlay-name` für Beschreibungen und Nutzungsinformationen aus.

Vollständige Anweisungen zur Verwendung von Gerätebaum-Overlays finden Sie auf [dieser Seite](device-tree.md). Kurz gesagt, fügen Sie der Datei 'config.txt' eine Zeile hinzu, um ein Gerätebaum-Overlay anzuwenden. Beachten Sie, dass der `-overlay.dts`-Teil des Dateinamens entfernt wird. Zum Beispiel:
```
dtoverlay=disable-bt
```

## Relevante Unterschiede zwischen PL011 und Mini-UART

Der Mini-UART hat kleinere FIFOs. In Kombination mit der fehlenden Flusskontrolle macht dies es anfälliger, bei höheren Baudraten Zeichen zu verlieren. Es ist auch im Allgemeinen weniger leistungsfähig als ein PL011, hauptsächlich aufgrund seiner Baudratenverbindung zur VPU-Taktgeschwindigkeit.

Die besonderen Mängel des Mini-UART im Vergleich zu einem PL011 sind:
- Keine Brucherkennung
- Keine Erkennung von Framing-Fehlern
- Kein Paritätsbit
- Kein Empfangs-Timeout-Interrupt
- Keine DCD-, DSR-, DTR- oder RI-Signale

Weitere Dokumentation zum Mini-UART finden Sie im SoC-Peripheriedokument [hier](../hardware/raspberrypi/bcm2835/BCM2835-ARM-Peripherals.pdf).