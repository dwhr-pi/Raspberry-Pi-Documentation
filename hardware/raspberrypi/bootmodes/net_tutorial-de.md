# Netzwerk booten Sie Ihren Raspberry Pi

Dieses Tutorial wurde geschrieben, um zu erklären, wie Sie einen einfachen DHCP/TFTP-Server einrichten, mit dem Sie einen Raspberry Pi 3 aus dem Netzwerk booten können. Das Tutorial geht davon aus, dass Sie über ein bestehendes Heimnetzwerk verfügen und einen Raspberry Pi für den **Server** verwenden möchten. Sie benötigen zum Booten einen zweiten Raspberry Pi 3 als **Client**. Es wird nur eine SD-Karte benötigt, da der Client nach der anfänglichen Client-Konfiguration vom Server gebootet wird.

Aufgrund der großen Auswahl an verfügbaren Netzwerkgeräten können wir nicht garantieren, dass das Booten über das Netzwerk mit jedem Gerät funktioniert. Wir haben Berichte erhalten, dass das Deaktivieren von STP-Frames in Ihrem Netzwerk hilfreich sein kann, wenn Sie den Netzwerkstart nicht zum Laufen bringen.

## Client-Konfiguration

Dieser Abschnitt gilt nur für den **original** Raspberry Pi 3B; Wenn Sie 3B+ verwenden, ignorieren Sie diesen Abschnitt und springen Sie dann zum Serverabschnitt weiter unten.

Bevor ein Raspberry Pi über das Netzwerk booten kann, muss er von einer SD-Karte mit einer Konfigurationsoption gebootet werden, um den USB-Boot-Modus zu aktivieren. Dadurch wird ein Bit im OTP-Speicher (One Time Programmable) im Raspberry Pi SoC gesetzt, das das Booten über das Netzwerk ermöglicht. Danach wird die SD-Karte nicht mehr benötigt.

Installieren Sie Raspberry Pi OS Lite (oder Raspberry Pi OS mit Raspberry Pi Desktop) auf der SD-Karte [wie gewohnt](../../../installation/installing-images/README.md).

Richten Sie anschließend den USB-Boot-Modus ein, indem Sie das Verzeichnis `/boot` mit den neuesten Boot-Dateien vorbereiten:

```bash
sudo apt update && sudo apt full-upgrade
```

Aktivieren Sie nun den USB-Boot-Modus mit dem folgenden Befehl:

```bash
echo program_usb_boot_mode=1 | sudo tee -a /boot/config.txt
```

Dies fügt `program_usb_boot_mode=1` am Ende von `/boot/config.txt` hinzu. Starten Sie den Raspberry Pi mit `sudo reboot` neu. Überprüfen Sie nach dem Neustart des Client Raspberry Pi, ob das OTP programmiert wurde mit:

```
$ vcgencmd otp_dump | grep 17:
17:3020000a
```

Stellen Sie sicher, dass die Ausgabe `0x3020000a` korrekt ist.

Die Client-Konfiguration ist fast abgeschlossen. Als letztes entfernen Sie die Zeile `program_usb_boot_mode` aus der `config.txt` (stellen Sie sicher, dass am Ende keine Leerzeile steht). Dies können Sie beispielsweise mit `sudo nano /boot/config.txt` tun. Beenden Sie abschließend den Client Raspberry Pi mit `sudo poweroff`.

## Serverkonfiguration

Stecken Sie die SD-Karte in den Server Raspberry Pi und booten Sie dann den Server. Bevor Sie etwas anderes tun, stellen Sie sicher, dass Sie `sudo raspi-config` ausgeführt und das Root-Dateisystem so erweitert haben, dass es die gesamte SD-Karte einnimmt.

Der Client Raspberry Pi benötigt zum Booten ein Root-Dateisystem. Bevor Sie also etwas anderes auf dem Server tun, erstellen Sie eine vollständige Kopie seines Dateisystems und legen Sie es in ein Verzeichnis namens `/nfs/client1`:

