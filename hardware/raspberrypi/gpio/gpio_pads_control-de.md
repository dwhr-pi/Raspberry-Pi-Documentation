# GPIO-Pads-Steuerung

Diese Seite erklärt die aktuellen Fähigkeiten der Raspberry Pi GPIO-Pins. Sie gilt für alle Modelle bis einschließlich 3B+ Modell. Zu beachten ist, dass die GPIO-Laufwerkstärken keinen maximalen Strom angeben, sondern einen maximalen Strom, unter dem das Pad noch die Spezifikation erfüllt. Sie sollten die GPIO-Laufwerkstärken so einstellen, dass sie mit dem angeschlossenen Gerät übereinstimmen, damit das Gerät ordnungsgemäß funktioniert.

#### Wie die Antriebsstärke gesteuert wird

Innerhalb des Pads befinden sich mehrere Treiber parallel. Wenn die Antriebsstärke niedrig (000) eingestellt ist, sind die meisten davon dreistufig, sodass sie nichts zum Ausgangsstrom hinzufügen. Wird die Antriebsstärke erhöht, werden immer mehr Treiber parallel geschaltet. Das folgende Diagramm zeigt dieses Verhalten:

![GPIO-Laufwerkstärkediagramm](./images/pi_gpio_drive_strength_diagram.png)

#### Was bedeutet der aktuelle Wert?

**Der Stromwert gibt den maximalen Strom an, bei dem das Pad noch die Spezifikation erfüllt**.

1. Es ist **nicht** der Strom, den das Pad liefert
1. Es ist **keine** Strombegrenzung, damit das Pad nicht explodiert

Der Pad-Ausgang ist eine Spannungsquelle:

* Bei hoher Einstellung versucht das Pad, den Ausgang auf die Schienenspannung zu treiben, die auf dem Raspberry Pi 3V3 (3,3 Volt) beträgt.
* Wenn niedrig eingestellt, versucht das Pad, den Ausgang auf Masse (0 Volt) zu treiben.

Das Pad versucht, den Ausgang hoch oder niedrig zu treiben. Der Erfolg hängt von den Anforderungen des Verbundenen ab. Wenn das Pad mit Masse kurzgeschlossen ist, kann es nicht hochfahren. Es wird tatsächlich versuchen, so viel Strom wie möglich zu liefern, und der Strom wird nur durch den Innenwiderstand begrenzt.

Wenn der Bremsbelag hochgefahren wird und mit Masse kurzgeschlossen wird, wird er zu gegebener Zeit ausfallen. Das gleiche gilt, wenn Sie es an 3V3 anschließen und es niedrig fahren.

Die Einhaltung der Spezifikation wird durch die garantierten Spannungspegel bestimmt. Da die Pads digital sind, gibt es zwei Spannungspegel, hoch und niedrig. Die I/O-Ports haben zwei Parameter, die sich mit dem Ausgangspegel befassen:

* V<sub>IL</sub>, die maximale Low-Level-Spannung (0,9 V bei 3 V3 VDD IO)
* V<sub>IH</sub>, die minimale Hochpegelspannung (1,6 V bei 3 V3 VDD IO)

V<sub>IL</sub>=0,9V bedeutet, dass bei einem niedrigen Ausgang <= 0,9V beträgt.
V<sub>IH</sub>=1.6V bedeutet, dass wenn der Ausgang High ist, er >= 1.6V ist.
   
Somit bedeutet eine Antriebsstärke von 16mA:

Wenn Sie das Pad hoch einstellen, können Sie bis zu 16 mA ziehen und die Ausgangsspannung beträgt garantiert >=V<sub>IH</sub>. Dies bedeutet auch, dass, wenn Sie eine Antriebsstärke von 2 mA einstellen und 16 mA ziehen, die Spannung **nicht** V<sub>IH</sub>, sondern niedriger ist. Tatsächlich ist es möglicherweise nicht hoch genug, um von einem externen Gerät als hoch angesehen zu werden.

Weitere Informationen zu den physikalischen Eigenschaften der GPIO-Pins gibt es [hier](./README.md). Beachten Sie, dass es bei den Compute Module-Geräten möglich ist, die VDD IO vom Standard 3V3 zu ändern. In diesem Fall ändern sich V<sub>IL</sub> und V<sub>IH</sub> entsprechend der Tabelle auf der verlinkten Seite.

#### Warum stelle ich nicht alle meine Pads auf den maximalen Strom ein?

Zwei Gründe:

1. Die Versorgung des Raspberry Pi 3V3 wurde mit einem maximalen Strom von ~3mA pro GPIO-Pin ausgelegt. Wenn Sie jeden Pin mit 16mA belasten, beträgt der Gesamtstrom 272mA. Die 3V3-Versorgung bricht unter dieser Last zusammen.
1. Es treten große Stromspitzen auf, insbesondere wenn Sie eine kapazitive Last haben. Das wird um alle anderen Pins in der Nähe "hüpfen". Es ist wahrscheinlich, dass es zu Störungen der SD-Karte oder sogar des SDRAM-Verhaltens kommt.

#### Was ist ein sicherer Strom?

Die gesamte Elektronik der Pads ist für 16mA ausgelegt. Das ist ein sicherer Wert, unter dem Sie das Gerät nicht beschädigen. Auch wenn Sie die Antriebsstärke auf 2mA einstellen und dann so laden, dass 16mA herauskommt, wird das Gerät dadurch nicht beschädigt. Ansonsten gibt es keinen garantierten maximalen sicheren Strom.

### GPIO-Adressen

* 0x 7e10 002c PADS (GPIO 0-27)
* 0x 7e10 0030 PADS (GPIO 28-45)
* 0x 7e10 0034 PADS (GPIO 46-53)

Bits | Feldname | Beschreibung | Typ | Zurücksetzen
--- | --- | --- | --- | ---
31:24 | KENNWORT | Muss beim Schreiben 5A betragen; versehentliches Schreibschutz-Passwort | W | 0
23:5 | | **Reserviert** - Als 0 schreiben, als egal lesen | |
4 | SCHWUNG | Anstiegsgeschwindigkeit; 0 = Anstiegsgeschwindigkeit begrenzt; 1 = Anstiegsgeschwindigkeit nicht begrenzt | RW | 0x1
3 | HYST | Eingangshysterese aktivieren; 0 = deaktiviert; 1 = aktiviert | RW | 0x1
2:0 | ANTRIEB | Antriebsstärke, siehe Aufschlüsselungsliste unten | RW | 0x3

Achten Sie auf Einschränkungen bei SSO (Simultaneous Switching Outputs), die geräteabhängig sind sowie von der Qualität und dem Layout der Leiterplatte, der Anzahl und Qualität der Entkopplungskondensatoren, der Art der Belastung der Pads (Widerstand, Kapazität) und andere Faktoren, die außerhalb der Kontrolle von Raspberry Pi liegen.

#### Liste der Antriebsstärke

  * 0 = 2 mA
  * 1 = 4 mA
  * 2 = 6 mA
  * 3 = 8 mA
  * 4 = 10 mA
  * 5 = 12 mA
  * 6 = 14 mA
  * 7 = 16 mA