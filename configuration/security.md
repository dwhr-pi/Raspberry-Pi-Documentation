# Sicherung Ihres Raspberry Pi

Die Sicherheit Ihres Raspberry Pi ist wichtig. Sicherheitslücken lassen Ihren Raspberry Pi für Hacker offen, die ihn dann ohne Ihre Erlaubnis verwenden können.

Welche Sicherheitsstufe Sie benötigen, hängt davon ab, wie Sie Ihren Raspberry Pi verwenden möchten. Wenn Sie Ihren Raspberry Pi beispielsweise einfach in Ihrem Heimnetzwerk, hinter einem Router mit Firewall, verwenden, dann ist dieser standardmäßig bereits recht sicher.

Wenn Sie Ihren Raspberry Pi jedoch direkt dem Internet zugänglich machen möchten, entweder mit einer direkten Verbindung (unwahrscheinlich) oder indem Sie bestimmte Protokolle durch Ihre Router-Firewall (z. B. SSH) lassen, müssen Sie einige grundlegende Sicherheitsänderungen vornehmen.

Auch wenn Sie sich hinter einer Firewall verstecken, ist es sinnvoll, die Sicherheit ernst zu nehmen. Diese Dokumentation beschreibt einige Möglichkeiten, die Sicherheit Ihres Raspberry Pi zu verbessern. Bitte beachten Sie jedoch, dass es nicht erschöpfend ist.

## Ändern Sie Ihr Standardpasswort

Der Standardbenutzername und das Standardkennwort werden für jeden einzelnen Raspberry Pi verwendet, auf dem Raspberry Pi OS ausgeführt wird. Wenn Sie also auf einen Raspberry Pi zugreifen können und diese Einstellungen nicht geändert wurden, haben Sie Root-Zugriff auf diesen Raspberry Pi.

Als erstes müssen Sie also das Passwort ändern. Dies kann über die Anwendung raspi-config oder über die Befehlszeile erfolgen.

```bash
sudo raspi-config
```

Wählen Sie Option 2 und befolgen Sie die Anweisungen zum Ändern des Kennworts.

Tatsächlich startet raspi-config nur die Befehlszeilenanwendung `passwd`, was Sie über die Befehlszeile tun können. Geben Sie einfach Ihr neues Passwort ein und bestätigen Sie es.

```bash
passwd
```

## Benutzernamen ändern

Natürlich können Sie Ihren Raspberry Pi noch sicherer machen, indem Sie auch Ihren Benutzernamen ändern. Alle Raspberry Pis werden mit dem Standardbenutzernamen "pi" geliefert. Wenn Sie diesen also ändern, wird Ihr Raspberry Pi sofort sicherer.

Geben Sie Folgendes ein, um einen neuen Benutzer hinzuzufügen:

```bash
sudo adduser alice
```

Sie werden aufgefordert, ein Passwort für den neuen Benutzer zu erstellen.

Der neue Benutzer hat ein Home-Verzeichnis unter `/home/alice/`.

Um sie der `sudo`-Gruppe hinzuzufügen, um ihnen `sudo`-Berechtigungen sowie alle anderen erforderlichen Berechtigungen zu erteilen:

```bash
sudo usermod -a -G adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,input,netdev,gpio,i2c,spi alice
```

Sie können überprüfen, ob Ihre Berechtigungen vorhanden sind (d. h. Sie können "sudo" verwenden), indem Sie Folgendes versuchen:

```bash
sudo su - alice
```

Wenn es erfolgreich ausgeführt wird, können Sie sicher sein, dass sich das neue Konto in der Gruppe `sudo` befindet.

Sobald Sie bestätigt haben, dass das neue Konto funktioniert, können Sie den Benutzer "pi" löschen. Um dies zu tun, müssen Sie zunächst den Prozess mit den folgenden Schritten schließen:

```bash
sudo pkill -u pi
```

