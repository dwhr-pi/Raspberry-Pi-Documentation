## pxetools
Wir haben ein Python-Skript erstellt, das intern verwendet wird, um schnell Pis einzurichten, die über das Netzwerk booten. Es braucht eine Seriennummer, die Sie in `cat /proc/cpuinfo` finden, einen Besitzernamen und den Namen des Pi. Es erstellt dann ein Root-Dateisystem für diesen Pi aus einem Raspberry Pi OS-Image. Es gibt auch eine --list-Option, die die IP-Adresse des Pi ausdruckt, und eine --remove-Option. Die folgende Anleitung beschreibt, wie Sie die vom Skript benötigte Umgebung ausgehend von einem frischen Raspberry Pi OS lite-Image einrichten. Es kann eine gute Idee sein, eine Festplatte oder ein Flash-Laufwerk auf /nfs zu mounten, damit Ihre SD-Karte keine Dateisysteme für mehrere Pis bereitstellt. Dies bleibt dem Leser als Übung überlassen.

```
sudo apt update
sudo apt full-upgrade -y
sudo reboot

wget https://raw.githubusercontent.com/raspberrypi/documentation/master/hardware/raspberrypi/bootmodes/pxetools/prepare_pxetools
bash prepare_pxetools
```

Wenn Sie zum Speichern von iptables-Regeln aufgefordert werden, sagen Sie Nein.

Prepare_pxetools sollte alles vorbereiten, was Sie brauchen, um pxetools zu verwenden.

Wir haben festgestellt, dass wir den nfs-Server nach der ersten Verwendung von pxetools neu starten mussten. Tun Sie dies mit:
```
sudo systemctl restart nfs-kernel-server
```

Dann stecken Sie Ihren Pi ein und er sollte booten!