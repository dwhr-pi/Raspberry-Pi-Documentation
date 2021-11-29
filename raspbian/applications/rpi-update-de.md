# rpi-Update

„rpi-update“ ist eine Befehlszeilenanwendung, die Ihren Raspberry Pi OS-Kernel und die VideoCore-Firmware auf die neuesten Vorabversionen aktualisiert.

**WARNUNG: Es wird nicht garantiert, dass Vorabversionen der Software funktionieren. Sie sollten "rpi-update" auf keinem System verwenden, es sei denn, dies wird von einem Raspberry Pi-Ingenieur empfohlen. Es kann Ihr System unzuverlässig oder sogar komplett kaputt machen. Es sollte nicht als Teil eines regulären Aktualisierungsprozesses verwendet werden.**

Das Skript "rpi-update" wird von einem Drittanbieter, "Hexxeh", bereitgestellt und auch von Raspberry Pi-Ingenieuren unterstützt. Die Skriptquelle finden Sie auf GitHub unter https://github.com/Hexxeh/rpi-update

## Was es macht

`rpi-update` lädt die neueste Vorabversion des Linux-Kernels, seine passenden Module, Gerätebaumdateien sowie die neuesten Versionen der VideoCore-Firmware herunter. Anschließend werden diese Dateien an den entsprechenden Stellen auf der SD-Karte installiert und alle früheren Versionen überschrieben.

Alle von `rpi-update` verwendeten Quelldaten stammen aus dem GitHub-Repository https://github.com/Hexxeh/rpi-firmware. Dieses Repository enthält lediglich eine Teilmenge der Daten aus dem [offiziellen Firmware-Repository] (https://github.com/raspberrypi/firmware), da nicht alle Daten aus diesem Repository erforderlich sind.

## Ausführen von `rpi-update`

Wenn Sie sicher sind, dass Sie `rpi-update` verwenden müssen, ist es ratsam, zuerst ein [backup](../../linux/filesystem/backup.md) Ihres aktuellen Systems zu erstellen, um `rpi-update . auszuführen ` könnte dazu führen, dass das System nicht bootet.

`rpi-update` muss als root ausgeführt werden. Sobald das Update abgeschlossen ist, müssen Sie neu starten.

```
sudo rpi-update
sudo reboot
```

Es hat eine Reihe von Optionen, die im Hexxeh GitHub-Repository unter https://github.com/Hexxeh/rpi-update dokumentiert sind

## So kommst du wieder in Sicherheit

Wenn Sie ein `rpi-update` durchgeführt haben und die Dinge nicht wie gewünscht funktionieren, können Sie, wenn Ihr Raspberry Pi noch bootfähig ist, zur stabilen Version zurückkehren mit:

```
sudo apt-get update
sudo apt install --reinstall libraspberrypi0 libraspberrypi-{bin,dev,doc} raspberrypi-bootloader raspberrypi-kernel
```
Sie müssen Ihren Raspberry Pi neu starten, damit diese Änderungen wirksam werden.