Bitte beachten Sie, dass es bei der aktuellen Raspberry Pi OS-Distribution einige Aspekte gibt, die die Anwesenheit des `pi`-Benutzers erfordern. Wenn Sie sich nicht sicher sind, ob Sie davon betroffen sind, lassen Sie den `pi`-Benutzer an Ort und Stelle. Es wird daran gearbeitet, die Abhängigkeit vom `pi`-Benutzer zu reduzieren.

Geben Sie Folgendes ein, um den Benutzer "pi" zu löschen:

```bash
sudo deluser pi
```

Dieser Befehl löscht den `pi`-Benutzer, verlässt aber den `/home/pi`-Ordner. Bei Bedarf können Sie mit dem folgenden Befehl gleichzeitig den Home-Ordner für den Benutzer `pi` entfernen. Beachten Sie, dass die Daten in diesem Ordner dauerhaft gelöscht werden. Stellen Sie daher sicher, dass alle erforderlichen Daten an anderer Stelle gespeichert werden.

```bash
sudo deluser -remove-home pi
```
Dieser Befehl führt zu einer Warnung, dass die Gruppe `pi` keine Mitglieder mehr hat. Der Befehl `deluser` entfernt jedoch sowohl den `pi`-Benutzer als auch die `pi`-Gruppe, sodass die Warnung getrost ignoriert werden kann.


## Machen Sie ein erforderliches `sudo` Passwort

Das Platzieren von `sudo` vor einem Befehl führt ihn als Superuser aus und benötigt standardmäßig kein Passwort. Im Allgemeinen ist dies kein Problem. Wenn Ihr Pi jedoch dem Internet ausgesetzt ist und irgendwie ausgenutzt wird (vielleicht zum Beispiel über einen Webseiten-Exploit), kann der Angreifer Dinge ändern, die Superuser-Anmeldeinformationen erfordern, es sei denn, Sie haben "sudo" so eingestellt, dass ein Passwort erforderlich ist.

Um `sudo` zu zwingen, ein Passwort zu verlangen, geben Sie Folgendes ein:

```bash
sudo visudo /etc/sudoers.d/010_pi-nopasswd
```

und ändern Sie den `pi`-Eintrag (oder welche Benutzernamen Superuser-Rechte haben) in:

```bash
pi ALL=(ALL) PASSWD: ALL
```

Speichern Sie dann die Datei: Sie wird auf Syntaxfehler überprüft. Wenn keine Fehler festgestellt wurden, wird die Datei gespeichert und Sie kehren zum Shell-Prompt zurück. Wenn Fehler festgestellt wurden, werden Sie gefragt, was nun? Drücken Sie die Eingabetaste auf Ihrer Tastatur: Dadurch wird eine Liste mit Optionen angezeigt. Wahrscheinlich möchten Sie 'e' für '(e)dit sudoers file again' verwenden, damit Sie die Datei bearbeiten und das Problem beheben können. **Beachten Sie, dass die Auswahl der Option 'Q' die Datei mit allen noch vorhandenen Syntaxfehlern speichert, was es _jedem_ Benutzern unmöglich macht, den sudo-Befehl zu verwenden.**

## Stellen Sie sicher, dass Sie über die neuesten Sicherheitsfixes verfügen

Dies kann so einfach sein, indem Sie sicherstellen, dass Ihre Version von Raspberry Pi OS auf dem neuesten Stand ist, da eine aktuelle Distribution alle neuesten Sicherheitsfixes enthält. Eine vollständige Anleitung finden Sie [hier](../raspbian/updating.md).

Wenn Sie SSH verwenden, um sich mit Ihrem Raspberry Pi zu verbinden, kann es sich lohnen, einen Cron-Job hinzuzufügen, der speziell den SSH-Server aktualisiert. Der folgende Befehl, vielleicht als täglicher Cron-Job, stellt sicher, dass Sie unabhängig von Ihrem normalen Update-Prozess umgehend über die neuesten SSH-Sicherheitsfixes verfügen. Weitere Informationen zum Einrichten von Cron finden Sie [hier](../linux/usage/cron.md)

