# Drahtlose Konnektivität im Raspberry Pi Desktop

Es wird eine GUI zum Einrichten von drahtlosen Verbindungen im Raspberry Pi OS mit dem Raspberry Pi Desktop bereitgestellt. Wenn Sie den Raspberry Pi Desktop nicht verwenden, können Sie über die [Befehlszeile] (wireless-cli.md) ein drahtloses Netzwerk einrichten.

Drahtlose Verbindungen können über das Netzwerksymbol am rechten Ende der Menüleiste hergestellt werden. Wenn Sie ein Pi mit integrierter drahtloser Konnektivität verwenden oder wenn ein drahtloser Dongle angeschlossen ist, wird durch Klicken mit der linken Maustaste auf dieses Symbol eine Liste der verfügbaren drahtlosen Netzwerke angezeigt, wie unten gezeigt. Wenn keine Netzwerke gefunden werden, wird die Meldung 'Keine APs gefunden - scannen...' angezeigt. Warten Sie einige Sekunden, ohne das Menü zu schließen, und es sollte Ihr Netzwerk finden.

Beachten Sie, dass auf Raspberry Pi-Geräten, die das 5-GHz-Band (Pi3B+, Pi4, CM4, Pi400) unterstützen, die drahtlose Vernetzung aus regulatorischen Gründen deaktiviert ist, bis der Ländercode festgelegt wurde. Um den Ländercode einzustellen, öffnen Sie die Anwendung `Raspberry Pi Configuration` aus dem Preferences Menu, wählen Sie **Localization** und stellen Sie den entsprechenden Code ein.

![wifi2](images/wifi2.png)

Die Symbole auf der rechten Seite zeigen an, ob ein Netzwerk gesichert ist oder nicht und geben einen Hinweis auf seine Signalstärke. Klicken Sie auf das Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Wenn es gesichert ist, werden Sie in einem Dialogfeld aufgefordert, den Netzwerkschlüssel einzugeben:

![key](images/key.png)

Geben Sie den Schlüssel ein und klicken Sie auf **OK**, dann warten Sie einige Sekunden. Das Netzwerksymbol blinkt kurz, um anzuzeigen, dass eine Verbindung hergestellt wird. Wenn es bereit ist, hört das Symbol auf zu blinken und zeigt die Signalstärke an.