```bash
sudo mkdir -p /nfs/client1
sudo apt install rsync
sudo rsync -xa --progress --exclude /nfs / /nfs/client1
```

Generieren Sie SSH-Hostschlüssel auf dem Client-Dateisystem neu, indem Sie darin chrooten:

```bash
cd /nfs/client1
sudo mount --bind /dev dev
sudo mount --bind /sys sys
sudo mount --bind /proc proc
sudo chroot .
rm /etc/ssh/ssh_host_*
dpkg-reconfigure openssh-server
exit
sudo umount dev sys proc
```

Suchen Sie die Einstellungen Ihres lokalen Netzwerks. Sie müssen die Adresse Ihres Routers (oder Gateways) finden, dies geht mit:

```bash
ip route | awk '/default/ {print $3}'
```

Dann renne:

```bash
ip -4 addr show dev eth0 | grep inet
```

was eine Ausgabe ergeben sollte wie:

```
inet 10.42.0.211/24 brd 10.42.0.255 scope global eth0
```

Die erste Adresse ist die IP-Adresse Ihres Servers Raspberry Pi im Netzwerk, und der Teil nach dem Schrägstrich ist die Netzwerkgröße. Es ist sehr wahrscheinlich, dass Ihre eine `/24` ist. Beachten Sie auch die `brd` (Broadcast) Adresse des Netzwerks. Notieren Sie sich die Ausgabe des vorherigen Befehls, die die IP-Adresse des Raspberry Pi und die Broadcast-Adresse des Netzwerks enthält.

Notieren Sie schließlich die Adresse Ihres DNS-Servers, die dieselbe Adresse wie Ihr Gateway ist. Sie finden dies mit:

```bash
cat /etc/resolv.conf
```

Konfigurieren Sie eine statische Netzwerkadresse auf Ihrem Server Raspberry Pi über das `systemd`-Netzwerk, das als Netzwerkhandler und DHCP-Server fungiert.

Dazu müssen Sie ein `10-eth0.netdev` und ein `11-eth0.network` wie folgt erstellen:

```
sudo nano /etc/systemd/network/10-eth0.netdev
```

Fügen Sie die folgenden Zeilen hinzu:

```
[Match]
Name=eth0

[Network]
DHCP=no
```

Erstellen Sie dann eine Netzwerkdatei:

```
sudo nano /etc/systemd/network/11-eth0.network
```

Fügen Sie die folgenden Inhalte hinzu:

```
[Match]
Name=eth0

[Network]
Address=10.42.0.211/24
DNS=10.42.0.1

[Route]
Gateway=10.42.0.1
```

An diesem Punkt haben Sie kein funktionierendes DNS, also müssen Sie den zuvor notierten Server zu `systemd/resolved.conf` hinzufügen. In diesem Beispiel lautet die Gateway-Adresse 10.42.0.1.

```bash
sudo nano /etc/systemd/resolved.conf
```

Entkommentieren Sie die DNS-Zeile und fügen Sie dort die DNS-IP-Adresse hinzu. Wenn Sie über einen Fallback-DNS-Server verfügen, fügen Sie ihn außerdem dort hinzu.

```bash
[Resolve]
DNS=10.42.0.1
#FallbackDNS=
```

Aktivieren Sie `systemd-networkd` und starten Sie dann neu, damit die Änderungen wirksam werden:

```bash
sudo systemctl enable systemd-networkd
sudo reboot
```

Starten Sie nun `tcpdump`, damit Sie nach DHCP-Paketen vom Client Raspberry Pi suchen können:

```bash
sudo apt install tcpdump dnsmasq
sudo systemctl enable dnsmasq
sudo tcpdump -i eth0 port bootpc
```

Verbinden Sie den Client Raspberry Pi mit Ihrem Netzwerk und schalten Sie ihn ein. Prüfen Sie, ob die LEDs am Client nach ca. 10 Sekunden leuchten, dann sollten Sie vom Client ein Paket "DHCP/BOOTP, Request from ..." bekommen.

