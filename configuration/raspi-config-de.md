#raspi-config

Diese Seite beschreibt die konsolenbasierte Anwendung raspi-config. Wenn Sie den Raspberry Pi-Desktop verwenden, können Sie die grafische Anwendung "Raspberry Pi Configuration" aus dem Menü "Preferences" verwenden, um Ihren Raspberry Pi zu konfigurieren.

`raspi-config` ist das Raspberry Pi-Konfigurationstool, das ursprünglich von [Alex Bradbury] (https://github.com/asb) geschrieben wurde. Es zielt auf Raspberry Pi OS ab.

<a name="usage"></a>
## Verwendungszweck

Beim ersten Booten in Raspberry Pi OS wird Ihnen `raspi-config` angezeigt. Um das Konfigurationstool danach zu öffnen, führen Sie einfach Folgendes über die Befehlszeile aus:

```
sudo raspi-config
```

Das `sudo` ist erforderlich, da Sie Dateien ändern werden, die Ihnen als `pi`-Benutzer nicht gehören.

Sie sollten einen blauen Bildschirm mit Optionen in einem grauen Feld sehen:

![raspi-config Hauptbildschirm](images/raspi-config.png)

Beachten Sie, dass das angezeigte Menü geringfügig abweichen kann.

Es stehen die folgenden Top-Level-Optionen zur Verfügung:

```
┌───────────────────┤ Raspberry Pi Software-Konfigurationstool (raspi-config) ├────────────────── ──┐
│ │
│ 1 Systemoptionen Konfigurieren Sie die Systemeinstellungen │
│ 2 Anzeigeoptionen Anzeigeeinstellungen konfigurieren │
│ 3 Schnittstellenoptionen Verbindungen zu Peripheriegeräten konfigurieren │
│ 4 Leistungsoptionen Konfigurieren Sie die Leistungseinstellungen │
│ 5 Lokalisierungsoptionen Konfigurieren von Sprach- und Regionaleinstellungen │
│ 6 Erweiterte Optionen Erweiterte Einstellungen konfigurieren │ │
│ 8 Update Aktualisieren Sie dieses Tool auf die neueste Version │
│ 9 Über raspi-config Informationen zu diesem Konfigurationstool │
│ │
│ │
│ <Auswählen> <Fertig stellen> │
│ │
└───────────────────────────────────────────────── ─────────────────────────────────────────────────┘
```

<a name="moving-around-the-menu"></a>
### Im Menü bewegen

Verwenden Sie die Pfeiltasten nach oben und nach unten, um die markierte Auswahl zwischen den verfügbaren Optionen zu verschieben. Durch Drücken der Pfeiltaste „rechts“ wird das Optionsmenü verlassen und Sie gelangen zu den Schaltflächen „<Auswählen>“ und „<Fertigstellen>“. Durch Drücken von `links` gelangen Sie zurück zu den Optionen. Alternativ können Sie mit der `Tab`-Taste zwischen diesen wechseln.

Beachten Sie, dass Sie in langen Listen mit Optionswerten (wie der Liste der Zeitzonenstädte) auch einen Buchstaben eingeben können, um zu diesem Abschnitt der Liste zu springen. Wenn Sie beispielsweise "L" eingeben, gelangen Sie nach Lissabon, nur zwei Optionen von London entfernt, um Ihnen das Scrollen durch das Alphabet zu ersparen.

<a name="was-raspi-config-macht"></a>
### Was macht raspi-config

Im Allgemeinen zielt `raspi-config` darauf ab, die Funktionalität bereitzustellen, um die gängigsten Konfigurationsänderungen vorzunehmen. Dies kann zu automatisierten Änderungen an `/boot/config.txt` und verschiedenen Standard-Linux-Konfigurationsdateien führen. Einige Optionen erfordern einen Neustart, um wirksam zu werden. Wenn Sie eine dieser Einstellungen geändert haben, fragt `raspi-config`, ob Sie jetzt neu starten möchten, wenn Sie die Schaltfläche `<Fertigstellen>` auswählen.

<a name="menu-options"></a>
## Menüpunkte

Hinweis: Aufgrund der ständigen Weiterentwicklung des Tools `raspi-config` kann es sein, dass die unten stehende Liste der Optionen nicht ganz aktuell ist. Bitte beachten Sie auch, dass verschiedene Modelle von Raspberry Pi möglicherweise unterschiedliche Optionen zur Verfügung haben.

### Systemoptionen

Im Untermenü Systemoptionen können Sie Konfigurationsänderungen an verschiedenen Teilen des Boot-, Anmelde- und Netzwerkprozesses sowie einige andere Änderungen auf Systemebene vornehmen.

#### WLAN

Ermöglicht die Einstellung der WLAN-SSID und der Passphrase.

#### Audio

Geben Sie das Audioausgabeziel an.

<a name="change-user-password"></a>
#### Passwort

Der Standardbenutzer auf Raspberry Pi OS ist `pi` mit dem Passwort `raspberry`. Das kannst du hier ändern. Lesen Sie mehr über andere [users](../linux/usage/users.md).
 
<a name="hostname"></a>
#### Hostname

Legen Sie den sichtbaren Namen für diesen Pi in einem Netzwerk fest.

<a name="boot-options"></a>
#### Booten / Auto-Login

In diesem Untermenü können Sie auswählen, ob Sie von der Konsole oder dem Desktop booten und ob Sie sich anmelden müssen oder nicht. Wenn Sie die automatische Anmeldung auswählen, werden Sie als `pi`-Benutzer angemeldet.

#### Netzwerk beim Booten

Verwenden Sie diese Option, um auf eine Netzwerkverbindung zu warten, bevor der Bootvorgang fortgesetzt wird.

#### Begrüßungsbildschirm

Aktivieren oder deaktivieren Sie den beim Booten angezeigten Begrüßungsbildschirm

#### Power LED

Wenn das Modell von Pi es zulässt, können Sie mit dieser Option das Verhalten der Power-LED ändern.

### Anzeigeoptionen

<a name="resolution"></a>
#### Auflösung

Definieren Sie die standardmäßige HDMI/DVI-Videoauflösung, die verwendet werden soll, wenn das System startet, ohne dass ein Fernseher oder Monitor angeschlossen ist. Dies kann sich auf RealVNC auswirken, wenn die VNC-Option aktiviert ist.

<a name="underscan"></a>
#### Underscan

Alte Fernsehgeräte wiesen erhebliche Unterschiede in der Größe des von ihnen produzierten Bildes auf; einige hatten Schränke, die den Bildschirm überlappten. Fernsehbilder erhielten deshalb einen schwarzen Rand, damit nichts vom Bild verloren ging; dies wird als Overscan bezeichnet. Moderne Fernseher und Monitore brauchen die Grenze nicht, und das Signal lässt sie nicht zu. Wenn der auf dem Bildschirm angezeigte Anfangstext am Rand verschwindet, müssen Sie Overscan aktivieren, um den Rand wieder anzuzeigen.

Alle Änderungen werden nach einem Neustart wirksam. Sie können die Einstellungen besser kontrollieren, indem Sie [config.txt](config-txt/README.md) bearbeiten.

Auf einigen Displays, insbesondere Monitoren, wird das Bild durch Deaktivieren des Overscans den gesamten Bildschirm ausfüllen und die Auflösung korrigieren. Bei anderen Displays kann es erforderlich sein, Overscan aktiviert zu lassen und seine Werte anzupassen.

<a name="pixel-doubling"></a>
#### Pixelverdopplung

Aktivieren/deaktivieren Sie die 2x2-Pixel-Zuordnung.

#### Composite-Video

Aktivieren Sie auf dem Raspberry Pi4 Composite-Video. Bei Modellen vor dem Raspberry Pi4 ist Composite-Video standardmäßig aktiviert, sodass diese Option nicht angezeigt wird.

#### Bildschirmausblendung

Aktivieren oder deaktivieren Sie die Bildschirmausblendung.

<a name="interfacing-options"></a>
### Schnittstellenoptionen

In diesem Untermenü gibt es die folgenden Optionen zum Aktivieren/Deaktivieren: Kamera, SSH, VNC, SPI, I2C, Seriell, 1-Draht und Remote GPIO.

<a name="camera"></a>
#### Kamera

Aktivieren/deaktivieren Sie die CSI-Kameraschnittstelle.

<a name="ssh"></a>
#### SSH

Aktivieren/deaktivieren Sie den Remote-Befehlszeilenzugriff auf Ihren Pi mit SSH.

Mit SSH können Sie von einem anderen Computer aus auf die Befehlszeile des Raspberry Pi zugreifen. SSH ist standardmäßig deaktiviert. Lesen Sie mehr über die Verwendung von SSH auf der [SSH-Dokumentationsseite](../remote-access/ssh/README.md). Wenn Sie Ihren Pi direkt mit einem öffentlichen Netzwerk verbinden, sollten Sie SSH nicht aktivieren, es sei denn, Sie haben sichere Passwörter für alle Benutzer eingerichtet.

<a name="VNC"></a>
#### VNC

Aktivieren/deaktivieren Sie den virtuellen RealVNC-Netzwerk-Computing-Server.

<a name="spi"></a>
#### SPI

Aktivieren/Deaktivieren von SPI-Schnittstellen und automatisches Laden des SPI-Kernelmoduls, erforderlich für Produkte wie PiFace.

<a name="i2c"></a>
####I2C

Aktivieren/Deaktivieren von I2C-Schnittstellen und automatisches Laden des I2C-Kernelmoduls.

<a name="serial"></a>
#### Seriennummer

Aktivieren/Deaktivieren von Shell- und Kernel-Meldungen auf der seriellen Verbindung.

<a name="1-wire"></a>
#### 1-adrig

Aktivieren/deaktivieren Sie die Dallas 1-Wire-Schnittstelle. Dies wird normalerweise für DS18B20-Temperatursensoren verwendet.

#### Remote-GPIO

Aktivieren oder deaktivieren Sie den Fernzugriff auf die GPIO-Pins.

### Performance-Optionen

<a name="overclock"></a>
### Übertakten

Bei einigen Modellen ist es möglich, die CPU Ihres Raspberry Pi mit diesem Tool zu übertakten. Die Übertaktung, die Sie erreichen können, variiert; Eine zu hohe Übertaktung kann zu Instabilität führen. Bei Auswahl dieser Option wird die folgende Warnung angezeigt:

**Beachten Sie, dass Übertakten die Lebensdauer Ihres Raspberry Pi verkürzen kann.** Wenn Übertakten auf einem bestimmten Niveau zu Systeminstabilität führt, versuchen Sie es mit einer bescheideneren Übertaktung. Halten Sie während des Bootvorgangs die Umschalttaste gedrückt, um die Übertaktung vorübergehend zu deaktivieren.

<a name="memory-split"></a>
#### GPU-Speicher

Ändern Sie die der GPU zur Verfügung gestellte Speichermenge.

#### Overlay-Dateisystem

Aktivieren oder deaktivieren Sie ein schreibgeschütztes Dateisystem

#### Fan

Stellen Sie das Verhalten eines GPIO-verbundenen Lüfters ein

<a name="localisation-options"></a>
### Lokalisierungsoptionen

Das Untermenü Lokalisierung bietet Ihnen die folgenden Optionen zur Auswahl: Tastaturlayout, Zeitzone, Gebietsschema und WLAN-Ländercode.

#### Gebietsschema

Wählen Sie ein Gebietsschema aus, zum Beispiel `en_GB.UTF-8 UTF-8`.

#### Zeitzone

Wählen Sie Ihre lokale Zeitzone, beginnend mit der Region, z.B. Europa, dann eine Stadt auswählen, z.B. London. Geben Sie einen Buchstaben ein, um die Liste zu dieser Stelle im Alphabet zu überspringen.

#### Klaviatur

Diese Option öffnet ein weiteres Menü, in dem Sie Ihr Tastaturlayout auswählen können. Die Anzeige dauert lange, während alle Tastaturtypen gelesen werden. Änderungen werden normalerweise sofort wirksam, erfordern jedoch möglicherweise einen Neustart.

#### WLAN-Land
Diese Option legt den Ländercode für Ihr drahtloses Netzwerk fest.

<a name="advanced-options"></a>
### Erweiterte Optionen

<a name="expand-filesystem"></a>
#### Dateisystem erweitern

Wenn Sie Raspberry Pi OS mit NOOBS installiert haben, wurde das Dateisystem automatisch erweitert. In seltenen Fällen kann dies nicht der Fall sein, z. wenn Sie eine kleinere SD-Karte auf eine größere kopiert haben. In diesem Fall sollten Sie diese Option verwenden, um Ihre Installation so zu erweitern, dass sie die gesamte SD-Karte ausfüllt, sodass Sie mehr Platz für Dateien haben. Sie müssen den Raspberry Pi neu starten, um dies verfügbar zu machen. Beachten Sie, dass es keine Bestätigung gibt: Wenn Sie die Option auswählen, beginnt die Partitionserweiterung sofort.

<a name="GL-driver"></a>
#### GL-Treiber

Aktivieren/deaktivieren Sie die experimentellen GL-Desktop-Grafiktreiber.

<a name="GL-full-KMS"></a>
##### GL (Volle KMS)

Aktivieren/deaktivieren Sie den experimentellen OpenGL Full KMS (Kernel-Modus-Einstellung) Desktop-Grafiktreiber.

<a name="GL-fake-KMS"></a>
##### GL (Fake KMS)

Aktivieren/deaktivieren Sie den experimentellen OpenGL Fake KMS-Desktop-Grafiktreiber.

<a name="legacy"></a>
##### Erbe

Aktivieren/deaktivieren Sie den ursprünglichen Legacy-Nicht-GL-VideoCore-Desktop-Grafiktreiber.

#### Komponist

Aktivieren/Anzeigen des xcompmgr-Kompositionsmanagers

#### Netzwerkschnittstellennamen

Aktivieren oder deaktivieren Sie vorhersagbare Netzwerkschnittstellennamen.

#### Netzwerk-Proxy-Einstellungen

Konfigurieren Sie die Proxy-Einstellungen des Netzwerks.

#### Startreihenfolge

Auf dem Raspberry Pi4 können Sie festlegen, ob von USB oder Netzwerk gebootet werden soll, wenn die SD-Karte nicht eingesteckt ist. Weitere Informationen finden Sie auf [dieser Seite](../hardware/raspberrypi/bcm2711_bootloader_config.md).

#### Bootloader-Version

Auf dem Raspberry Pi4 können Sie das System anweisen, die neueste Boot-ROM-Software zu verwenden oder auf die Werkseinstellungen zurückzusetzen, wenn die neueste Version Probleme verursacht.

<a name="update"></a>
### Aktualisieren

Aktualisieren Sie dieses Tool auf die neueste Version.

<a name="about"></a>
### Über raspi-config

Bei Auswahl dieser Option wird der folgende Text angezeigt:

```
Dieses Tool bietet eine einfache Möglichkeit zur Erstkonfiguration des Raspberry Pi. Obwohl es jederzeit ausgeführt werden kann, können einige der Optionen Schwierigkeiten bereiten, wenn Sie Ihre Installation stark angepasst haben.
```

<a name="finish"></a>
### Beenden

Verwenden Sie diese Schaltfläche, wenn Sie Ihre Änderungen abgeschlossen haben. Sie werden gefragt, ob Sie einen Neustart durchführen möchten oder nicht. Bei der ersten Verwendung ist es am besten, einen Neustart durchzuführen. Es kommt zu einer Verzögerung beim Neustart, wenn Sie die Größe Ihrer SD-Karte geändert haben.

<a name="development-of-this-tool"></a>
## Entwicklung dieses Tools

Siehe die Quelle dieses Tools unter [github.com/RPi-Distro/raspi-config](https://github.com/RPi-Distro/raspi-config), wo Sie Issues öffnen und Pull-Requests erstellen können.

---

*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPi raspi-config](http://elinux.org/RPi_raspi-config), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http:// creativecommons.org/licenses/by-sa/3.0/)*