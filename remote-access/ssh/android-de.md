# SSH mit Android

Um SSH auf Ihrem Mobilgerät zu verwenden, müssen Sie einen Client herunterladen. Es stehen verschiedene Clients von guter Qualität zur Verfügung, wie z. B. [Termius](http://www.termius.com), [JuiceSSH](https://juicessh.com/) und [Connectbot](https://connectbot .org/). Für dieses Tutorial verwenden wir Termius, da es ein beliebter plattformübergreifender SSH-Client ist. Für andere Clients wird der Prozess ähnlich sein.


## 1. Fügen Sie Ihren Raspberry Pi als Host hinzu

Laden Sie Termius von [Google Play](https://play.google.com/store/apps/details?id=com.server.auditor.ssh.client) herunter, falls Sie es noch nicht installiert haben. Klicken Sie auf die App, um diese zu öffnen. 

Die App sollte sich öffnen und "Keine Hosts" anzeigen. Um zu beginnen, sollten Sie auf die blaue Schaltfläche "+" in der unteren linken Ecke tippen. Tippen Sie dann auf „Neuer Gastgeber“.

![Termius ‘Neuer Host’-Konfiguration](images/ssh-android-config.png)

Geben Sie einen "Alias" ein, z. B. Raspberry Pi. Geben Sie dann unter `Hostname` die IP-Adresse ein. Geben Sie `Benutzername` und `Passwort` ein und klicken Sie auf das Häkchen `✓` in der oberen rechten Ecke.

Wenn Sie die IP-Adresse nicht kennen, geben Sie `hostname -I` in die Befehlszeile des Raspberry Pi ein. Weitere Möglichkeiten zum Ermitteln Ihrer IP-Adresse finden Sie [hier](../ip-address.md). Die Standardanmeldung für Raspberry Pi OS ist `pi` mit dem Passwort `raspberry`.


## 2. Verbinden

Wenn Sie den neuen Host gespeichert haben, werden Sie zurück zum Bildschirm „Hosts“ geleitet. Dort finden Sie den neuen Eintrag. Stellen Sie sicher, dass auf Ihrem Mobilgerät die drahtlose Verbindung aktiviert ist und dass es mit demselben Netzwerk wie Ihr Raspberry Pi verbunden ist.

Tippen Sie einmal auf den neuen Eintrag. Wenn die Verbindung funktioniert, wird eine Sicherheitswarnung angezeigt. Keine Sorge: Alles ist in Ordnung. Klicken Sie auf „Verbinden“. Sie sehen diese Warnung nur, wenn Termius zum ersten Mal eine Verbindung zu einem Pi herstellt, das es noch nie zuvor gesehen hat.

![Termius ‘Sicherheitswarnung’](images/ssh-android-warning.png)

Sie sollten nun die Eingabeaufforderung des Raspberry Pi sehen, die mit der auf dem Raspberry Pi selbst identisch ist.

```
pi@raspberrypi ~ $
```

Sie können `exit` eingeben, um das Terminalfenster zu schließen.

![Termius-Terminal](images/ssh-android-window.png)

Wenn ein Dialog mit der Meldung `Verbindung fehlgeschlagen Verbindung mit 192.xxx.xxx.xxx Port 22` erscheint, ist es wahrscheinlich, dass Sie eine falsche IP-Adresse eingegeben haben. Wenn die IP-Adresse korrekt ist, ist die drahtlose Verbindung auf Ihrem Mobilgerät möglicherweise deaktiviert; der Raspberry Pi ist möglicherweise ausgeschaltet; oder der Raspberry Pi und Ihr Mobilgerät können mit verschiedenen Netzwerken verbunden sein.


## 3. Ändern eines Eintrags, Fehlerbehebung und mehr

Eine Verbindung kann aus verschiedenen Gründen nicht erfolgreich sein. Die wahrscheinlichsten Gründe sind, dass Ihr Gerät oder Raspberry Pi [nicht richtig verbunden](../../configuration/wireless/wireless-cli.md); [SSH ist deaktiviert](../../configuration/raspi-config.md); Ihr Code enthält einen Tippfehler oder die IP-Adresse oder die Anmeldeinformationen haben sich geändert. In letzteren Fällen müssen Sie den Host aktualisieren.

Gehen Sie dazu zum Bildschirm Hosts und tippen und halten Sie den entsprechenden Eintrag. In der oberen rechten Ecke werden neue Funktionen angezeigt. Tippen Sie auf das Bleistiftsymbol. Ein neuer Bildschirm mit dem Titel „Host bearbeiten“ wird angezeigt.