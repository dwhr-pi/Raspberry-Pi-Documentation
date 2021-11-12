# Linux-Befehle

Hier sind einige grundlegende und gängige Linux-Befehle mit Beispielverwendung:

## Dateisystem

### ls

Der Befehl `ls` listet den Inhalt des aktuellen Verzeichnisses (oder eines angegebenen) auf. Es kann mit dem Flag `-l` verwendet werden, um zusätzliche Informationen (Berechtigungen, Besitzer, Gruppe, Größe, Datum und Zeitstempel der letzten Bearbeitung) über jede Datei und jedes Verzeichnis in einem Listenformat anzuzeigen. Mit dem Flag `-a` können Sie Dateien anzeigen, die mit `.` beginnen (d.h. Punktdateien).

### cd

Die Verwendung von `cd` ändert das aktuelle Verzeichnis in das angegebene. Sie können relative (d.h. `cd directoryA`) oder absolute (d.h. `cd /home/pi/directoryA`) Pfade verwenden.

### pwd

Der Befehl `pwd` zeigt den Namen des aktuellen Arbeitsverzeichnisses an: Auf einem Raspberry Pi wird durch Eingabe von `pwd` etwas wie `/home/pi` ausgegeben.

### mkdir

Mit `mkdir` können Sie ein neues Verzeichnis erstellen, z.B. `mkdir newDir` würde das Verzeichnis `newDir` im aktuellen Arbeitsverzeichnis erstellen.

### rmdir

Um leere Verzeichnisse zu entfernen, verwenden Sie `rmdir`. So entfernt beispielsweise `rmdir oldDir` das Verzeichnis `oldDir` nur, wenn es leer ist.

### rm

Der Befehl `rm` entfernt die angegebene Datei (oder rekursiv aus einem Verzeichnis bei Verwendung mit `-r`). Seien Sie vorsichtig mit diesem Befehl: Auf diese Weise gelöschte Dateien sind meistens endgültig verschwunden!

### cp

Die Verwendung von `cp` erstellt eine Kopie einer Datei und platziert sie an der angegebenen Stelle (dies ähnelt dem Kopieren und Einfügen). Zum Beispiel würde `cp ~/fileA /home/otherUser/` die Datei `fileA` aus Ihrem Home-Verzeichnis in das des Benutzers `otherUser` kopieren (vorausgesetzt, Sie haben die Berechtigung, sie dorthin zu kopieren). Dieser Befehl kann entweder `FILE FILE` (`cp fileA fileB`), `FILE DIR` (`cp fileA /directoryB/`) oder `-r DIR DIR` (das den Inhalt von Verzeichnissen rekursiv kopiert) als Argumente annehmen.

### mv

Der Befehl `mv` verschiebt eine Datei und platziert sie an der angegebenen Stelle (also wo `cp` ein 'Kopieren-Einfügen' ausführt, `mv` ein 'Ausschneiden-Einfügen'). Die Verwendung ist ähnlich wie bei `cp`. Also würde `mv ~/fileA /home/otherUser/` die Datei `fileA` aus Ihrem Home-Verzeichnis in das des Benutzers otherUser verschieben. Dieser Befehl kann entweder `FILE FILE` (`mv fileA fileB`), `FILE DIR` (`mv fileA /directoryB/`) oder `DIR DIR` (`mv /directoryB /directoryC`) als Argumente annehmen. Dieser Befehl ist auch als Methode zum Umbenennen von Dateien und Verzeichnissen nützlich, nachdem sie erstellt wurden.

### touch

Der Befehl `touch` setzt den zuletzt geänderten Zeitstempel der angegebenen Datei(en) oder erstellt ihn, falls er noch nicht existiert.

### cat

Mit `cat` können Sie den Inhalt der Datei(en) auflisten, z.B. `cat thisFile` zeigt den Inhalt von `thisFile` an. Kann verwendet werden, um den Inhalt mehrerer Dateien aufzulisten, d.h. `cat *.txt` listet den Inhalt aller `.txt`-Dateien im aktuellen Verzeichnis auf.

### head

Der Befehl `head` zeigt den Anfang einer Datei an. Kann mit `-n` verwendet werden, um die Anzahl der anzuzeigenden Zeilen anzugeben (standardmäßig zehn), oder mit `-c`, um die Anzahl der Bytes anzugeben.

### tail

Das Gegenteil von `head`, `tail` zeigt das Ende einer Datei an. Der Startpunkt in der Datei kann entweder durch `-b` für 512 Byte Blöcke, `-c` für Bytes oder `-n` für Zeilenanzahl angegeben werden.

### chmod

Normalerweise würden Sie `chmod` verwenden, um die Berechtigungen für eine Datei zu ändern. Der Befehl `chmod` kann die Symbole `u` (Benutzer, dem die Datei gehört), `g` (die Dateigruppe) und `o` (andere Benutzer) und die Berechtigungen `r` (lesen), `w` ( schreiben) und `x` (ausführen). Die Verwendung von `chmod u+x *filename*` fügt dem Eigentümer der Datei die Ausführungsberechtigung hinzu.

### chown

Der Befehl `chown` ändert den Benutzer und/oder die Gruppe, die eine Datei besitzt. Es muss normalerweise als root mit sudo ausgeführt werden, z. `sudo chown pi:root *filename*` ändert den Besitzer in pi und die Gruppe in root.

### ssh

`ssh` bezeichnet die sichere Shell. Stellen Sie über eine verschlüsselte Netzwerkverbindung eine Verbindung zu einem anderen Computer her.
Für weitere Details siehe [SSH (sichere Shell)](../../remote-access/ssh/)

### scp

