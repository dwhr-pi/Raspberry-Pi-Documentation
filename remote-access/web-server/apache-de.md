# Einrichten eines Apache Webservers auf einem Raspberry Pi

Apache ist eine beliebte Webserver-Anwendung, die Sie auf dem Raspberry Pi installieren können, um Webseiten bereitzustellen.

Apache allein kann HTML-Dateien über HTTP bereitstellen und mit zusätzlichen Modulen dynamische Webseiten mit Skriptsprachen wie PHP bereitstellen.

## Apache installieren

Aktualisieren Sie zunächst die verfügbaren Pakete, indem Sie den folgenden Befehl in das Terminal eingeben:

```bash
sudo apt update
```

Installieren Sie dann das Paket `apache2` mit diesem Befehl:

```bash
sudo apt install apache2 -y
```

## Testen Sie den Webserver

Standardmäßig legt Apache eine HTML-Testdatei im Webordner ab. Diese Standard-Webseite wird bedient, wenn Sie auf dem Pi selbst zu `http://localhost/` oder `http://192.168.1.10` (was auch immer die IP-Adresse des Pi ist) von einem anderen Computer im Netzwerk aus navigieren. Um die IP-Adresse des Pi zu finden, geben Sie `hostname -I` in die Befehlszeile ein (oder lesen Sie mehr über das Finden Ihrer [IP-Adresse](../ip-address.md)).

Navigieren Sie entweder auf dem Pi oder von einem anderen Computer im Netzwerk zur Standardwebseite und Sie sollten Folgendes sehen:

![Apache-Erfolgsmeldung](images/apache-it-works.png)

Dies bedeutet, dass Apache funktioniert!

### Ändern der Standard-Webseite

Diese Standard-Webseite ist nur eine HTML-Datei im Dateisystem. Es befindet sich unter `/var/www/html/index.html`.

Navigieren Sie in einem Terminalfenster zu diesem Verzeichnis und sehen Sie sich an, was drin ist:

```
cd /var/www/html
ls -al
```

Dies wird Ihnen zeigen:

```bash
total 12
drwxr-xr-x  2 root root 4096 Jan  8 01:29 .
drwxr-xr-x 12 root root 4096 Jan  8 01:28 ..
-rw-r--r--  1 root root  177 Jan  8 01:29 index.html
```

Dies zeigt, dass es standardmäßig eine Datei in `/var/www/html/` mit dem Namen `index.html` gibt und die dem `root`-Benutzer gehört (wie auch der umgebende Ordner). Um die Datei zu bearbeiten, müssen Sie deren Eigentümerschaft in Ihren eigenen Benutzernamen ändern. Ändern Sie den Besitzer der Datei (hier wird der Standardbenutzer `pi` angenommen) mit `sudo chown pi: index.html`.

Sie können nun versuchen, diese Datei zu bearbeiten und dann den Browser zu aktualisieren, um die Änderung der Webseite zu sehen.

### Deine eigene Website

Wenn Sie HTML-Kenntnisse haben, können Sie Ihre eigenen HTML-Dateien und andere Assets in dieses Verzeichnis legen und als Website in Ihrem lokalen Netzwerk bereitstellen.

## Zusätzlich - PHP installieren

Damit Ihr Apache-Server PHP-Dateien verarbeiten kann, müssen Sie die neueste Version von PHP und das PHP-Modul für Apache installieren. Geben Sie den folgenden Befehl ein, um diese zu installieren:

```bash
sudo apt install php libapache2-mod-php -y
```

Entfernen Sie nun die Datei `index.html`:

```bash
sudo rm index.html
```

und erstelle die Datei `index.php`:

```bash
sudo nano index.php
```

Fügen Sie einige PHP-Inhalte ein:

```php
<?php echo "Hallo Welt"; ?>
```

Speichern und aktualisieren Sie nun Ihren Browser. Sie sollten "Hallo Welt" sehen. Dies ist nicht dynamisch, wird aber dennoch von PHP bedient. Versuchen Sie es mit etwas Dynamischem:

```php
<?php echo date('Y-m-d H:i:s'); ?>
```

oder zeigen Sie Ihre PHP-Informationen an:

```php
<?php phpinfo(); ?>
```

### Weiter - WordPress

Nachdem Sie Apache und PHP installiert haben, können Sie mit der Einrichtung einer WordPress-Site auf Ihrem Pi fortfahren. Weiter zu [WordPress-Nutzung](../../usage/wordpress/README.md).