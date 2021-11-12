# Heim

Wenn Sie sich bei einem Pi anmelden und ein Terminalfenster öffnen oder über die Befehlszeile anstelle der grafischen Benutzeroberfläche booten, starten Sie in Ihrem Home-Ordner; diese befindet sich unter `/home/pi`, vorausgesetzt Ihr Benutzername ist `pi`.

Hier werden die eigenen Dateien des Benutzers gespeichert. Der Inhalt des Desktops des Benutzers befindet sich zusammen mit anderen Dateien und Ordnern in einem Verzeichnis namens `Desktop`.

Um in der Befehlszeile zu Ihrem Home-Ordner zu navigieren, geben Sie einfach „cd“ ein und drücken Sie „Enter“. Dies entspricht der Eingabe von `cd /home/pi`, wobei `pi` Ihr Benutzername ist. Sie können auch die Tilde-Taste (`~`) verwenden, zum Beispiel `cd ~`, die verwendet werden kann, um relativ zurück zu Ihrem Home-Ordner zu gelangen. Zum Beispiel ist `cd ~/Desktop/` dasselbe wie `cd /home/pi/Desktop`.

Navigieren Sie zu `/home/` und führen Sie `ls` aus, und Sie sehen die Home-Ordner jedes Benutzers auf dem System.

Beachten Sie, dass wenn Sie als Root-Benutzer angemeldet sind, die Eingabe von `cd` oder `cd ~` Sie zum Home-Verzeichnis des Root-Benutzers führt; im Gegensatz zu normalen Benutzern befindet sich dies unter `/root/` und nicht in `/home/root/`. Lesen Sie mehr über den [root-Benutzer](../usage/root.md).

Wenn Sie Dateien haben, die Sie nicht verlieren möchten, sollten Sie Ihren Home-Ordner sichern. Lesen Sie mehr über [Backup](backup.md).