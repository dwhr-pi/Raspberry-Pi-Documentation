# SCP (Sichere Kopie)

`scp` ist ein Befehl zum Senden von Dateien über SSH. Das bedeutet, dass Sie Dateien zwischen Computern kopieren können, beispielsweise von Ihrem Raspberry Pi auf Ihren Desktop oder Laptop oder umgekehrt.

Zunächst müssen Sie die [IP-Adresse] Ihres Raspberry Pi kennen (../ip-address.md).

## Dateien auf Ihren Raspberry Pi kopieren

Kopieren Sie die Datei `myfile.txt` von Ihrem Computer in den Home-Ordner des `pi`-Benutzers Ihres Raspberry Pi unter der IP-Adresse `192.168.1.3` mit dem folgenden Befehl:

```bash
scp myfile.txt pi@192.168.1.3:
```

Kopieren Sie die Datei in das Verzeichnis `/home/pi/project/` auf Ihrem Raspberry Pi (der Ordner `project` muss bereits vorhanden sein):

```bash
scp myfile.txt pi@192.168.1.3:project/
```

## Dateien von Ihrem Raspberry Pi kopieren

Kopieren Sie die Datei `myfile.txt` von Ihrem Raspberry Pi in das aktuelle Verzeichnis auf Ihrem anderen Computer:

```bash
scp pi@192.168.1.3:myfile.txt .
```

## Mehrere Dateien kopieren

Kopieren Sie mehrere Dateien, indem Sie sie durch Leerzeichen trennen:

```bash
scp myfile.txt myfile2.txt pi@192.168.1.3:
```

Verwenden Sie alternativ einen Platzhalter, um alle Dateien zu kopieren, die einer bestimmten Suche entsprechen:

```bash
scp *.txt pi@192.168.1.3:
```

(alle Dateien mit der Endung `.txt`)

```bash
scp m* pi@192.168.1.3:
```

(alle Dateien beginnen mit `m`)

```bash
scp m*.txt pi@192.168.1.3:
```

(alle Dateien beginnen mit `m` und enden auf `.txt`)

## Dateinamen mit Leerzeichen

Beachten Sie, dass einige der obigen Beispiele nicht für Dateinamen funktionieren, die Leerzeichen enthalten. Namen wie dieser müssen in Anführungszeichen gesetzt werden:

```bash
scp "my file.txt" pi@192.168.1.3:
```