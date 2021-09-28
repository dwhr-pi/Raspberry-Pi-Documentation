# Einstellen des Bildschirmschoners / Bildschirmausblendung

## Auf der Konsole

Wenn es ohne grafischen Desktop läuft, blendet Raspberry Pi OS den Bildschirm nach 10 Minuten ohne Benutzereingaben aus, z. Mausbewegung oder Tastendruck.

Die aktuelle Einstellung in Sekunden kann angezeigt werden mit:
```
cat /sys/module/kernel/parameters/consoleblank
```

Um die `consoleblank`-Einstellung zu ändern, bearbeiten Sie die Kernel-Befehlszeile:

```
sudo nano /boot/cmdline.txt
```

Die Datei `/boot/cmdline.txt` enthält eine einzelne Textzeile. Fügen Sie `consoleblank=n` hinzu, damit die Konsole nach `n` Sekunden Inaktivität leer bleibt. Zum Beispiel bewirkt `consoleblank=300`, dass die Konsole nach 300 Sekunden, 5 Minuten Inaktivität, leer wird. Stellen Sie sicher, dass Sie die Option `consoleblank` zu der einzelnen Textzeile hinzufügen, die sich bereits in der Datei `cmdline.txt` befindet. Um die Bildschirmausblendung zu deaktivieren, setzen Sie `consoleblank=0`.

Sie können auch das Tool `raspi-config` verwenden, um die Bildschirmausblendung zu deaktivieren. Beachten Sie, dass die Einstellung für die Bildschirmausblendung in `raspi-config` auch die Bildschirmausblendung steuert, wenn der grafische Desktop ausgeführt wird.

## Auf dem Raspberry Pi-Desktop

Raspberry Pi OS leert den grafischen Desktop nach 10 Minuten ohne Benutzereingabe. Sie können dies deaktivieren, indem Sie die Option "Screen Blanking" im Raspberry Pi-Konfigurationstool ändern, das im Menü "Einstellungen" verfügbar ist. Beachten Sie, dass die Option 'Screen Blanking' auch die Bildschirmausblendung steuert, wenn der grafische Desktop nicht ausgeführt wird.

Es steht auch ein grafischer Bildschirmschoner zur Verfügung, der wie folgt installiert werden kann:

```
sudo apt install xscreensaver
```

Das kann ein paar minuten dauern.

Nach der Installation finden Sie die Anwendung Bildschirmschoner im Menü Einstellungen: Sie bietet viele Optionen zum Einrichten des Bildschirmschoners, einschließlich der vollständigen Deaktivierung.