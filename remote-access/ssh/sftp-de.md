# SFTP

Das SSH File Transfer Protocol ist ein Netzwerkprotokoll, das Dateizugriffs-, Dateiübertragungs- und Dateiverwaltungsfunktionen über SSH bietet.

Mit SFTP können Sie Dateien auf Ihrem Raspberry Pi ganz einfach ändern, durchsuchen und bearbeiten. SFTP ist einfacher einzurichten als [FTP](../ftp.md), wenn das Raspberry Pi OS SSH aktiviert hat. Aus Sicherheitsgründen ist der SSH-Server seit der Veröffentlichung des Raspberry Pi OS vom November 2016 standardmäßig deaktiviert. Um es zu aktivieren, folgen Sie bitte [diesen Anweisungen](./README.md).)

## WinSCP unter Windows

Wir empfehlen die Verwendung des [WinSCP SFTP-Client](https://winscp.net/eng/index.php). Folgen Sie den Anweisungen auf der WinSCP-Website, um den Client zu installieren, und folgen Sie dann den [WinSCP-Schnellstartanweisungen](https://winscp.net/eng/docs/getting_started).

## FileZilla unter Linux

Installieren Sie FileZilla auf Ihrem Linux-System mit dem Standard-Paketmanager für Ihre Distribution (z. B. `sudo apt install filezilla`).

Starten Sie FileZilla und gehen Sie zu **Datei > Site-Manager**.

Geben Sie [IP-Adresse](../ip-address.md), Benutzername und Passwort (standardmäßig ist der Benutzername `pi` und das Passwort `raspberry`) Ihres Raspberry Pi in den Dialog ein und wählen Sie **SFTP* * als Protokoll.

Klicken Sie auf `Verbinden` und Sie sehen den Home-Ordner des Benutzers.

## Ubuntu mit Nautilus

Öffnen Sie Nautilus auf dem Client-Rechner.

Wählen Sie **Datei > Mit Server verbinden**.

```
Type: SSH
Server: <Die IP-Adresse des Pi>
Port: 22 (Standard)
Benutzername: pi (Standard)
Passwort: Himbeere (Standard)
```
## Chrome OS-Dateimanager (getestet auf Acer Chromebook)

Öffnen Sie die Dateimanager-App des Chromebooks.

Scrollen Sie zum Ende des Dateibaums im linken Bereich.

Klicken Sie auf **Add new services** bzw. **Neue Dienste hinzufügen**

```
Select and click
SFTP file system
```

Geben Sie in das sich öffnende Dialogfeld ein:

```
Die IP-Adresse oder
Der Hostname (Standard ist `raspberrypi`)
Geben Sie Port 22 ein (nicht der, der neben der IP-Adresse auf Ihrem Pi angezeigt wird)
Fügen Sie den Benutzer `pi` und das Passwort hinzu (Standard ist `raspberry`)
```
Aus Sicherheitsgründen wird möglicherweise ein weiteres Dialogfeld angezeigt. Klicken Sie in diesem Fall auf **Allow** bzw. **Zulassen** oder **Accept** bzw. **Akzeptieren**.