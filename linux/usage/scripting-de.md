# Shell-Skripte

Befehle können in einer Datei zusammengefasst werden, die dann ausgeführt werden kann. Kopieren Sie als Beispiel Folgendes in Ihren bevorzugten Texteditor:

```bash
while :
do
echo Raspberry Pi!
done
```

Speichern Sie diese unter dem Namen `fun-script`. Bevor Sie es ausführen können, müssen Sie es zunächst ausführbar machen; Dies kann mit dem Befehl zum Ändern des Modus `chmod` erfolgen. Jede Datei und jedes Verzeichnis hat seine eigenen Berechtigungen, die vorschreiben, was ein Benutzer damit tun kann und was nicht. In diesem Fall wird durch Ausführen des Befehls `chmod +x fun-script` die Datei `fun-script` nun ausführbar. Sie können es dann ausführen, indem Sie `./fun-script` eingeben (vorausgesetzt, es befindet sich in Ihrem aktuellen Verzeichnis). Dieses Skript durchläuft eine Endlosschleife und druckt `Raspberry Pi!`; Um es zu stoppen, drücken Sie `Strg + C`. Dadurch werden alle Befehle beendet, die derzeit im Terminal ausgeführt werden.