# GPIO

Allgemeine Eingangs-/Ausgangspins auf dem Raspberry Pi

## Überblick

Diese Seite erweitert die technischen Eigenschaften der GPIO-Pins, die auf dem BCM2835 im Allgemeinen verfügbar sind. Anwendungsbeispiele finden Sie unter [GPIO-Verwendung](../../../usage/gpio/README.md). Beim Lesen dieser Seite sollte auf die BCM2835 ARM-Peripherie [Datenblatt](../bcm2835/README.md), Abschnitt 6 verwiesen werden.

GPIO-Pins können entweder als Universaleingang, Universalausgang oder als eine von bis zu sechs speziellen alternativen Einstellungen konfiguriert werden, deren Funktionen pinabhängig sind.

Auf dem BCM2835 befinden sich drei GPIO-Bänke.

Jede der drei Bänke hat ihren eigenen VDD-Eingangspin. Auf Raspberry Pi werden alle GPIO-Bänke aus 3.3V versorgt. **Der Anschluss eines GPIO an eine Spannung von mehr als 3,3 V wird wahrscheinlich den GPIO-Block im SoC zerstören.**

Eine Auswahl von Pins aus Bank 0 ist auf dem P1-Header des Raspberry Pi verfügbar.

## GPIO-Pads

Die GPIO-Anschlüsse des BCM2835-Gehäuses werden im Datenblatt der Peripheriegeräte manchmal als „Pads“ bezeichnet – ein Begriff aus dem Halbleiterdesign, der „Chipverbindung zur Außenwelt“ bedeutet.

Die Pads sind konfigurierbare CMOS-Push-Pull-Ausgangstreiber/Eingangspuffer. Registerbasierte Steuerungseinstellungen sind verfügbar für:

- Interner Pull-Up / Pull-Down aktivieren/deaktivieren
- Ausgabe [Antriebsstärke](gpio_pads_control.md)
- Eingangs-Schmitt-Trigger-Filterung

###Einschaltzustände

Alle GPIO-Pins kehren beim Zurücksetzen beim Einschalten zu Allzweckeingängen zurück. Es werden auch die Standard-Pull-Zustände angewendet, die in der Tabelle der alternativen Funktionen im ARM-Peripheriedatenblatt aufgeführt sind. Bei den meisten GPIOs wird ein Standard-Pull angewendet.

## Unterbrechungen

Jeder GPIO-Pin kann, wenn er als Universaleingang konfiguriert ist, als Interrupt-Quelle für den ARM konfiguriert werden. Mehrere Interrupt-Generierungsquellen sind konfigurierbar:

- Pegelsensitiv (hoch/niedrig)
- Steigende/fallende Flanke
- Asynchrone steigende/fallende Flanke

Level-Interrupts behalten den Interrupt-Status bei, bis der Level durch die Systemsoftware gelöscht wurde (z. B. durch Bedienen des angeschlossenen Peripheriegeräts, das den Interrupt erzeugt).

Bei der normalen Erkennung von steigenden/fallenden Flanken ist eine kleine Menge an Synchronisation in die Erkennung integriert. Bei der Systemtaktfrequenz wird der Pin abgetastet, wobei das Kriterium für die Erzeugung eines Interrupts ein stabiler Übergang innerhalb eines Fensters mit drei Zyklen ist, d. h. ein Datensatz von '1 0 0' oder '0 1 1'. Die asynchrone Erkennung umgeht diese Synchronisierung, um die Erkennung sehr enger Ereignisse zu ermöglichen.

## Alternative Funktionen

Fast alle GPIO-Pins haben alternative Funktionen. BCM2835-interne Peripherieblöcke können so ausgewählt werden, dass sie auf einem oder mehreren einer Reihe von GPIO-Pins erscheinen, zum Beispiel können die I2C-Busse auf mindestens 3 separate Positionen konfiguriert werden. Pad-Steuerung, wie Drive-Stärke oder Schmitt-Filterung, gilt weiterhin, wenn der Pin als alternative Funktion konfiguriert ist.

## Spannungsspezifikationen

Die folgende Tabelle enthält die verschiedenen Spannungsspezifikationen für die GPIO-Pins, sie wurde dem Datenblatt des Compute Module [hier] entnommen (../../computemodule/datasheet.md).

| Symbol | Parameter | Bedingungen &emsp;| Min | Typisch | Max | Einheit |
|--------|-----------|------------|------|-------- -|------|------|
|V<sub>IL</sub>|Eingangsniederspannung | VDD IO = 1,8V | - | - |0,6 | V |
| | | VDD IO = 2,7V | - | - | 0,8 | V |
| | | VDD-E/A = 3,3 V | - | - | 0,9 | V |
|V<sub>IH</sub>| Eingangshochspannung<sup>a</sup> | VDD IO = 1,8V | 1,0 | - | - | V |
| | | VDD IO = 2,7V | 1,3 | - | - | V |
| | |VDD IO = 3,3V | 1,6 | - | - | V |
|I<sub>IL</sub>| Eingangsleckstrom | TA = +85◦C | - | - | 5 | µA |
|C<sub>IN</sub>| Eingangskapazität | - | - | 5 | - | pF |
|V<sub>OL</sub>| Ausgangsniederspannung<sup>b</sup> | VDD IO = 1,8 V, IOL = -2 mA | - | - | 0,2 | V |
| | | VDD IO = 2,7 V, IOL = -2 mA | - | - | 0,15 | V |
| | | VDD IO = 3,3 V, IOL = -2 mA | - | - | 0,14 | V |
|V<sub>OH</sub>| Ausgangshochspannung<sup>b</sup> | VDD IO = 1,8V, IOH = 2mA | 1,6 | - | - | V |
| | | VDD IO = 2,7 V, IOH = 2 mA | 2,5 | - | - | V |
| | | VDD IO = 3,3 V, IOH = 2 mA | 3,0 | - | - | V |
|I<sub>OL</sub>| Niedriger Ausgangsstrom<sup>c</sup> | VDD IO = 1,8 V, VO = 0,4 V | 12 | - | - | mA |
| | | VDD IO = 2,7 V, VO = 0,4 V | 17 | - | - | mA |
| | | VDD IO = 3,3 V, VO = 0,4 V | 18 | - | - | mA |
|I<sub>OH</sub>| Hoher Ausgangsstrom<sup>c</sup> | VDD IO = 1,8 V, VO = 1,4 V | 10 | - | - | mA |
| | | VDD IO = 2,7 V, VO = 2,3 V | 16 | - | - | mA |
| | | VDD IO = 3,3 V, VO = 2,3 V | 17 | - | - | mA |
| R<sub>PU</sub> | Pullup-Widerstand | - | 50 | - | 65 | kΩ |
| R<sub>PD</sub> | Pulldown-Widerstand | - | 50 | - |65 | kΩ |

<sup>a</sup> Hysterese aktiviert
<sup>b</sup> Standard-Laufwerksstärke (8 mA)
<sup>c</sup> Maximale Antriebsstärke (16 mA)