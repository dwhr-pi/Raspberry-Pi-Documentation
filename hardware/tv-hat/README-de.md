## Erste Schritte mit dem Raspberry Pi TV HAT

Der TV HAT verfügt über einen integrierten DVB-T2-Tuner, mit dem Sie digitale Fernsehstreams auf Ihrem Raspberry Pi empfangen und dekodieren können. Dann können Sie diese Streams auf dem Pi oder auf jedem Computer ansehen, der mit demselben Netzwerk wie der Pi verbunden ist.

Die von uns empfohlene Software zum Decodieren der Streams (bekannt als Multiplexe oder kurz Muxe) und zum Anzeigen von Inhalten heißt TVHeadend. Anweisungen zur Einrichtung finden Sie unten. Der TV HAT kann jeweils einen Mux dekodieren, und jeder Mux kann mehrere Kanäle zur Auswahl enthalten. Inhalte können entweder auf dem Raspberry Pi angezeigt werden, an den der TV-HAT angeschlossen ist, oder an ein anderes Gerät im selben Netzwerk gesendet werden.

**Du wirst brauchen:**
* Eine Fernsehantenne
* Ein Raspberry Pi TV HAT mit seinen Abstandshaltern, Schrauben und Antennenadapter
* Ein Raspberry Pi, der mit dem Internet verbunden ist (plus Maus, Tastatur und Display, falls
Sie greifen nicht aus der Ferne auf den Pi zu)
* Optional: ein anderer Computer, der mit demselben Netzwerk verbunden ist

### Setup-Anweisungen

**Auf Ihrem Raspberry Pi:**

* Verbinden Sie den Antennenadapter mit dem TV HAT:
  * Wenn der Adapter von den USB-Anschlüssen weg zeigt, drücken Sie den HAT vorsichtig über die GPIO-Pins des Raspberry Pi
  * Platzieren Sie die Abstandshalter an zwei oder drei Ecken des HAT und ziehen Sie die Schrauben durch die Halterung fest
