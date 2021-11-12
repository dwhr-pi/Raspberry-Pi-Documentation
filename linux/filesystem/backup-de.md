# Sicherungen

Es wird dringend empfohlen, regelmäßig Sicherungskopien aller wichtigen Dateien zu erstellen. Backups sind oft nicht auf Benutzerdateien beschränkt; sie können Konfigurationsdateien, Datenbanken, installierte Software, Einstellungen und sogar einen kompletten Snapshot eines Systems umfassen.

Hier führen wir Sie durch einige Backup-Techniken für Ihr Raspberry Pi-System.

## Home-Ordner

Eine sinnvolle Möglichkeit, Ihren Privatordner zu sichern, besteht darin, mit dem Befehl `tar` ein Schnappschussarchiv des Ordners zu erstellen und eine Kopie davon auf Ihrem Heim-PC oder im Cloud-Speicher zu speichern. Geben Sie dazu die folgenden Befehle ein:

```bash
cd /home/
sudo tar czf pi_home.tar.gz pi
```

Dies erstellt ein tar-Archiv namens `pi_home.tar.gz` in `/home/`. Sie sollten diese Datei auf einen USB-Stick kopieren oder auf einen anderen Rechner in Ihrem Netzwerk übertragen.

## SD-Karten-Kopierer (empfohlen)

Die Anwendung SD Card Copier, die sich im Menü "Zubehör" des Raspberry Pi Desktops befindet, kopiert das Raspberry Pi OS von einer Karte auf eine andere. Um es zu verwenden, benötigen Sie einen USB-SD-Kartenschreiber.

Um Ihre vorhandene Raspberry Pi OS-Installation zu sichern, legen Sie eine leere SD-Karte in Ihren USB-Kartenschreiber ein, stecken Sie sie in Ihren Pi und starten Sie dann SD Card Copier. Wählen Sie im Feld „Von Gerät kopieren“ die interne SD-Karte aus. Dies kann verschiedene Namen haben und so etwas wie `(/dev/mmcblk0)` im Eintrag haben, wird aber normalerweise das erste Element in der Liste sein. Wählen Sie dann den USB-Kartenschreiber im Feld „Auf Gerät kopieren“ aus (wo es wahrscheinlich das einzige aufgeführte Gerät ist). Drücke Start'. Das Kopieren kann je nach Größe der SD-Karte zehn oder fünfzehn Minuten dauern, und wenn Sie fertig sind, sollten Sie einen Klon Ihrer aktuellen Installation auf der neuen SD-Karte haben. Sie können es testen, indem Sie die neu kopierte Karte in den SD-Kartensteckplatz des Pi stecken und booten. Es sollte booten und genauso aussehen wie Ihre ursprüngliche Installation, mit allen Ihren Daten und Anwendungen intakt.

Sie können direkt vom Backup aus starten, aber wenn Sie Ihre ursprüngliche Karte aus Ihrem Backup wiederherstellen möchten, kehren Sie den Vorgang einfach um – booten Sie Ihren Pi von der Backup-Karte, legen Sie die Karte, auf die Sie wiederherstellen möchten, in den SD-Kartenschreiber und wiederholen Sie den obigen Vorgang.

Das Programm beschränkt Sie nicht darauf, nur auf eine Karte zu kopieren, die dieselbe Größe wie die Quelle hat; Sie können auf eine größere Karte kopieren, wenn der Speicherplatz auf Ihrer vorhandenen Karte knapp wird, oder sogar auf eine kleinere Karte (solange sie genug Platz hat, um alle Ihre Dateien zu speichern – das Programm warnt Sie, wenn nicht genügend Platz vorhanden ist Platz). Es wurde entwickelt, um mit Raspberry Pi OS- und NOOBS-Images zu arbeiten. es funktioniert möglicherweise mit anderen Betriebssystemen oder benutzerdefinierten Kartenformaten, dies kann jedoch nicht garantiert werden.

Die einzige Einschränkung besteht darin, dass Sie nicht auf den internen SD-Kartenleser schreiben können, da dies das tatsächlich ausgeführte Betriebssystem überschreiben würde, was die Installation vollständig unterbrechen könnte.

