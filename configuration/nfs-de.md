# Netzwerkdateisystem (NFS)

Ein **Network File System** (NFS) ermöglicht Ihnen die gemeinsame Nutzung eines Verzeichnisses auf einem vernetzten Computer mit anderen Computern oder Geräten im selben Netzwerk. Der Computer, auf dem sich das Verzeichnis befindet, wird als **Server** bezeichnet, und Computer oder Geräte, die mit diesem Server verbunden sind, werden als **Clients** bezeichnet. Clients "mounten" normalerweise das freigegebene Verzeichnis, um es zu einem Teil ihrer eigenen Verzeichnisstruktur zu machen. Das freigegebene Verzeichnis ist ein Beispiel für eine freigegebene Ressource oder Netzwerkfreigabe.

Für kleinere Netzwerke eignet sich ein NFS perfekt zum Erstellen eines einfachen NAS (Network Attached Storage) in einer Linux/Unix-Umgebung.

Ein NFS eignet sich vielleicht am besten für dauerhaftere, im Netzwerk gemountete Verzeichnisse, wie zB `/home`-Verzeichnisse oder regelmäßig zugegriffene gemeinsam genutzte Ressourcen. Wenn Sie eine Netzwerkfreigabe wünschen, zu der sich Gastbenutzer leicht verbinden können, ist Samba besser für diese Aufgabe geeignet. Dies liegt daran, dass Tools zum vorübergehenden Mounten und Trennen von Samba-Freigaben über alte und proprietäre Betriebssysteme leichter verfügbar sind.

Bevor Sie ein NFS bereitstellen, sollten Sie Folgendes kennen:

* Linux-Datei- und Verzeichnisberechtigungen
* Dateisysteme ein- und aushängen

## Richten Sie einen einfachen NFS-Server ein

Installieren Sie die erforderlichen Pakete mit dem folgenden Befehl:

```bash
sudo apt install nfs-kernel-server
```

Zur einfacheren Wartung werden wir alle NFS-Exporte in einem einzigen Verzeichnis isolieren, in das die echten Verzeichnisse mit der Option `--bind` eingehängt werden.

Angenommen, wir möchten die Home-Verzeichnisse unserer Benutzer exportieren, die sich in `/home/users` befinden. Zuerst erstellen wir das Export-Dateisystem:

```bash
sudo mkdir -p /export/users
```

Beachten Sie, dass `/export` und `/export/users` 777-Berechtigungen benötigen, da wir vom Client ohne LDAP/NIS-Authentifizierung auf die NFS-Freigabe zugreifen. Dies gilt nicht, wenn die Authentifizierung verwendet wird (siehe unten). Mounten Sie nun das echte `users`-Verzeichnis mit:

```bash
sudo mount --bind /home/users /export/users
```

Damit wir dies nicht nach jedem Neustart erneut eingeben müssen, fügen wir die folgende Zeile zu `/etc/fstab` hinzu:

```
/home/users    /export/users   none    bind  0  0
```

Es gibt drei Konfigurationsdateien, die sich auf einen NFS-Server beziehen:

1. `/etc/default/nfs-kernel-server`
1. `/etc/default/nfs-common`
1. `/etc/exports`

Die einzige wichtige Option in `/etc/default/nfs-kernel-server` ist momentan `NEED_SVCGSSD`. Es ist standardmäßig auf "nein" gesetzt, was in Ordnung ist, da wir diesmal die NFSv4-Sicherheit nicht aktivieren.

Damit die ID-Namen automatisch gemappt werden, muss sowohl auf dem Client als auch auf dem Server die Datei `/etc/idmapd.conf` mit dem gleichen Inhalt und den korrekten Domänennamen vorhanden sein. Außerdem sollte diese Datei im Abschnitt `Mapping` die folgenden Zeilen enthalten:

```
[Mapping]

Nobody-User = nobody
Nobody-Group = nogroup
```

Beachten Sie jedoch, dass der Client unterschiedliche Anforderungen an den Niemand-Benutzer und die Niemand-Gruppe haben kann. Bei RedHat-Varianten ist es zum Beispiel `nfsnobody` für beide. Wenn Sie sich nicht sicher sind, überprüfen Sie mit den folgenden Befehlen, ob `nobody` und `nogroup` vorhanden sind:

```bash
cat /etc/passwd
cat /etc/group
```

Auf diese Weise müssen Server und Client die Benutzer nicht dieselbe UID/GUID teilen. Für diejenigen, die LDAP-basierte Authentifizierung verwenden, fügen Sie der `idmapd.conf` Ihrer Clients die folgenden Zeilen hinzu:

```
[Translation]

Method = nsswitch
```

