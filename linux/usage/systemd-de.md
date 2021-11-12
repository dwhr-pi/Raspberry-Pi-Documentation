# systemd

Damit beim Booten des Pi ein Befehl oder ein Programm ausgeführt wird, können Sie es als Dienst hinzufügen. Sobald dies erledigt ist, können Sie die Aktivierung/Deaktivierung über die Linux-Eingabeaufforderung starten/stoppen.

## Dienst erstellen

Erstellen Sie auf Ihrem Pi eine .service-Datei für Ihren Dienst, zum Beispiel:

myscript.service

```
[Unit]
Description=My service
After=network.target

[Service]
ExecStart=/usr/bin/python3 -u main.py
WorkingDirectory=/home/pi/myscript
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
```
In diesem Fall würde der Dienst Python 3 aus unserem Arbeitsverzeichnis `/home/pi/myscript` ausführen, das unser Python-Programm zum Ausführen von `main.py` enthält. Aber Sie sind nicht auf Python-Programme beschränkt: Ändern Sie einfach die ExecStart-Zeile in den Befehl zum Starten jedes Programms/Skripts, das Sie beim Booten ausführen möchten.

Kopieren Sie diese Datei als root nach `/etc/systemd/system`, zum Beispiel:
```
sudo cp myscript.service /etc/systemd/system/myscript.service
```

Nachdem diese kopiert wurde, können Sie versuchen, den Dienst mit dem folgenden Befehl zu starten:
```
sudo systemctl start myscript.service
```

Stoppen Sie es mit dem folgenden Befehl:
```
sudo systemctl stop myscript.service
```
Wenn Sie damit zufrieden sind, dass Ihre App gestartet und gestoppt wird, können Sie sie mit diesem Befehl beim Neustart automatisch starten lassen:
```
sudo systemctl enable myscript.service
```

Der Befehl `systemctl` kann auch verwendet werden, um den Dienst neu zu starten oder ihn beim Booten zu deaktivieren!

Einige Dinge, die Sie beachten sollten:
+ Die Reihenfolge, in der Dinge gestartet werden, basiert auf ihren Abhängigkeiten – dieses spezielle Skript sollte relativ spät im Boot-Prozess starten, nachdem ein Netzwerk verfügbar ist (siehe Abschnitt Nachher).
+ Sie können je nach Ihren Anforderungen verschiedene Abhängigkeiten und Reihenfolgen konfigurieren.


Weitere Informationen erhalten Sie bei:
``` man systemctl```
oder hier: https://fedoramagazine.org/what-is-an-init-system/