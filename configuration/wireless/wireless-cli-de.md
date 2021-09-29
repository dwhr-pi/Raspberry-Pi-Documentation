# Einrichten eines WLAN über die Befehlszeile


Diese Methode eignet sich, wenn Sie keinen Zugriff auf die grafische Benutzeroberfläche haben, die normalerweise zum Einrichten eines WLANs auf dem Raspberry Pi verwendet wird. Es eignet sich besonders für die Verwendung mit einem seriellen Konsolenkabel, wenn Sie keinen Zugriff auf einen Bildschirm oder ein kabelgebundenes Ethernet-Netzwerk haben. Beachten Sie auch, dass keine zusätzliche Software erforderlich ist; Auf dem Raspberry Pi ist alles, was Sie brauchen, bereits enthalten.

## raspi-config verwenden

Der schnellste Weg zum Aktivieren des drahtlosen Netzwerks ist die Verwendung des Befehlszeilentools `raspi-config`.

`sudo raspi-config`

Wählen Sie im Menü den Punkt **Lokalisierungsoptionen** und dann die Option **WLAN-Land ändern**. Bei einer Neuinstallation müssen Sie aus regulatorischen Gründen das Land angeben, in dem das Gerät verwendet wird. Legen Sie dann die SSID des Netzwerks und die Passphrase für das Netzwerk fest. Wenn Sie die SSID des Netzwerks, mit dem Sie sich verbinden möchten, nicht kennen, lesen Sie den nächsten Abschnitt zum Auflisten verfügbarer Netzwerke, bevor Sie `raspi-config` ausführen.

Beachten Sie, dass `raspi-config` keinen vollständigen Satz von Optionen zum Einrichten eines drahtlosen Netzwerks bereitstellt; Sie müssen möglicherweise in den zusätzlichen Abschnitten unten nachsehen, um weitere Informationen zu erhalten, wenn `raspi-config` den Pi nicht mit Ihrem angeforderten Netzwerk verbindet.

## Details zum WLAN-Netzwerk abrufen

Um nach drahtlosen Netzwerken zu suchen, verwenden Sie den Befehl `sudo iwlist wlan0 scan`. Dadurch werden alle verfügbaren drahtlosen Netzwerke zusammen mit anderen nützlichen Informationen aufgelistet. Achten Sie auf:

1. 'ESSID:"testing"' ist der Name des drahtlosen Netzwerks.

