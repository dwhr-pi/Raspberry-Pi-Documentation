# SSH unter Linux oder Mac OS

Sie können SSH verwenden, um sich von einem Linux-Computer, einem Mac oder einem anderen Raspberry Pi mit Ihrem Raspberry Pi zu verbinden, ohne zusätzliche Software zu installieren.

Sie müssen die IP-Adresse Ihres Raspberry Pi kennen, um eine Verbindung herzustellen. Um dies zu finden, geben Sie `hostname -I` von Ihrem Raspberry Pi-Terminal ein.

Wenn Sie den Pi ohne Bildschirm (headless) betreiben, können Sie auch die Geräteliste Ihres Routers einsehen oder ein Tool wie `nmap` verwenden, das in unserer [IP-Adresse](../ip- Adresse.md) Dokument.

Um sich von einem anderen Computer aus mit Ihrem Pi zu verbinden, kopieren Sie den folgenden Befehl und fügen Sie ihn in das Terminalfenster ein, ersetzen Sie jedoch `<IP>` durch die IP-Adresse des Raspberry Pi. Verwenden Sie `Strg + Umschalt + V`, um das Terminal einzufügen.

```
ssh pi@<IP>
```

Wenn Sie die Fehlermeldung "Verbindungszeit überschritten" erhalten, haben Sie wahrscheinlich die falsche IP-Adresse für den Raspberry Pi eingegeben.

Wenn die Verbindung funktioniert, wird eine Sicherheits-/Authentizitätswarnung angezeigt. Geben Sie "ja" ein, um fortzufahren. Diese Warnung wird nur bei der ersten Verbindung angezeigt.

Falls Ihr Pi die IP-Adresse eines Geräts übernommen hat, mit dem Ihr Computer zuvor verbunden war (auch wenn sich dieses in einem anderen Netzwerk befand), werden Sie möglicherweise gewarnt und aufgefordert, den Eintrag aus Ihrer Liste der bekannten Geräte zu löschen. Befolgen Sie diese Anweisung und versuchen Sie den Befehl `ssh` erneut, um erfolgreich zu sein.

Als nächstes werden Sie nach dem Passwort für die `pi`-Anmeldung gefragt: Das Standardpasswort auf Raspberry Pi OS ist `raspberry`. Aus Sicherheitsgründen wird dringend empfohlen, das Standardpasswort auf dem Raspberry Pi zu ändern. Sie sollten jetzt die Raspberry Pi-Eingabeaufforderung sehen können, die mit der auf dem Raspberry Pi selbst identisch ist.

Wenn Sie auf dem Raspberry Pi einen anderen Benutzer eingerichtet haben, können Sie sich auf die gleiche Weise mit diesem verbinden, indem Sie den Benutzernamen durch Ihren eigenen ersetzen, z. `eben@192.168.1.5`

```
pi@raspberrypi ~ $
```

Sie sind jetzt aus der Ferne mit dem Pi verbunden und können Befehle ausführen.

## X-Weiterleitung

Sie können Ihre X-Sitzung auch über SSH weiterleiten, um die Verwendung grafischer Anwendungen zu ermöglichen, indem Sie das Flag `-Y` verwenden:

```bash
ssh -Y pi@192.168.1.5
```
Beachten Sie, dass [X11 auf Macs mit OSX nicht mehr vorhanden ist](https://support.apple.com/en-gb/HT201341), also müssen Sie [herunterladen](https://www.xquartz.org/) und installieren Sie es.

Jetzt befinden Sie sich wie zuvor auf der Befehlszeile, haben jedoch die Möglichkeit, grafische Fenster zu öffnen. Geben Sie beispielsweise Folgendes ein:

```bash
geany &
```

öffnet den Geany-Editor in einem grafischen Fenster.

Eingabe:

```bash
scratch &
```

wird Scratch öffnen.

Für weitere Dokumentation zum `ssh`-Befehl geben Sie einfach `man ssh` in das Terminal ein.

Um Ihren Pi so zu konfigurieren, dass er einen passwortlosen SSH-Zugriff mit einem öffentlichen/privaten Schlüsselpaar zulässt, lesen Sie die Anleitung [passwortloses SSH](passwordless.md).