```
IP 0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from b8:27:eb...
```

Jetzt müssen Sie die `dnsmasq`-Konfiguration ändern, damit DHCP dem Gerät antworten kann. Drücken Sie <kbd>STRG + C</kbd>, um das Programm `tcpdump` zu beenden, und geben Sie dann Folgendes ein:

```bash
echo | sudo tee /etc/dnsmasq.conf
sudo nano /etc/dnsmasq.conf
```

Ersetzen Sie dann den Inhalt von `dnsmasq.conf` durch:

```
port=0
dhcp-range=10.42.0.255,proxy
log-dhcp
enable-tftp
tftp-root=/tftpboot
pxe-service=0,"Raspberry Pi Boot"
```

Wo die erste Adresse der `dhcp-range`-Zeile steht, verwenden Sie die zuvor notierte Broadcast-Adresse.

Erstellen Sie nun ein `/tftpboot`-Verzeichnis:

```bash
sudo mkdir /tftpboot
sudo chmod 777 /tftpboot
sudo systemctl enable dnsmasq.service
sudo systemctl restart dnsmasq.service
```

Überwachen Sie nun das `dnsmasq`-Protokoll:

```bash
tail -F /var/log/daemon.log
```

Sie sollten so etwas sehen:

```
raspberrypi dnsmasq-tftp[1903]: file /tftpboot/bootcode.bin not found
```

Als nächstes müssen Sie den Inhalt des Boot-Ordners in das Verzeichnis `/tftpboot` kopieren.

Drücken Sie zuerst <kbd>STRG + C</kbd>, um den Überwachungsstatus zu verlassen. Geben Sie dann Folgendes ein:

```bash
cp -r /boot/* /tftpboot
```

Da sich der TFTP-Speicherort geändert hat, starten Sie `dnsmasq` neu:

```bash
sudo systemctl restart dnsmasq
```

### NFS-Root einrichten

Dies sollte Ihrem Raspberry Pi-Client jetzt ermöglichen, zu versuchen, durchzustarten, bis er versucht, ein Root-Dateisystem zu laden (das er nicht hat).

Exportieren Sie an dieser Stelle das zuvor erstellte Dateisystem `/nfs/client1` und den TFTP-Boot-Ordner.

```bash
sudo apt install nfs-kernel-server
echo "/nfs/client1 *(rw,sync,no_subtree_check,no_root_squash)" | sudo tee -a /etc/exports
echo "/tftpboot *(rw,sync,no_subtree_check,no_root_squash)" | sudo tee -a /etc/exports
```

Starten Sie RPC-Bind und den NFS-Server neu, damit sie die neuen Dateien erkennen.

```bash
sudo systemctl enable rpcbind
sudo systemctl restart rpcbind
sudo systemctl enable nfs-kernel-server
sudo systemctl restart nfs-kernel-server
```

Bearbeiten Sie `/tftpboot/cmdline.txt` und ab `root=` und ersetzen Sie es durch:

```
root=/dev/nfs nfsroot=10.42.0.211:/nfs/client1,vers=4.1,proto=tcp rw ip=dhcp rootwait elevator=deadline
```

Hier sollten Sie die IP-Adresse durch die notierte IP-Adresse ersetzen. Entfernen Sie auch alle Teile der Befehlszeile, die mit init= beginnen.

Bearbeiten Sie abschließend `/nfs/client1/etc/fstab` und entfernen Sie die Zeilen `/dev/mmcblk0p1` und `p2` (nur `proc` sollte übrig bleiben). Fügen Sie dann die Bootpartition wieder hinzu:
```
echo "10.42.0.211:/tftpboot /boot nfs defaults,vers=4.1,proto=tcp 0 0" | sudo tee -a /nfs/client1/etc/fstab
```

Viel Glück! Wenn es nicht beim ersten Versuch bootet, versuchen Sie es weiter. Es kann etwa eine Minute dauern, bis der Raspberry Pi hochfährt, also seien Sie geduldig.