# rc.local

Damit beim Booten des Pi ein Befehl oder ein Programm ausgeführt wird, können Sie der Datei "rc.local" Befehle hinzufügen. Dies ist besonders nützlich, wenn Sie Ihren Pi Headless an die Stromversorgung anschließen und ein Programm ohne Konfiguration oder manuellen Start ausführen lassen möchten.

HINWEIS: Auf Jessie, Stretch und Buster (die systemd verwenden) hat `rc.local` Nachteile: Nicht alle Programme werden zuverlässig ausgeführt, da möglicherweise nicht alle Dienste verfügbar sind, wenn `rc.local` ausgeführt wird.
Siehe [systemd](./systemd.md) für eine andere Möglichkeit, einen Befehl oder ein Programm ausführen zu lassen, wenn Raspberry Pi bootet.

Eine Alternative für die Verwaltung geplanter Aufgaben ist [cron](cron.md).

## Bearbeiten von rc.local

Bearbeiten Sie auf Ihrem Pi die Datei `/etc/rc.local` mit einem Editor Ihrer Wahl. Sie müssen mit root bearbeiten, zum Beispiel:

```bash
sudo nano /etc/rc.local
```

Fügen Sie Befehle unter dem Kommentar hinzu, lassen Sie jedoch die Zeile `exit 0` am Ende, speichern Sie die Datei und beenden Sie sie.

### Warnung

Wenn Ihr Befehl kontinuierlich ausgeführt wird (vielleicht eine Endlosschleife ausführt) oder wahrscheinlich nicht beendet wird, müssen Sie den Prozess unbedingt abzweigen, indem Sie am Ende des Befehls ein kaufmännisches Und wie folgt hinzufügen:

```
python3 /home/pi/myscript.py &
```

Andernfalls wird das Skript nicht beendet und der Pi bootet nicht. Das kaufmännische Und-Zeichen ermöglicht, dass der Befehl in einem separaten Prozess ausgeführt wird und das Booten bei laufendem Prozess fortgesetzt wird.

Stellen Sie außerdem sicher, dass Sie auf absolute Dateinamen verweisen und nicht auf Ihren Home-Ordner; beispielsweise `/home/pi/myscript.py` statt `myscript.py`.

Ein weiterer zu beachtender Punkt ist, dass alle Befehle vom Root-Benutzer ausgeführt werden. Dies kann zu unerwartetem Verhalten führen: Wenn beispielsweise ein Ordner durch einen `mkdir`-Befehl im Skript erstellt wird, hätte der Ordner Root-Eigentümer und wäre für niemanden außer dem Root-Benutzer zugänglich.