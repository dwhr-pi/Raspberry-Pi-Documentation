# SSH unter Windows

Abhängig von der von Ihnen verwendeten Windows-Version und der bereits installierten Software müssen Sie möglicherweise einen SSH-Client herunterladen. Der am häufigsten verwendete Client heißt PuTTY und kann von [greenend.org.uk] (http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) heruntergeladen werden.

Suchen Sie nach „putty.exe“ unter der Überschrift „Für Windows auf Intel x86“.

## 1. Fügen Sie Ihren Raspberry Pi als Host hinzu
PuTTY starten. Sie sehen den folgenden Konfigurationsbildschirm:

![PuTTY-Konfiguration](images/ssh-win-config.png)

Geben Sie die IP-Adresse des Pi in das Feld "Hostname" ein und klicken Sie auf die Schaltfläche "Öffnen". Wenn beim Klicken auf die Schaltfläche "Öffnen" nichts passiert und Sie schließlich die Meldung "Netzwerkfehler: Zeitüberschreitung der Verbindung" sehen, haben Sie wahrscheinlich die falsche IP-Adresse für den Pi eingegeben.

Wenn Sie die IP-Adresse nicht kennen, geben Sie `hostname -I` in die Raspberry Pi-Befehlszeile ein. Es gibt weitere Möglichkeiten, Ihre IP-Adresse [hier] (../ip-address.md) zu finden.

## 2. Verbinden
Wenn die Verbindung funktioniert, sehen Sie die unten angezeigte Sicherheitswarnung. Sie können es getrost ignorieren und auf die Schaltfläche "Ja" klicken. Sie sehen diese Warnung nur, wenn PuTTY zum ersten Mal eine Verbindung zu einem Raspberry Pi herstellt, die es noch nie zuvor gesehen hat.

![PuTTY-Warnung](images/ssh-win-warning.png)

Sie sehen nun die übliche Anmeldeaufforderung. Melden Sie sich mit demselben Benutzernamen und Passwort an, das Sie auf dem Pi selbst verwenden würden. Die Standardanmeldung für Raspberry Pi OS ist `pi` mit dem Passwort `raspberry`.

Sie sollten jetzt die Raspberry Pi-Eingabeaufforderung haben, die mit der auf dem Raspberry Pi selbst identisch ist.

```
pi@raspberrypi ~ $
```

![PuTTY-Fenster](images/ssh-win-window.png)

Sie können `exit` eingeben, um das PuTTY-Fenster zu schließen.

## 3. Modifikation, Fehlerbehebung und mehr
Wenn Sie PuTTY das nächste Mal verwenden, suchen Sie in der unteren Hälfte des Konfigurationsbildschirms nach dem Abschnitt "Gespeicherte Sitzungen". Wenn Sie dies verwenden, empfehlen wir, auf die Seite `Verbindung` im linken Baum zu wechseln und den Wert `Sekunden zwischen Keepalives` auf `30` zu setzen. Wechseln Sie dann zurück zur Seite `Sitzung` im Baum, bevor Sie auf `Speichern` klicken. Mit dieser Einstellung können Sie ein PuTTY-Fenster für längere Zeit ohne Aktivität geöffnet lassen, ohne dass das Pi-Zeitlimit überschritten wird und Sie die Verbindung trennen.

Eine Verbindung kann aus verschiedenen Gründen nicht erfolgreich sein. Es ist sehr wahrscheinlich, dass Ihr Gerät oder Raspberry Pi [nicht richtig verbunden](../../configuration/wireless/wireless-cli.md); [SSH ist deaktiviert](../../configuration/raspi-config.md); Ihr Code enthält einen Tippfehler; oder die IP-Adresse oder die Anmeldeinformationen haben sich geändert. In letzteren Fällen müssen Sie den Host aktualisieren. Anweisungen zum Aktualisieren eines Hosts und weitere PuTTY-Dokumentation finden Sie in den [PuTTY-Dokumenten](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

