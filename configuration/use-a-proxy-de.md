# Verwenden eines Proxy-Servers

Wenn Sie möchten, dass Ihr Raspberry Pi über einen Proxy-Server auf das Internet zugreift (vielleicht von einer Schule oder einem anderen Arbeitsplatz), müssen Sie Ihren Pi so konfigurieren, dass er den Server verwendet, bevor Sie online gehen können.

## Was wirst du brauchen

Du wirst brauchen:

+ Die IP-Adresse oder der Hostname und Port Ihres Proxy-Servers
+ Ein Benutzername und ein Passwort für Ihren Proxy (falls erforderlich)

## Konfigurieren Ihres Pi

Sie müssen drei Umgebungsvariablen (`http_proxy`, `https_proxy` und `no_proxy`) einrichten, damit Ihr Raspberry Pi auf den Proxy-Server zugreifen kann.

+ Öffnen Sie ein Terminalfenster und öffnen Sie die Datei `/etc/environment` mit nano:

```
sudo nano /etc/environment
```

![Open etc.-Umgebung](images/proxy-open-environment.png)

+ Fügen Sie der Datei `/etc/environment` Folgendes hinzu, um die Variable `http_proxy` zu erstellen:

```
export http_proxy="http://proxyipaddress:proxyport"
```

+ Ersetzen Sie `proxyipaddress` und `proxyport` durch die IP-Adresse und den Port Ihres Proxys.

**Hinweis**: Wenn Ihr Proxy einen Benutzernamen und ein Passwort erfordert, fügen Sie diese im folgenden Format hinzu:

```
export http_proxy="http://username:password@proxyipaddress:proxyport"
```

+ Geben Sie die gleichen Informationen für die Umgebungsvariable `https_proxy` ein:

```
export https_proxy="http://username:password@proxyipaddress:proxyport"
```
+ Erstellen Sie die Umgebungsvariable `no_proxy`, die eine durch Kommas getrennte Liste von Adressen ist, für die Ihr Pi den Proxy nicht verwenden sollte:

```
export no_proxy="localhost, 127.0.0.1"
```

Ihre Datei `/etc/environment` sollte nun so aussehen:

```
export http_proxy="http://username:password@proxyipaddress:proxyport"
export https_proxy="http://username:password@proxyipaddress:proxyport"
export no_proxy="localhost, 127.0.0.1"
```

![Umgebungsvariablen](images/proxy-environment-variables.png)

+ Drücken Sie <kbd>Strg + X</kbd> zum Speichern und Beenden.

## Sudoers aktualisieren

Damit Operationen, die als "sudo" ausgeführt werden (z. B. das Herunterladen und Installieren von Software), die neuen Umgebungsvariablen verwenden, müssen Sie "sudoers" aktualisieren.

+ Verwenden Sie den folgenden Befehl, um `sudoers` zu öffnen:

```
sudo visudo
```

+ Fügen Sie der Datei die folgende Zeile hinzu, damit `sudo` die soeben erstellten Umgebungsvariablen verwendet:

```
Standardwerte env_keep+="http_proxy https_proxy no_proxy"
```

![sudoers bearbeiten](images/proxy-edit-sudoers.png)

+ Drücken Sie <kbd>Strg + X</kbd> zum Speichern und Beenden.

## Neustart

+ Starten Sie Ihren Raspberry Pi neu, damit die Änderungen wirksam werden.

Sie sollten nun über Ihren Proxy-Server auf das Internet zugreifen können.