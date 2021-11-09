# Betriebssystem-Images unter Windows installieren

[Raspberry Pi Imager](README.md) ist unsere empfohlene Option für die meisten Benutzer, um Images auf SD-Karten zu schreiben, daher ist dies ein guter Ausgangspunkt. Wenn Sie eine Alternative unter Windows suchen, können Sie balenaEtcher, Win32DiskImager oder imgFlasher verwenden.

## balenaEtcher

- Laden Sie das Windows-Installationsprogramm von [balena.io](https://www.balena.io/etcher/) herunter
- Führen Sie balenaEtcher aus und wählen Sie die entpackte Raspberry Pi OS-Image-Datei aus
- Wählen Sie das SD-Kartenlaufwerk
- Klicken Sie abschließend auf **Brennen**, um das Raspberry Pi OS-Image auf die SD-Karte zu schreiben
- Sie sehen einen Fortschrittsbalken. Sobald dies abgeschlossen ist, wird das Dienstprogramm die SD-Karte automatisch aushängen, damit Sie sie sicher von Ihrem Computer entfernen können.

## Win32DiskImager

- Legen Sie die SD-Karte in Ihren SD-Kartenleser ein. Sie können den SD-Kartensteckplatz verwenden, wenn Sie einen haben, oder einen SD-Adapter in einem USB-Anschluss. Notieren Sie den der SD-Karte zugewiesenen Laufwerksbuchstaben. Sie können den Laufwerksbuchstaben in der linken Spalte von Windows Explorer sehen, zum Beispiel **G:**
- Laden Sie das Dienstprogramm Win32DiskImager von der [Sourceforge-Projektseite](http://sourceforge.net/projects/win32diskimager/) als Installationsdatei herunter und führen Sie es aus, um die Software zu installieren.
- Führen Sie das Dienstprogramm `Win32DiskImager` von Ihrem Desktop oder Menü aus.
- Wählen Sie die zuvor extrahierte Bilddatei aus.
- Wählen Sie im Gerätefeld den Laufwerksbuchstaben der SD-Karte aus. Achten Sie darauf, das richtige Laufwerk auszuwählen: Wenn Sie das falsche Laufwerk wählen, können Sie die Daten auf der Festplatte Ihres Computers zerstören! Wenn Sie einen SD-Kartensteckplatz in Ihrem Computer verwenden und das Laufwerk im Win32DiskImager-Fenster nicht sehen können, versuchen Sie es mit einem externen SD-Adapter.
- Klicken Sie auf „Schreiben“ und warten Sie, bis der Schreibvorgang abgeschlossen ist.
- Beenden Sie den Imager und werfen Sie die SD-Karte aus.

## Upswift imgFlasher

- Tragbare Windows-Version von [upswift.io](https://www.upswift.io/imgflasher/) herunterladen
- Führen Sie imgFlasher aus und wählen Sie ein Bild oder eine Zip-Datei aus
- Wählen Sie eine SD-Karte oder ein USB-Laufwerk
- Klicken Sie auf 'Flash'
- Warten Sie, bis der Blitz abgeschlossen ist.

---

*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPi_Easy_SD_Card_Setup](http://elinux.org/RPi_Easy_SD_Card_Setup), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses/by-sa/3.0/) geteilt wird.*