1. 'IE: IEEE 802.11i/WPA2 Version 1' ist die verwendete Authentifizierung. In diesem Fall ist es WPA2, der neuere und sicherere Funkstandard, der WPA ersetzt. Dieses Handbuch sollte für WPA oder WPA2 funktionieren, funktioniert jedoch möglicherweise nicht für WPA2-Unternehmen. Für WEP-Hex-Schlüssel siehe das letzte Beispiel [hier](http://www.freebsd.org/cgi/man.cgi?query=wpa_supplicant.conf&sektion=5&apropos=0&manpath=NetBSD+6.1.5). Außerdem benötigen Sie das Passwort für das drahtlose Netzwerk. Bei den meisten Heimrouter finden Sie dies auf einem Aufkleber auf der Rückseite des Routers. Die ESSID (ssid) für die folgenden Beispiele ist `testing` und das Passwort (psk) ist `testingPassword`.

## Hinzufügen der Netzwerkdetails zum Raspberry Pi

Öffnen Sie die Konfigurationsdatei `wpa-supplicant` in nano:

`sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

Gehen Sie zum Ende der Datei und fügen Sie Folgendes hinzu:
```
network={
    ssid="testing"
    psk="testingPassword"
}
```
Das Passwort kann entweder als ASCII-Darstellung, in Anführungszeichen wie im obigen Beispiel, oder als vorverschlüsselte 32-Byte-Hexadezimalzahl konfiguriert werden. Sie können das Dienstprogramm `wpa_passphrase` verwenden, um ein verschlüsseltes PSK zu generieren. Dieser nimmt die SSID und das Passwort und generiert den verschlüsselten PSK. Mit dem Beispiel von oben können Sie den PSK mit `wpa_passphrase "testing"` generieren. Anschließend werden Sie nach dem Passwort des drahtlosen Netzwerks gefragt (in diesem Fall `testingPassword`). Die Ausgabe ist wie folgt:

```
network={
    ssid="testing"
    #psk="testingPassword"
    psk=131e1e221f6e06e3911a2d11ff2fac9182665c004de85300f9cac208a6a80531
}
```
Beachten Sie, dass die Klartextversion des Codes vorhanden, aber auskommentiert ist. Sie sollten diese Zeile aus der endgültigen `wpa_supplicant`-Datei für zusätzliche Sicherheit löschen.

Das Tool `wpa_passphrase` erfordert ein Passwort mit 8 bis 63 Zeichen. Um ein komplexeres Passwort zu verwenden, können Sie den Inhalt einer Textdatei extrahieren und als Eingabe für `wpa_passphrase` verwenden. Speichern Sie das Passwort in einer Textdatei und geben Sie es in `wpa_passphrase` ein, indem Sie `wpa_passphrase "testing" < file_where_password_is_stored` aufrufen. Aus Sicherheitsgründen sollten Sie anschließend die Datei `file_where_password_is_stored` löschen, damit keine Klartextkopie des ursprünglichen Kennworts auf dem System vorhanden ist.

Um das mit `wpa_passphrase` verschlüsselte PSK zu verwenden, können Sie entweder das verschlüsselte PSK kopieren und in die Datei `wpa_supplicant.conf` einfügen oder die Ausgabe des Tools auf zwei Arten in die Konfigurationsdatei umleiten:
- Wechseln Sie entweder zu `root`, indem Sie `sudo su` ausführen, dann rufen Sie `wpa_passphrase "testing" >> /etc/wpa_supplicant/wpa_supplicant.conf` auf und geben Sie das Testing-Passwort ein, wenn Sie dazu aufgefordert werden
- Oder verwenden Sie `wpa_passphrase "testing" | sudo tee -a /etc/wpa_supplicant/wpa_supplicant.conf > /dev/null` und geben Sie das Testpasswort ein, wenn Sie dazu aufgefordert werden; die Umleitung auf `/dev/null` verhindert, dass `tee` **auch** auf dem Bildschirm ausgibt (Standardausgabe).

Wenn Sie eine dieser beiden Optionen verwenden möchten, **stellen Sie sicher, dass Sie `>>` verwenden, oder verwenden Sie `-a` mit `tee`** — beide werden Text **an eine vorhandene Datei anhängen**. Die Verwendung eines einzelnen Chevrons `>` oder das Weglassen von `-a` bei der Verwendung von `tee` löscht den gesamten Inhalt und hängt **dann** die Ausgabe an die angegebene Datei an.

Speichern Sie nun die Datei, indem Sie `Strg+X`, dann `Y` und schließlich `Enter` drücken.

Konfigurieren Sie die Schnittstelle mit `wpa_cli -i wlan0 reconfigure` neu.

Sie können mit `ifconfig wlan0` überprüfen, ob es erfolgreich verbunden wurde. Wenn neben dem Feld `inet addr` eine Adresse steht, hat sich der Raspberry Pi mit dem Netzwerk verbunden. Wenn nicht, überprüfen Sie, ob Ihr Passwort und Ihre ESSID korrekt sind.

Auf dem Raspberry Pi 3B+ und Raspberry Pi 4B müssen Sie außerdem den Ländercode einstellen, damit das 5-GHz-Netzwerk die richtigen Frequenzbänder auswählen kann. Sie können dies mit der Anwendung „raspi-config“ tun: Wählen Sie das Menü „Lokalisierungsoptionen“ und dann „WLAN-Land ändern“. Alternativ können Sie die Datei `wpa_supplicant.conf` bearbeiten und Folgendes hinzufügen. (Hinweis: Sie müssen 'GB' durch den 2-Buchstaben-ISO-Code Ihres Landes ersetzen. Siehe [Wikipedia](https://en.wikipedia.org/wiki/ISO_3166-1) für eine Liste der 2-Buchstaben-ISO 3166- 1 Ländercodes.)
```
country=GB
```

Beachten Sie, dass Sie bei der neuesten Buster Raspberry Pi OS-Version sicherstellen müssen, dass die Datei `wpa_supplicant.conf` die folgenden Informationen oben enthält:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<Hier 2 Buchstaben ISO 3166-1 Ländercode einfügen>
```

## Ungesicherte Netzwerke

Wenn das Netzwerk, mit dem Sie sich verbinden, kein Passwort verwendet, muss der `wpa_supplicant`-Eintrag für das Netzwerk den richtigen `key_mgmt`-Eintrag enthalten.
z.B.
```
network={
    ssid="testen"
    key_mgmt=NONE
}
```

## Versteckte Netzwerke

Wenn Sie ein verstecktes Netzwerk verwenden, kann eine zusätzliche Option in der `wpa_supplicant-Datei`, `scan_ssid`, die Verbindung erleichtern.

```
network={
    ssid="IhreVersteckteSSID"
    scan_ssid=1
    psk="Ihr_drahtloses_Netzwerk_Passwort"
}
```

Sie können mit `ifconfig wlan0` überprüfen, ob es erfolgreich verbunden wurde. Wenn neben dem Feld `inet addr` eine Adresse steht, hat sich der Raspberry Pi mit dem Netzwerk verbunden. Wenn nicht, überprüfen Sie, ob Ihr Passwort und Ihre ESSID korrekt sind.

## Mehrere drahtlose Netzwerkkonfigurationen hinzufügen

Bei neueren Versionen von Raspberry Pi OS ist es möglich, mehrere Konfigurationen für die drahtlose Vernetzung einzurichten. Sie können beispielsweise einen für zu Hause und einen für die Schule einrichten.

Zum Beispiel
```
network={
    ssid="SchulnetzwerkSSID"
    psk="PasswortSchule"
    id_str="Schule"
}

network={
    ssid="HeimnetzwerkSSID"
    psk="PasswortHome"
    id_str="Zuhause"
}
```

Wenn Sie zwei Netzwerke in Reichweite haben, können Sie die Prioritätsoption hinzufügen, um zwischen ihnen zu wählen. Das Netzwerk in Reichweite mit der höchsten Priorität ist dasjenige, das verbunden ist.

```
network={
    ssid="HomeOneSSID"
    psk="passwordOne"
    priority=1
    id_str="Zuhause1"
}

network={
    ssid="HomeTwoSSID"
    psk="passwordTwo"
    priority=2
    id_str="Zuhause2"
}
```