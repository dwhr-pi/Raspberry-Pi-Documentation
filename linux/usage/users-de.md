# Linux-Benutzer

Die Benutzerverwaltung im Raspberry Pi OS erfolgt über die Befehlszeile. Der Standardbenutzer ist `pi` und das Passwort ist `raspberry`. Sie können Benutzer hinzufügen und das Kennwort jedes Benutzers ändern.

## Ändern Sie Ihr Passwort

Sobald Sie als `pi`-Benutzer angemeldet sind, ist es sehr ratsam, den `passwd`-Befehl zu verwenden, um das Standardpasswort zu ändern, um die Sicherheit Ihres Pi zu verbessern.

Geben Sie `passwd` in die Befehlszeile ein und drücken Sie `Enter`. Sie werden zur Authentifizierung aufgefordert, Ihr aktuelles Passwort einzugeben, und dann nach einem neuen Passwort gefragt. Drücken Sie zum Abschluss die Eingabetaste und Sie werden aufgefordert, dies zu bestätigen. Beachten Sie, dass bei der Eingabe Ihres Passworts keine Zeichen angezeigt werden. Sobald Sie Ihr Passwort korrekt bestätigt haben, wird Ihnen eine Erfolgsmeldung angezeigt (`passwd: Passwort erfolgreich aktualisiert`) und das neue Passwort wird sofort angewendet.

Wenn Ihr Benutzer `sudo`-Berechtigungen hat, können Sie das Passwort eines anderen Benutzers mit `passwd` gefolgt vom Benutzernamen des Benutzers ändern. Mit `sudo passwd bob` können Sie beispielsweise das Passwort des Benutzers `bob` setzen und dann einige zusätzliche optionale Werte für den Benutzer wie seinen Namen. Drücken Sie einfach die Eingabetaste, um jede dieser Optionen zu überspringen.

### Passwort eines Benutzers entfernen

Das Passwort für den Benutzer `bob` können Sie mit `sudo passwd bob -d` entfernen.

## Neuen Benutzer erstellen

Mit dem Befehl `adduser` können Sie zusätzliche Benutzer auf Ihrer Raspberry Pi OS-Installation erstellen.

Geben Sie `sudo adduser bob` ein und Sie werden nach einem Passwort für den neuen Benutzer `bob` gefragt. Lassen Sie dieses Feld leer, wenn Sie kein Passwort wünschen.

### Home-Ordner

Wenn Sie einen neuen Benutzer erstellen, hat dieser einen Home-Ordner in `/home/`. Der Home-Ordner des `pi`-Benutzers befindet sich unter `/home/pi/`.

#### skel

Beim Anlegen eines neuen Benutzers wird der Inhalt von `/etc/skel/` in den Home-Ordner des neuen Benutzers kopiert. Sie können Punktdateien wie die `.bashrc` in `/etc/skel/` nach Ihren Wünschen hinzufügen oder ändern, und diese Version wird auf neue Benutzer angewendet.

## Sudoers

Der Standardbenutzer "pi" auf Raspberry Pi OS ist Mitglied der Gruppe "sudo". Dies gibt die Möglichkeit, Befehle als root auszuführen, wenn ihnen `sudo` vorangestellt ist, und mit `sudo su` zum Root-Benutzer zu wechseln.

Um einen neuen Benutzer zur Gruppe `sudo` hinzuzufügen, verwenden Sie den Befehl `adduser`:

```bash
sudo adduser bob sudo
```

Beachten Sie, dass der Benutzer `bob` aufgefordert wird, sein Passwort einzugeben, wenn er `sudo` ausführt. Dies unterscheidet sich vom Verhalten des `pi`-Benutzers, da `pi` nicht nach seinem Passwort gefragt wird. Wenn Sie die Passwortabfrage des neuen Benutzers entfernen möchten, erstellen Sie eine benutzerdefinierte sudoers-Datei und legen Sie sie im Verzeichnis `/etc/sudoers.d` ab.

1. Erstellen Sie die Datei mit `sudo visudo /etc/sudoers.d/010_bob-nopasswd`.
1. Fügen Sie folgenden Inhalt in eine einzige Zeile ein: `bob ALL=(ALL) NOPASSWD: ALL`
1. Speichern Sie die Datei und beenden Sie sie.

Nachdem Sie den Editor verlassen haben, wird die Datei auf Syntaxfehler überprüft. Wenn keine Fehler festgestellt wurden, wird die Datei gespeichert und Sie kehren zum Shell-Prompt zurück. Wenn Fehler festgestellt wurden, werden Sie gefragt, was nun? Drücken Sie die Eingabetaste auf Ihrer Tastatur: Dadurch wird eine Liste mit Optionen angezeigt. Wahrscheinlich möchten Sie 'e' für '(e)dit sudoers file again' verwenden, damit Sie die Datei bearbeiten und das Problem beheben können. **Beachten Sie, dass die Auswahl der Option 'Q' die Datei mit allen noch vorhandenen Syntaxfehlern speichert, was es _jedem_ Benutzern unmöglich macht, den sudo-Befehl zu verwenden.**

Beachten Sie, dass es unter Linux üblich ist, dass der Benutzer beim Ausführen von `sudo` nach seinem Passwort gefragt wird, da dies das System etwas sicherer macht.

## Einen Benutzer löschen

Sie können einen Benutzer auf Ihrem System mit dem Befehl `userdel` löschen. Wenden Sie das Flag "-r" an, um auch ihren Home-Ordner zu entfernen:

```bash
sudo userdel -r bob
```