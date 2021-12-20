# SSH (Secure Shell)

Sie können von einem anderen Computer oder Gerät im selben Netzwerk mit SSH aus der Ferne auf die Befehlszeile eines Raspberry Pi zugreifen.

Der Raspberry Pi fungiert als Remote-Gerät: Sie können sich über einen Client auf einem anderen Computer mit ihm verbinden.

Sie haben nur Zugriff auf die Befehlszeile, nicht auf die vollständige Desktop-Umgebung. Einen vollständigen Remote-Desktop finden Sie unter [VNC](../vnc/README.md).

## 1. Richten Sie Ihr lokales Netzwerk und Ihre drahtlose Verbindung ein

Stellen Sie sicher, dass Ihr Raspberry Pi richtig eingerichtet und angeschlossen ist. Wenn Sie ein drahtloses Netzwerk verwenden, kann dies über die [Benutzeroberfläche] des Desktops (../../configuration/wireless/README.md) oder über die [Befehlszeile](../../configuration/ wireless/wireless-cli.md).

Wenn Sie keine drahtlose Verbindung verwenden, schließen Sie Ihren Raspberry Pi direkt an den Router an.

Sie müssen sich die IP-Adresse Ihres Pi notieren, um sich später damit zu verbinden. Wenn Sie den Befehl `ifconfig` verwenden, werden Informationen über den aktuellen Netzwerkstatus angezeigt, einschließlich der IP-Adresse, oder Sie können `hostname -I` verwenden, um die mit dem Gerät verknüpften IP-Adressen anzuzeigen.

## 2. SSH aktivieren

Ab der Version vom November 2016 ist bei Raspberry Pi OS der SSH-Server standardmäßig deaktiviert. Es kann manuell vom Desktop aus aktiviert werden:

1. Starten Sie `Raspberry Pi Configuration` aus dem Menü `Preferences`
1. Navigieren Sie zum Reiter `Interfaces` bzw. „Schnittstellen“.
1. Wählen Sie `Enabled` bzw. „Aktiviert“ neben dem `SSH`
1. Klicken Sie auf `OK`

Alternativ kann im Terminal auch [raspi-config](../../configuration/raspi-config.md) verwendet werden:

1. Geben Sie `sudo raspi-config` in ein Terminalfenster ein
1. Wählen Sie `Interfacing Options` bzw. `Schnittstellenoptionen`
1. Navigieren Sie zu und wählen Sie `SSH`
1. Wählen Sie `Yes` bzw. `Ja`
1. Wählen Sie `Ok`
1. Wählen Sie `Finish` bzw. `Fertigstellen`

Alternativ verwenden Sie `systemctl`, um den Dienst zu starten

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

Wenn Sie SSH auf einem Pi aktivieren, das möglicherweise mit dem Internet verbunden ist, sollten Sie das Standardkennwort ändern, um sicherzustellen, dass es sicher bleibt. Weitere Informationen finden Sie auf der [Sicherheitsseite](../../configuration/security.md).

## 3. SSH auf einem kopflosen Raspberry Pi aktivieren (Datei zur SD-Karte auf einem anderen Computer hinzufügen)

Für das Headless-Setup kann SSH aktiviert werden, indem eine Datei namens `ssh` ohne Erweiterung auf der Boot-Partition der SD-Karte von einem anderen Computer platziert wird. Wenn der Pi bootet, sucht er nach der `ssh`-Datei. Wenn es gefunden wird, wird SSH aktiviert und die Datei gelöscht. Der Inhalt der Datei spielt keine Rolle; es kann Text oder gar nichts enthalten.

Wenn Sie Raspberry Pi OS auf eine leere SD-Karte geladen haben, haben Sie zwei Partitionen. Die erste, die kleinere ist, ist die Boot-Partition. Legen Sie die Datei in diese ein.

## 4. Richten Sie Ihren Client ein

SSH ist in Linux-Distributionen und Mac OS integriert und ist eine optionale Funktion in Windows 10. Für ältere Windows-Versionen und mobile Geräte sind SSH-Clients von Drittanbietern verfügbar. Lesen Sie die folgenden Anleitungen zur Verwendung von SSH mit dem Betriebssystem auf Ihrem Computer oder Gerät:

- [Linux & Mac OS](unix.md)
- [Windows 10](windows10.md)
- [Windows](windows.md)
- [iOS](ios.md)
- [Android](android.md)