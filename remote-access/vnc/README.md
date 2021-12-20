# VNC (Virtuelles Netzwerk-Computing)

Manchmal ist es nicht bequem, direkt auf dem Raspberry Pi zu arbeiten. Vielleicht möchten Sie von einem anderen Gerät aus per Fernbedienung daran arbeiten.

VNC ist ein grafisches Desktop-Sharing-System, mit dem Sie die Desktop-Oberfläche eines Computers (auf dem VNC Server ausgeführt wird) von einem anderen Computer oder Mobilgerät (auf dem VNC Viewer ausgeführt wird) aus der Ferne steuern können. Der VNC Viewer überträgt die Tastatur- und entweder Maus- oder Berührungsereignisse an den VNC-Server und empfängt im Gegenzug Aktualisierungen des Bildschirms.

Sie sehen den Desktop des Raspberry Pi in einem Fenster auf Ihrem Computer oder Mobilgerät. Sie können es so steuern, als würden Sie am Raspberry Pi selbst arbeiten.

![Pi Desktop von einem mobilen Gerät aus gesehen](images/raspberry-pi-connect.png)

VNC Connect von RealVNC ist im Raspberry Pi OS enthalten. Es besteht sowohl aus VNC Server, mit dem Sie Ihren Raspberry Pi aus der Ferne steuern können, als auch aus VNC Viewer, mit dem Sie Desktop-Computer bei Bedarf von Ihrem Raspberry Pi aus fernsteuern können.

Sie müssen den VNC-Server aktivieren, bevor Sie ihn verwenden können: Anweisungen dazu finden Sie unten. Standardmäßig bietet Ihnen VNC Server Fernzugriff auf den grafischen Desktop, der auf Ihrem Raspberry Pi ausgeführt wird, als ob Sie davor sitzen würden.

Sie können jedoch auch VNC Server verwenden, um einen grafischen Fernzugriff auf Ihren Raspberry Pi zu erhalten, wenn dieser ohne Kopf ist oder keinen grafischen Desktop ausführt. Weitere Informationen hierzu finden Sie weiter unten unter **Virtuellen Desktop erstellen**.

## VNC installieren

VNC ist bereits auf dem vollständigen Raspberry Pi OS-Image installiert und kann bei anderen Versionen über "Empfohlene Software" aus dem Menü "Einstellungen" installiert werden.

Wenn Sie keinen Desktop verwenden, können Sie ihn wie folgt über die Befehlszeile installieren:

```bash
sudo apt update
sudo apt install realvnc-vnc-server realvnc-vnc-viewer
```

## VNC-Server aktivieren

Sie können dies grafisch oder über die Befehlszeile tun.

### VNC-Server grafisch aktivieren

- Booten Sie auf Ihrem Raspberry Pi in den grafischen Desktop.

- Wählen Sie **Menü > Einstellungen > Raspberry Pi Konfiguration > Schnittstellen**.

- Stellen Sie sicher, dass **VNC** **aktiviert** ist.

### Aktivieren des VNC-Servers über die Befehlszeile

Sie können den VNC-Server über die Befehlszeile mit [raspi-config](../../configuration/raspi-config.md) aktivieren:

```bash
sudo raspi-config
```

Aktivieren Sie nun den VNC-Server, indem Sie Folgendes tun:

- Navigieren Sie zu **Schnittstellenoptionen**.

- Scrollen Sie nach unten und wählen Sie **VNC > Ja**.

## Verbinden mit Ihrem Raspberry Pi mit VNC Viewer

Es gibt zwei Möglichkeiten, eine Verbindung zu Ihrem Raspberry Pi herzustellen. Sie können eines oder beide verwenden, je nachdem, was für Sie am besten funktioniert.

### Direktverbindung herstellen

