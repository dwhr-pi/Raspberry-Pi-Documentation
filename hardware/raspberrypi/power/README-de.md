# Energieversorgung

Die Anforderungen an die Stromversorgung unterscheiden sich je nach Raspberry Pi-Modell. Alle Modelle benötigen eine 5,1-V-Versorgung, aber der gelieferte Strom erhöht sich in der Regel je nach Modell. Alle Modelle bis hin zum Raspberry Pi 3 benötigen einen microUSB-Stromanschluss, während der Raspberry Pi 4 einen USB-C-Anschluss verwendet.

Wie viel Strom (mA) der Raspberry Pi genau benötigt, hängt davon ab, was Sie daran anschließen. In der folgenden Tabelle sind verschiedene aktuelle Anforderungen aufgeführt.

| Produkt | Empfohlene Stromkapazität des Netzteils | Maximale Gesamtstromaufnahme von USB-Peripheriegeräten | Typischer aktiver Stromverbrauch des Bare-Boards |
|-|-|-|-|
| Raspberry Pi Modell A | 700mA | 500mA | 200mA |
| Raspberry Pi Modell B |1,2A | 500mA | 500mA |
| Raspberry Pi Modell A+ | 700mA | 500mA | 180mA
| Raspberry Pi Modell B+ | 1,8A | 1,2A | 330mA |
| Raspberry Pi 2 Modell B | 1,8A | 1,2A | 350mA |
| Raspberry Pi 3 Modell B | 2,5A | 1,2A | 400mA |
| Raspberry Pi 3 Modell A+ | 2,5A | Begrenzt nur durch Netzteil-, Platinen- und Anschlusswerte. | 350mA |
| Raspberry Pi 3 Modell B+ | 2,5A | 1,2A | 500mA |
| Raspberry Pi 4 Modell B | 3,0A | 1,2A | 600mA |
| Raspberry Pi Zero | 1,2A | Begrenzt nur durch Netzteil-, Platinen- und Anschlusswerte | 100mA |
| Raspberry Pi Zero W/WH | 1,2A | Begrenzt nur durch Netzteil-, Platinen- und Anschlusswerte.| 150mA |

Raspberry Pi hat für alle Modelle eigene Netzteile entwickelt. Diese sind zuverlässig, verwenden dickwandige Drähte und sind preisgünstig.

Für Raspberry Pi 0-3 empfehlen wir unser [2.5A micro USB Supply](https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/). Für Raspberry Pi 4 empfehlen wir unser [3A USB-C Supply](https://www.raspberrypi.org/products/type-c-power-supply/)

Der Strombedarf des Raspberry Pi steigt, wenn Sie die verschiedenen Schnittstellen des Raspberry Pi nutzen. Die GPIO-Pins können sicher 50 mA ziehen, verteilt auf alle Pins; ein einzelner GPIO-Pin kann nur 16mA sicher ziehen. Der HDMI-Anschluss verbraucht 50 mA, das Kameramodul benötigt 250 mA und Tastaturen und Mäuse können nur 100 mA oder über 1000 mA aufnehmen! Überprüfen Sie die Nennleistung der Geräte, die Sie an den Pi anschließen möchten, und kaufen Sie ein entsprechendes Netzteil.

Wenn Sie ein USB-Gerät anschließen müssen, dessen Strombedarf über den in der obigen Tabelle angegebenen Werten liegt, müssen Sie es an einen extern mit Strom versorgten USB-Hub anschließen.

## Warnungen zur Stromversorgung

Bei allen Raspberry Pi-Modellen seit dem Raspberry Pi B+ (2014) mit Ausnahme des Zero-Bereichs gibt es eine Unterspannungserkennungsschaltung, die erkennt, wenn die Versorgungsspannung unter 4,63 V (+/- 5%) fällt. Dies führt dazu, dass auf allen angeschlossenen Displays ein [Warnungssymbol](../../../configuration/warning-icons.md) angezeigt und ein Eintrag zum Kernel-Log hinzugefügt wird.

Wenn Warnungen angezeigt werden, sollten Sie die Stromversorgung und/oder das Kabel verbessern, da eine geringe Leistung zu Problemen mit der Beschädigung von SD-Karten oder einem unregelmäßigen Verhalten des Pi selbst führen kann. zum Beispiel unerklärliche Abstürze.

Spannungen können aus verschiedenen Gründen abfallen, zum Beispiel wenn die Stromversorgung selbst nicht ausreicht, das Stromkabel zu dünne Drähte hat oder Sie USB-Geräte mit hoher Nachfrage angeschlossen haben.

## Backpowering

Backpowering tritt auf, wenn USB-Hubs keine Diode zur Verfügung stellen, um zu verhindern, dass der Hub den Host-Computer mit Strom versorgt. Andere Hubs liefern an jedem Port so viel Strom, wie Sie möchten. Bitte beachten Sie auch, dass einige Hubs den Raspberry Pi zurückspeisen. Dies bedeutet, dass die Hubs den Raspberry Pi über das USB-Kabeleingangskabel mit Strom versorgen, ohne dass ein separates Micro-USB-Stromkabel erforderlich ist, und den Spannungsschutz umgehen. Wenn Sie einen Hub verwenden, der zum Raspberry Pi zurückgespeist wird und der Hub einen Stromstoß erfährt, könnte Ihr Raspberry Pi möglicherweise beschädigt werden.