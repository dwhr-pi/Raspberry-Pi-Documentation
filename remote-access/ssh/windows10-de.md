# SSH unter Windows 10

Sie können SSH verwenden, um eine Verbindung zu Ihrem Raspberry Pi von einem Windows 10-Computer herzustellen, der das *Oktober 2018-Update* oder höher verwendet, ohne Clients von Drittanbietern verwenden zu müssen.

Wenn Sie dies noch nicht getan haben, gehen Sie zu Einstellungen > Apps > Apps & Funktionen > Optionale Funktionen verwalten > Funktion hinzufügen und installieren Sie *OpenSSH-Client*.

Sie müssen die IP-Adresse Ihres Raspberry Pi kennen, um eine Verbindung herzustellen. Um dies zu finden, geben Sie `hostname -I` von Ihrem Raspberry Pi-Terminal ein.

Wenn Sie den Pi ohne Bildschirm (headless) betreiben, können Sie auch die Geräteliste Ihres Routers einsehen oder ein Tool wie `nmap` verwenden, das in unserer [IP-Adresse](../ip- Adresse.md) Dokument.

Um von einem anderen Computer aus eine Verbindung zu Ihrem Pi herzustellen, kopieren Sie den folgenden Befehl und fügen Sie ihn in das Terminalfenster ein, ersetzen Sie jedoch `<IP>` durch die IP-Adresse des Raspberry Pi. Verwenden Sie `Strg + Umschalt + V`, um das Terminal einzufügen.

```
ssh pi@<IP>
```

Wenn Sie auf dem Raspberry Pi einen anderen Benutzer eingerichtet haben, können Sie sich auf die gleiche Weise mit diesem verbinden, indem Sie den Standardbenutzer `pi` durch Ihren eigenen Benutzernamen ersetzen, z. `ssh eben@192.168.1.5`.

Wenn Sie die Fehlermeldung "Verbindungszeit überschritten" erhalten, haben Sie wahrscheinlich die falsche IP-Adresse für den Raspberry Pi eingegeben.

Wenn die Verbindung funktioniert, wird eine Sicherheits-/Authentizitätswarnung angezeigt. Geben Sie "ja" ein, um fortzufahren. Diese Warnung wird nur bei der ersten Verbindung angezeigt.

Für den Fall, dass Ihr Pi die IP-Adresse eines Geräts übernommen hat, mit dem Ihr Computer zuvor verbunden war (auch wenn sich dieses in einem anderen Netzwerk befand), werden Sie möglicherweise gewarnt und aufgefordert, den Eintrag aus Ihrer Liste der bekannten Geräte zu löschen. Wenn Sie dieser Anweisung folgen und den Befehl `ssh` erneut versuchen, sollte es erfolgreich sein.

Als nächstes werden Sie nach dem Passwort für den Benutzer gefragt, mit dem Sie versuchen, eine Verbindung herzustellen: Das Standardpasswort für den Benutzer "pi" auf Raspberry Pi OS ist "raspberry". Aus Sicherheitsgründen wird dringend empfohlen, das Standardpasswort auf dem Raspberry Pi zu ändern. Sie sollten jetzt die Raspberry Pi-Eingabeaufforderung sehen können, die mit der auf dem Raspberry Pi selbst identisch ist.

```
pi@raspberrypi ~ $
```

Sie sind nun remote mit dem Raspberry Pi verbunden und können Befehle ausführen.

Um Ihren Raspberry Pi so zu konfigurieren, dass er einen passwortlosen SSH-Zugriff mit einem öffentlichen/privaten Schlüsselpaar zulässt, lesen Sie die Anleitung [passwortloses SSH](passwordless.md).