Direkte Verbindungen sind schnell und einfach, vorausgesetzt, Sie sind mit demselben privaten lokalen Netzwerk verbunden wie Ihr Raspberry Pi. Dies kann beispielsweise ein kabelgebundenes oder kabelloses Netzwerk zu Hause, in der Schule oder im Büro sein.

- Verwenden Sie auf Ihrem Raspberry Pi (über ein Terminalfenster oder über SSH) [diese Anleitung](../ip-address.md) oder führen Sie `ifconfig` aus, um Ihre private IP-Adresse zu ermitteln.

- Laden Sie auf dem Gerät, mit dem Sie die Kontrolle übernehmen, VNC Viewer herunter. Für beste Ergebnisse verwenden Sie die [kompatible App](https://www.realvnc.com/download/viewer/) von RealVNC.

- Geben Sie die private IP-Adresse Ihres Raspberry Pi in den VNC Viewer ein:

  ![VNC-Viewer-Dialog mit IP-Adresse](images/vnc-viewer-direct-dialog.png)

### Cloud-Verbindung herstellen

Sie sind berechtigt, den Cloud-Dienst von RealVNC kostenlos zu nutzen, vorausgesetzt, der Fernzugriff dient ausschließlich Bildungs- oder nicht kommerziellen Zwecken.

Cloud-Verbindungen sind bequem und Ende-zu-Ende verschlüsselt. Sie werden dringend empfohlen, um sich über das Internet mit Ihrem Raspberry Pi zu verbinden. Es gibt keine Neukonfiguration der Firewall oder des Routers, und Sie müssen die IP-Adresse Ihres Raspberry Pi nicht kennen oder eine statische angeben.

- Melden Sie sich [hier](https://www.realvnc.com/raspberrypi/#sign-up) für ein RealVNC-Konto an: Es ist kostenlos und dauert nur wenige Sekunden.

- Melden Sie sich auf Ihrem Raspberry Pi mit Ihren neuen RealVNC-Kontoanmeldeinformationen beim VNC-Server an:

  ![VNC-Server-Dialog mit Anmeldung](images/vnc-server-cloud-dialog.png)

- Laden Sie auf dem Gerät, mit dem Sie die Kontrolle übernehmen, VNC Viewer herunter. Sie **müssen** die [kompatible App](https://www.realvnc.com/download/viewer/) von RealVNC verwenden.

- Melden Sie sich mit denselben RealVNC-Kontoanmeldeinformationen beim VNC Viewer an und tippen oder klicken Sie dann, um eine Verbindung zu Ihrem Raspberry Pi herzustellen:

  ![VNC-Viewer-Dialog mit Anmeldung](images/vnc-viewer-cloud-dialog.png)

### Authentifizierung beim VNC-Server

Um entweder eine Direkt- oder Cloud-Verbindung herzustellen, müssen Sie sich beim VNC-Server authentifizieren.

Wenn Sie eine Verbindung über die [kompatible VNC Viewer-App](https://www.realvnc.com/download/viewer/) von RealVNC herstellen, geben Sie den Benutzernamen und das Passwort ein, die Sie normalerweise verwenden, um sich bei Ihrem Benutzerkonto auf dem Himbeer-Pi. Standardmäßig lauten diese Anmeldeinformationen `pi` und `raspberry`.

Wenn Sie eine Verbindung über eine Nicht-RealVNC Viewer-App herstellen, müssen Sie zuerst das Authentifizierungsschema von VNC Server herabstufen, ein für VNC Server eindeutiges Kennwort angeben und dieses dann stattdessen eingeben.
* Wenn Sie sich vor Ihrem Raspberry Pi befinden und seinen Bildschirm sehen können, öffnen Sie den VNC-Server-Dialog auf Ihrem Raspberry Pi, wählen Sie **Menü > Optionen > Sicherheit** und wählen Sie **VNC-Passwort** aus der **Authentifizierung ** Dropdown-Liste.
* **Oder**, wenn Sie Ihren Raspberry Pi remote über die Befehlszeile konfigurieren, nehmen Sie die Änderungen für den Servicemodus (die Standardkonfiguration für den Raspberry Pi) vor:
  * Öffnen Sie die Konfigurationsdatei `/root/.vnc/config.d/vncserver-x11`.
  * Ersetzen Sie `Authentication=SystemAuth` durch `Authentication=VncAuth` und speichern Sie die Datei.
  * Führen Sie in der Befehlszeile `sudo vncpasswd -service` aus. Dies fordert Sie auf, ein Passwort festzulegen, und fügt es für Sie in die richtige Konfigurationsdatei für den im Servicemodus ausgeführten VNC-Server ein.
  * Starten Sie den VNC-Server neu.

## Minecraft und andere direkt gerenderte Apps aus der Ferne spielen

Sie können aus der Ferne auf Apps zugreifen, die ein direkt gerendertes Overlay verwenden, wie Minecraft, die Textkonsole, das Raspberry Pi-Kameramodul und mehr.

![Minecraft läuft auf Raspberry Pi über VNC](images/raspberry-pi-minecraft.png)

So schalten Sie diese Funktion ein:

- Öffnen Sie auf Ihrem Raspberry Pi den VNC-Server-Dialog.

- Navigieren Sie zu **Menü > Optionen > Fehlerbehebung** und wählen Sie **Experimentellen Direktaufnahmemodus aktivieren**.

- Auf dem Gerät, mit dem Sie die Kontrolle übernehmen, führen Sie VNC Viewer aus und stellen Sie eine Verbindung her.

  **Hinweis:** bestehende Verbindungen müssen neu gestartet werden, damit diese Änderungen wirksam werden.

Bitte beachten Sie, dass die direkte Bildschirmaufnahme eine experimentelle Funktion ist. Wenn Sie von einem Desktop-Computer aus eine Verbindung herstellen und die Mausbewegungen unregelmäßig erscheinen, versuchen Sie, **F8** zu drücken, um das Kontextmenü des VNC-Viewers zu öffnen, und wählen Sie **Relative Zeigerbewegung**.

Wenn die Leistung beeinträchtigt zu sein scheint, versuchen Sie [diese Schritte zur Fehlerbehebung](https://www.realvnc.com/docs/raspberry-pi.html#raspberry-pi-minecraft-troubleshoot) oder [lassen Sie es RealVNC wissen](https://support.realvnc.com/index.php?/Tickets/Submit).

## Erstellen eines virtuellen Desktops

Wenn Ihr Raspberry Pi kopflos ist (d. h. nicht an einen Monitor angeschlossen ist) oder einen Roboter steuert, ist es unwahrscheinlich, dass er einen grafischen Desktop verwendet.

VNC Server kann für Sie einen **virtuellen Desktop** erstellen, der Ihnen bei Bedarf einen grafischen Fernzugriff ermöglicht. Dieser virtuelle Desktop existiert nur im Speicher Ihres Raspberry Pi:

![Verbinden mit einem virtuellen In-Memory-Desktop](images/raspberry-pi-virtual.png)

So erstellen Sie einen virtuellen Desktop und stellen eine Verbindung zu ihm her:

- Führen Sie auf Ihrem Raspberry Pi (mit Terminal oder über SSH) `vncserver` aus. Notieren Sie sich die IP-Adresse/Anzeigenummer, die der VNC-Server auf Ihrem Terminal ausgibt (z. B. `192.167.5.149:1`).

- Geben Sie auf dem Gerät, mit dem Sie die Kontrolle übernehmen, diese Informationen in [VNC Viewer](https://www.realvnc.com/download/viewer/) ein.

Führen Sie den folgenden Befehl aus, um einen virtuellen Desktop zu zerstören:

```bash
vncserver -kill :<Display-Nummer>
```

Dadurch werden auch alle bestehenden Verbindungen zu diesem virtuellen Desktop beendet.