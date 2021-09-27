# FTP

FTP (File Transfer Protocol) kann verwendet werden, um Dateien zwischen einem Raspberry Pi und einem anderen Computer zu übertragen. Obwohl mit dem Standardprogramm `sftp-server` von Raspberry Pi OS die Benutzer mit ausreichenden Berechtigungen Dateien oder Verzeichnisse übertragen können, wird oft auch Zugriff auf das Dateisystem der eingeschränkten Benutzer benötigt. Führen Sie die folgenden Schritte aus, um einen FTP-Server einzurichten:

## Pure-FTPd installieren

Installieren Sie zuerst `Pure-FTPd` mit der folgenden Befehlszeile im Terminal:

```bash
sudo apt install pure-ftpd
```

## Grundkonfigurationen

Wir müssen eine neue Benutzergruppe namens `ftpgroup` und einen neuen Benutzer namens `ftpuser` für FTP-Benutzer erstellen und sicherstellen, dass dieser "Benutzer" **keine** Anmeldeberechtigung und **kein** Heimatverzeichnis hat:

```bash
sudo groupadd ftpgroup
sudo useradd ftpuser -g ftpgroup -s /sbin/nologin -d /dev/null
```

### FTP-Basisverzeichnis, virtueller Benutzer und Benutzergruppe

Erstellen Sie beispielsweise ein neues Verzeichnis namens `FTP` für den ersten Benutzer:

```bash
sudo mkdir /home/pi/FTP
```

Stellen Sie sicher, dass das Verzeichnis für `ftpuser` zugänglich ist:

```bash
sudo chown -R ftpuser:ftpgroup /home/pi/FTP
```

Erstellen Sie einen virtuellen Benutzer namens `upload`, ordnen Sie den virtuellen Benutzer `ftpuser` und `ftpgroup` zu, legen Sie das Home-Verzeichnis `/home/pi/FTP` fest und notieren Sie das Passwort des Benutzers in der Datenbank:

```bash
sudo pure-pw useradd upload -u ftpuser -g ftpgroup -d /home/pi/FTP -m
```

Nach der Eingabe dieser Befehlszeile ist ein Kennwort dieses virtuellen Benutzers erforderlich. Als nächstes richten Sie eine virtuelle Benutzerdatenbank ein, indem Sie Folgendes eingeben:

```bash
sudo pure-pw mkdb
```

Definieren Sie zu guter Letzt eine Authentifizierungsmethode, indem Sie einen Link der Datei `/etc/pure-ftpd/conf/PureDB` erstellen, die Nummer `60` dient nur zur Demonstration, machen Sie sie so klein wie nötig:

```bash
sudo ln -s /etc/pure-ftpd/conf/PureDB /etc/pure-ftpd/auth/60puredb
```

Starten Sie das Programm neu:

```bash
sudo service pure-ftpd neustart
```

Testen Sie es mit einem FTP-Client wie FileZilla.

## Detailliertere Konfigurationen:

Die Konfiguration von Pure-FTPd ist einfach und intuitiv. Der Administrator muss nur die notwendigen Einstellungen vornehmen, indem er Dateien mit Optionsnamen wie `ChrootEveryone` erstellt und `yes` eingibt und dann im Verzeichnis `/etc/pure-ftpd/conf` ablegt, wenn alle FTP-Benutzer sein sollen in ihrem FTP-Home-Verzeichnis (`/home/pi/FTP`) gesperrt. Hier sind einige empfohlene Einstellungen:

```bash
sudo nano /etc/pure-ftpd/conf/ChrootEveryone
```

Geben Sie „ja“ ein und drücken Sie „Strg + X“, „Y“ und die Eingabetaste.

Gleichfalls,

Erstellen Sie eine Datei namens `NoAnonymous` und geben Sie `yes` ein;

Erstellen Sie eine Datei namens `AnonymousCantUpload` und geben Sie `yes` ein;

Erstellen Sie eine Datei namens `AnonymousCanCreateDirs` und geben Sie `no` ein;

Erstellen Sie eine Datei namens `DisplayDotFiles` und geben Sie `no` ein;

Erstellen Sie eine Datei namens `DontResolve` und geben Sie `yes` ein;

Erstellen Sie eine Datei namens `ProhibitDotFilesRead` und geben Sie `yes` ein;

Erstellen Sie eine Datei namens `ProhibitDotFilesWrite` und geben Sie `yes` ein;

Erstellen Sie eine Datei mit dem Namen `FSCharset` und geben Sie `UTF-8` ein;

...

Starten Sie `pure-ftpd` erneut und übernehmen Sie die obigen Einstellungen.

```bash
sudo service pure-ftpd neustart
```

Weitere Informationen zu Pure-FTPd und Dokumentation finden Sie auf der offiziellen Website von [Pure-FTPd] (https://www.pureftpd.org/project/pure-ftpd).