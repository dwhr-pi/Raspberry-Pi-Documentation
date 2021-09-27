# Formatieren einer SDXC-Karte für die Verwendung mit NOOBS

Gemäß den [SD-Spezifikationen](https://www.sdcard.org/developers/overview/capacity/) ist jede SD-Karte, die größer als 32 GB ist, eine SDXC-Karte und muss mit dem exFAT-Dateisystem formatiert werden. Dies bedeutet, dass das offizielle SD-Formatierungstool *immer* Karten mit 64 GB oder mehr als exFAT formatiert.

Der in die GPU integrierte und nicht aktualisierbare Bootloader des Raspberry Pi unterstützt nur das Lesen von FAT-Dateisystemen (sowohl FAT16 als auch FAT32) und kann nicht von einem exFAT-Dateisystem booten. Wenn Sie also NOOBS auf einer Karte mit 64 GB oder mehr verwenden möchten, müssen Sie sie zuerst als FAT32 neu formatieren, bevor Sie die NOOBS-Dateien darauf kopieren.

## Raspberry Pi Imager verwenden

Unser Imaging-Tool bietet die Möglichkeit, eine SD-Karte mit dem richtigen FAT-Dateisystem zu formatieren. Laden Sie das Tool von [hier](https://www.raspberrypi.org/downloads/) herunter.

Führen Sie die Raspberry Pi Imager-Anwendung aus, und wählen Sie dann aus der Option "Betriebssystem auswählen" die Option "Löschen (Karte als FAT32 formatieren)". Wählen Sie nun die SD-Karte, die Sie formatieren möchten, aus der Option "SD-Karte auswählen" aus und klicken Sie abschließend auf "Schreiben".

## Andere Optionen

###Linux und Mac OS

Die in diesen Betriebssystemen integrierten Standardformatierungstools können FAT32-Partitionen erstellen. sie können auch als FAT oder MS-DOS bezeichnet werden. Löschen Sie einfach die vorhandene exFAT-Partition und erstellen und formatieren Sie eine neue primäre FAT32-Partition, bevor Sie mit dem Rest der [NOOBS-Anweisungen](noobs.md) fortfahren. Auf einem Mac bedeutet dies, dass Sie das Befehlszeilenprogramm diskutil verwenden und das Master Boot Record-Schema auswählen.

### Fenster

Die in Windows integrierten Standardformatierungstools sind begrenzt, da nur Partitionen bis zu 32 GB als FAT32 formatiert werden können. Um eine 64 GB-Partition als FAT32 zu formatieren, müssen Sie also ein Formatierungstool eines Drittanbieters verwenden. Ein einfaches Tool dafür ist [FAT32 Format](http://www.ridgecrop.demon.co.uk/guiformat.htm), das als einzelne Datei namens `guiformat.exe` heruntergeladen wird - es ist keine Installation erforderlich.

Führen Sie zuerst das Tool [SD Formatter](https://www.sdcard.org/downloads/formatter_4/) aus, um sicherzustellen, dass alle anderen Partitionen auf der SD-Karte gelöscht werden. Führen Sie dann das FAT32-Format (guiformat.exe)-Tool aus, stellen Sie sicher, dass Sie den richtigen Laufwerksbuchstaben wählen, belassen Sie die anderen Optionen auf ihren Standardeinstellungen und klicken Sie auf "Start". Nach Abschluss können Sie mit dem Rest der [NOOBS-Anweisungen] (noobs.md) fortfahren.

Wenn das FAT32-Formatierungstool für Sie nicht funktioniert, sind alternative Optionen [MiniTool Partition Wizard Free Edition](http://www.minitool.com/partition-manager/partition-wizard-home.html) und [EaseUS Partition Master Free](http://www.easeus.com/partition-manager/epm-free.html), die "Heimbenutzer"-Versionen von voll ausgestatteten Partitionseditor-Tools sind und daher nicht so einfach zu verwenden sind.