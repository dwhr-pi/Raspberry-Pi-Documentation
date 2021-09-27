##Samba/CIFS

Samba ist eine Implementierung des SMB/CIFS-Netzwerkprotokolls, das von Windows-Geräten verwendet wird, um gemeinsamen Zugriff auf Dateien, Drucker und serielle Ports usw. bereitzustellen. Es gibt eine umfassende [Wikipedia-Seite](https://en.wikipedia.org/ wiki/Samba_%28software%29) über Samba und seine Fähigkeiten.

Auf dieser Seite wird erklärt, wie Sie eine Teilmenge des Samba-Systems verwenden, um einen freigegebenen Ordner auf einem Windows-Gerät zu "mounten", damit er auf Ihrem Raspberry Pi angezeigt wird, oder um einen Ordner auf Ihrem Raspberry Pi freizugeben, damit ein Windows-Client darauf zugreifen kann .

### CIFS/Samba-Unterstützung installieren

Das Betriebssystem Raspberry Pi bietet standardmäßig keine CIFS/Samba-Unterstützung, die jedoch problemlos hinzugefügt werden kann. Die folgenden Befehle installieren alle erforderlichen Komponenten, um Samba als Server oder Client zu verwenden.

```bash
sudo apt-Update
sudo apt install samba samba-common-bin smbclient cifs-utils
```

### Verwenden eines freigegebenen Windows-Ordners

Zuerst müssen Sie einen Ordner auf Ihrem Windows-Gerät freigeben. Das ist ein ziemlich komplizierter Prozess!

#### Freigabe aktivieren

1. Öffnen Sie das Netzwerk- und Freigabecenter, indem Sie mit der rechten Maustaste auf die Taskleiste klicken und sie auswählen
1. Klicken Sie auf **Erweiterte Freigabeeinstellungen ändern**
1. Wählen Sie **Netzwerkerkennung aktivieren**
1. Wählen Sie **Datei- und Druckerfreigabe aktivieren**.
1. Änderungen speichern

#### Den Ordner teilen

Sie können jeden beliebigen Ordner freigeben, aber für dieses Beispiel erstellen Sie einfach einen Ordner namens `share`.

1. Erstellen Sie auf Ihrem Desktop den Ordner `share`.
1. Klicken Sie mit der rechten Maustaste auf den neuen Ordner und wählen Sie **Eigenschaften**.
1. Klicken Sie auf die Registerkarte **Freigabe** und dann auf die Schaltfläche **Erweiterte Freigabe**
1. Wählen Sie **Diesen Ordner freigeben**; Standardmäßig ist der Freigabename der Name des Ordners
1. Klicken Sie auf die Schaltfläche **Berechtigungen**
1. Wählen Sie für dieses Beispiel **Alle** und **Vollzugriff** (Sie können den Zugriff bei Bedarf auf bestimmte Benutzer beschränken); Klicken Sie auf **OK**, wenn Sie fertig sind, und dann erneut auf **OK**, um die Seite **Erweiterte Freigabe** zu verlassen
1. Klicken Sie auf die Registerkarte **Sicherheit**, da wir jetzt dieselben Berechtigungen konfigurieren müssen
1. Wählen Sie die gleichen Einstellungen wie auf der Registerkarte **Berechtigungen** und fügen Sie bei Bedarf den ausgewählten Benutzer hinzu
1. Klicken Sie auf **OK**

Der Ordner sollte jetzt freigegeben werden.

#### Freigabeassistent für Windows 10

Unter Windows 10 gibt es einen Freigabeassistenten, der bei einigen dieser Schritte hilft.

1. Führen Sie die Computerverwaltungsanwendung über die Startleiste aus
1. Wählen Sie **Freigegebene Ordner** und dann **Freigaben**
1. Klicken Sie mit der rechten Maustaste und wählen Sie **Neue Freigabe**, wodurch der Freigabeassistent gestartet wird; Weiter klicken**
1. Wählen Sie den Ordner aus, den Sie freigeben möchten, und klicken Sie auf **Weiter**
1. Klicken Sie auf **Weiter**, um alle Freigabeeinstellungen zu verwenden
1. Wählen Sie **Benutzerdefiniert** und legen Sie die erforderlichen Berechtigungen fest. Klicken Sie auf **OK** und dann auf **Fertig stellen**

#### Mounten Sie den Ordner auf dem Raspberry Pi

**Mounting** in Linux ist der Vorgang des Anhängens eines Ordners an einen Speicherort, daher benötigen wir zuerst diesen Speicherort.

```bash
mkdir Fensterfreigabe
```

Jetzt müssen wir den Remote-Ordner an diesem Ort mounten. Der Remote-Ordner ist der Hostname oder die IP-Adresse des Windows-PCs und der bei der Freigabe verwendete Freigabename. Wir müssen auch den Windows-Benutzernamen angeben, der für den Zugriff auf den Remote-Computer verwendet wird.

```bash
sudo mount.cifs //<Hostname oder IP-Adresse>/share /home/pi/windowshare -o user=<name>
```

Sie sollten jetzt den Inhalt der Windows-Freigabe auf Ihrem Raspberry Pi anzeigen können.

```bash
CD-Fensterfreigabe
ls
```

### Einen Ordner zur Verwendung durch Windows freigeben

Erstellen Sie zunächst einen Ordner zum Freigeben. Dieses Beispiel erstellt einen Ordner namens `shared` im `home`-Ordner des aktuellen Benutzers und geht davon aus, dass der aktuelle Benutzer `pi` ist.

```bash
cd ~
mkdir hat geteilt
```

Jetzt müssen wir Samba anweisen, diesen Ordner zu teilen, indem wir die Samba-Konfigurationsdatei verwenden.

```bash
sudo nano /etc/samba/smb.conf
```

Fügen Sie am Ende der Datei Folgendes hinzu, um den Ordner freizugeben, und geben Sie dem Remote-Benutzer Lese-/Schreibberechtigungen:

```
[Teilen]
    Pfad = /home/pi/shared
    nur lesen = nein
    öffentlich = ja
    beschreibbar = ja
```

Suchen Sie in derselben Datei die Zeile `workgroup` und ändern Sie sie gegebenenfalls in den Namen der Arbeitsgruppe Ihres lokalen Windows-Netzwerks.

```bash
Arbeitsgruppe = <Ihr Arbeitsgruppenname hier>
```

Das sollte ausreichen, um den Ordner freizugeben. Wenn Sie auf Ihrem Windows-Gerät im Netzwerk surfen, sollte der Ordner angezeigt werden und Sie sollten sich damit verbinden können.