```bash
apt install openssh-server
```

## SSH-Sicherheit verbessern

SSH ist eine gängige Methode, um aus der Ferne auf einen Raspberry Pi zuzugreifen. Standardmäßig erfordert die Anmeldung mit SSH ein Benutzername/Passwort-Paar, und es gibt Möglichkeiten, dies sicherer zu machen. Eine noch sicherere Methode ist die schlüsselbasierte Authentifizierung.

### Verbesserung der Benutzername/Passwort-Sicherheit

Das Wichtigste ist, dass Sie ein sehr robustes Passwort haben. Wenn Ihr Raspberry Pi dem Internet ausgesetzt ist, muss das Passwort sehr sicher sein. Dies wird dazu beitragen, Wörterbuchangriffe oder ähnliches zu vermeiden.

Sie können auch bestimmte Benutzer **erlauben** oder **verweigern**, indem Sie die `sshd`-Konfiguration ändern.

```bash
sudo nano /etc/ssh/sshd_config
```

Fügen Sie die folgende Zeile hinzu, bearbeiten Sie sie oder fügen Sie sie am Ende der Datei an, die die Benutzernamen enthält, die Sie für die Anmeldung zulassen möchten:

```
AllowUsers alice bob
```

Sie können auch `DenyUsers` verwenden, um gezielt einige Benutzernamen daran zu hindern, sich anzumelden:

```
DenyUsers Jane John
```

Nach der Änderung müssen Sie den `sshd`-Dienst mit `sudo systemctl restart ssh` neu starten oder neu starten, damit die Änderungen wirksam werden.

### Verwenden der schlüsselbasierten Authentifizierung.

Schlüsselpaare sind zwei kryptographisch sichere Schlüssel. Einer ist privat und einer ist öffentlich. Sie können verwendet werden, um einen Client bei einem SSH-Server (in diesem Fall dem Raspberry Pi) zu authentifizieren.

Der Client generiert zwei Schlüssel, die kryptographisch miteinander verknüpft sind. Der private Schlüssel sollte niemals freigegeben werden, aber der öffentliche Schlüssel kann frei geteilt werden. Der SSH-Server nimmt eine Kopie des öffentlichen Schlüssels und verwendet diesen Schlüssel, wenn ein Link angefordert wird, um dem Client eine Challenge-Nachricht zu senden, die der Client mit dem privaten Schlüssel verschlüsselt. Wenn der Server den öffentlichen Schlüssel verwenden kann, um diese Nachricht zurück in die ursprüngliche Challenge-Nachricht zu entschlüsseln, kann die Identität des Clients bestätigt werden.

Das Generieren eines Schlüsselpaars unter Linux erfolgt mit dem Befehl `ssh-keygen` auf dem **Client**; die Schlüssel werden standardmäßig im `.ssh`-Ordner im Home-Verzeichnis des Benutzers gespeichert. Der private Schlüssel heißt `id_rsa` und der zugehörige öffentliche Schlüssel heißt `id_rsa.pub`. Der Schlüssel wird 2048 Bit lang sein: Das Aufbrechen der Verschlüsselung eines Schlüssels dieser Länge würde extrem lange dauern, daher ist es sehr sicher. Sie können längere Schlüssel erstellen, wenn die Situation es erfordert. Beachten Sie, dass Sie den Generierungsvorgang nur einmal durchführen sollten: Wenn er wiederholt wird, werden alle zuvor generierten Schlüssel überschrieben. Alles, was auf diesen alten Schlüsseln beruht, muss auf die neuen Schlüssel aktualisiert werden.

Während der Schlüsselgenerierung werden Sie zur Eingabe einer Passphrase aufgefordert: Dies ist eine zusätzliche Sicherheitsstufe. Lassen Sie dieses Feld vorerst leer.