Löcher, um sie an Ort und Stelle zu halten.
* Verbinden Sie den Antennenadapter des TV HAT mit dem Kabel Ihrer TV-Antenne.
* Richten Sie den Raspberry Pi mit der neuesten Version des Betriebssystems Raspberry Pi OS ein, die Sie von unserer [Download-Seite] (https://www.raspberrypi.org/downloads/raspbian/) herunterladen können.
 * Wenn Sie nicht wissen, wie das geht, folgen Sie unserer Anleitung [hier](https://projects.raspberrypi.org/en/pathways/getting-started-with-raspberry-pi)
* Starten Sie Ihren Pi, öffnen Sie ein Terminalfenster und führen Sie die folgenden zwei Befehle aus, um die `tvheadend`-Software zu installieren:
```
sudo apt-Update
sudo apt installieren tvheadend
```
  * Wenn Sie nicht wissen, wie das geht, folgen Sie unserer Anleitung [hier] (https://projects.raspberrypi.org/en/projects/raspberry-pi-using/9)
* Während der Installation von `tvheadend` werden Sie aufgefordert, einen Administratorkontonamen und ein Passwort zu wählen. Sie werden diese später brauchen, also stellen Sie sicher, dass Sie etwas auswählen, an das Sie sich erinnern können.

**In einem Webbrowser auf einem anderen Computer:**

* Geben Sie Folgendes in die Adressleiste ein: `http://raspberrypi.local:9981/extjs.html`
* Dies sollte eine Verbindung zu `tvheadend` herstellen, das auf dem Raspberry Pi läuft.
  * Wenn die obige Adresse nicht funktioniert, müssen Sie die IP-Adresse des Pi herausfinden. Öffnen Sie ein Terminalfenster auf Ihrem Pi und führen Sie den Befehl `hostname -I` . aus
  * Sie sehen die IP-Adresse in einem oder zwei Formaten: eine Reihe von vier durch Punkte getrennten Zahlen, dann, wenn Sie sich in einem IPv6-Netzwerk befinden, ein Leerzeichen, dann eine lange Reihe von Zahlen und Buchstaben, die durch Doppelpunkte getrennt sind.
  * Notieren Sie alles vor dem Leerzeichen (die vier Zahlen und Punkte) und geben Sie dies anstelle des raspberrypi.local-Teils der Adresse in die Adressleiste ein.
* Sobald Sie sich über den Browser mit `tvheadend` verbunden haben, werden Sie aufgefordert, sich anzumelden. Verwenden Sie den Kontonamen und das Passwort, die Sie bei der Installation von `tvheadend` auf dem Pi gewählt haben. Ein Setup-Assistent sollte erscheinen.
* Stellen Sie zuerst die Sprache ein, die `tvheadend` verwenden soll (**Englisch (GB)** hat bei uns funktioniert; andere Sprachen haben wir noch nicht getestet).
* Richten Sie als Nächstes den Netzwerk-, Benutzer- und Administratorzugriff ein. Wenn Sie keine bestimmten Einstellungen haben, lassen Sie **Zugelassenes Netzwerk** leer und geben Sie ein Sternchen (*) in die Felder **Benutzername** und **Passwort** ein. Dadurch kann jeder, der mit Ihrem lokalen Netzwerk verbunden ist, auf "tvheadend" zugreifen.
* Sie sollten ein Fenster mit dem Titel **Netzwerkeinstellungen** sehen. Unter **Netzwerk 2** sollten Sie `Tuner: Sony CDX2880 #0 : DVB-T #0` sehen. Wählen Sie für **Netzwerktyp** `DVB-T-Netzwerk`.
* Das nächste Fenster ist **Vordefinierte Muxe den Netzwerken zuweisen**; hier wählen Sie den zu empfangenden und zu dekodierenden TV-Stream aus. Wählen Sie unter Netzwerk 1 für vordefinierte Muxe Ihren lokalen TV-Sender aus.
  * Ihren lokalen Sender finden Sie auf der [Freeview-Website](https://www.freeview.co.uk/help). Geben Sie Ihre Postleitzahl ein, um zu sehen, welcher Sender Ihnen ein gutes Signal geben soll.
* Wenn Sie auf **Speichern & Weiter** klicken, beginnt die Software mit der Suche nach dem ausgewählten Mux und zeigt einen Fortschrittsbalken an. Nach etwa zwei Minuten sollten Sie etwa Folgendes sehen:
```
Gefundene Muxe: 8
Gefundene Dienste: 172
```
* Aktivieren Sie im nächsten Fenster mit dem Titel **Dienstzuordnung** alle drei Kästchen: **Alle Dienste zuordnen**, **Anbieter-Tags erstellen** und **Netzwerk-Tags erstellen**.
* Als nächstes sollten Sie eine Liste der TV-Kanäle sehen, die Sie sehen können, zusammen mit den Programmen, die sie gerade zeigen.
* Um einen TV-Kanal im Browser anzusehen, klicken Sie auf das kleine TV-Symbol links neben dem Kanal
Auflistung, direkt rechts neben dem **i**-Symbol. Dies ruft einen Mediaplayer im Browser auf. Abhängig von den in Ihrem Browser integrierten Decodierungsfunktionen und der Art des wiedergegebenen Streams kann es sein, dass die Wiedergabe ruckelt. In diesen Fällen empfehlen wir die Verwendung eines lokalen Mediaplayers als Wiedergabeanwendung.
* Um einen Fernsehsender in einem lokalen Mediaplayer anzusehen, z.B. VLC [www.videolan.org/vlc](https://www.videolan.org/vlc), müssen Sie es als Stream herunterladen. Klicken Sie auf das „i“-Symbol links neben einer Kanalliste, um das Informationsfenster für diesen Kanal anzuzeigen. Hier sehen Sie eine Stream-Datei, die Sie herunterladen können.

`tvheadend` wird von zahlreichen Apps unterstützt, wie z.B. TvhClient für iOS, die TV vom Pi abspielen. OMXPlayer, der mit Raspberry Pi OS geliefert wird, unterstützt auch die Anzeige von TV-Streams von `tvheadend`. Kodi, das in den Raspberry Pi OS-Repos verfügbar ist, bietet hervorragende Möglichkeiten zum Abspielen von Live-TV, zusammen mit zuvor aufgezeichneten Kanälen und zeitgesteuerten Serienaufnahmen.

Um andere Funktionen oder Verwendungen des TV HAT zu diskutieren, besuchen Sie bitte unsere [Foren](https://www.raspberrypi.org/forums).