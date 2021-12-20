# SSH MIT iOS

Um SSH auf Ihrem Mobilgerät zu verwenden, müssen Sie einen Client herunterladen. Es stehen mehrere Clients von guter Qualität zur Verfügung, wie z. B. [Termius](http://www.termius.com) und [Prompt 2](https://panic.com/prompt/).

Für dieses Tutorial verwenden wir Termius, da es ein beliebter plattformübergreifender SSH-Client ist. Für andere Clients wird der Prozess ähnlich sein.

## 1. Fügen Sie Ihren Raspberry Pi als Host hinzu.
Laden Sie Termius von [iTunes](https://itunes.apple.com/us/app/termius-ssh-shell-console/id549039908?mt=8) herunter, falls Sie es noch nicht installiert haben. Klicken Sie auf , um die App zu öffnen.

Es wird eine Aufforderung angezeigt, in der Sie aufgefordert werden, Benachrichtigungen zuzulassen. Sie sollten auf „Zulassen“ klicken (empfohlen). Folgen Sie nun den Anweisungen auf dem Bildschirm: `Beginnen Sie mit dem Hinzufügen eines neuen Hosts`. Tippen Sie auf "Neuer Host" und ein neues Fenster wird angezeigt.

![Termius ‘Neuer Host’-Konfiguration](images/ssh-ios-config.png)

Geben Sie einen "Alias" ein, beispielsweise "Raspberry Pi". Geben Sie dann unter `Hostname` die IP-Adresse ein. Füllen Sie die Felder "Benutzername" und "Passwort" aus und klicken Sie oben rechts auf "Speichern".

Wenn Sie die IP-Adresse nicht kennen, geben Sie `hostname -I` in die Befehlszeile des Raspberry Pi ein. Weitere Möglichkeiten zum Ermitteln Ihrer IP-Adresse finden Sie [hier](../ip-address.md). Die Standardanmeldung für Raspberry Pi OS ist `pi` mit dem Passwort `raspberry`.


## 2. Verbinden

Wenn Sie den neuen Host gespeichert haben, werden Sie zurück zum Bildschirm „Hosts“ geleitet. Dort finden Sie den neuen Eintrag. Stellen Sie sicher, dass auf Ihrem Mobilgerät die drahtlose Verbindung aktiviert ist und es mit demselben Netzwerk wie Ihr Raspberry Pi verbunden ist.

Tippen Sie einmal auf den neuen Eintrag. Wenn die Verbindung funktioniert, wird eine Sicherheitswarnung angezeigt. Keine Sorge, alles ist in Ordnung! Klicken Sie auf „Weiter“. Sie sehen diese Warnung nur, wenn Termius zum ersten Mal eine Verbindung zu einem Raspberry Pi herstellt, den es noch nie zuvor gesehen hat.


![Termius ‘Sicherheitswarnung’](images/ssh-ios-warning.png)

Sie sollten jetzt die Raspberry Pi-Eingabeaufforderung haben, die mit der auf dem Raspberry Pi selbst identisch ist.

```
pi@raspberrypi ~ $
```

Sie können `exit` eingeben, um das Terminalfenster zu schließen.

![Termius-Terminal](images/ssh-ios-window.png)

Wenn ein rotes Ausrufezeichen erscheint, bedeutet dies, dass etwas schief gelaufen ist. Tippen Sie auf das Ausrufezeichen, um die Fehlerbeschreibung anzuzeigen. „Zeitüberschreitung beim Verbindungsaufbau“ weist darauf hin, dass Sie wahrscheinlich eine falsche IP-Adresse eingegeben haben. Wenn die IP-Adresse korrekt ist, ist die drahtlose Verbindung auf Ihrem Mobilgerät möglicherweise deaktiviert; der Raspberry Pi ist möglicherweise ausgeschaltet; oder der Raspberry Pi und Ihr Mobilgerät sind möglicherweise mit verschiedenen Netzwerken verbunden.

## 3. Ändern eines Eintrags, Fehlerbehebung und mehr
Eine Verbindung kann aus verschiedenen Gründen nicht erfolgreich sein. Es ist wahrscheinlich, dass Ihr Gerät oder Raspberry Pi [nicht richtig verbunden](../../configuration/wireless/wireless-cli.md); [SSH ist deaktiviert](../../configuration/raspi-config.md); Ihr Code enthält einen Tippfehler; oder die IP-Adresse oder die Anmeldeinformationen haben sich geändert. In letzteren Fällen müssen Sie den Host aktualisieren.

Gehen Sie dazu zum Bildschirm „Hosts“, streichen Sie auf dem Host, den Sie bearbeiten möchten, nach links, und neue Funktionen werden angezeigt. Tippen Sie auf Bearbeiten. Ein neuer Bildschirm mit dem Titel „Host bearbeiten“ wird angezeigt.