# Aufgaben planen mit Cron

Cron ist ein Werkzeug zum Konfigurieren geplanter Tasks auf Unix-Systemen. Es wird verwendet, um Befehle oder Skripte so zu planen, dass sie regelmäßig und in festen Intervallen ausgeführt werden. Die Aufgaben reichen von der täglichen Sicherung der Benutzerordner des Benutzers um Mitternacht bis hin zur stündlichen Protokollierung von CPU-Informationen.

Der Befehl `crontab` (Cron-Tabelle) wird verwendet, um die Liste der geplanten Aufgaben im Betrieb zu bearbeiten, und wird pro Benutzer ausgeführt; jeder Benutzer (einschließlich `root`) hat seine eigene `crontab`.

## Crontab bearbeiten

Führen Sie `crontab` mit dem `-e` Flag aus, um die Cron-Tabelle zu bearbeiten:

```bash
crontab -e
```

### Wähle einen Editor

Wenn Sie `crontab` zum ersten Mal ausführen, werden Sie aufgefordert, einen Editor auszuwählen; Wenn Sie sich nicht sicher sind, welches Sie verwenden sollen, wählen Sie `nano`, indem Sie `Enter` drücken.

### Geplante Aufgabe hinzufügen

Das Layout für einen Cron-Eintrag besteht aus sechs Komponenten: m = Minute, h = Stunde, dom = Tag des Monats, mon = Monat des Jahres, dow = Wochentag und den auszuführenden Befehl.

```
# m h  dom mon dow   command
```

```
# * * * * * Befehl zum Ausführen
# ┬ ┬ ┬ ┬ ┬
# │ │ │ │ │
# │ │ │ │ │
# │ │ │ │ └───── dow = day of week - Wochentag (0 - 7) (0 bis 6 sind Sonntag bis Samstag, oder verwenden Sie Namen; 7 ist Sunday = Sonntag, das gleiche wie 0)
# │ │ │ └──────────mom =  Monat (1 - 12)
# │ │ └─────────────── dom = Tag des Monats (1 - 31)
# │ └──────────────────── h = Stunde (0 - 23)
# └───────────────────────── m = min (0 - 59)
```

Zum Beispiel:

```
0 0 * * *  /home/pi/backup.sh
```

Dieser Cron-Eintrag würde das Skript `backup.sh` jeden Tag um Mitternacht ausführen.

### Geplante Aufgaben anzeigen

Zeigen Sie Ihre aktuell gespeicherten geplanten Aufgaben an mit:

```bash
crontab -l
````

### Beim Neustart eine Aufgabe ausführen

Um bei jedem Start des Raspberry Pi einen Befehl auszuführen, schreiben Sie `@reboot` anstelle von Uhrzeit und Datum. Zum Beispiel:

```
@reboot python /home/pi/myscript.py
```

Dadurch wird Ihr Python-Skript jedes Mal ausgeführt, wenn der Raspberry Pi neu gestartet wird. Wenn Sie möchten, dass Ihr Befehl im Hintergrund ausgeführt wird, während der Raspberry Pi weiter startet, fügen Sie ein Leerzeichen und ein `&` am Ende der Zeile hinzu, wie folgt:

```
@reboot python /home/pi/myscript.py &
```