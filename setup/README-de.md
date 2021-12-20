# Aufstellen

Eine Anleitung zum Einrichten Ihres Raspberry Pi

## Was wirst du brauchen

### Unverzichtbar (für den allgemeinen Gebrauch)

- [SD-Karte](../installation/sd-cards.md)
    - Wir empfehlen mindestens 8 GB Klasse 4 oder Klasse 10 microSD-Karte. Um Zeit zu sparen, können Sie eine Karte mit vorinstalliertem [NOOBS](../installation/noobs.md) oder [Raspberry Pi OS](../installation/installing-images/README.md) erwerben, obwohl Die Einrichtung einer eigenen Karte ist einfach.
- [Display- und Verbindungskabel](monitor-connection.md)
    - Jeder HDMI/DVI-Monitor oder Fernseher sollte als Anzeige für den Pi funktionieren. Verwenden Sie für beste Ergebnisse ein Display mit HDMI-Eingang; auch andere Anschlussarten für ältere Geräte stehen zur Verfügung.
- Tastatur und Maus
    - Jede Standard-USB-Tastatur und -Maus funktioniert mit Ihrem Raspberry Pi.
    - Drahtlose Tastaturen und Mäuse funktionieren, wenn sie bereits gekoppelt sind.
    - Für Konfigurationsoptionen für das Tastaturlayout siehe [raspi-config](../configuration/raspi-config.md).
- [Stromversorgung](../hardware/raspberrypi/power/README.md)
    - Der Pi wird von einem USB Micro [Modelle vor 4B] oder USB Typ-C [Modell 4B] Netzteil mit Strom versorgt (wie die meisten Standard-Handy-Ladegeräte).
    - Sie benötigen ein hochwertiges Netzteil, das mindestens 3 A bei 5 V für das Modell 4B, 2 A bei 5 V für das Modell 3B und 3B+ oder 700 mA bei 5 V für die früheren Pi-Modelle mit geringerer Leistung liefern kann. Wir empfehlen die Verwendung des [offiziellen Raspberry Pi-Netzteils](https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/), das speziell für Raspberry Pi entwickelt wurde.
    - Niedrigstrom-Netzteile (~ 700 mA) funktionieren für den grundlegenden Gebrauch, führen jedoch wahrscheinlich zu einem Neustart des Pi, wenn er zu viel Strom verbraucht. Sie sind nicht für die Verwendung mit dem Pi 3 oder 4 geeignet.

### Optional

- Ethernet-(Netzwerk-)Kabel [nur Modell B/B+/2B/3B/3B+/4B]
    - Ein Ethernet-Kabel wird verwendet, um Ihren Pi mit einem lokalen Netzwerk und dem Internet zu verbinden.
- [USB-Wireless-Dongle](../configuration/wireless/README.md)
    - Nur erforderlich, wenn Sie eine Wireless-Konnektivität benötigen und ein älteres Modell ohne integrierte Wireless-Funktionalität verwenden.
- Audiokabel
    - Audio kann über Lautsprecher oder Kopfhörer mit einer Standard-3,5-mm-Buchse wiedergegeben werden.
    - Ohne HDMI-Kabel ist zur Tonerzeugung ein Audiokabel erforderlich.
    - Wenn Sie ein HDMI-Kabel verwenden, um einen Monitor mit Lautsprechern anzuschließen, ist kein separates Audiokabel erforderlich, da Audio direkt über das Display wiedergegeben werden kann; es ist jedoch möglich, einen anzuschließen, wenn Sie das Audio lieber über andere Lautsprecher abspielen möchten - dies erfordert [configuration](../configuration/audio-config.md).

## Fehlerbehebung

Suchen Sie bei Problemen während der Einrichtung [die Foren](https://www.raspberrypi.org/forums/) nach einer Lösung. Wenn Sie keinen finden können, posten Sie bitte Ihr Problem und geben Sie so viele Details wie möglich an.