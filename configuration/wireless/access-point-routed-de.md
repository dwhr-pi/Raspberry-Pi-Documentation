# Einrichten eines Raspberry Pi als gerouteten drahtlosen Zugangspunkt

Ein Raspberry Pi in einem Ethernet-Netzwerk kann als drahtloser Zugangspunkt verwendet werden, wodurch ein sekundäres Netzwerk entsteht. Das resultierende neue drahtlose Netzwerk wird vollständig vom Raspberry Pi verwaltet.

Wenn Sie ein vorhandenes Ethernet-Netzwerk auf drahtlose Clients erweitern möchten, sollten Sie stattdessen einen [Bridged Access Point.](./access-point-bridged.md) einrichten.

```
                                              +- RPi --------+
                                          +---+  10.10.0.2   |         +- Laptop -----+
                                          |   |  WLAN-AP     +-))) (((-+  WLAN-Client |
                                          |   |  192.168.4.1 |         |  192.168.4.2 |
                                          |   +--------------+         +--------------+
                   +- Router -----+       |
                   |  Firewall    |       |   +- PC#2 -------+
(Internet)--- WAN -+  DHCP-Server +- LAN -+---+  10.10.0.3   |
                   |  10.10.0.1   |       |   +--------------+
                   +--------------+       |
                                          |   +- PC#1 -------+
                                          +---+  10.10.0.4   |
                                              +--------------+
```
Ein gerouteter Wireless Access Point kann mit den integrierten Wireless-Funktionen des Raspberry Pi 4, Raspberry Pi 3 oder Raspberry Pi Zero W oder mit einem geeigneten USB-Wireless-Dongle, der den Access Point-Modus unterstützt, erstellt werden.
Es ist möglich, dass bei einigen USB-Dongles geringfügige Änderungen an ihren Einstellungen erforderlich sind. Wenn Sie Probleme mit einem drahtlosen USB-Dongle haben, überprüfen Sie bitte die [Foren](https://www.raspberrypi.org/forums/).

Diese Dokumentation wurde auf einem Raspberry Pi 3B mit einer Neuinstallation von Raspberry Pi OS Buster getestet.

<a name="intro"></a>
## Bevor du anfängst

* Stellen Sie sicher, dass Sie über Administratorzugriff auf Ihren Raspberry Pi verfügen. Das Netzwerk-Setup wird im Rahmen der Installation angepasst: Ein lokaler Zugriff mit Bildschirm und Tastatur, die mit Ihrem Raspberry Pi verbunden sind, wird empfohlen.
* Verbinden Sie Ihren Raspberry Pi mit dem Ethernet-Netzwerk und booten Sie das Raspberry Pi OS.
* Stellen Sie sicher, dass das Raspberry Pi-Betriebssystem auf Ihrem Raspberry Pi [aktuell] ist (../../raspbian/updating.md) und starten Sie neu, wenn dabei Pakete installiert wurden.
* Beachten Sie die IP-Konfiguration des Ethernet-Netzwerks, mit dem der Raspberry Pi verbunden ist:
    * In diesem Dokument gehen wir davon aus, dass das IP-Netzwerk „10.10.0.0/24“ im Ethernet-LAN ​​konfiguriert ist und der Raspberry Pi das IP-Netzwerk „192.168.4.0/24“ für drahtlose Clients verwaltet.
    * Bitte wählen Sie ein anderes IP-Netzwerk für WLAN, z.B. `192.168.10.0/24`, wenn das IP-Netzwerk `192.168.4.0/24` bereits von Ihrem Ethernet-LAN ​​verwendet wird.
* Halten Sie einen Wireless-Client (Laptop, Smartphone, ...) bereit, um Ihren neuen Access Point zu testen.

<a name="software-install"></a>
## Installieren Sie den Zugangspunkt und die Netzwerkverwaltungssoftware

Um als Access Point arbeiten zu können, muss auf dem Raspberry Pi das Access Point-Softwarepaket `hostapd` installiert sein:

```
sudo apt install hostapd
```
Aktivieren Sie den Wireless Access Point-Dienst und stellen Sie ihn so ein, dass er beim Booten Ihres Raspberry Pi gestartet wird:

```
sudo systemctl unmask hostapd
sudo systemctl aktivieren hostapd
```

Um drahtlosen Clients Netzwerkverwaltungsdienste (DNS, DHCP) bereitzustellen, muss auf dem Raspberry Pi das Softwarepaket `dnsmasq` installiert sein:

```
sudo apt install dnsmasq
```
Zum Schluss installieren Sie `netfilter-persistent` und sein Plugin `iptables-persistent`. Dieses Dienstprogramm hilft, indem es Firewall-Regeln speichert und beim Booten des Raspberry Pi wiederherstellt:

```
sudo DEBIAN_FRONTEND=noninteractive apt install -y netfilter-persistent iptables-persistent
```
Die Softwareinstallation ist abgeschlossen. Wir werden die Softwarepakete später konfigurieren.

<a name="routing"></a>
## Richten Sie den Netzwerkrouter ein

Der Raspberry Pi wird ein eigenständiges drahtloses Netzwerk ausführen und verwalten. Es wird auch zwischen den drahtlosen und Ethernet-Netzwerken routen und so den Internetzugang für drahtlose Clients bereitstellen. Wenn Sie es vorziehen, können Sie das Routing überspringen, indem Sie den Abschnitt "Routing und IP-Masquerading aktivieren" unten überspringen und das drahtlose Netzwerk vollständig isoliert betreiben.

### Definieren Sie die IP-Konfiguration der drahtlosen Schnittstelle

Der Raspberry Pi betreibt einen DHCP-Server für das drahtlose Netzwerk; dies erfordert eine statische IP-Konfiguration für die Funkschnittstelle (`wlan0`) im Raspberry Pi.
Der Raspberry Pi fungiert auch als Router im drahtlosen Netzwerk, und wir geben ihm wie üblich die erste IP-Adresse im Netzwerk: `192.168.4.1`.

Um die statische IP-Adresse zu konfigurieren, bearbeiten Sie die Konfigurationsdatei für `dhcpcd` mit:

```
sudo nano /etc/dhcpcd.conf
```

Gehen Sie zum Ende der Datei und fügen Sie Folgendes hinzu:

```
interface wlan0
    static ip_address=192.168.4.1/24
    nohook wpa_supplicant
```

### Routing und IP-Masquerading aktivieren

In diesem Abschnitt wird der Raspberry Pi so konfiguriert, dass drahtlose Clients auf Computer im Hauptnetzwerk (Ethernet) und von dort auf das Internet zugreifen können.
**HINWEIS:** Wenn Sie den Zugriff von Wireless-Clients auf das Ethernet-Netzwerk und das Internet blockieren möchten, überspringen Sie diesen Abschnitt.

Um das Routing zu aktivieren, d. h. den Datenverkehr von einem Netzwerk zum anderen im Raspberry Pi fließen zu lassen, erstellen Sie mit dem folgenden Befehl eine Datei mit folgendem Inhalt:
```
sudo nano /etc/sysctl.d/routed-ap.conf
```
Dateiinhalt:
```
# https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md
# IPv4-Routing aktivieren
net.ipv4.ip_forward=1
```
Durch Aktivieren des Routings können Hosts aus dem Netzwerk `192.168.4.0/24` das LAN und den Hauptrouter in Richtung Internet erreichen. Um den Datenverkehr zwischen Clients in diesem fremden WLAN und dem Internet zu ermöglichen, ohne die Konfiguration des Hauptrouters zu ändern, kann der Raspberry Pi die IP-Adresse von WLAN-Clients durch seine eigene IP-Adresse im LAN mit einer "Masquerade"-Firewall-Regel ersetzen .
* Der Hauptrouter sieht den gesamten ausgehenden Datenverkehr von drahtlosen Clients als vom Raspberry Pi kommend, was die Kommunikation mit dem Internet ermöglicht.
* Der Raspberry Pi empfängt den gesamten eingehenden Datenverkehr, ersetzt die IP-Adressen zurück und leitet den Datenverkehr an den ursprünglichen drahtlosen Client weiter.

Dieser Vorgang wird konfiguriert, indem eine einzelne Firewall-Regel im Raspberry Pi hinzugefügt wird:

```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
Speichern Sie nun die aktuellen Firewall-Regeln für IPv4 (einschließlich der obigen Regel) und IPv6, die beim Booten vom Dienst `netfilter-persistent` geladen werden:
```
sudo netfilter-persistent save
```
Filterregeln werden im Verzeichnis `/etc/iptables/` gespeichert. Wenn Sie in Zukunft die Konfiguration Ihrer Firewall ändern, speichern Sie die Konfiguration vor dem Neustart.

### Konfigurieren Sie die DHCP- und DNS-Dienste für das drahtlose Netzwerk

Die DHCP- und DNS-Dienste werden von `dnsmasq` bereitgestellt. Die Standardkonfigurationsdatei dient als Vorlage für alle möglichen Konfigurationsoptionen, wobei wir nur wenige benötigen. Es ist einfacher, mit einer leeren Datei zu beginnen.

Benennen Sie die Standardkonfigurationsdatei um und bearbeiten Sie eine neue:

```
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
sudo nano /etc/dnsmasq.conf
```
Fügen Sie der Datei Folgendes hinzu und speichern Sie sie:

```
interface wlan0 # Abhörende Schnittstelle
dhcp-Bereich=192.168.4.2,192.168.4.20,255.255.255.0,24h
                # Pool von IP-Adressen, die über DHCP bereitgestellt werden
domain=wlan # Lokale WLAN-DNS-Domain
Adresse=/gw.wlan/192.168.4.1
                # Alias ​​für diesen Router
```

Der Raspberry Pi liefert IP-Adressen zwischen `192.168.4.2` und `192.168.4.20` mit einer Lease-Time von 24 Stunden an drahtlose DHCP-Clients. Sie sollten den Raspberry Pi unter dem Namen `gw.wlan` von WLAN-Clients erreichen können.

Es gibt viele weitere Optionen für `dnsmasq`; Einzelheiten finden Sie in der Standardkonfigurationsdatei (`/etc/dnsmasq.conf`) oder in der [Online-Dokumentation](http://www.thekelleys.org.uk/dnsmasq/doc.html).

<a name="wifi-cc-rfkill"></a>
## Stellen Sie den drahtlosen Betrieb sicher

Länder auf der ganzen Welt regulieren die Nutzung von Telekommunikations-Funkfrequenzbändern, um einen störungsfreien Betrieb zu gewährleisten.
Das Linux-Betriebssystem hilft Benutzern [comply](https://wireless.wiki.kernel.org/en/developers/regulatory/statement) mit diesen Regeln, indem es Anwendungen erlaubt, mit einem zweibuchstabigen "WiFi-Ländercode" konfiguriert zu werden, z.B. `US` für einen Computer, der in den Vereinigten Staaten verwendet wird.

Im Raspberry Pi-Betriebssystem ist das 5-GHz-Funknetzwerk deaktiviert, bis ein WLAN-Ländercode vom Benutzer konfiguriert wurde, normalerweise im Rahmen der Erstinstallation (Details finden Sie auf den Drahtloskonfigurationsseiten in diesem [Abschnitt](README.md). )

Um sicherzustellen, dass WiFi-Radio auf Ihrem Raspberry Pi nicht blockiert wird, führen Sie den folgenden Befehl aus:

```
sudo rfkill unblock wlan
```

Diese Einstellung wird beim Booten automatisch wiederhergestellt. Als nächstes definieren wir einen entsprechenden Ländercode in der Softwarekonfiguration des Access Points.

<a name="ap-config"></a>
## Konfigurieren Sie die Access Point-Software

Erstellen Sie die Konfigurationsdatei `hostapd`, die sich unter `/etc/hostapd/hostapd.conf` befindet, um die verschiedenen Parameter für Ihr neues drahtloses Netzwerk hinzuzufügen.

```
sudo nano /etc/hostapd/hostapd.conf
```

Fügen Sie die folgenden Informationen zur Konfigurationsdatei hinzu. Diese Konfiguration geht davon aus, dass wir Kanal 7 mit dem Netzwerknamen `NameDesNetzwerks` und dem Passwort `ErdferkelDachsIgel` verwenden. Beachten Sie, dass Name und Passwort **keine** Anführungszeichen enthalten sollten. Die Passphrase sollte zwischen 8 und 64 Zeichen lang sein.

```
country_code=GB
interface=wlan0
ssid=NameDesNetzwerks
hw_mode=g
channel=7
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=ErdferkelDachsIgel
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

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

Wenn SSH auf dem Raspberry Pi aktiviert ist, sollte es möglich sein, sich von Ihrem WLAN-Client aus wie folgt zu verbinden, vorausgesetzt, das `pi`-Konto ist vorhanden: `ssh pi@192.168.4.1` oder `ssh pi@gw.wlan`

Wenn Ihr WLAN-Client Zugriff auf Ihren Raspberry Pi (und das Internet, wenn Sie Routing eingerichtet haben) hat, herzlichen Glückwunsch zum Einrichten Ihres neuen Access Points!

Wenn Sie auf Schwierigkeiten stoßen, wenden Sie sich an die [Foren](https://www.raspberrypi.org/forums/), um Hilfe zu erhalten. Bitte beziehen Sie sich in Ihrer Nachricht auf diese Seite.