Dadurch weiß `idmapd` in `nsswitch.conf` nachzusehen, wo es nach Anmeldeinformationen suchen soll. Wenn die LDAP-Authentifizierung bereits funktioniert, sollte `nsswitch` keiner weiteren Erklärung bedürfen.

Um unsere Verzeichnisse in ein lokales Netzwerk `192.168.1.0/24` zu exportieren, fügen wir die folgenden zwei Zeilen zu `/etc/exports` hinzu:

```
/export       192.168.1.0/24(rw,fsid=0,insecure,no_subtree_check,async)
/export/users 192.168.1.0/24(rw,nohide,insecure,no_subtree_check,async)
```

### Portmap-Sperre (optional)
Die Dateien auf Ihrem NFS sind für jeden im Netzwerk zugänglich. Aus Sicherheitsgründen können Sie den Zugriff auf bestimmte Clients beschränken.

Fügen Sie die folgende Zeile zu `/etc/hosts.deny` hinzu:

```
rpcbind mountd nfsd statd lockd rquotad : ALL
```

Indem Sie zuerst alle Clients blockieren, wird nur Clients in `/etc/hosts.allow` (unten hinzugefügt) der Zugriff auf den Server erlaubt.

Fügen Sie nun die folgende Zeile zu `/etc/hosts.allow` hinzu:

```
rpcbind mountd nfsd statd lockd rquotad : <Liste der IPv4s>
```

wobei `<Liste der IPv4s>` eine Liste der IP-Adressen des Servers und aller Clients ist. (Dies müssen IP-Adressen sein, wegen einer Einschränkung in `rpcbind`, die keine Hostnamen mag.) Beachten Sie, dass Sie, wenn Sie NIS eingerichtet haben, diese einfach in dieselbe Zeile einfügen können.

Bitte stellen Sie sicher, dass die Liste der autorisierten IP-Adressen die `localhost`-Adresse (`127.0.0.1`) enthält, da die Startskripts in den neueren Ubuntu-Versionen den `rpcinfo`-Befehl verwenden, um die NFSv3-Unterstützung zu erkennen. Diese wird deaktiviert, wenn ` localhost` kann keine Verbindung herstellen.

Damit Ihre Änderungen wirksam werden, starten Sie den Dienst abschließend neu:

```bash
sudo systemctl restart nfs-kernel-server
```

## Richten Sie einen NFSv4-Client ein

Da Ihr Server nun läuft, müssen Sie alle Clients einrichten, um darauf zugreifen zu können. Installieren Sie zunächst die erforderlichen Pakete:

```bash
sudo apt install nfs-common
```

Auf dem Client können wir den kompletten Exportbaum mit einem Befehl mounten:

```bash
mount -t nfs -o proto=tcp,port=2049 <nfs-server-IP>:/ /mnt
```

Sie können statt seiner IP-Adresse auch den Hostnamen des NFS-Servers angeben, müssen jedoch in diesem Fall sicherstellen, dass der Hostname auf der Clientseite in eine IP aufgelöst werden kann. Eine robuste Methode, um sicherzustellen, dass dies immer behoben wird, ist die Verwendung der Datei `/etc/hosts`.

Beachten Sie, dass `<nfs-server-IP>:/export` in NFSv4 nicht erforderlich ist, wie es in NFSv3 der Fall war. Der Root-Export `:/` wird standardmäßig mit `fsid=0` exportiert.

Wir können auch einen exportierten Teilbaum mounten mit:

```bash
mount -t nfs -o proto=tcp,port=2049 <nfs-server-IP>:/users /home/users
```

Um sicherzustellen, dass dies bei jedem Neustart eingehängt wird, fügen Sie die folgende Zeile zu `/etc/fstab` hinzu:

```
<nfs-server-IP>:/   /mnt   nfs    auto  0  0
```

Wenn nach dem Mounten der Eintrag in `/proc/mounts` als `<nfs-server-IP>://` (mit zwei Schrägstrichen) erscheint, müssen Sie möglicherweise zwei Schrägstriche in `/etc/fstab` angeben. andernfalls könnte sich `umount` beschweren, dass es das Mount nicht finden kann.

### Portmap-Sperre (optional)

Fügen Sie die folgende Zeile zu `/etc/hosts.deny` hinzu:

```
rpcbind : ALL
```

Wenn Sie zuerst alle Clients blockieren, dürfen nur Clients in `/etc/hosts.allow` (unten hinzugefügt) auf den Server zugreifen.

Fügen Sie nun die folgende Zeile zu `/etc/hosts.allow` hinzu:

```
rpcbind : <NFS-Server-IP-Adresse>
```