Der Befehl `scp` kopiert eine Datei mit `ssh` von einem Computer auf einen anderen.
Für weitere Details siehe [SCP (sichere Kopie)](../../remote-access/ssh/scp.md)

### sudo

Mit dem Befehl `sudo` können Sie einen Befehl als Superuser oder als anderer Benutzer ausführen. Verwenden Sie `sudo -s` für eine Superuser-Shell.
Für weitere Details siehe [Root user / sudo](root.md)

### dd

Der Befehl `dd` kopiert eine Datei und konvertiert die Datei wie angegeben. Es wird oft verwendet, um eine ganze Festplatte in eine einzelne Datei oder wieder zurück zu kopieren. So erstellt beispielsweise `dd if=/dev/sdd of=backup.img` ein Backup-Image von einer SD-Karte oder einem USB-Laufwerk unter /dev/sdd. Stellen Sie sicher, dass Sie beim Kopieren eines Images auf die SD-Karte das richtige Laufwerk verwenden, da dies die gesamte Festplatte überschreiben kann.

### df

Verwenden Sie `df`, um den verfügbaren und verwendeten Speicherplatz auf den eingehängten Dateisystemen anzuzeigen. Verwenden Sie `df -h`, um die Ausgabe in einem für Menschen lesbaren Format anzuzeigen, wobei M für MBs verwendet wird, anstatt die Anzahl der Bytes anzuzeigen.

### unzip

Der Befehl `unzip` extrahiert die Dateien aus einer komprimierten Zip-Datei.

### tar

Verwenden Sie `tar`, um Dateien aus einer Bandarchivdatei zu speichern oder zu extrahieren. Es kann auch den erforderlichen Speicherplatz reduzieren, indem die Datei ähnlich einer ZIP-Datei komprimiert wird.

Um eine komprimierte Datei zu erstellen, verwenden Sie `tar -cvzf *filename.tar.gz* *directory/*`
Um den Inhalt einer Datei zu extrahieren, verwenden Sie `tar -xvzf *filename.tar.gz*`


### pipes

Eine Pipe ermöglicht es, die Ausgabe eines Befehls als Eingabe für einen anderen Befehl zu verwenden. Das Pipe-Symbol ist eine vertikale Linie `|`. Um beispielsweise nur die ersten zehn Einträge des `ls`-Befehls anzuzeigen, kann er über den Headbefehl `ls | head`

### tree

Verwenden Sie den Befehl `tree`, um ein Verzeichnis und alle Unterverzeichnisse und Dateien eingerückt als Baumstruktur anzuzeigen.

### &

Führen Sie einen Befehl im Hintergrund mit `&` aus, um die Shell für zukünftige Befehle freizugeben.

### wget

Laden Sie mit `wget` eine Datei aus dem Internet direkt auf den Computer herunter. `wget https://www.raspberrypi.org/documentation/linux/usage/commands.md` lädt diese Datei als `commands.md` auf Ihren Computer herunter

### curl

Verwenden Sie `curl`, um eine Datei auf einen/von einem Server herunterzuladen oder hochzuladen. Standardmäßig wird der Dateiinhalt der Datei auf dem Bildschirm ausgegeben.


### man

Zeigen Sie die Handbuchseite für eine Datei mit `man` an. Um mehr zu erfahren, führen Sie `man man` aus, um die Handbuchseite des man-Befehls anzuzeigen.


## Suche

### grep

Verwenden Sie `grep`, um in Dateien nach bestimmten Suchmustern zu suchen. Zum Beispiel sucht `grep "search" *.txt` in allen Dateien im aktuellen Verzeichnis mit der Endung .txt für die String-Suche.

Der Befehl `grep` unterstützt reguläre Ausdrücke, die es ermöglichen, spezielle Buchstabenkombinationen in die Suche einzubeziehen.

### awk

`awk` ist eine Programmiersprache, die zum Suchen und Bearbeiten von Textdateien nützlich ist.

### find

Der Befehl `find` durchsucht ein Verzeichnis und Unterverzeichnisse nach Dateien, die bestimmten Mustern entsprechen.


### whereis

Verwenden Sie `whereis`, um den Speicherort eines Befehls zu finden. Es durchsucht Standardprogrammspeicherorte, bis es den angeforderten Befehl findet.


## Netzwerk

### ping

Das Dienstprogramm `ping` wird normalerweise verwendet, um zu überprüfen, ob eine Kommunikation mit einem anderen Host möglich ist. Es kann mit Standardeinstellungen verwendet werden, indem einfach ein Hostname (z. B. `ping raspberrypi.org`) oder eine IP-Adresse (z. B. `ping 8.8.8.8`) angegeben wird. Es kann die Anzahl der zu sendenden Pakete mit dem Flag `-c` angeben.

### nmap

`nmap` ist ein Netzwerk-Explorations- und Scan-Tool. Es kann Port- und Betriebssysteminformationen zu einem Host oder einer Reihe von Hosts zurückgeben. Wenn Sie nur `nmap` ausführen, werden die verfügbaren Optionen sowie ein Anwendungsbeispiel angezeigt.

### hostname

Der Befehl `hostname` zeigt den aktuellen Hostnamen des Systems an. Ein privilegierter (Super-)Benutzer kann den Hostnamen auf einen neuen setzen, indem er ihn als Argument angibt (z. B. `hostname new-host`).

### ifconfig

Verwenden Sie `ifconfig`, um die Netzwerkkonfigurationsdetails für die Schnittstellen auf dem aktuellen System anzuzeigen, wenn sie ohne Argumente ausgeführt werden (d. h. `ifconfig`). Indem Sie dem Befehl den Namen einer Schnittstelle übergeben (z. B. `eth0` oder `lo`) können Sie dann die Konfiguration ändern: Lesen Sie die Handbuchseite für weitere Details.