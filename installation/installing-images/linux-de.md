# Betriebssystem-Images unter Linux installieren

[Raspberry Pi Imager](README.md) ist in der Regel die einfachste Option für die meisten Benutzer, um Images auf SD-Karten zu schreiben, daher ist es ein guter Ausgangspunkt. Wenn Sie unter Linux nach erweiterten Optionen suchen, können Sie die folgenden Standardbefehlszeilentools verwenden.

**Hinweis**: Die Verwendung des `dd`-Tools kann jede Partition Ihres Computers überschreiben. Wenn Sie in den folgenden Anweisungen das falsche Gerät angeben, können Sie Ihre primäre Linux-Partition löschen. Seien Sie bitte vorsichtig.

### Den Mountpoint der SD-Karte erkennen und aushängen
- Führen Sie `lsblk -p` aus, um zu sehen, welche Geräte derzeit mit Ihrem Computer verbunden sind.

- Wenn Ihr Computer über einen Steckplatz für SD-Karten verfügt, legen Sie die Karte ein. Wenn nicht, legen Sie die Karte in einen SD-Kartenleser ein und schließen Sie den Leser an Ihren Computer an.

- Führen Sie `lsblk -p` erneut aus. Das neu aufgetauchte Gerät ist Ihre SD-Karte (Sie können es in der Regel auch an der aufgelisteten Gerätegröße erkennen). Die Benennung des Geräts erfolgt nach dem im nächsten Absatz beschriebenen Format.

- Die linke Spalte der Ergebnisse des Befehls `lsblk -p` enthält den Gerätenamen Ihrer SD-Karte und die Namen aller Partitionen darauf (normalerweise nur eine, aber es können mehrere sein, wenn die Karte zuvor verwendet wurde). Es wird als `/dev/mmcblk0` oder `/dev/sdX` (mit den Partitionsnamen `/dev/mmcblk0p1` bzw. `/dev/sdX1`) aufgelistet, wobei `X` ein Kleinbuchstabe ist gibt das Gerät an (zB `/dev/sdb1`). Die rechte Spalte zeigt, wo die Partitionen gemountet wurden (wenn nicht, ist sie leer).

- Wenn Partitionen auf der SD-Karte gemountet wurden, unmounten Sie sie alle mit `umount`, zum Beispiel `umount /dev/sdX1` (ersetzen Sie `sdX1` durch den Gerätenamen Ihrer SD-Karte und ändern Sie die Nummer für alle anderen Partitionen) .

### Das Image auf die SD-Karte kopieren

- Schreiben Sie in einem Terminalfenster das Image mit dem folgenden Befehl auf die Karte. Stellen Sie sicher, dass Sie das Argument `if=` der Eingabedatei durch den Pfad zu Ihrer `.img`-Datei und das `/dev/sdX` in der ersetzen Ausgabedatei `of=` Argument mit dem richtigen Gerätenamen. **Dies ist sehr wichtig, da Sie alle Daten auf der Festplatte verlieren, wenn Sie den falschen Gerätenamen angeben.** Stellen Sie sicher, dass der Gerätename wie oben beschrieben der Name der gesamten SD-Karte ist und nicht nur eine Partition. Beispiel: `sdd`, nicht `sdds1` oder `sddp1`; `mmcblk0`, nicht `mmcblk0p1`.

    ```bash
    dd bs=4M if=2020-08-20-raspios-buster-armhf.img of=/dev/sdX conv=fsync
    ```

- Bitte beachten Sie, dass die Blockgröße, die auf "4M" eingestellt ist, die meiste Zeit funktioniert. Wenn nicht, versuchen Sie es mit `1M`, obwohl dies erheblich länger dauert.

- Beachten Sie auch, dass Sie, wenn Sie nicht als Root angemeldet sind, "sudo" voranstellen müssen.

### Ein gezipptes Image auf die SD-Karte kopieren

Unter Linux ist es möglich, den Entpack- und SD-Kopiervorgang in einem Befehl zu kombinieren, wodurch Probleme vermieden werden, die auftreten können, wenn das entpackte Image größer als 4 GB ist. Dies kann bei bestimmten Dateisystemen passieren, die keine Dateien größer als 4 GB unterstützen (z. B. FAT), wobei zu beachten ist, dass die meisten Linux-Installationen kein FAT verwenden und daher diese Einschränkung nicht haben.

