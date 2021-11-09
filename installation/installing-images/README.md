# Betriebssystem-Images installieren

Diese Ressource erklärt, wie Sie ein Raspberry Pi-Betriebssystem-Image auf einer SD-Karte installieren. Sie benötigen einen anderen Computer mit einem SD-Kartenleser, um das Image zu installieren.

Bevor Sie beginnen, vergessen Sie nicht, [die SD-Kartenanforderungen](../sd-cards.md) zu überprüfen.

## Raspberry Pi Imager verwenden

Raspberry Pi hat ein grafisches Tool zum Schreiben von SD-Karten entwickelt, das unter Mac OS, Ubuntu 18.04 und Windows funktioniert und für die meisten Benutzer die einfachste Option ist, da es das Image herunterlädt und automatisch auf der SD-Karte installiert.

- Laden Sie die neueste Version von [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) herunter und installieren Sie sie.
  - Wenn Sie Raspberry Pi Imager auf dem Raspberry Pi selbst verwenden möchten, können Sie es von einem Terminal aus mit `sudo apt install rpi-imager` installieren.
- Verbinden Sie einen SD-Kartenleser mit der darin befindlichen SD-Karte.
- Öffnen Sie Raspberry Pi Imager und wählen Sie das erforderliche Betriebssystem aus der angezeigten Liste aus.
- Wählen Sie die SD-Karte aus, auf die Sie Ihr Bild schreiben möchten.
- Überprüfen Sie Ihre Auswahl und klicken Sie auf „WRITE“, um mit dem Schreiben von Daten auf die SD-Karte zu beginnen.

**Hinweis**: Wenn Sie den Raspberry Pi Imager unter Windows 10 mit aktiviertem kontrollierten Ordnerzugriff verwenden, müssen Sie dem Raspberry Pi Imager explizit die Berechtigung zum Schreiben der SD-Karte erlauben. Geschieht dies nicht, schlägt Raspberry Pi Imager mit einem Fehler beim Schreiben fehlgeschlagen.

## Andere Tools verwenden

Bei den meisten anderen Tools müssen Sie zuerst das Image herunterladen und es dann mit dem Tool auf Ihre SD-Karte schreiben.

### Image herunterladen

Offizielle Bilder für empfohlene Betriebssysteme stehen auf der Raspberry Pi-Website [Download-Seite](https://www.raspberrypi.org/downloads/) zum Download bereit.

Alternative Distributionen sind von Drittanbietern erhältlich.

Möglicherweise müssen Sie `.zip`-Downloads entpacken, um die Bilddatei (`.img`) auf Ihre SD-Karte zu schreiben.

**Hinweis**: Das im ZIP-Archiv enthaltene Raspberry Pi OS mit Desktop-Image ist über 4 GB groß und verwendet das [ZIP64](https://en.wikipedia.org/wiki/Zip_%28file_format%29#ZIP64) Format. Zum Entpacken des Archivs ist ein Entpack-Tool erforderlich, das ZIP64 unterstützt. Die folgenden Zip-Tools unterstützen ZIP64:

- [7-Zip](http://www.7-zip.org/) (Windows)
- [The Unarchiver](http://unarchiver.c3.cx/unarchiver) (Mac)
- [Unzip](https://linux.die.net/man/1/unzip) (Linux)

### Das Image schreiben

Wie Sie das Image auf die SD-Karte schreiben, hängt vom verwendeten Betriebssystem ab.

- [Linux](linux.md)
- [Mac OS](mac.md)
- [Windows](windows.md)
- [Chrome OS](chromeos.md)


## Booten Sie Ihr neues Betriebssystem

Sie können nun die SD-Karte in den Raspberry Pi einlegen und ihn einschalten.

Wenn Sie sich für das offizielle Raspberry Pi-Betriebssystem manuell anmelden müssen, ist der Standardbenutzername `pi` mit dem Passwort `raspberry`. Denken Sie daran, dass das Standard-Tastaturlayout auf UK eingestellt ist.

Sie sollten das Standardpasswort sofort ändern, um sicherzustellen, dass Ihr Raspberry Pi [sicher](../../configuration/security.md) ist.