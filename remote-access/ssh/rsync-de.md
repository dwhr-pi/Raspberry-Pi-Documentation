#rsync

Sie können das Tool `rsync` verwenden, um Ordner zwischen Computern zu synchronisieren. Vielleicht möchten Sie beispielsweise einige Dateien von Ihrem Desktop-Computer oder Laptop auf Ihren Pi übertragen und auf dem neuesten Stand halten, oder Sie möchten, dass die von Ihrem Pi aufgenommenen Bilder automatisch auf Ihren Computer übertragen werden.

Mit `rsync` über SSH können Sie Dateien automatisch auf Ihren Computer übertragen.

Hier ist ein Beispiel dafür, wie Sie die Synchronisierung eines Ordners mit Bildern auf Ihrem Pi mit Ihrem Computer einrichten:

Erstellen Sie auf Ihrem Computer einen Ordner namens `camera`:

```
mkdir camera
```

Schlagen Sie die IP-Adresse des Pi nach, indem Sie sich dort anmelden und `hostname -I` ausführen. In diesem Beispiel erstellt der Pi einen Zeitraffer, indem er jede Minute ein Foto aufnimmt und das Bild mit einem Zeitstempel im lokalen Ordner `camera` auf seiner SD-Karte speichert.

Führen Sie nun den folgenden Befehl aus (ersetzen Sie die IP-Adresse Ihres eigenen Pi):

```
rsync -avz -e ssh pi@192.168.1.10:camera/ camera/
```

Dadurch werden alle Dateien aus dem Ordner "Kamera" des Pi in den neuen Ordner "Kamera" Ihres Computers kopiert.

Um die Ordner synchron zu halten, führen Sie diesen Befehl in [cron](../../linux/usage/cron.md) aus.