Der öffentliche Schlüssel muss nun auf den Server verschoben werden: siehe [Kopieren Sie Ihren öffentlichen Schlüssel auf Ihren Raspberry Pi](../remote-access/ssh/passwordless.md#copy-your-public-key-to-your-raspberry-pi).

Schließlich müssen wir die Passwortanmeldungen deaktivieren, damit die gesamte Authentifizierung durch die Schlüsselpaare erfolgt.

```bash
sudo nano /etc/ssh/sshd_config
```

Es gibt drei Zeilen, die in `no` geändert werden müssen, wenn sie nicht bereits so eingestellt sind:

```bash
ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
```

Speichern Sie die Datei und starten Sie das SSH-System entweder mit `sudo service ssh reload` neu oder starten Sie es neu.

## Installieren Sie eine Firewall

Es gibt viele Firewall-Lösungen für Linux. Die meisten verwenden das zugrunde liegende Projekt [iptables](http://www.netfilter.org/projects/iptables/index.html), um Paketfilterung bereitzustellen. Dieses Projekt sitzt über dem Linux-Netfiltering-System. `iptables` ist auf Raspberry Pi OS standardmäßig installiert, aber nicht eingerichtet. Die Einrichtung kann eine komplizierte Aufgabe sein, und ein Projekt, das eine einfachere Benutzeroberfläche als `iptables` bietet, ist [ufw](https://www.linux.com/learn/introduction-uncomplicated-firewall-ufw), was für steht 'Unkomplizierte Brandmauer'. Dies ist das Standard-Firewall-Tool in Ubuntu und kann einfach auf Ihrem Raspberry Pi installiert werden:

```bash
sudo apt install ufw
```

`ufw` ist ein recht unkompliziertes Kommandozeilen-Tool, obwohl es dafür einige GUIs gibt. In diesem Dokument werden einige der grundlegenden Befehlszeilenoptionen beschrieben. Beachten Sie, dass `ufw` mit Superuser-Rechten ausgeführt werden muss, daher wird allen Befehlen `sudo` vorangestellt. Es ist auch möglich, die Option `--dry-run` für beliebige `ufw`-Befehle zu verwenden, die die Ergebnisse des Befehls anzeigt, ohne tatsächlich Änderungen vorzunehmen.

Um die Firewall zu aktivieren, die auch dafür sorgt, dass sie beim Booten gestartet wird, verwenden Sie:

```bash
sudo ufw enable
```

Um die Firewall zu deaktivieren und den Start beim Booten zu deaktivieren, verwenden Sie:

```bash
sudo ufw disable
```

Erlauben Sie einem bestimmten Port den Zugriff (wir haben in unserem Beispiel Port 22 verwendet):

```bash
sudo ufw allow 22
```

Das Verweigern des Zugriffs auf einen Port ist ebenfalls sehr einfach (wieder haben wir Port 22 als Beispiel verwendet):

```bash
sudo ufw deny 22
```

Sie können auch angeben, welchen Dienst Sie an einem Port zulassen oder verweigern. In diesem Beispiel verweigern wir TCP auf Port 22:

```bash
sudo ufw deny 22/tcp
```

Sie können den Dienst auch dann angeben, wenn Sie nicht wissen, welchen Port er verwendet. Dieses Beispiel erlaubt dem SSH-Dienst den Zugriff durch die Firewall:

```bash
sudo ufw allow ssh
```

Der Befehl status listet alle aktuellen Einstellungen für die Firewall auf:

```bash
sudo ufw status
```

Die Regeln können recht kompliziert sein und erlauben das Blockieren bestimmter IP-Adressen, geben an, in welche Richtung Datenverkehr erlaubt ist, oder begrenzen die Anzahl der Verbindungsversuche, um beispielsweise einen Denial-of-Service-Angriff (DoS) abzuwehren. Sie können auch angeben, auf welche Geräteregeln angewendet werden sollen (z. B. eth0, wlan0). Weitere Einzelheiten finden Sie auf der `ufw`-Manpage (`man ufw`), aber hier sind einige Beispiele für komplexere Befehle.

Beschränken Sie Anmeldeversuche am SSH-Port mit tcp: Dies verweigert die Verbindung, wenn eine IP-Adresse in den letzten 30 Sekunden sechs oder mehr versucht hat, eine Verbindung herzustellen:

```bash
sudo ufw limit ssh/tcp
```

Zugriff auf Port 30 von IP-Adresse 192.168.2.1 verweigern

```bash
sudo ufw deny from 192.168.2.1 port 30
```

## Installation von fail2ban

Wenn Sie Ihren Raspberry Pi als eine Art Server verwenden, zum Beispiel als `ssh` oder als Webserver, hat Ihre Firewall bewusste 'Löcher', um den Serververkehr durchzulassen. In diesen Fällen kann [Fail2ban](http://www.fail2ban.org) hilfreich sein. Fail2ban, geschrieben in Python, ist ein Scanner, der die vom Raspberry Pi erzeugten Log-Dateien untersucht und auf verdächtige Aktivitäten überprüft. Es fängt Dinge wie mehrere Brute-Force-Anmeldeversuche ab und kann jede installierte Firewall informieren, um weitere Anmeldeversuche von verdächtigen IP-Adressen zu stoppen. Es erspart Ihnen, Protokolldateien manuell auf Einbruchsversuche zu überprüfen und dann die Firewall (über `iptables`) zu aktualisieren, um diese zu verhindern.

Installieren Sie fail2ban mit dem folgenden Befehl:

```bash
sudo apt install fail2ban
```

Bei der Installation erstellt Fail2ban einen Ordner `/etc/fail2ban`, in dem sich eine Konfigurationsdatei namens `jail.conf` befindet. Dies muss nach `jail.local` kopiert werden, um es zu aktivieren. In dieser Konfigurationsdatei befinden sich eine Reihe von Standardoptionen sowie Optionen zum Überprüfen bestimmter Dienste auf Anomalien. Gehen Sie wie folgt vor, um die für `ssh` verwendeten Regeln zu überprüfen/zu ändern:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

Fügen Sie der Datei `jail.conf` den folgenden Abschnitt hinzu. Bei einigen Versionen von fail2ban ist dieser Abschnitt möglicherweise bereits vorhanden, also aktualisieren Sie diesen bereits vorhandenen Abschnitt, falls er vorhanden ist.

```
[ssh]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 6
```

Wie Sie sehen, heißt dieser Abschnitt ssh, ist aktiviert, untersucht den ssh-Port, filtert mit den `sshd`-Parametern, analysiert `/var/log/auth.log` auf bösartige Aktivitäten und erlaubt sechs Wiederholungen vor der Erkennung Schwelle erreicht ist. Wenn wir den Standardabschnitt überprüfen, können wir sehen, dass die Standardaktion zum Sperren ist:

```bash
# Standard-Sperraktion (z. B. iptables, iptables-new,
# iptables-multiport, Shorewall, etc) Es wird verwendet, um zu definieren
# action_*-Variablen. Kann global oder per
# Abschnitt in der Datei jail.local überschrieben werden
banaction = iptables-multiport
```

`iptables-multiport` bedeutet, dass das Fail2ban-System die Datei `/etc/fail2ban/action.d/iptables-multiport.conf` ausführt, wenn die Erkennungsschwelle erreicht wird. Es gibt eine Reihe verschiedener Aktionskonfigurationsdateien, die verwendet werden können. Multiport sperrt den Zugriff auf alle Ports.

Wenn Sie eine IP-Adresse nach drei fehlgeschlagenen Versuchen dauerhaft sperren möchten, können Sie den maxretry-Wert im Abschnitt `[ssh]` ändern und die Bantime auf eine negative Zahl setzen:

```
[ssh]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 3
bantime = -1
```

Es gibt ein gutes Tutorial zu einigen der Interna von Fail2ban [hier](https://www.digitalocean.com/community/tutorials/how-fail2ban-works-to-protect-services-on-a-linux-server).