wobei `<NFS-Server-IP-Adresse>` die IP-Adresse des Servers ist, beispielsweise in Ihrem lokalen Netzwerk.

## NFS-Server mit komplexen Benutzerberechtigungen

NFS-Benutzerberechtigungen basieren auf der Benutzer-ID (UID). Die UIDs aller Benutzer auf dem Client müssen mit denen auf dem Server übereinstimmen, damit die Benutzer Zugriff haben. Die typischen Vorgehensweisen hierfür sind:

* Manuelle Synchronisation der Passwortdatei
* Verwendung von LDAP
* Verwendung von DNS
* Verwendung von NIS

* Manual password file synchronisation
* Use of LDAP
* Use of DNS
* Use of NIS

Beachten Sie, dass Sie auf Systemen vorsichtig sein müssen, auf denen der Hauptbenutzer Root-Zugriff hat: Dieser Benutzer kann die UIDs auf dem System ändern, um sich selbst den Zugriff auf die Dateien anderer zu ermöglichen. Auf dieser Seite wird davon ausgegangen, dass das Verwaltungsteam die einzige Gruppe mit Root-Zugriff ist und dass sie alle vertrauenswürdig sind. Alles andere stellt eine erweiterte Konfiguration dar und wird hier nicht behandelt.

###Gruppenberechtigungen

Der Dateizugriff eines Benutzers wird durch seine Zugehörigkeit zu Gruppen auf dem Client und nicht auf dem Server bestimmt. Es gibt jedoch eine wichtige Einschränkung: Es werden maximal 16 Gruppen vom Client an den Server übergeben, und wenn ein Benutzer Mitglied von mehr als 16 Gruppen auf dem Client ist, kann auf einige Dateien oder Verzeichnisse unerwarteterweise nicht zugegriffen werden.

### DNS (optional, nur bei Verwendung von DNS)

Fügen Sie alle Clientnamen und IP-Adressen zu `/etc/hosts` hinzu. (Die IP-Adresse des Servers sollte bereits vorhanden sein.) Dadurch wird sichergestellt, dass NFS auch dann noch funktioniert, wenn DNS ausfällt. Alternativ können Sie sich auf DNS verlassen, wenn Sie möchten - es liegt an Ihnen.

### NIS (optional, nur bei Verwendung von NIS)

Dies gilt für Clients, die NIS verwenden. Andernfalls können Sie keine Netzgruppen verwenden und sollten einzelne IPs oder Hostnamen in `/etc/exports` angeben. Lesen Sie den Abschnitt BUGS in `man netgroup` für weitere Informationen.

Bearbeiten Sie zuerst `/etc/netgroup` und fügen Sie eine Zeile hinzu, um Ihre Clients zu klassifizieren (dieser Schritt ist nicht notwendig, dient aber der Einfachheit):

```
myclients (client1,,) (client2,,) ...
```

wobei `myclients` der Netzgruppenname ist.

Führen Sie als nächstes diesen Befehl aus, um die NIS-Datenbank neu zu erstellen:

```bash
sudo make -C /var/yp
```

Der Dateiname `yp` bezieht sich auf Yellow Pages, den früheren Namen von NIS.

### Portmap-Sperre (optional)

Fügen Sie die folgende Zeile zu `/etc/hosts.deny` hinzu:

```
rpcbind mountd nfsd statd lockd rquotad : ALL
```

Indem Sie zuerst alle Clients blockieren, wird nur Clients in `/etc/hosts.allow` (unten hinzugefügt) der Zugriff auf den Server erlaubt.

Ziehen Sie in Erwägung, die folgende Zeile zu `/etc/hosts.allow` hinzuzufügen:

```
rpcbind mountd nfsd statd lockd rquotad : <Liste der IPs>
```

wobei `<Liste der IPs>` eine Liste der IP-Adressen des Servers und aller Clients ist. Diese müssen aufgrund einer Einschränkung in `rpcbind` IP-Adressen sein. Beachten Sie, dass Sie diese einfach in derselben Zeile hinzufügen können, wenn Sie NIS eingerichtet haben.

### Paketinstallation und -konfiguration

Installieren Sie die erforderlichen Pakete:

```bash
sudo apt install rpcbind nfs-kernel-server
```

Bearbeiten Sie `/etc/exports` und fügen Sie die Freigaben hinzu:

```
/home @myclients(rw,sync,no_subtree_check)
/usr/local @myclients(rw,sync,no_subtree_check)
```

Das obige Beispiel gibt `/home` und `/usr/local` für alle Clients in der `myclients`-Netzgruppe frei.

