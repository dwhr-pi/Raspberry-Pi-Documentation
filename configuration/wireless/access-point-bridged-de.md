# Einrichten eines Raspberry Pi als überbrückten Wireless Access Point

Der Raspberry Pi kann als überbrückter drahtloser Zugangspunkt innerhalb eines bestehenden Ethernet-Netzwerks verwendet werden. Dadurch wird das Netzwerk auf drahtlose Computer und Geräte erweitert.

Wenn Sie ein eigenständiges drahtloses Netzwerk erstellen möchten, sollten Sie stattdessen die Einrichtung eines [routed access point] (./access-point-routed.md) in Betracht ziehen.

```
                                              +- RPi -----------+
                                          +---+  10.10.0.2      |         +- Laptop -----+
                                          |   |  WLAN-AP Brücke +-))) (((-+  WLAN-Client |
                                          |   |                 |         |  10.10.0.5   |
                                          |   +-----------------+         +--------------+
                   +- Router -----+       |
                   |  Firewall    |       |   +-- PC#2 -----+
(Internet)--- WAN -+  DHCP-Server +- LAN -+---+   10.10.0.3 |
                   |  10.10.0.1   |       |   +-------------+
                   +--------------+       |
                                          |   +-- PC#1 -----+
                                          +---+   10.10.0.4 |
                                              +-------------+
```