Beachten Sie, dass alles auf der Zielkarte überschrieben wird, stellen Sie also sicher, dass Sie keine kritischen Daten darauf haben, bevor Sie mit dem Kopieren beginnen.

## SD-Kartenbild

Es kann sinnvoll sein, eine Kopie des gesamten SD-Karten-Images aufzubewahren, damit Sie die Karte wiederherstellen können, wenn sie verloren geht oder beschädigt wird. Sie können dies mit der gleichen Methode tun, mit der Sie ein Bild auf eine neue Karte schreiben würden, jedoch umgekehrt.

Unter Linux:

```bash
sudo dd bs=4M if=/dev/sdb of=PiOS.img
```

Dadurch wird eine Image-Datei auf Ihrem Computer erstellt, die Sie zum Schreiben auf eine andere SD-Karte verwenden können, und behält genau die gleichen Inhalte und Einstellungen bei. Um eine andere Karte wiederherzustellen oder auf eine andere Karte zu klonen, verwenden Sie `dd` in umgekehrter Reihenfolge:

```bash
sudo dd bs=4M if=PiOS.img of=/dev/sdb
```

Diese Dateien können sehr groß sein und gut komprimieren. Zum Komprimieren können Sie die Ausgabe von `dd` an `gzip` weiterleiten, um eine komprimierte Datei zu erhalten, die deutlich kleiner als die Originalgröße ist:

```bash
sudo dd bs=4M if=/dev/sdb | gzip > PiOS.img.gz
```

Um die Wiederherstellung wiederherzustellen, leiten Sie die Ausgabe von `gunzip` an `dd` weiter:

```bash
gunzip --stdout PiOS.img.gz | sudo dd bs=4M of=/dev/sdb
```

Wenn Sie einen Mac verwenden, sind die verwendeten Befehle fast identisch, aber `4M` in den obigen Beispielen sollte durch `4m` mit einem Kleinbuchstaben ersetzt werden.

Weitere Informationen finden Sie unter [Installation von SD-Karten-Images](../../installation/installing-images/README.md).

## MySQL

Wenn auf Ihrem Raspberry Pi MySQL-Datenbanken ausgeführt werden, ist es ratsam, diese ebenfalls zu sichern. Um eine einzelne Datenbank zu sichern, verwenden Sie den Befehl `mysqldump`:

```bash
mysqldump recipes > recipes.sql
```

Dieser Befehl sichert die `recipes`-Datenbank in der Datei `recipes.sql`. Beachten Sie, dass in diesem Fall dem Befehl `mysqldump` kein Benutzername und kein Kennwort übergeben wurden. Wenn Sie Ihre MySQL-Anmeldeinformationen nicht in einer `.my.cnf`-Konfigurationsdatei in Ihrem Home-Ordner haben, geben Sie den Benutzernamen und das Kennwort mit Flags an:

```bash
mysqldump -uroot -ppass recipes > recipes.sql
```

Um eine MySQL-Datenbank aus einer Dumpdatei wiederherzustellen, leiten Sie die Dumpdatei in den Befehl `mysql` ein. Geben Sie bei Bedarf Anmeldeinformationen und den Datenbanknamen an. Beachten Sie, dass die Datenbank vorhanden sein muss, also erstellen Sie sie zuerst:

```bash
mysql -Bse "create database recipes"
cat recipes.sql | mysql recipes
```

Alternativ können Sie den Befehl `pv` verwenden, um eine Fortschrittsanzeige anzuzeigen, während die Dumpdatei von MySQL verarbeitet wird. Dies ist nicht standardmäßig installiert, also mit `sudo apt install pv` installieren. Dieser Befehl ist nützlich für große Dateien:

```bash
pv recipes.sql | mysql recipes
```


## Automatisierung

Sie könnten ein [Bash-Skript](../usage/scripting.md) schreiben, um jeden dieser Prozesse automatisch auszuführen, und es sogar regelmäßig mit [cron](../usage/cron.md) ausführen lassen.