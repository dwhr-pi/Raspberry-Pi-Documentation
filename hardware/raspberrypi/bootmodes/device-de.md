# Bootmodus des USB-Geräts

Die folgenden Geräte können im USB-Gerätestartmodus booten:

* Pi-Rechenmodul
* Pi-Rechenmodul 3
* Pi Null
* Pi Null W
* Pi A
* Pi A+
* Pi 3A+

Wenn dieser Bootmodus aktiviert ist (normalerweise nach einem fehlgeschlagenen Booten von der SD-Karte), versetzt der Raspberry Pi seinen USB-Port in den Gerätemodus und wartet auf einen USB-Reset vom Host. Beispielcode, der zeigt, wie der Host mit dem Pi kommunizieren muss, finden Sie [hier](https://github.com/raspberrypi/usbboot).

Der Host sendet zuerst eine Struktur an den Geräte-Down-Control-Endpunkt 0. Diese enthält die Größe und Signatur für den Bootvorgang (Sicherheit ist nicht aktiviert, daher ist keine Signatur erforderlich). Zweitens wird der Code an Endpunkt 1 (bootcode.bin) übertragen. Schließlich antwortet das Gerät mit einem Erfolgscode von:

* 0 - Erfolg
* 0x80 - Fehlgeschlagen