Ein überbrückter Wireless Access Point kann mit den integrierten Wireless-Funktionen des Raspberry Pi 4, Raspberry Pi 3 oder Raspberry Pi Zero W oder mit einem geeigneten USB-Wireless-Dongle, der den Access Point-Modus unterstützt, erstellt werden.
Es ist möglich, dass bei einigen USB-Dongles geringfügige Änderungen an ihren Einstellungen erforderlich sind. Wenn Sie Probleme mit einem drahtlosen USB-Dongle haben, überprüfen Sie bitte die [Foren](https://www.raspberrypi.org/forums/).

Diese Dokumentation wurde auf einem Raspberry Pi 3B mit einer Neuinstallation von Raspberry Pi OS Buster getestet.

<a name="intro"></a>
## Bevor du anfängst

* Stellen Sie sicher, dass Sie über Administratorzugriff auf Ihren Raspberry Pi verfügen. Das Netzwerk-Setup wird im Rahmen der Installation vollständig zurückgesetzt: Ein lokaler Zugriff mit Bildschirm und Tastatur, die mit Ihrem Raspberry Pi verbunden sind, wird empfohlen.

  **Hinweis:** Bei einer Remote-Installation über SSH,
    * Verbinden Sie sich mit Ihrem Raspberry Pi **nach Namen**, z.B. `ssh pi@raspberrypi.local`. Die IP-Adresse Ihres Raspberry Pi im Netzwerk **wird sich wahrscheinlich nach der Installation ändern**.
    * Seien Sie bereit, Bildschirm und Tastatur hinzuzufügen, falls Sie nach der Installation den Kontakt zu Ihrem Raspberry Pi verlieren.
* Verbinden Sie Ihren Raspberry Pi mit dem Ethernet-Netzwerk und booten Sie das Raspberry Pi OS.
* Stellen Sie sicher, dass das Raspberry Pi-Betriebssystem auf Ihrem Raspberry Pi [aktuell] ist (../../raspbian/updating.md) und starten Sie neu, wenn dabei Pakete installiert wurden.
* Halten Sie einen Wireless-Client (Laptop, Smartphone, ...) bereit, um Ihren neuen Access Point zu testen.

<a name="software-install"></a>
## Installieren Sie die Access Point-Software

Um als Bridged Access Point arbeiten zu können, muss auf dem Raspberry Pi das Access Point-Softwarepaket `hostapd` installiert sein:

```
sudo apt install hostapd
```
Aktivieren Sie den Wireless Access Point-Dienst und stellen Sie ihn so ein, dass er beim Booten Ihres Raspberry Pi gestartet wird:

```
sudo systemctl unmask hostapd
sudo systemctl aktivieren hostapd
```

Die Softwareinstallation ist abgeschlossen. Wir werden die Access Point Software später konfigurieren.

<a name="bridging"></a>
## Richten Sie die Netzwerkbrücke ein

Ein auf dem Raspberry Pi ausgeführtes Bridge-Netzwerkgerät verbindet das Ethernet- und das drahtlose Netzwerk über seine integrierten Schnittstellen.

### Erstellen Sie ein Bridge-Gerät und füllen Sie die Bridge

Fügen Sie ein Bridge-Netzwerkgerät namens `br0` hinzu, indem Sie mit dem folgenden Befehl eine Datei mit folgendem Inhalt erstellen:

```
sudo nano /etc/systemd/network/bridge-br0.netdev
```

Dateiinhalt:
```
[NetDev]
Name=br0
Art=Brücke
```

Um das Ethernet-Netzwerk mit dem drahtlosen Netzwerk zu überbrücken, fügen Sie zuerst die integrierte Ethernet-Schnittstelle (`eth0`) als Bridge-Mitglied hinzu, indem Sie die folgende Datei erstellen:

```
sudo nano /etc/systemd/network/br0-member-eth0.network
```

Dateiinhalt:
```
[Spiel]
Name=eth0

[Netzwerk]
Brücke=br0
```

**Hinweis:** Die Access Point-Software fügt der Bridge beim Start des Dienstes die drahtlose Schnittstelle `wlan0` hinzu. Es ist nicht erforderlich, eine Datei für diese Schnittstelle zu erstellen. Diese Situation ist speziell für drahtlose LAN-Schnittstellen.

Aktivieren Sie nun den Dienst `systemd-networkd`, um die Bridge beim Booten Ihres Raspberry Pi zu erstellen und zu füllen:

```
sudo systemctl enable systemd-networkd
```

### Definieren Sie die IP-Konfiguration des Bridge-Geräts

Netzwerkschnittstellen, die Mitglieder eines Bridge-Geräts sind, wird nie eine IP-Adresse zugewiesen, da sie über die Bridge kommunizieren. Das Bridge-Gerät selbst benötigt eine IP-Adresse, damit Sie Ihren Raspberry Pi im Netzwerk erreichen können.

`dhcpcd`, der DHCP-Client auf dem Raspberry Pi, fordert automatisch für jede aktive Schnittstelle eine IP-Adresse an. Wir müssen also die Verarbeitung der Schnittstellen `eth0` und `wlan0` blockieren und `dhcpcd` nur `br0` über DHCP konfigurieren lassen.

```
sudo nano /etc/dhcpcd.conf
```

Fügen Sie die folgende Zeile am Anfang der Datei hinzu (über der ersten `interface xxx`-Zeile, falls vorhanden):
```
denyinterfaces wlan0 eth0
```
Gehen Sie zum Ende der Datei und fügen Sie Folgendes hinzu:

```

Schnittstelle br0
```
Mit dieser Zeile wird die Schnittstelle `br0` gemäß den Voreinstellungen über DHCP konfiguriert. Speichern Sie die Datei, um die IP-Konfiguration des Geräts abzuschließen.

<a name="wifi-cc-rfkill"></a>
## Stellen Sie den drahtlosen Betrieb sicher

Länder auf der ganzen Welt regulieren die Nutzung von Telekommunikations-Funkfrequenzbändern, um einen störungsfreien Betrieb zu gewährleisten.
Das Linux-Betriebssystem hilft Benutzern [comply](https://wireless.wiki.kernel.org/en/developers/regulatory/statement) mit diesen Regeln, indem es Anwendungen erlaubt, mit einem zweibuchstabigen "WiFi-Ländercode" konfiguriert zu werden, z.B. `US` für einen Computer, der in den Vereinigten Staaten verwendet wird.

Im Raspberry Pi-Betriebssystem ist das 5-GHz-Funknetzwerk deaktiviert, bis ein WLAN-Ländercode vom Benutzer konfiguriert wurde, normalerweise im Rahmen der Erstinstallation (Details finden Sie auf den Drahtloskonfigurationsseiten in diesem [Abschnitt](README.md). )

Um sicherzustellen, dass WiFi-Radio auf Ihrem Raspberry Pi nicht blockiert wird, führen Sie den folgenden Befehl aus:

```
sudo rfkill wlan entsperren
```

Diese Einstellung wird beim Booten automatisch wiederhergestellt. Als nächstes definieren wir einen entsprechenden Ländercode in der Softwarekonfiguration des Access Points.

<a name="ap-config"></a>
## Konfigurieren Sie die Access Point-Software

Erstellen Sie die Konfigurationsdatei `hostapd`, die sich unter `/etc/hostapd/hostapd.conf` befindet, um die verschiedenen Parameter für Ihr neues drahtloses Netzwerk hinzuzufügen.

```
sudo nano /etc/hostapd/hostapd.conf
```

Fügen Sie die folgenden Informationen zur Konfigurationsdatei hinzu. Diese Konfiguration geht davon aus, dass wir Kanal 7 mit dem Netzwerknamen `NameOfNetwork` und dem Passwort `AardvarkBadgerHedgehog` verwenden. Beachten Sie, dass Name und Passwort **keine** Anführungszeichen enthalten sollten. Die Passphrase sollte zwischen 8 und 64 Zeichen lang sein.

```
country_code=GB
Schnittstelle=wlan0
Brücke=br0
ssid=NameOfNetwork
hw_mode=g
Kanal=7
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=ErdferkelBadgerIgel
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```
Beachten Sie die Zeilen `interface=wlan0` und `bridge=br0`: Diese weisen `hostapd` an, die `wlan0`-Schnittstelle als Bridge-Mitglied zu `br0` hinzuzufügen, wenn der Access Point startet und die Brücke zwischen Ethernet und Wireless vervollständigt.

Beachten Sie die Zeile `country_code=GB`: Sie konfiguriert den Computer so, dass er die richtigen Funkfrequenzen im Vereinigten Königreich verwendet. **Passen Sie diese Zeile an** und geben Sie den aus zwei Buchstaben bestehenden ISO-Code Ihres Landes an. In [Wikipedia](https://en.wikipedia.org/wiki/ISO_3166-1) finden Sie eine Liste der zweibuchstabigen ISO 3166-1-Ländercodes.

Um das 5-GHz-Band zu verwenden, können Sie den Betriebsmodus von `hw_mode=g` auf `hw_mode=a` ändern. Mögliche Werte für `hw_mode` sind:
 - a = IEEE 802.11a (5 GHz) (ab Raspberry Pi 3B+)
 - b = IEEE 802.11b (2,4 GHz)
 - g = IEEE 802.11g (2,4 GHz)

Beachten Sie, dass Sie beim Ändern des "hw_mode" möglicherweise auch den "Kanal" ändern müssen - eine Liste der zulässigen Kombinationen finden Sie in [Wikipedia](https://en.wikipedia.org/wiki/List_of_WLAN_channels).

<a name="conclusion"></a>
## Führen Sie Ihren neuen drahtlosen Zugangspunkt aus

Starten Sie nun Ihren Raspberry Pi neu und vergewissern Sie sich, dass der Wireless Access Point automatisch verfügbar wird.

```
sudo systemctl reboot
```
Sobald Ihr Raspberry Pi neu gestartet wurde, suchen Sie mit Ihrem drahtlosen Client nach drahtlosen Netzwerken. Die Netzwerk-SSID, die Sie in der Datei `/etc/hostapd/hostapd.conf` angegeben haben, sollte jetzt vorhanden sein und mit dem angegebenen Passwort zugänglich sein.

Wenn Ihr WLAN-Client Zugriff auf das lokale Netzwerk und das Internet hat, herzlichen Glückwunsch zur Einrichtung Ihres neuen Access Points!

Wenn Sie auf Schwierigkeiten stoßen, wenden Sie sich an die [Foren](https://www.raspberrypi.org/forums/), um Hilfe zu erhalten. Bitte beziehen Sie sich in Ihrer Nachricht auf diese Seite.