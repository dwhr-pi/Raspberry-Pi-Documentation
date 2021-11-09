# Netzwerk-Booten

In diesem Abschnitt wird beschrieben, wie das Booten über das Netzwerk auf dem Raspberry Pi 3B, 3B+ und 2B v1.2 funktioniert. Auf dem Raspberry Pi 4 ist das Booten über das Netzwerk im Bootloader der zweiten Stufe im EEPROM implementiert. Weitere Informationen finden Sie auf der Seite [Pi 4 Bootloader Configuration] (../bcm2711_bootloader_config.md).
Wir haben auch ein [Tutorial zum Einrichten eines Netzwerk-Boot-Systems](net_tutorial.md). Das Booten über das Netzwerk funktioniert nur für den kabelgebundenen Adapter, der in die oben genannten Modelle von Raspberry Pi eingebaut ist. Das Booten über WLAN wird nicht unterstützt, ebensowenig das Booten von einem anderen kabelgebundenen Netzwerkgerät.

Um über das Netzwerk zu booten, führt das Boot-ROM Folgendes aus:

* On-Board-Ethernet-Gerät initialisieren (Microchip LAN9500 ​​oder LAN7500)
* DHCP-Anfrage senden
* DHCP-Antwort erhalten
* (optional) DHCP-Proxy-Antwort empfangen
* ARP zu tftpboot-Server
* ARP-Antwort enthält die Ethernet-Adresse des tftpboot-Servers
* TFTP-RRQ 'bootcode.bin'
    * Datei nicht gefunden: Server antwortet mit TFTP-Fehlerantwort mit Textfehlermeldung
    * Datei vorhanden: Der Server antwortet mit dem ersten Datenblock (512 Byte) für die Datei mit einer Blocknummer im Header
        * Pi antwortet mit einem TFTP-ACK-Paket, das die Blocknummer enthält, und wiederholt sich bis zum letzten Block, der nicht 512 Byte groß ist
* TFTP-RRQ 'bootsig.bin'
    * Dies führt normalerweise zu einem Fehler `Datei nicht gefunden`. Dies ist zu erwarten, und TFTP-Bootserver sollten damit umgehen können.

Ab diesem Punkt lädt der `bootcode.bin`-Code das System weiter. Die erste Datei, auf die es zuzugreifen versucht, ist [`serial_number`]/start.elf. Wenn dies nicht zu einem Fehler führt, wird allen anderen zu lesenden Dateien die `Seriennummer` vorangestellt. Dies ist nützlich, da Sie damit separate Verzeichnisse mit separaten start.elf / kernels für Ihren Pis erstellen können
Um die Seriennummer für das Gerät zu erhalten, können Sie entweder diesen Bootmodus ausprobieren und mit tcpdump / Wireshark sehen, auf welche Datei zugegriffen wird, oder Sie können eine Standard-Raspberry-Pi-OS-SD-Karte und `cat /proc/cpuinfo` ausführen.

Wenn Sie alle Ihre Dateien im Stammverzeichnis Ihres tftp-Verzeichnisses ablegen, wird von dort auf alle folgenden Dateien zugegriffen.

## Debuggen des Netzwerk-Boot-Modus

Als erstes ist zu prüfen, ob das OTP-Bit richtig programmiert ist. Um dies zu tun, müssen Sie `program_usb_boot_mode=1` zur config.txt hinzufügen und neu starten (mit einer Standard-SD-Karte, die korrekt in Raspberry Pi OS bootet). Sobald Sie dies getan haben, sollten Sie in der Lage sein:

```
$ vcgencmd otp_dump | grep 17:
17:3020000a
```

Wenn Zeile 17 diesen Wert enthält, ist das OTP korrekt programmiert. Sie sollten jetzt in der Lage sein, die SD-Karte zu entfernen, Ethernet anzuschließen,
und dann sollten die Ethernet-LEDs etwa 5 Sekunden nach dem Einschalten des Pi aufleuchten.

Um die Ethernet-Pakete auf dem Server zu erfassen, verwenden Sie tcpdump auf dem tftpboot-Server (oder DHCP-Server, wenn sie unterschiedlich sind). Sie müssen die Pakete dort erfassen, sonst können Sie Pakete, die direkt gesendet werden, nicht sehen, da Netzwerk-Switches keine Hubs sind!

