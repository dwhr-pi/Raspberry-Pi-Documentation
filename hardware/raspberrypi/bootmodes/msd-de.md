# USB-Massenspeicherboot

**Nur auf Raspberry Pi 2B v1.2, 3A+, 3B, 3B+ und 4B verfügbar.**

Auf dieser Seite wird erklärt, wie Sie Ihren Raspberry Pi von einem USB-Massenspeichergerät wie einem Flash-Laufwerk oder einer USB-Festplatte booten. Beachten Sie beim Anschließen von USB-Geräten, insbesondere Festplatten und SSDs, deren Strombedarf. Wenn Sie mehr als eine SSD oder Festplatte an den Pi anschließen möchten, erfordert dies normalerweise eine externe Stromversorgung - entweder ein mit Strom versorgtes Festplattengehäuse oder einen mit Strom versorgten USB-Hub. Beachten Sie, dass Modelle vor dem Pi 4 bekannte Probleme haben, die das Booten mit einigen USB-Geräten verhindern.

Siehe die [Bootmodes-Dokumentation](README.md) für die Bootreihenfolge und alternative Bootmodi (Netzwerk, USB-Gerät, GPIO- oder SD-Boot).

Beachten Sie, dass sich „USB-Massenspeicher-Boot“ vom „USB-Geräte-Boot-Modus“ unterscheidet. [USB-Gerätestartmodus](device.md) ermöglicht es einem Raspberry Pi, der an einen Computer angeschlossen ist, als USB-Gerät zu booten, wobei Dateien von diesem Computer verwendet werden.

Wenn Sie ein bestimmtes USB-Gerät nicht zum Booten Ihres Raspberry Pi verwenden können, können Sie alternativ den speziellen bootcode.bin-only Boot-Modus verwenden, wie [hier] (README.md) beschrieben. Dieser Pi bootet immer noch von der SD-Karte, aber `bootcode.bin` ist die einzige Datei, die von ihm gelesen wird.

<a name="pi4"></a>
## Raspberry Pi 4
So aktivieren Sie den USB-Massenspeicher-Boot auf einem Raspberry Pi 4:

* Verwenden Sie die Option "Misc Utility Images" im [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/), um eine SD-Karte mit dem neuesten Image "Raspberry Pi 4 EEPROM boot recovery" zu erstellen.
* Update auf Raspberry Pi OS 2020-08-20 oder neuer.
* Verwenden Sie [raspi-config](../../../configuration/raspi-config.md), um zwischen SD/USB (Standard) oder SD/Netzwerk Boot-Modi zu wählen.

Der vollständige Satz der Bootmodus-Optionen ist auf der Seite [Bootloader-Konfiguration](../bcm2711_bootloader_config.md) dokumentiert.


## Raspberry Pi 2B v1.2, 3A+, 3B, Rechenmodul 3

Auf dem Raspberry Pi 2B v1.2, 3A+, 3B und Compute Module 3 müssen Sie zuerst den [USB-Host-Boot-Modus] (host.md) aktivieren. Dies ermöglicht das Booten von USB-Massenspeicher und [Netzwerkboot](net.md). Beachten Sie, dass der Netzwerkstart auf dem Raspberry Pi 3A+ nicht unterstützt wird.

Um den USB-Host-Boot-Modus zu aktivieren, muss der Raspberry Pi von einer SD-Karte mit einer speziellen Option gebootet werden, um das USB-Host-Boot-Modus-Bit im einmalig programmierbaren (OTP) Speicher zu setzen. Nach dem Setzen dieses Bits wird die SD-Karte nicht mehr benötigt. **Beachten Sie, dass alle Änderungen, die Sie am OTP vornehmen, dauerhaft sind und nicht rückgängig gemacht werden können.**

**Auf dem Raspberry Pi 3A+ verhindert das Setzen des OTP-Bits zum Aktivieren des USB-Host-Boot-Modus dauerhaft, dass Pi im USB-Gerätemodus bootet.**

Sie können jede SD-Karte mit Raspberry Pi OS verwenden, um das OTP-Bit zu programmieren.

Aktivieren Sie den USB-Host-Boot-Modus mit diesem Code:

```bash
echo program_usb_boot_mode=1 | sudo tee -a /boot/config.txt
```

Dies fügt `program_usb_boot_mode=1` am Ende von `/boot/config.txt` hinzu.

Beachten Sie, dass die Option, obwohl sie `program_usb_boot_mode` heißt, nur den USB-*host*-Boot-Modus aktiviert. Der USB-Boot-Modus *device* ist nur bei bestimmten Raspberry Pi-Modellen verfügbar - siehe [USB-Device-Boot-Modus](device.md).

Der nächste Schritt besteht darin, den Raspberry Pi mit `sudo reboot` neu zu starten und zu überprüfen, ob das OTP programmiert wurde mit:

```bash
$ vcgencmd otp_dump | grep 17:
17:3020000a
```

Prüfen Sie, ob die Ausgabe `0x3020000a` angezeigt wird. Ist dies nicht der Fall, wurde das OTP-Bit nicht erfolgreich programmiert. Führen Sie in diesem Fall den Programmiervorgang erneut durch. Wenn das Bit immer noch nicht gesetzt ist, kann dies auf einen Fehler in der Pi-Hardware selbst hinweisen.

Wenn Sie möchten, können Sie die Zeile `program_usb_boot_mode` aus der `config.txt` entfernen, so dass, wenn Sie die SD-Karte in einen anderen Raspberry Pi stecken, dieser den USB-Host-Boot-Modus nicht programmiert. Stellen Sie sicher, dass sich am Ende von `config.txt` keine Leerzeile befindet.

Sie können nun genauso von einem USB-Massenspeichergerät booten wie von einer SD-Karte - weitere Informationen finden Sie im folgenden Abschnitt.

## Raspberry Pi 3B+, Rechenmodul 3+

Der Raspberry Pi 3B+ und das Compute Module 3+ unterstützen den USB-Massenspeicher-Boot out of the box. Die für frühere Versionen von Raspberry Pi spezifischen Schritte müssen nicht ausgeführt werden.

Das [Vorgehen](../../../installation/installing-images) ist das gleiche wie bei SD-Karten - einfach das USB-Speichergerät mit dem Betriebssystem-Image abbilden.

Nachdem Sie das Speichergerät vorbereitet haben, schließen Sie das Laufwerk an den Raspberry Pi an und schalten Sie den Pi ein. Beachten Sie dabei den zusätzlichen USB-Strombedarf des externen Laufwerks.
Nach fünf bis zehn Sekunden sollte der Raspberry Pi mit dem Booten beginnen und den Rainbow Splash-Screen auf einem angeschlossenen Display anzeigen. Stellen Sie sicher, dass keine SD-Karte in den Pi eingelegt ist, da er sonst zuerst von dieser bootet.

## Bekannte Probleme (nicht Pi 4)

- Das Standard-Timeout für die Überprüfung bootfähiger USB-Geräte beträgt 2 Sekunden. Einige Flash-Laufwerke und Festplatten werden zu langsam hochgefahren. Es ist möglich, dieses Timeout auf fünf Sekunden zu verlängern (fügen Sie der SD-Karte eine neue Datei `timeout` hinzu), aber beachten Sie, dass einige Geräte noch länger brauchen, um zu reagieren.
- Einige Flash-Laufwerke haben eine sehr spezifische Protokollanforderung, die vom Bootcode nicht verarbeitet wird und daher möglicherweise inkompatibel ist.