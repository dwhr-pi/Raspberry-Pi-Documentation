# Einrichten eines NGINX-Webservers auf einem Raspberry Pi

NGINX (ausgesprochen *engine x*) ist eine beliebte, leichte Webserver-Anwendung, die Sie auf dem Raspberry Pi installieren können, um Webseiten bereitzustellen.

Wie Apache kann NGINX HTML-Dateien über HTTP bereitstellen und mit zusätzlichen Modulen dynamische Webseiten mit Skriptsprachen wie PHP bereitstellen.

## Datenbank der verfügbaren Pakete aktualisieren

Stellen Sie sicher, dass der Paketmanager über aktuelle Informationen zu den verfügbaren Paketen verfügt:

```bash
sudo apt update
```

Sie müssen dies nur gelegentlich tun, aber es ist die wahrscheinlichste Lösung, wenn nachfolgende Schritte mit Meldungen wie der folgenden fehlschlagen:
```
  404  Not Found [IP: 93.93.128.193 80]
```

## NGINX installieren

Installieren Sie zuerst das `nginx`-Paket, indem Sie den folgenden Befehl in das Terminal eingeben:

```bash
sudo apt install nginx
```

und starte den Server mit:

```bash
sudo /etc/init.d/nginx start
```

## Testen Sie den Webserver

Standardmäßig legt NGINX eine Test-HTML-Datei im Webordner ab. Diese Standard-Webseite wird bedient, wenn Sie auf dem Pi selbst zu `http://localhost/` oder `http://192.168.1.10` (was auch immer die IP-Adresse des Pi ist) von einem anderen Computer im Netzwerk aus navigieren. Um die IP-Adresse des Pi zu finden, geben Sie `hostname -I` in die Befehlszeile ein (oder lesen Sie mehr über das Finden Ihrer [IP-Adresse](../ip-address.md)).

Navigieren Sie entweder auf dem Pi oder von einem anderen Computer im Netzwerk zur Standardwebseite und Sie sollten Folgendes sehen:

![NGINX-Willkommensseite](images/nginx-welcome.png)

### Ändern der Standard-Webseite

NGINX legt den Speicherort der Webseite auf dem Raspberry Pi OS standardmäßig auf `/var/www/html` fest. Navigieren Sie zu diesem Ordner und bearbeiten oder ersetzen Sie index.nginx-debian.html nach Belieben. Sie können den Standardspeicherort der Seite unter `/etc/nginx/sites-available` in der Zeile, die mit 'root' beginnt, bei Bedarf bestätigen.


##Zusätzlich - PHP installieren

```bash
sudo apt install php-fpm
```

### PHP in NGINX aktivieren

```bash
cd /etc/nginx
sudo nano sites-enabled/default
```

finde die Zeile

```
index index.html index.htm;
```

ungefähr um Zeile 25 (Drücken Sie "STRG + C" in Nano, um die aktuelle Zeilennummer anzuzeigen)

Fügen Sie `index.php` nach `index` hinzu, um wie folgt auszusehen:

```
index index.php index.html index.htm;
```

Scrollen Sie nach unten, bis Sie einen Abschnitt mit folgendem Inhalt finden:

```
# übergeben Sie die PHP-Skripte an den FastCGI-Server, der auf 127.0.0.1:9000 lauscht
#
# location ~ \.php$ {
```

Bearbeiten Sie, indem Sie die `#`-Zeichen in den folgenden Zeilen entfernen:

```
location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/var/run/php5-fpm.sock;
}
```

Es sollte so aussehen:

```
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
        
		# With php-fpm (or other unix sockets):
		fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
		# With php-cgi (or other tcp sockets):
	#	fastcgi_pass 127.0.0.1:9000;
        }
```

Laden Sie die Konfigurationsdatei neu

```bash
sudo /etc/init.d/nginx reload
```

### PHP testen

Benennen Sie `index.nginx-debian.html` in `index.php` um:

```bash
cd /var/www/html/
sudo mv index.nginx-debian.html index.php
```

Öffnen Sie `index.php` mit einem Texteditor:

```bash
sudo nano index.php
```

Fügen Sie dynamischen PHP-Inhalt hinzu, indem Sie den aktuellen Inhalt ersetzen:
```php
<?php echo phpinfo(); ?>
```

Speichern und aktualisieren Sie Ihren Browser. Sie sollten eine Seite mit der PHP-Version, dem Logo und den aktuellen Konfigurationseinstellungen sehen.