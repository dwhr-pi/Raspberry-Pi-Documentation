# IP Adresse

Jedem mit einem lokalen Netzwerk verbundenen Gerät wird eine IP-Adresse zugewiesen.

Um sich von einem anderen Computer mit [SSH](ssh/README.md) oder [VNC](vnc/README.md) mit Ihrem Raspberry Pi zu verbinden, müssen Sie die IP-Adresse des Pi kennen. Dies ist einfach, wenn Sie ein Display angeschlossen haben, und es gibt eine Reihe von Methoden, um es von einem anderen Computer im Netzwerk aus zu finden.

## Verwenden des Pi mit einem Display

Wenn Sie statt des Desktops über die Befehlszeile booten, sollte Ihre IP-Adresse in den letzten Nachrichten vor der Anmeldeaufforderung angezeigt werden.

Verwenden Sie das Terminal (booten Sie über die Befehlszeile oder öffnen Sie ein Terminalfenster vom Desktop aus), geben Sie einfach "hostname -I" ein, wodurch die IP-Adresse Ihres Pi angezeigt wird.

## Verwenden des Pi Headless (ohne Display)

Es ist möglich, die IP-Adresse Ihres Pi zu finden, ohne mit einer der folgenden Methoden eine Verbindung zu einem Bildschirm herzustellen:

### Liste der Router-Geräte

Navigieren Sie in einem Webbrowser zur IP-Adresse Ihres Routers, z. `http://192.168.1.1`, das normalerweise auf einem Etikett auf Ihrem Router aufgedruckt ist; Dies führt Sie zu einem Kontrollfeld. Melden Sie sich anschließend mit Ihren Zugangsdaten an, die in der Regel auch auf dem Router ausgedruckt oder Ihnen in den Begleitpapieren zugesandt werden. Navigieren Sie zur Liste der verbundenen Geräte oder ähnlichem (alle Router sind unterschiedlich), und Sie sollten einige Geräte sehen, die Sie kennen. Einige Geräte werden als PCs, Tablets, Telefone, Drucker usw. erkannt. Sie sollten also einige erkennen und ausschließen, um herauszufinden, welches Ihr Raspberry Pi ist. Beachten Sie auch die Verbindungsart; Wenn Ihr Pi mit einem Kabel verbunden ist, sollten weniger Geräte zur Auswahl stehen.

### `raspberrypi.local` mit mDNS auflösen

