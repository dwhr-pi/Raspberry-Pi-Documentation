# Konfiguration des externen Speichers
Sie können Ihre externe Festplatte, SSD oder Ihren USB-Stick an einen der USB-Ports des Raspberry Pi anschließen und das Dateisystem mounten, um auf die darauf gespeicherten Daten zuzugreifen.

Standardmäßig mountet Ihr Raspberry Pi einige der gängigen Dateisysteme wie FAT, NTFS und HFS+ automatisch am Speicherort `/media/pi/<HARD-DRIVE-LABEL>`.

Um Ihr Speichergerät so einzurichten, dass es immer an einem bestimmten Ort Ihrer Wahl gemountet wird, müssen Sie es manuell mounten.

## Speichergerät montieren
Sie können Ihr Speichergerät an einem bestimmten Ordnerspeicherort bereitstellen. Üblicherweise geschieht dies im Ordner /mnt, zum Beispiel /mnt/mydisk. Beachten Sie, dass der Ordner leer sein muss.

1. Stecken Sie das Speichergerät in einen USB-Port des Raspberry Pi.
2. Listen Sie alle Festplattenpartitionen auf dem Pi mit dem folgenden Befehl auf:

    ```
    sudo lsblk -o UUID,NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL,MODEL
    ```
   Der Raspberry Pi verwendet die Mountpoints `/` und `/boot`. Ihr Speichergerät wird in dieser Liste zusammen mit allen anderen verbundenen Speichergeräten angezeigt.
3. Verwenden Sie die Spalten SIZE, LABEL und MODEL, um den Namen der Festplattenpartition zu identifizieren, die auf Ihr Speichergerät verweist. Zum Beispiel `sda1`.
4. Die Spalte FSTYPE enthält den Dateisystemtyp. Wenn Ihr Speichergerät ein exFAT-Dateisystem verwendet, installieren Sie den exFAT-Treiber:

    ```
    sudo apt update
    sudo apt install exfat-fuse
    ```
5. Wenn Ihr Speichergerät ein NTFS-Dateisystem verwendet, haben Sie nur Lesezugriff darauf. Wenn Sie auf das Gerät schreiben möchten, können Sie den ntfs-3g-Treiber installieren:

    ```
    sudo apt-Update
    sudo apt install ntfs-3g
    ```
6. Führen Sie den folgenden Befehl aus, um den Speicherort der Festplattenpartition abzurufen:

    ```
    sudo blkid
    ```
    Zum Beispiel `/dev/sda1`.
7. Erstellen Sie einen Zielordner als Bereitstellungspunkt des Speichergeräts.
   Der in diesem Fall verwendete Mount-Point-Name ist `mydisk`. Sie können einen Namen Ihrer Wahl angeben:

    ```
    sudo mkdir /mnt/mydisk
    ```
8. Mounten Sie das Speichergerät an dem von Ihnen erstellten Mount-Punkt:

    ```
    sudo mount /dev/sda1 /mnt/mydisk
    ```
9. Überprüfen Sie, ob das Speichergerät erfolgreich gemountet wurde, indem Sie den Inhalt auflisten:

    ```
    ls /mnt/mydisk
    ```

## Automatische Montage einrichten
Sie können die Datei `fstab` ändern, um den Speicherort festzulegen, an dem das Speichergerät beim Start des Raspberry Pi automatisch gemountet wird. In der Datei `fstab` wird die Festplattenpartition durch den universell eindeutigen Bezeichner (UUID) identifiziert.

1. Rufen Sie die UUID der Festplattenpartition ab:

    ```
    sudo blkid
    ```
2. Suchen Sie die Festplattenpartition aus der Liste und notieren Sie sich die UUID. Beispiel: "5C24-1453".
3. Öffnen Sie die fstab-Datei mit einem Befehlszeileneditor wie nano:

    ```
    sudo nano /etc/fstab
    ```
4. Fügen Sie die folgende Zeile in die Datei `fstab` ein:

    ```
    UUID=5C24-1453 /mnt/mydisk fstype defaults,auto,users,rw,nofail 0 0
    ```
   Ersetzen Sie `fstype` durch den Typ Ihres Dateisystems, den Sie in Schritt 2 von 'Speichergerät einbinden' oben gefunden haben, zum Beispiel: `ntfs`.
   
5. Wenn der Dateisystemtyp FAT oder NTFS ist, fügen Sie `,umask=000` unmittelbar nach `nofail` hinzu - dies ermöglicht allen Benutzern vollen Lese-/Schreibzugriff auf jede Datei auf dem Speichergerät.

Nachdem Sie nun einen Eintrag in `fstab` gesetzt haben, können Sie Ihren Raspberry Pi mit oder ohne angeschlossenem Speichergerät starten. Bevor Sie das Gerät vom Stromnetz trennen, müssen Sie entweder den Pi herunterfahren oder ihn manuell aushängen, indem Sie die Schritte unter "Speichergerät aushängen" unten ausführen.

**Hinweis:** Wenn das Speichergerät beim Start des Pi nicht angeschlossen ist, dauert der Start des Pi zusätzliche 90 Sekunden. Sie können dies verkürzen, indem Sie `,x-systemd.device-timeout=30` direkt nach `nofail` in Schritt 4 hinzufügen. Dadurch wird die Zeitüberschreitung auf 30 Sekunden geändert, was bedeutet, dass das System nur 30 Sekunden wartet, bevor es den Mountversuch aufgibt. die Scheibe.

Weitere Informationen zu jedem Linux-Befehl finden Sie auf der jeweiligen Handbuchseite mit dem Befehl `man`. Zum Beispiel `man fstab`.

## Ein Speichergerät aushängen

Wenn der Raspberry Pi herunterfährt, sorgt das System dafür, dass das Speichergerät ausgehängt wird, damit es sicher entfernt werden kann. Wenn Sie ein Gerät manuell aushängen möchten, können Sie den folgenden Befehl verwenden:

```
sudo umount /mnt/mydisk
```
Wenn Sie die Fehlermeldung 'Target is busy' erhalten, bedeutet dies, dass das Speichergerät nicht ausgehängt wurde. Wenn kein Fehler angezeigt wurde, können Sie das Gerät jetzt sicher vom Netz trennen.

### Umgang mit 'Ziel ist beschäftigt'
    
Die Meldung „Ziel ist beschäftigt“ bedeutet, dass sich Dateien auf dem Speichergerät befinden, die von einem Programm verwendet werden. Um die Dateien zu schließen, gehen Sie wie folgt vor.

1. Schließen Sie alle Programme, die geöffnete Dateien auf dem Speichergerät haben.

2. Wenn ein Terminal geöffnet ist, stellen Sie sicher, dass Sie sich nicht in dem Ordner befinden, in dem das Speichergerät gemountet ist, oder in einem Unterordner davon.

3. Wenn Sie das Speichergerät immer noch nicht aushängen können, können Sie mit dem `lsof`-Tool überprüfen, welches Programm Dateien auf dem Gerät geöffnet hat. Sie müssen zuerst `lsof` mit `apt` installieren:

    ```
    sudo apt update
    sudo apt install lsof
    ```
   So verwenden Sie lsof:
   
    ```
    lsof /mnt/mydisk
    ```