```
/home 192.168.0.10(rw,sync,no_subtree_check) 192.168.0.11(rw,sync,no_subtree_check)
/usr/local 192.168.0.10(rw,sync,no_subtree_check) 192.168.0.11(rw,sync,no_subtree_check)
```

Das obige Beispiel teilt `/home` und `/usr/local` für zwei Clients mit statischen IP-Adressen. Wenn Sie stattdessen allen Clients im privaten Netzwerk, die in einen bestimmten IP-Adressbereich fallen, den Zugriff erlauben möchten, beachten Sie Folgendes:

```
/home 192.168.0.0/255.255.255.0(rw,sync,no_subtree_check)
/usr/local 192.168.0.0/255.255.255.0(rw,sync,no_subtree_check)
```

Hier macht `rw` die Freigabe lesen/schreiben, und `sync` verlangt, dass der Server nur auf Anfragen antwortet, wenn alle Änderungen auf die Festplatte übertragen wurden. Dies ist die sicherste Option; `async` ist schneller, aber gefährlich. Es wird dringend empfohlen, `man exports` zu lesen, wenn Sie andere Optionen in Betracht ziehen.

Exportieren Sie nach dem Einrichten von `/etc/exports` die Freigaben:

```bash
sudo exportfs -ra
```

Sie sollten diesen Befehl ausführen, wenn `/etc/exports` geändert wird.

### Dienste neu starten

Standardmäßig bindet `rpcbind` nur an die Loopback-Schnittstelle. Um den Zugriff auf `rpcbind` von entfernten Maschinen zu ermöglichen, müssen Sie `/etc/conf.d/rpcbind` ändern, um entweder `-l` oder `-i 127.0.0.1` loszuwerden.

Wenn Änderungen vorgenommen werden, müssen rpcbind und NFS neu gestartet werden:

```bash
sudo systemctl restart rpcbind
sudo systemctl restart nfs-kernel-server
```

### Zu berücksichtigende Sicherheitselemente

Abgesehen von den oben besprochenen UID-Problemen sollte beachtet werden, dass sich ein Angreifer möglicherweise als Computer ausgeben könnte, der die Freigabe zuordnen darf, wodurch er beliebige UIDs für den Zugriff auf Ihre Dateien erstellen kann. Eine mögliche Lösung hierfür ist IPSec. Sie können alle Ihre Domänenmitglieder so einrichten, dass sie nur über IPSec miteinander kommunizieren, wodurch effektiv authentifiziert wird, dass Ihr Client der ist, der er vorgibt.

IPSec funktioniert, indem der Verkehr zum Server mit dem öffentlichen Schlüssel des Servers verschlüsselt wird, und der Server sendet alle Antworten verschlüsselt mit dem öffentlichen Schlüssel des Clients zurück. Der Verkehr wird mit den jeweiligen privaten Schlüsseln entschlüsselt. Wenn der Client nicht über die Schlüssel verfügt, die er haben soll, kann er keine Daten senden oder empfangen.

Eine Alternative zu IPSec sind physisch getrennte Netzwerke. Dies erfordert einen separaten Netzwerk-Switch und separate Ethernet-Karten sowie die physische Sicherheit dieses Netzwerks.

## Fehlerbehebung

Das Mounten einer NFS-Freigabe in ein verschlüsseltes Home-Verzeichnis funktioniert nur, nachdem Sie sich erfolgreich angemeldet und Ihr Home entschlüsselt wurde. Dies bedeutet, dass die Verwendung von /etc/fstab zum Mounten von NFS-Freigaben beim Booten nicht funktioniert, da Ihr Zuhause zum Zeitpunkt des Mountens nicht entschlüsselt wurde. Es gibt eine einfache Möglichkeit, dies mit symbolischen Links zu umgehen:

1. Erstellen Sie ein alternatives Verzeichnis zum Mounten der NFS-Freigaben in:

```bash
sudo mkdir /nfs
sudo mkdir /nfs/music
```

2. Bearbeiten Sie `/etc/fstab`, um die NFS-Freigabe stattdessen in dieses Verzeichnis einzuhängen:

```
nfsServer:music    /nfs/music    nfs    auto    0 0
```

3. Erstellen Sie einen symbolischen Link in Ihrem Zuhause, der auf den tatsächlichen Montageort verweist. Zum Beispiel und in diesem Fall zuerst das dort bereits vorhandene `Music`-Verzeichnis löschen:

```bash
rmdir /home/user/Music
ln -s /nfs/music/ /home/user/Music
```

## Informationen zum Autor

Diese Anleitung basiert auf Dokumenten im offiziellen Ubuntu-Wiki:

1. https://help.ubuntu.com/community/SettingUpNFSHowTo
1. https://help.ubuntu.com/stable/serverguide/network-file-system.html