Auf Raspberry Pi OS wird [Multicast DNS](https://en.wikipedia.org/wiki/Multicast_DNS) standardmäßig von [Avahi](https://en.wikipedia.org/wiki /Avahi_%28software%29) Dienst.

Wenn Ihr Gerät mDNS unterstützt, können Sie Ihren Raspberry Pi über seinen Hostnamen und die Endung „.local“ erreichen.
Der Standard-Hostname bei einer frischen Raspberry Pi OS-Installation ist "raspberrypi", daher antwortet standardmäßig jeder Raspberry Pi, auf dem Raspberry Pi OS ausgeführt wird, auf:

```bash
ping raspberrypi.local
```

Wenn der Raspberry Pi erreichbar ist, zeigt 'ping' seine IP-Adresse an:

```
PING raspberrypi.local (192.168.1.131): 56 Datenbytes
64 Byte von 192.168.1.131: icmp_seq=0 ttl=255 Zeit=2.618 ms
```

Wenn Sie den System-Hostnamen des Raspberry Pi ändern (z. B. durch Bearbeiten von `/etc/hostname`), ändert Avahi auch die mDNS-Adresse `.local`.

Wenn Sie sich nicht an den Hostnamen des Raspberry Pi erinnern, aber ein System mit installiertem Avahi haben, können Sie mit dem [`avahi-browse`](https://linux.die. net/man/1/avahi-browse) Befehl.

### nmap-Befehl

Der Befehl 'nmap' (Network Mapper) ist ein kostenloses Open-Source-Tool zur Netzwerkerkennung, das für Linux, macOS und Windows verfügbar ist.

- Um auf **Linux** zu installieren, installieren Sie das `nmap`-Paket z.B. `apt install nmap`.

- Informationen zur Installation auf **macOS** oder **Windows** finden Sie auf der [nmap.org-Downloadseite](http://nmap.org/download.html).

Um `nmap` zum Scannen der Geräte in Ihrem Netzwerk zu verwenden, müssen Sie das Subnetz kennen, mit dem Sie verbunden sind. Finden Sie zuerst Ihre eigene IP-Adresse, mit anderen Worten die des Computers, den Sie verwenden, um die IP-Adresse Ihres Pi zu finden:

- Unter **Linux** geben Sie `hostname -I` in ein Terminalfenster ein
- Unter **macOS** gehen Sie zu "Systemeinstellungen" und dann zu "Netzwerk" und wählen Sie Ihre aktive Netzwerkverbindung aus, um die IP-Adresse anzuzeigen
- Unter **Windows** gehen Sie zur Systemsteuerung, klicken Sie dann unter "Netzwerk- und Freigabecenter" auf "Netzwerkverbindungen anzeigen", wählen Sie Ihre aktive Netzwerkverbindung aus und klicken Sie auf "Status dieser Verbindung anzeigen", um die IP-Adresse anzuzeigen

Jetzt haben Sie die IP-Adresse Ihres Computers und scannen das gesamte Subnetz nach anderen Geräten. Wenn Ihre IP-Adresse beispielsweise „192.168.1.5“ lautet, befinden sich andere Geräte an Adressen wie „192.168.1.2“, „192.168.1.3“, „192.168.1.4“ usw. Die Notation dieses Subnetzbereichs lautet „192.168“. .1.0/24` (dies umfasst `192.168.1.0` bis `192.168.1.255`).

Verwenden Sie nun den `nmap`-Befehl mit dem `-sn`-Flag (Ping-Scan) im gesamten Subnetzbereich. Dies kann einige Sekunden dauern:

```bash
nmap -sn 192.168.1.0/24
```

Der Ping-Scan pingt nur alle IP-Adressen, um zu sehen, ob sie antworten. Für jedes Gerät, das auf den Ping antwortet, zeigt die Ausgabe den Hostnamen und die IP-Adresse wie folgt an:

```
Start von Nmap 6.40 ( http://nmap.org ) um 2014-03-10 12:46 GMT
Nmap-Scanbericht für hpprinter (192.168.1.2)
Host ist aktiv (0,00044s Latenz).
Nmap-Scanbericht für Gordons-MBP (192.168.1.4)
Host ist aktiv (0,0010s Latenz).
Nmap-Scanbericht für Ubuntu (192.168.1.5)
Host ist aktiv (0,0010s Latenz).
Nmap-Scan-Bericht für Raspberrypi (192.168.1.8)
Host ist aktiv (0,0030s Latenz).
Nmap fertig: 256 IP-Adressen (4 Hosts up) in 2,41 Sekunden gescannt
```

Hier sehen Sie, dass ein Gerät mit dem Hostnamen `raspberrypi` die IP-Adresse `192.168.1.8` hat. Beachten Sie, dass Sie nmap als root ausführen müssen, um die Hostnamen anzuzeigen, indem Sie dem Befehl `sudo` voranstellen.

### Abrufen der IP-Adresse eines Pi mit Ihrem Smartphone

Die Fing App ist ein kostenloser Netzwerkscanner für Smartphones. Es ist verfügbar für [Android](https://play.google.com/store/apps/details?id=com.overlook.android.fing) und [iOS](https://itunes.apple.com/gb /app/fing-network-scanner/id430921107?mt=8).

Ihr Telefon und Ihr Raspberry Pi müssen sich im selben Netzwerk befinden, also verbinden Sie Ihr Telefon mit dem richtigen drahtlosen Netzwerk.

Wenn Sie die Fing-App öffnen, tippen Sie auf die Schaltfläche zum Aktualisieren in der oberen rechten Ecke des Bildschirms. Nach wenigen Sekunden erhalten Sie eine Liste mit allen Geräten, die mit Ihrem Netzwerk verbunden sind. Scrollen Sie nach unten zum Eintrag mit dem Hersteller "Raspberry Pi". Sie sehen die IP-Adresse in der unteren linken Ecke und die MAC-Adresse in der unteren rechten Ecke des Eintrags.

### Mehr Werkzeuge

Siehe auch [lsleases](https://github.com/j-keck/lsleases)