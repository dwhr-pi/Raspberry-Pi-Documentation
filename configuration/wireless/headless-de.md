# Einrichten eines Raspberry Pi Headless

Wenn Sie keinen Monitor oder keine Tastatur verwenden, um Ihren Pi (bekannt als Headless) zu betreiben, Sie aber dennoch einige drahtlose Einstellungen vornehmen müssen, gibt es eine Möglichkeit, drahtloses Netzwerk und SSH beim Erstellen eines Images zu aktivieren.

Sobald ein Image auf einer SD-Karte erstellt wurde, kann durch Einlegen in einen Kartenleser auf einem Linux- oder Windows-Rechner auf den [Boot-Ordner] (../boot_folder.md) zugegriffen werden. Das Hinzufügen bestimmter Dateien zu diesem Ordner aktiviert bestimmte Setup-Funktionen beim ersten Start des Pi selbst.

## Drahtloses Netzwerk einrichten

Sie müssen eine `wpa_supplicant.conf`-Datei für Ihr spezielles drahtloses Netzwerk definieren. Legen Sie diese Datei in den `boot`-Ordner, und wenn der Pi zum ersten Mal bootet, kopiert er diese Datei an den richtigen Ort im Linux-Root-Dateisystem und verwendet diese Einstellungen, um das drahtlose Netzwerk zu starten. Je nach Betriebssystem und Editor, auf dem Sie dies erstellen, kann die Datei falsche Zeilenumbrüche oder die falsche Dateierweiterung haben. Stellen Sie also sicher, dass Sie einen Editor verwenden, der dies berücksichtigt. Linux erwartet das Zeilenvorschub (LF) Newline-Zeichen. Weitere Informationen finden Sie in diesem [Wikipedia-Artikel](https://en.wikipedia.org/wiki/Newline).

Beispiel für die Datei `wpa_supplicant.conf`:
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<Hier 2 Buchstaben ISO 3166-1 Ländercode einfügen>

Netzwerk={
 ssid="<Name Ihres WLAN>"
 psk="<Passwort für Ihr WLAN>"
}
```

Beachten Sie, dass einige ältere drahtlose Dongles keine 5-GHz-Netzwerke unterstützen.

Weitere Informationen zur Datei `wpa_supplicant.conf` finden Sie [hier](wireless-cli.md). Siehe [Wikipedia](https://en.wikipedia.org/wiki/ISO_3166-1) für eine Liste der 2-Buchstaben-ISO 3166-1-Ländercodes.

## Fernzugriff

Ohne Tastatur oder Monitor benötigen Sie eine Möglichkeit, auf den kopflosen Raspberry Pi zuzugreifen. Dafür gibt es verschiedene Möglichkeiten, Details dazu finden Sie [hier](../../remote-access/README.md).
