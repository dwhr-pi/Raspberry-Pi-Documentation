# TCP/IP-Netzwerk

Der Raspberry Pi verwendet "dhcpcd", um TCP/IP über alle seine Netzwerkschnittstellen zu konfigurieren. Der dhcpcd-Daemon wurde von Roy Marples geschrieben und soll ein All-in-One-ZeroConf-Client für UNIX-ähnliche Systeme sein. Dazu gehört, jeder Schnittstelle eine IP-Adresse zuzuweisen, Netzmasken festzulegen und die DNS-Auflösung über die Name Service Switch (NSS)-Funktion zu konfigurieren.

Standardmäßig versucht Raspberry Pi OS, alle Netzwerkschnittstellen automatisch per DHCP zu konfigurieren, und greift auf automatische private Adressen im Bereich 169.254.0.0/16 zurück, wenn DHCP fehlschlägt. Dies steht im Einklang mit dem Verhalten anderer Linux-Varianten und von Microsoft Windows.

## Statische IP-Adresse

Wenn Sie die automatische Konfiguration für eine Schnittstelle deaktivieren und sie stattdessen statisch konfigurieren möchten, fügen Sie die Details zu `/etc/dhcpcd.conf` hinzu. Zum Beispiel:

```
interface eth0
static ip_address=192.168.0.4/24	
static routers=192.168.0.254
static domain_name_servers=192.168.0.254 8.8.8.8
```

Die Namen der auf Ihrem System vorhandenen Schnittstellen finden Sie mit dem Befehl `ip link`.

Beachten Sie, dass es möglicherweise einfacher ist, Adressreservierungen auf Ihrem DHCP-Server einzurichten, wenn Sie mehrere Raspberry Pis mit demselben Netzwerk verbunden haben. Auf diese Weise behält jeder Pi die gleiche IP-Adresse, aber sie werden alle an einem Ort verwaltet, was die Neukonfiguration Ihres Netzwerks in Zukunft einfacher macht.

Auf Raspberry Pi-Systemen, auf denen der grafische Desktop installiert ist, wird ein GUI-Tool namens `lxplug-network` verwendet, um dem Benutzer zu ermöglichen, Änderungen an der Konfiguration von `dhcpcd` vorzunehmen, einschließlich der Einstellung statischer IP-Adressen. Das Tool `lxplug-network` basiert auf `dhcpcd-ui`, das ebenfalls von Roy Marples entwickelt wurde.