```
sudo tcpdump -i eth0 -w dump.pcap
```

Dadurch wird alles von eth0 in eine Datei dump.pcap geschrieben, die Sie dann nachbearbeiten oder zur Kommunikation auf Cloudshark.com hochladen können

### DHCP-Anfrage/Antwort

Sie sollten mindestens eine DHCP-Anfrage und -Antwort sehen, die wie folgt aussieht:

```
6:44:38.717115 IP (tos 0x0, ttl 128, id 0, offset 0, Flags [keine], proto UDP (17), Länge 348)
    0.0.0.0.68 > 255.255.255.255.67: [no cksum] BOOTP/DHCP, Request from b8:27:eb:28:f6:6d, length 320, xid 0x26f30339, Flags [none] (0x0000)
Client-Ethernet-Adresse b8:27:eb:28:f6:6d
Vendor-rfc1048-Erweiterungen
Magischer Keks 0x63825363
DHCP-Message Option 53, Länge 1: Discover
Parameter-Request Option 55, Länge 12:
Anbieter-Option, Anbieter-Klasse, BF, Option 128
Option 129, Option 130, Option 131, Option 132
Option 133, Option 134, Option 135, TFTP
ARCH Option 93, Länge 2: 0
NDI Option 94, Länge 3: 1.2.1
GUID Option 97, Länge 17: 0.68.68.68.68.68.68.68.68.68.68.68.68.68.68.68.68.68
Vendor-Class Option 60, Länge 32: "PXEClient:Arch:00000:UNDI:002001"
END Option 255, Länge 0
16:44:41.224619 IP (tos 0x0, ttl 64, id 57713, Offset 0, Flags [keine], proto UDP (17), Länge 372)
    192.168.1.1.67 > 192.168.1.139.68: [udp sum ok] BOOTP/DHCP, Antwort, Länge 344, xid 0x26f30339, Flags [keine] (0x0000)
Ihre-IP 192.168.1.139
Server-IP 192.168.1.1
Client-Ethernet-Adresse b8:27:eb:28:f6:6d
Vendor-rfc1048-Erweiterungen
Magischer Keks 0x63825363
DHCP-Message Option 53, Länge 1: Angebot
Server-ID Option 54, Länge 4: 192.168.1.1
Lease-Time-Option 51, Länge 4: 43200
RN Option 58, Länge 4: 21600
RB Option 59, Länge 4: 37800
Subnetz-Maske Option 1, Länge 4: 255.255.255.0
BR Option 28, Länge 4: 192.168.1.255
Vendor-Class Option 60, Länge 9: "PXEClient"
GUID Option 97, Länge 17: 0.68.68.68.68.68.68.68.68.68.68.68.68.68.68.68.68.68
Vendor-Option Option 43, Länge 32: 6.1.3.10.4.0.80.88.69.9.20.0.0.17.82.97.115.112.98.101.114.114.121.32.80.105.32.66.111.111.116.255
END Option 255, Länge 0
```

Der wichtige Teil der Antwort ist die Vendor-Option Option 43. Diese muss den String "Raspberry Pi Boot" enthalten, obwohl aufgrund
zu einem Fehler im Boot-ROM müssen Sie möglicherweise drei Leerzeichen am Ende der Zeichenfolge hinzufügen.

### TFTP-Datei gelesen

Sie wissen, ob die Vendor-Option richtig angegeben ist: Wenn ja, wird ein nachfolgendes TFTP-RRQ-Paket gesendet. RRQs können entweder durch den ersten Datenblock oder eine Fehlermeldung, die besagt, dass die Datei nicht gefunden wurde, beantwortet werden. In einigen Fällen erhalten sie sogar das erste Paket und dann wird die Übertragung vom Pi abgebrochen (dies passiert bei der Überprüfung, ob eine Datei existiert). Das folgende Beispiel besteht aus nur drei Paketen: die ursprüngliche Leseanforderung, der erste Datenblock (der immer 516 Byte groß ist und einen Header und 512 Byte Daten enthält, obwohl der letzte Block immer weniger als 512 Byte hat und eine Länge von Null haben kann) und das dritte Paket (das ACK, das eine Rahmennummer enthält, die mit der Rahmennummer im Datenblock übereinstimmt).

