# SD-Karten

Der Raspberry Pi sollte mit jeder kompatiblen SD-Karte funktionieren, obwohl es einige Richtlinien gibt, die befolgt werden sollten:

## SD-Kartengröße (Kapazität)

Für die Installation von Raspberry Pi OS mit Desktop und empfohlener Software (Full) über NOOBS beträgt die minimale Kartengröße 16 GB. Für die Image-Installation von Raspberry Pi OS mit Desktop und empfohlener Software beträgt die minimale Kartengröße 8 GB. Für Raspberry Pi OS Lite Image-Installationen empfehlen wir mindestens 4 GB. Einige Distributionen, zum Beispiel LibreELEC und Arch, können auf viel kleineren Karten laufen. Wenn Sie planen, eine Karte mit 64 GB oder mehr mit NOOBS zu verwenden, lesen Sie zuerst [diese Seite](sdxc_formatting.md).

**Hinweis:** Aufgrund einer Einschränkung in den Versionen von SoCs, die in Raspberry Pi Zero, 1 und 2 verwendet werden, beträgt die Größenbeschränkung für die SD-Kartenpartition 256 GB. Ab dem Raspberry Pi 3 gilt diese Einschränkung nicht mehr.

## SD-Kartenklasse

Die Kartenklasse bestimmt die anhaltende Schreibgeschwindigkeit für die Karte; eine Karte der Klasse 4 kann mit 4 MB/s schreiben, während eine Karte der Klasse 10 in der Lage sein sollte, 10 MB/s zu erreichen. Es sollte jedoch beachtet werden, dass dies nicht bedeutet, dass eine Karte der Klasse 10 eine Karte der Klasse 4 für den allgemeinen Gebrauch übertrifft, da diese Schreibgeschwindigkeit oft auf Kosten der Lesegeschwindigkeit und erhöhter Suchzeiten erreicht wird.

## Physische Größe der SD-Karte

Das ursprüngliche Raspberry Pi Model A und Raspberry Pi Model B erfordern SD-Karten in voller Größe. Ab Modell B+ (2014) wird eine Micro-SD-Karte benötigt.

## Fehlerbehebung

Wir empfehlen den Kauf der Raspberry Pi SD-Karte, die [hier](https://shop.pimoroni.com/products/noobs-8gb-sd-card) erhältlich ist, sowie bei anderen Händlern; Dies ist eine 8-GB-Micro-SD-Karte der Klasse 6 (mit einem SD-Adapter in voller Größe), die fast alle anderen SD-Karten auf dem Markt übertrifft und eine preiswerte Lösung darstellt.

Wenn Sie Probleme mit der Beschädigung Ihrer SD-Karten haben, stellen Sie sicher, dass Sie die folgenden Schritte ausführen:

1. Stellen Sie sicher, dass Sie eine echte SD-Karte verwenden. Es gibt viele billige SD-Karten, die tatsächlich kleiner sind als beworben oder die nicht sehr lange halten.
2. Stellen Sie sicher, dass Sie ein hochwertiges Netzteil verwenden. Sie können Ihre Stromversorgung überprüfen, indem Sie die Spannung zwischen TP1 und TP2 auf dem Raspberry Pi messen; Wenn diese bei komplexen Aufgaben unter 4,75 V fällt, ist dies höchstwahrscheinlich ungeeignet.
3. Stellen Sie sicher, dass Sie ein hochwertiges USB-Kabel für die Stromversorgung verwenden. Bei Verwendung eines Netzteils mit geringerer Qualität kann die TP1->TP2-Spannung unter 4,75 V fallen. Dies ist im Allgemeinen auf den Widerstand der Drähte im USB-Stromkabel zurückzuführen; Um Geld zu sparen, enthalten USB-Kabel so wenig Kupfer wie möglich, und über die Länge des Kabels können bis zu 1 V (oder 1 W) verloren gehen.
4. Stellen Sie sicher, dass Sie Ihren Raspberry Pi ordnungsgemäß herunterfahren, bevor Sie ihn ausschalten. Geben Sie "sudo halt" ein und warten Sie, bis der Pi durch Blinken der Aktivitäts-LED signalisiert, dass er bereit ist, ausgeschaltet zu werden.
5. Schließlich wurde Korruption beobachtet, wenn Sie den Pi übertakten. Dieses Problem wurde zuvor behoben, obwohl die verwendete Problemumgehung bedeuten kann, dass es immer noch auftreten kann. Wenn Sie nach Überprüfung der obigen Schritte immer noch Probleme mit der Beschädigung haben, teilen Sie uns dies bitte mit.