# USB

## Seiteninhalt
- [Übersicht für Geräte vor dem Pi 4](#Übersicht)
- [Übersicht für den Pi 4](#Übersicht_pi4)
- [Unterstützte Geräte](#support)
- [Allgemeine Einschränkungen (nicht Pi 4)](#genlimits)
- [Port-Leistungslimits](#powerlimits)
- [Bekannte Probleme (nicht Pi 4)](#knownissues)
- [Fehlerbehebung](#Fehlerbehebung)

<a name="Übersicht"></a>
## Übersicht für Geräte vor Pi 4

Die Anzahl und Art der USB-Anschlüsse am Raspberry Pi hängt vom Modell ab. Das Raspberry Pi Model B ist mit zwei USB 2.0 Ports ausgestattet; B+, 2B, 3B und 3B+ verfügen über vier USB 2.0-Ports. Der Pi 4 verfügt über zwei USB-2.0-Ports und zwei USB-3.0-Ports. Bei allen Modellen vor dem Pi 4 sind die USB-Anschlüsse mit einem Combo-Hub/Ethernet-Chip verbunden, der selbst ein USB-Gerät ist, das an den einzelnen Upstream-USB-Anschluss des BCM2835 angeschlossen ist. Beim Pi 4 ist der USB-Hub-Chip über einen PCIe-Bus mit dem SoC verbunden.

Bei den Modellen A und Zero ist der einzelne USB 2.0-Port direkt mit dem SoC verbunden.

Die USB-Ports ermöglichen den Anschluss von Peripheriegeräten wie Tastaturen, Mäusen, Webcams, die dem Pi zusätzliche Funktionalität verleihen.

Es gibt einige Unterschiede zwischen der USB-Hardware auf dem Raspberry Pi und der USB-Hardware auf Desktop-Computern oder Laptop-/Tablet-Geräten.

Der USB-Host-Port im Pi ist ein On-The-Go-Host (OTG), da der Anwendungsprozessor des Pi, BCM2835, ursprünglich für den mobilen Markt gedacht war: dh als einzelner USB-Port an einem Telefon für die Verbindung an einen PC oder an ein einzelnes Gerät. Im Wesentlichen ist die OTG-Hardware einfacher als die entsprechende Hardware auf einem PC.

OTG unterstützt im Allgemeinen die Kommunikation mit allen Arten von USB-Geräten, aber um für die meisten USB-Geräte, die man an einen Pi anschließen kann, ein angemessenes Maß an Funktionalität zu bieten, muss die Systemsoftware mehr Arbeit leisten.

<a name="Übersicht_pi4"></a>
## Übersicht für den Pi 4

Beim Pi 4 steuert ein voll ausgestatteter Host-Controller die Downstream-USB-Ports. Downstream-USB wird von einem Via Labs VL805-Chip bereitgestellt, der zwei USB-2.0-Ports und zwei USB-3.0-Ports unterstützt. Dieser ist über einen extrem schnellen PCIe-Link mit dem SoC BCM2711 verbunden. Daher hat der Pi 4 nicht die Geschwindigkeitsbeschränkungen der Vorgängermodelle, was sehr schnelle Datenübertragungsraten bedeutet, insbesondere bei Verwendung der USB-3.0-Ports. Alle angeschlossenen USB 2.0-Geräte werden über einen internen Hub verbunden, der über einen einzigen USB 2.0-Bus mit dem Upstream-PCIe-Link verbunden ist, was eine maximale kombinierte Bandbreite für alle USB 2.0-Geräte von 480 Mbit/s ergibt. Alle vier USB-Ports des Geräts sind mit dem USB 2.0-Hub verbunden, während die USB 3.0-Ports (blau) AUCH über die USB 3.0-spezifischen Pins in der Buchse mit dem USB 3.0-Bus verbunden sind. USB 3.0-Geräte sind nur durch die über den PCIe-Link verfügbare Gesamtbandbreite eingeschränkt.

Mit `lsusb -t` können Sie sich anzeigen lassen, wie die USB-Geräte und -Hubs angeordnet sind und welche Geschwindigkeiten zugewiesen sind.

Die meisten technischen Einschränkungen der USB-Implementierung bei früheren Modellen sind nicht mehr vorhanden.

Die bei früheren Pi-Modellen vorhandene OTG-Hardware ist weiterhin verfügbar und wurde auf einen einzigen Anschluss am USB-C-Port umgestellt. Die OTG-Hardware soll im Nur-Geräte-Modus auf Pi 4 verwendet werden.

<a name="support"></a>
## Unterstützte Geräte

Im Allgemeinen kann jedes von Linux unterstützte Gerät mit dem Pi verwendet werden, vorbehaltlich einiger Einschränkungen, die weiter unten beschrieben werden. Linux hat wahrscheinlich die umfassendste Treiberdatenbank für Legacy-Hardware aller Betriebssysteme (sie kann bei der Unterstützung moderner Geräte zurückbleiben, da Open-Source-Treiber erforderlich sind, damit Linux das Gerät standardmäßig erkennt).

Wenn Sie ein Gerät haben und es mit einem Pi verwenden möchten, schließen Sie es an. Die Chancen stehen gut, dass es "einfach funktioniert". Wenn Sie eine grafische Benutzeroberfläche verwenden (z. B. die LXDE-Desktopumgebung in Raspberry Pi OS), wird wahrscheinlich ein Symbol oder ähnliches angezeigt, das das neue Gerät ankündigt.

Wenn das Gerät nicht zu funktionieren scheint, lesen Sie den Abschnitt Fehlerbehebung unten.

<a name="genlimits"></a>
### Allgemeine Einschränkungen (nicht Pi 4)

Die OTG-Hardware auf Raspberry Pi bietet eine einfachere Unterstützung für bestimmte Geräte, was einen höheren Software-Verarbeitungsaufwand verursachen kann. Der Raspberry Pi hat auch nur einen Root-USB-Port: Der gesamte Datenverkehr aller angeschlossenen Geräte wird über diesen Bus geleitet, der mit einer maximalen Geschwindigkeit von 480 Mbit/s arbeitet.

Die USB-Spezifikation definiert drei Gerätegeschwindigkeiten – Niedrig, Voll und Hoch. Die meisten Mäuse und Tastaturen sind Low-Speed, die meisten USB-Soundgeräte sind Full-Speed ​​und die meisten Videogeräte (Webcams oder Videoaufnahme) sind High-Speed.

Im Allgemeinen gibt es keine Probleme beim Anschließen mehrerer High-Speed-USB-Geräte an einen Pi.

Der Software-Overhead, der bei der Kommunikation mit Low- und Full-Speed-Geräten entsteht, bedeutet, dass die Anzahl der gleichzeitig aktiven Low- und Full-Speed-Geräte weich begrenzt ist. Eine kleine Anzahl dieser Gerätetypen, die mit einem Pi verbunden sind, verursacht keine Probleme.

<a name="powerlimits"></a>
### Port-Leistungsgrenzen

USB-Geräte haben definierte Stromanforderungen in Einheiten von 100 mA von 100 mA bis 500 mA. Das Gerät teilt dem USB-Host seinen eigenen Strombedarf mit, wenn es zum ersten Mal angeschlossen wird. Theoretisch sollte die tatsächliche Leistungsaufnahme des Geräts den angegebenen Bedarf nicht überschreiten.

Die USB-Anschlüsse eines Raspberry Pi 1 haben eine Designbelastung von jeweils 100 mA - ausreichend, um "Low-Power"-Geräte wie Mäuse und Tastaturen anzusteuern. Geräte wie WLAN-Adapter, USB-Festplatten, USB-Sticks verbrauchen alle viel mehr Strom und sollten von einem externen Hub mit eigener Stromversorgung versorgt werden. Es ist zwar möglich, ein 500-mA-Gerät an einen Raspberry Pi 1 anzuschließen und mit einer ausreichend starken Stromversorgung arbeiten zu lassen, ein zuverlässiger Betrieb ist jedoch nicht gewährleistet. Darüber hinaus kann das Hot-Plug-Einstecken von Hochleistungsgeräten in die USB-Anschlüsse des Raspberry Pi 1 einen Brownout verursachen, der dazu führen kann, dass der Pi zurückgesetzt wird.

Ab dem Raspberry Pi 2 beträgt die Gesamtstromversorgung aller USB-Ports insgesamt 1200 mA.

Weitere Informationen finden Sie unter [Power](../power/README.md).

<a name="knownissues"></a>
## Geräte mit bekannten Problemen (nicht Pi 4)

**1. Interoperabilität zwischen Raspberry Pi und USB 3.0-Hubs**
   Es gibt ein Problem mit USB 3.0-Hubs in Verbindung mit der Verwendung von Full- oder Low-Speed-Geräten (die meisten Mäuse, die meisten Tastaturen) und dem Raspberry Pi. Ein Fehler in der meisten USB 3.0-Hub-Hardware bedeutet, dass der Raspberry Pi nicht mit Full- oder Low-Speed-Geräten kommunizieren kann, die an einen USB 3.0-Hub angeschlossen sind.

   USB 2.0-Hochgeschwindigkeitsgeräte, einschließlich USB 2.0-Hubs, funktionieren ordnungsgemäß, wenn sie über einen USB 3.0-Hub angeschlossen sind.

   Vermeiden Sie es, Geräte mit niedriger oder hoher Geschwindigkeit an einen USB 3.0-Hub anzuschließen. Schließen Sie als Workaround einen USB 2.0-Hub an den Downstream-Port des USB 3.0-Hubs an und schließen Sie das Low-Speed-Gerät an, oder verwenden Sie einen USB 2.0-Hub zwischen dem Pi und dem USB 3.0-Hub, und schließen Sie dann Low-Speed-Geräte an den USB an 2.0-Hub.

**2. USB 1.1-Webcams**
   Alte Webcams können Full-Speed-Geräte sein. Da diese Geräte viele Daten übertragen und zusätzlichen Software-Overhead verursachen, ist ein zuverlässiger Betrieb nicht gewährleistet.
   Versuchen Sie als Workaround, die Kamera mit einer niedrigeren Auflösung zu verwenden.

**3. Esoterische USB-Soundkarten**
  Teure "audiophile" Soundkarten verbrauchen in der Regel weit mehr Bandbreite, als zum Streamen der Audiowiedergabe erforderlich ist. Ein zuverlässiger Betrieb mit 96kHz/192kHz DACs kann nicht garantiert werden.
  Als Workaround wird die Streambandbreite auf ein zuverlässiges Niveau reduziert, wenn der Ausgabestream in CD-Qualität (44,1 kHz/48 kHz 16 Bit) erzwungen wird.

**4. Single-TT-USB-Hubs**
  USB 2.0- und 3.0-Hubs verfügen über einen Mechanismus zur Kommunikation mit Full- oder Low-Speed-Geräten, die an ihre Downstream-Ports angeschlossen sind, die als Transaktionsübersetzer bezeichnet werden. Dieses Gerät puffert Hochgeschwindigkeitsanforderungen vom Host (d. h. dem Pi) und überträgt sie mit voller oder niedriger Geschwindigkeit an das nachgeschaltete Gerät. Die USB-Spezifikation erlaubt zwei Hubkonfigurationen: Single-TT (ein TT für alle Ports) und Multi-TT (ein TT pro Port).
  Aufgrund der Einschränkungen der OTG-Hardware kann es zu einem unzuverlässigen Betrieb der Geräte kommen, wenn zu viele Full- oder Low-Speed-Geräte an einen Single-TT-Hub angeschlossen werden. Es wird empfohlen, einen Multi-TT-Hub zu verwenden, um eine Verbindung mit mehreren Geräten mit niedrigerer Geschwindigkeit herzustellen.
  Verteilen Sie als Workaround Geräte mit niedrigerer Geschwindigkeit zwischen dem eigenen USB-Port des Pi und dem Single-TT-Hub.

<a name="Fehlerbehebung"></a>
## Fehlerbehebung

### Wenn dein Gerät überhaupt nicht funktioniert

Der erste Schritt besteht darin, zu sehen, ob es überhaupt erkannt wird. Dazu gibt es zwei Befehle, die in ein Terminal eingegeben werden können: `lsusb` und `dmesg`. Die erste druckt alle an USB angeschlossenen Geräte aus, unabhängig davon, ob sie tatsächlich von einem Gerätetreiber erkannt werden oder nicht, und die zweite druckt den Kernel-Nachrichtenpuffer aus (der nach dem Booten ziemlich groß sein kann - versuchen Sie es mit `sudo dmesg -C` schließen Sie dann Ihr Gerät an und geben Sie `dmesg` erneut ein, um neue Nachrichten zu sehen).

Als Beispiel mit einem USB-Stick:

```bash
pi@raspberrypi ~ $ lsusb
Bus 001 Device 002: ID 0424:9512 Standard Microsystems Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
Bus 001 Device 005: ID 05dc:a781 Lexar Media, Inc.
pi@raspberrypi ~ $ dmesg
... Stuff that happened before ...
[ 8904.228539] usb 1-1.3: new high-speed USB device number 5 using dwc_otg
[ 8904.332308] usb 1-1.3: New USB device found, idVendor=05dc, idProduct=a781
[ 8904.332347] usb 1-1.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 8904.332368] usb 1-1.3: Product: JD Firefly
[ 8904.332386] usb 1-1.3: Manufacturer: Lexar
[ 8904.332403] usb 1-1.3: SerialNumber: AACU6B4JZVH31337
[ 8904.336583] usb-storage 1-1.3:1.0: USB Mass Storage device detected
[ 8904.337483] scsi1 : usb-storage 1-1.3:1.0
[ 8908.114261] scsi 1:0:0:0: Direct-Access     Lexar    JD Firefly       0100 PQ: 0 ANSI: 0 CCS
[ 8908.185048] sd 1:0:0:0: [sda] 4048896 512-byte logical blocks: (2.07 GB/1.93 GiB)
[ 8908.186152] sd 1:0:0:0: [sda] Write Protect is off
[ 8908.186194] sd 1:0:0:0: [sda] Mode Sense: 43 00 00 00
[ 8908.187274] sd 1:0:0:0: [sda] No Caching mode page present
[ 8908.187312] sd 1:0:0:0: [sda] Assuming drive cache: write through
[ 8908.205534] sd 1:0:0:0: [sda] No Caching mode page present
[ 8908.205577] sd 1:0:0:0: [sda] Assuming drive cache: write through
[ 8908.207226]  sda: sda1
[ 8908.213652] sd 1:0:0:0: [sda] No Caching mode page present
[ 8908.213697] sd 1:0:0:0: [sda] Assuming drive cache: write through
[ 8908.213724] sd 1:0:0:0: [sda] Attached SCSI removable disk
```

In diesem Fall gibt es keine Fehlermeldungen in `dmesg` und der USB-Stick wird vom USB-Speichertreiber erkannt. Wenn für Ihr Gerät kein Treiber verfügbar war, erscheinen normalerweise nur die ersten 6 neuen Zeilen im dmesg-Ausdruck.

Wenn ein Gerät ohne Fehler aufgezählt wird, aber anscheinend nichts tut, sind wahrscheinlich keine Treiber dafür installiert. Suchen Sie anhand des Herstellernamens nach dem Gerät oder den USB-IDs, die in lsusb angezeigt werden (z. B. 05dc:a781). Das Gerät wird möglicherweise nicht mit Standard-Linux-Treibern unterstützt - und Sie müssen möglicherweise Ihre eigene Software von Drittanbietern herunterladen oder kompilieren.

#### Wenn Ihr Gerät intermittierendes Verhalten hat

Schlechte Stromqualität ist die häufigste Ursache dafür, dass Geräte nicht funktionieren, sich trennen oder allgemein unzuverlässig sind.

- Wenn Sie einen Hub mit externer Stromversorgung verwenden, versuchen Sie, das mit dem Hub gelieferte Netzteil gegen ein anderes kompatibles Netzteil mit derselben Nennspannung und Polarität auszutauschen.
- Prüfen Sie, ob sich das Problem von selbst löst, wenn Sie andere Geräte von den Downstream-Ports des Hubs entfernen.
- Schließen Sie das Gerät vorübergehend direkt an den Pi an und sehen Sie, ob sich das Verhalten verbessert.