```
16:44:41.224964 IP (tos 0x0, ttl 128, id 0, offset 0, Flags [keine], proto UDP (17), Länge 49)
    192.168.1.139.49152 > 192.168.1.1.69: [keine cksum] 21 RRQ "bootcode.bin" Oktett
16:44:41.227223 IP (tos 0x0, ttl 64, id 57714, Offset 0, Flags [keine], proto UDP (17), Länge 544)
    192.168.1.1.55985 > 192.168.1.139.49152: [udp sum ok] UDP, Länge 516
16:44:41.227418 IP (tos 0x0, ttl 128, id 0, offset 0, Flags [keine], proto UDP (17), Länge 32)
    192.168.1.139.49152 > 192.168.1.1.55985: [keine cksum] UDP, Länge 4
```

## Bekannte Probleme

Es gibt eine Reihe bekannter Probleme mit dem Ethernet-Boot-Modus. Da die Implementierung der Boot-Modi im Chip selbst steckt, gibt es keine anderen Workarounds, als eine SD-Karte nur mit der Datei bootcode.bin zu verwenden.

### Zeitüberschreitung bei DHCP-Anfragen nach fünf Versuchen

Der Raspberry Pi versucht fünfmal eine DHCP-Anfrage mit fünf Sekunden dazwischen, insgesamt 25 Sekunden lang. Wenn der Server in dieser Zeit nicht verfügbar ist, um zu antworten, fällt der Pi in einen Energiesparzustand. Es gibt keine andere Problemumgehung als bootcode.bin auf einer SD-Karte.

### TFTP-Server in separatem Subnetz wird nicht unterstützt

Behoben in Raspberry Pi 3 Modell B+ (BCM2837B0).

### DHCP-Relay defekt

Der DHCP-Check prüfte auch, ob der Hops-Wert `1` war, was bei DHCP-Relay nicht der Fall wäre.

Behoben in Raspberry Pi 3 Modell B+.

### Raspberry Pi Boot-String

Der String "Raspberry Pi Boot" in der DHCP-Antwort benötigt die zusätzlichen drei Leerzeichen aufgrund eines Fehlers bei der Berechnung der Stringlänge.

Behoben in Raspberry Pi 3 Modell B+

### DHCP-UUID-Konstante

Die DHCP-UUID ist auf einen konstanten Wert eingestellt.

Behoben in Raspberry Pi 3 Modell B+; der Wert wird auf die 32-Bit-Seriennummer gesetzt.

### ARP-Prüfung kann während einer TFTP-Transaktion nicht reagieren

Der Raspberry Pi reagiert nur auf ARP-Anfragen, wenn er sich in der Initialisierungsphase befindet; Sobald es mit der Datenübertragung begonnen hat, wird es nicht mehr antworten.

Behoben in Raspberry Pi 3 Modell B+.

### DHCP-Anfrage/Antwort/Ack-Sequenz nicht korrekt implementiert

Beim Booten sendet Raspberry Pi ein DHCPDISCOVER-Paket. Der DHCP-Server antwortet mit einem DHCPOFFER-Paket. Der Pi fährt dann mit dem Booten fort, ohne einen DHCPREQUEST durchzuführen oder auf DHCPACK zu warten. Dies kann dazu führen, dass zwei separaten Geräten die gleiche IP-Adresse angeboten wird und diese verwendet wird, ohne dass sie dem Client richtig zugewiesen wird.

Unterschiedliche DHCP-Server haben in dieser Situation unterschiedliche Verhaltensweisen. dnsmasq (je nach Einstellungen) wird die MAC-Adresse hashen, um die IP-Adresse zu bestimmen, und die IP-Adresse pingen, um sicherzustellen, dass sie nicht bereits verwendet wird. Dies verringert die Wahrscheinlichkeit, dass dies geschieht, da es eine Kollision im Hash erfordert.


Siehe auch:
* [Netzwerk-Boot-Tutorial](net_tutorial.md)