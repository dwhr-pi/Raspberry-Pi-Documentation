# Kopieren eines Betriebssystem-Image auf eine SD-Karte mit Mac OS

[Raspberry Pi Imager](README.md) ist die empfohlene Option für die meisten Benutzer, um Bilder auf SD-Karten zu schreiben.

## SD-Gerät ermitteln

- Stecken Sie die SD-Karte in den Steckplatz oder verbinden Sie den SD-Kartenleser mit der darin befindlichen SD-Karte.

### Befehlszeile

- `diskutil list`

    Beispiel (die SD-Karte ist /dev/disk2 - Ihre Festplatten- und Partitionsliste kann variieren):

    ```bash
    ❯ diskutil list
    /dev/disk0 (internal):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:                       GUID_partition_scheme        500.3 GB   disk0
        1:                       EFI EFI                      314.6 MB   disk0s1
        2:                       Apple_APFS Container disk1   500.0 GB   disk0s2

    /dev/disk1 (synthesized):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:                       APFS Container Scheme -      +500.0 GB   disk1
                                 Physical Store disk0s2
        1:                       APFS Volume Macintosh HD     89.6 GB    disk1s1
        2:                       APFS Volume Preboot          47.3 MB    disk1s2
        3:                       APFS Volume Recovery         510.4 MB   disk1s3
        4:                       APFS Volume VM               3.6 GB     disk1s4

    /dev/disk2 (external, physical):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:                       FDisk_partition_scheme       *15.9 GB    disk2
        1:                       Windows_FAT_32 boot          268.4 MB   disk2s1
        2:                       Linux                        15.7 GB    disk2s2
    ```

### Grafik-/Festplatten-Dienstprogramm

- Wählen Sie im Apple-Menü 'System Report' und klicken Sie dann auf 'More info...'.
- Klicken Sie auf „USB“ (oder „Kartenleser“, wenn Sie einen integrierten SD-Kartenleser verwenden) und suchen Sie dann oben rechts im Fenster nach Ihrer SD-Karte. Klicken Sie darauf und suchen Sie dann im unteren rechten Bereich nach dem BSD-Namen.
Es hat die Form `diskN` (zB `disk4`).
Notieren Sie diesen Namen.
- Unmounten Sie die Partition mit dem Festplatten-Dienstprogramm.
Werfen Sie es nicht aus.

## Image kopieren

### Befehlszeile

**Hinweis**: Die Verwendung des `dd`-Tools kann jede Partition Ihres Computers überschreiben.
Wenn Sie in der Anleitung das falsche Gerät angeben, können Sie Ihre primäre Mac OS-Partition überschreiben!

- Die Festplatte muss vor dem Kopieren des Images ausgehängt werden

    ```bash
    diskutil unmountDisk /dev/diskN
    ```

- Kopieren Sie das Image

  ```bash
  sudo dd bs=1m if=path_of_your_image.img of=/dev/rdiskN; sync
  ```

   Ersetzen Sie `N` durch die zuvor notierte Zahl. Beachten Sie die ```rdisk``` ('raw disk')
   anstelle von ```disk``` beschleunigt dies das Kopieren.

   Dies kann je nach Größe der Bilddatei mehr als 15 Minuten dauern.
   Überprüfen Sie den Fortschritt, indem Sie Strg+T drücken.
   
    Wenn der Befehl `dd: /dev/rdiskN: Resource busy` meldet, müssen Sie das Volume zuerst aushängen `sudo diskutil unmountDisk /dev/diskN`.

    Wenn der Befehl `dd: bs: illegal numeric value` meldet, ändern Sie die Blockgröße `bs=1m` in `bs=1M`.

    Wenn der Befehl `dd: /dev/rdiskN: Operation not permitted` meldet, gehen Sie zu `System Preferences` -> `Security & Privacy` -> `Privacy` -> `Files and Folders` -> `Give Removable Volumes access to Terminal`.

    Wenn der Befehl `dd: /dev/rdiskN: Permission denied` meldet, wird die Partitionstabelle der SD-Karte vor dem Überschreiben durch Mac OS geschützt. Löschen Sie die Partitionstabelle der SD-Karte mit diesem Befehl:
    
    ```
    sudo diskutil partitionDisk /dev/diskN 1 MBR "Free Space" "%noformat%" 100%
    ```

    Dieser Befehl legt auch die Berechtigungen auf dem Gerät fest, um das Schreiben zuzulassen.
    Geben Sie nun den Befehl `dd` erneut aus.

## Auswerfen

Nachdem der Befehl `dd` beendet ist, entfernen Sie die SD Karte mit:

```bash
sudo diskutil eject /dev/rdiskN
```