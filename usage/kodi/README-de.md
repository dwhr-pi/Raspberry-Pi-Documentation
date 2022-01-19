# Verwendung von Kodi auf dem Raspberry Pi

Kodi ist eine Media-Center-Software, die auf Raspberry Pi läuft.

![LibreELEC](images/openelec.png)

Zwei Kodi-Distributionen sind in unserem einfachen Betriebssystem-Installer [NOOBS](../../installation/noobs.md) enthalten: **LibreELEC** und **OSMC**.

## ANFÄNGER

Installieren Sie zunächst NOOBS auf einer SD-Karte. Befolgen Sie die Anweisungen auf der Seite [NOOBS](../../installation/noobs.md).

Mit NOOBS auf Ihrer SD-Karte sollten Sie in der Lage sein, den Raspberry Pi zum Auswahlbildschirm des NOOBS-Betriebssystems zu booten:

![NOOBS OS Auswahlbildschirm](../../installation/images/noobs.png)

Wählen Sie **LibreELEC** oder **OSMC** und drücken Sie die Schaltfläche „Installieren“.

Sie werden zur Bestätigung aufgefordert. Dadurch werden alle Daten auf der SD-Karte gelöscht. Wenn Sie also zuvor Raspberry Pi OS darauf hatten, stellen Sie sicher, dass Sie zuerst Ihre Dateien sichern. Wenn Sie sicher sind, klicken Sie auf „Ja“, um fortzufahren und die Installation zu starten. Dies wird einige Zeit dauern; Wenn es fertig ist, zeigt NOOBS ein Fenster mit der Aufschrift:

```
OS(es) Installed Successfully
Betriebssystem(e) erfolgreich installiert
```

Klicken Sie auf "OK" und Ihr Pi wird in der von Ihnen ausgewählten Distribution neu gestartet.

## Mit Kodi

Jetzt haben Sie Ihre Kodi-Distribution installiert, Sie können Mediendateien abspielen, das System konfigurieren und Add-Ons installieren, um weitere Funktionen hinzuzufügen.

Möglicherweise wird Ihnen ein Begrüßungsbildschirm angezeigt, der Ihnen bei der Konfiguration Ihres Setups und den ersten Schritten hilft.

![LibreELEC-Begrüßungsbildschirm](images/openelec-main.png)

## Leistung

Sie können Ihren Pi auf herkömmliche Weise mit einem USB-Mikrokabel an der Wandsteckdose mit Strom versorgen; Wenn Ihr Fernseher über einen USB-Anschluss verfügt, können Sie den Pi alternativ direkt mit einem USB-Mikrokabel anschließen. Dies bedeutet, dass Ihr Pi mit Strom versorgt wird, wenn der Fernseher eingeschaltet wird, und ausgeschaltet wird, wenn der Fernseher ausgeschaltet wird.

## Kontrolle

Sie können eine Tastatur und Maus mit Kodi verwenden, eine TV-Fernbedienung mit USB-Empfänger kaufen oder sogar einen Präsentationsklicker mit Richtungstasten verwenden.

Sie können auch eine Kodi-App auf Ihrem Smartphone verwenden; Suchen Sie im App Store Ihres Telefons nach "Kodi". Sobald Sie für die Verbindung mit der IP-Adresse Ihres Kodi Pi konfiguriert sind, können Sie die Fernbedienung auf dem Bildschirm verwenden oder die Dateien von Ihrem Telefon durchsuchen und auswählen, was abgespielt werden soll.

## Wiedergabe von Videodateien

Sie können Videodateien auf die SD-Karte Ihres Pi kopieren oder auf einen USB-Stick oder eine externe Festplatte speichern. Um diese Dateien abzuspielen, wählen Sie einfach **VIDEOS** im Schieberegler auf dem Hauptbildschirm und dann **Dateien**, und Sie sollten Ihre eingefügten Medien in der Liste der Quellen sehen. Wählen Sie Ihr Gerät aus und Sie sollten es wie auf einem Computer navigieren können. Suchen Sie die gewünschte Videodatei, wählen Sie sie aus und sie wird abgespielt.

## Netzlaufwerk verbinden

Sie können über eine Kabelverbindung eine Verbindung zu einem Netzwerkgerät wie einem NAS (Network Attached Storage) in Ihrem lokalen Netzwerk herstellen. Verbinden Sie Ihren Raspberry Pi über ein Ethernet-Kabel mit Ihrem Router. Um eine Verbindung zum Gerät herzustellen, wählen Sie auf dem Hauptbildschirm **VIDEOS** und klicken Sie auf **Videos hinzufügen...**.

Der Bildschirm **Videoquelle hinzufügen** wird angezeigt. Wählen Sie **Durchsuchen** und wählen Sie den Verbindungstyp aus. Wählen Sie für ein NAS **Windows-Netzwerk (SMB)**; Es wird das Samba-Protokoll verwenden, eine Open-Source-Implementierung des Windows-Dateifreigabeprotokolls. Das Gerät wird angezeigt, wenn es im Netzwerk gefunden werden kann. Fügen Sie dieses Gerät als Speicherort hinzu und Sie können in seinem Dateisystem navigieren und seine Mediendateien wiedergeben.

## Einstellungen

Kodi hat eine Vielzahl von konfigurierbaren Optionen. Sie können die Bildschirmauflösung ändern, ein anderes Skin auswählen, einen Bildschirmschoner einrichten, die Dateiansicht konfigurieren, Lokalisierungsoptionen festlegen, Untertitel konfigurieren und vieles mehr. Gehen Sie einfach auf dem Hauptbildschirm zu **SYSTEM** und **Einstellungen**.

## Add-Ons

Add-Ons bieten zusätzliche Funktionen oder ermöglichen die Verbindung zu Webdiensten wie YouTube.

Das YouTube-Add-on, das Suchergebnisse anzeigt:

![YouTube-Add-on](images/xbmc-youtube.jpg)

Sie können Add-Ons im Einstellungsmenü konfigurieren; eine Auswahl steht Ihnen zum Stöbern zur Verfügung. Wählen Sie eines aus und Sie werden aufgefordert, es zu installieren. Jedes Add-On hat seine eigenen Konfigurationen.