Der folgende Befehl entpackt die Zip-Datei (ersetzen Sie 2020-08-20-raspios-buster-armhf.zip durch den entsprechenden Zip-Dateinamen) und leitet die Ausgabe direkt an den dd-Befehl weiter. Dieser kopiert es wiederum auf die SD-Karte, wie im vorherigen Abschnitt beschrieben.
```
unzip -p 2020-08-20-raspios-buster-armhf.zip | sudo dd of=/dev/sdX bs=4M conv=fsync
```

### Überprüfung des Kopierfortschritts des Images

- Standardmäßig gibt der Befehl `dd` keine Informationen über seinen Fortschritt, daher kann es so aussehen, als ob er eingefroren wäre. Es kann mehr als fünf Minuten dauern, bis das Schreiben auf die Karte abgeschlossen ist. Wenn Ihr Kartenleser über eine LED verfügt, kann diese während des Schreibvorgangs blinken.

- Um den Fortschritt des Kopiervorgangs zu sehen, können Sie den Befehl dd mit der Statusoption ausführen.
   ```
    dd bs=4M if=2020-08-20-raspios-buster-armhf.img of=/dev/sdX conv=fsync
   ```
- Wenn Sie eine ältere Version von `dd` verwenden, ist die Statusoption möglicherweise nicht verfügbar. Möglicherweise können Sie stattdessen den Befehl `dcfldd` verwenden, der einen Fortschrittsbericht ausgibt, der zeigt, wie viel geschrieben wurde. Eine andere Methode besteht darin, ein USR1-Signal an `dd` zu senden, wodurch Statusinformationen gedruckt werden. Ermitteln Sie die PID von `dd` mit `pgrep -l dd` oder `ps a | grep dd`. Verwenden Sie dann `kill -USR1 PID`, um das USR1-Signal an `dd` zu senden.

### Optional: Prüfen, ob das Image richtig auf die SD-Karte geschrieben wurde

- Nachdem `dd` den Kopiervorgang abgeschlossen hat, können Sie überprüfen, was auf die SD-Karte geschrieben wurde, indem Sie von der Karte zurück zu einem anderen Image auf Ihrer Festplatte `dd` und das neue Image auf die gleiche Größe wie das Original kürzen. und dann `diff` (oder `md5sum`) auf diesen beiden Bildern ausführen.

- Wenn die SD-Karte viel größer als das Bild ist, möchten Sie nicht die gesamte SD-Karte zurücklesen, da sie meist leer ist. Sie müssen also die Anzahl der Blöcke überprüfen, die mit dem Befehl `dd` auf die Karte geschrieben wurden. Am Ende seines Laufs hat `dd` die Anzahl der geschriebenen Blöcke wie folgt angezeigt:
```
xxx+0 records in
yyy+0 records out
yyyyyyyyyy bytes (yyy kB, yyy KiB) copied, 0.00144744 s, 283 MB/s
```
Wir brauchen die Zahl `xxx`, das ist die Blockanzahl. Wir können die `yyy`-Zahlen ignorieren.

- Kopieren Sie den Inhalt der SD-Karte erneut mit `dd` auf ein Image auf Ihrer Festplatte:
    ```bash
    dd bs=4M if=/dev/sdX of=from-sd-card.img count=xxx
    ```
`if` ist die Eingabedatei (dh das SD-Kartengerät), `of` ist die Ausgabedatei, in die der Inhalt der SD-Karte kopiert werden soll (in diesem Beispiel `from-sd-card.img` genannt) und ` xxx` ist die Anzahl der Blöcke, die von der ursprünglichen `dd`-Operation geschrieben wurden.

- Falls das Image der SD-Karte immer noch größer als das Originalbild ist, kürzen Sie das neue Image mit dem folgenden Befehl auf die Größe des Originalbilds (ersetzen Sie das `reference`-Argument der Eingabedatei durch den Originalimagenamen):
    ```bash
    truncate --reference 2020-08-20-raspios-buster-armhf.img from-sd-card.img
    ```
- Vergleichen Sie die beiden Images: `diff` sollte melden, dass die Dateien identisch sind.
    ```bash
    diff -s from-sd-card.img 2020-08-20-raspios-buster-armhf.img
    ```
- Führen Sie `sync` aus. Dadurch wird sichergestellt, dass der Schreibcache geleert wird und Sie Ihre SD-Karte sicher aushängen können.

- Entfernen Sie die SD-Karte aus dem Kartenleser.