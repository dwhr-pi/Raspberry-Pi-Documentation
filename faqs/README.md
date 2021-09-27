# FAQs

## Inhaltsverzeichnis

### [Was ist ein Raspberry Pi?](#Einleitung)

### [Ihren ersten Raspberry Pi kaufen](#buying)

- [Wo kann ich einen Raspberry Pi kaufen und wie viel kostet er?](#buying-where)
- [Was bekomme ich, wenn ich eins kaufe?](#kaufen-was)
- [Ich mache mir Sorgen, dass ich einen gefälschten Raspberry Pi habe!](#buying-fake)

### [Gewerbliche und industrielle Anwendungen, Weiterverkauf](#kommerziell)
- [Ich möchte Reseller von Raspberry Pi werden.](#commercial-resell)
- [Welche Fertigungsstandards etc. erfüllt der Raspberry Pi?](#commercial-compliance)
- [Kann ich einen Raspberry Pi in einem kommerziellen Produkt verwenden?](#commercial-integrate)
- [Ist ein Raspberry Pi für industrielle Anwendungen geeignet?](#commercial-suitable)

### [Die Computerhardware](#hardware)

- [Was sind die Unterschiede zwischen Raspberry Pi-Modellen?](#hardware-compare)
- [Welche Hardware-Dokumentation ist verfügbar?](#hardware-doc)
- [Welche Hardwareschnittstellen hat es?](#hardware-interfaces)
- [Kann ich einen Raspberry Pi für die Audio- oder Videoeingabe verwenden?](#hardware-inputs)
- [Wo ist der Ein-/Ausschalter?](#hardware-onoff)
- [Wie sind die Abmessungen des Raspberry Pi?](#hardware-footprint)

### [Leistung](#pi-Leistung)

- [Wie mächtig ist es?](#pi-performance-perf)
- [Kann ich meinen Raspberry Pi als Desktop-Ersatz verwenden?](#pi-performance-desktop)
- [Kann ich zusätzlichen RAM hinzufügen?](#pi-performance-addram)
- [Kann ich mehrere Raspberry Pis miteinander verbinden, um einen schnelleren Computer zu machen?](#pi-performance-cluster)
- [Warum läuft mein Raspberry Pi langsamer als angegeben?](#pi-performance-underclock)
- [Übertaktet es?](#pi-performance-overclock)
- [Wie ist seine Betriebstemperatur? Braucht es einen Kühlkörper?](#pi-performance-temps)

### [Software](#pi-software)

- [Welches Betriebssystem (OS) verwendet es?](#pi-software-os)
- [Kann ich Windows 10 auf dem Raspberry Pi ausführen?](#pi-software-win)
- [Aktualisierung? Upgrades? Was soll ich tun?](#pi-software-update)
- [Ich habe von etwas namens `rpi-update` gehört. Wann sollte ich das verwenden?](#pi-software-rpiupdate)
- [Die Prozessoren der neuesten Raspberry Pi-Modelle sind 64-Bit, aber ich kann kein offizielles 64-Bit-Betriebssystem finden.](#pi-software-64)
- [Kann ich PC-Software auf dem Raspberry Pi ausführen?](#pi-software-intelcompat)
- [Wird es Android oder Android Things ausführen?](#pi-software-androidcompat)
- [Wird es alte Software ausführen?](#pi-software-oldcompat)
- [Meine `.exe`-Datei wird nicht ausgeführt!](#pi-software-exefail)
- [Kann ich Dateien von meinem Raspberry Pi mit meinen Windows-Rechnern teilen?](#pi-software-fileshare)
- [Warum meldet `cpuinfo`, dass ich ein bcm2835 habe?](#pi-software-cpuinfo)
- [Wie führe ich ein Programm beim Start aus?](#pi-software-autostart)
- [Wie führe ich ein Programm zu einem bestimmten Zeitpunkt aus?](#pi-software-timedstart)

### [Video](#pi-video)

- [Welche Displays kann ich verwenden?](#pi-video-displays)
- [Unterstützt der HDMI-Port CEC?](#pi-video-hdmicec)
- [Warum gibt es keine VGA-Unterstützung?](#pi-video-vga)
- [Kann ich einen Touchscreen hinzufügen?](#pi-video-touch)
- [Welche Codecs kann es abspielen?](#pi-video-codecs)

### [Audio](#pi-audio)

- [Wird Ton über HDMI unterstützt?](#pi-audio-hdmi)
- [Was ist mit Standard-Audio-Ein- und -Ausgang?](#pi-audio-analog)

### [Leistung](#pi-Leistung)

- [Ist es sicher, einfach den Strom zu ziehen?](#pi-power-kill)
- [Was ist mit ungeplanten Stromunterbrechungen?](#pi-power-events)
- [Wie sind die Stromanforderungen?](#pi-power-specs)
- [Kann ich den Raspberry Pi über einen USB-Hub mit Strom versorgen?](#pi-power-usbhub)
- [Kann ich den Raspberry Pi sowohl über Batterien als auch über eine Steckdose mit Strom versorgen?](#pi-power-battbackup)
- [Ist Power over Ethernet (PoE) möglich?](#pi-power-ethernet)
- [Welche Spannungsgeräte kann ich an die GPIO-Pins anschließen und wie viel Strom kann ich ziehen?](#pi-power-gpioout)

### [SD-Karten und Speicher](#sd-cards)

- [Welche SD-Kartengröße benötige ich?](#sd-cards-need)
- [Welche SD-Kartengröße kann sie unterstützen?](#sd-cards-want)
- [Kann ich einen Raspberry Pi von einer USB-angeschlossenen Festplatte statt von der SD-Karte booten?](#sd-cards-usbboot)
- [Was passiert, wenn ich das Gerät blockiere?](#sd-cards-recovery)

### [Netzwerk und drahtlose Konnektivität](#networking)

- [Unterstützt das Gerät Netzwerke?](#networking-support)
- [Gibt es ein integriertes drahtloses Netzwerk?](#networking-builtinwifi)
- [Gibt es integriertes Bluetooth?](#networking-builtinbt)
- [Ich scheine kein Full-Speed-Gigabit-Netzwerk auf meinem Raspberry Pi 3B+ zu bekommen.](#networking-gigaperf)
- [Unterstützt das Gerät jede Form von Netbooting oder PXE?](#networking-netboot)

### [Kameramodul](#kameramodul)

- [Was ist das Kameramodul?](#cameramodule-was)
- [Welches Kameramodell verwendet das Kameramodul?](#cameramodule-model)
- [Welche Auflösungen werden unterstützt?](#cameramodule-res)
- [Welche Bildformate werden unterstützt?](#cameramodule-format)
- [Wie verwende ich das Kameramodul?](#cameramodule-how)
- [Kann ich das Flachbandkabel verlängern?](#cameramodule-cable)
- [Wie viel Strom verbraucht das Kameramodul?](#cameramodule-power)

### [Fehlerbehebung](#Fehlerbehebung)

- [Wie lautet der Benutzername und das Passwort für den Raspberry Pi?](#troubleshoot-defpasswd)
- [Warum passiert nichts, wenn ich mein Passwort eingebe?](#troubleshoot-inputpasswd)
- [Warum startet/bootet mein Raspberry Pi nicht?](#troubleshoot-boot)
- [Warum ist mein Raspberry Pi heiß?](#troubleshoot-temp)
- [Ich erhalte immer wieder ein Blitzsymbol und Nachrichten über Energie...](#troubleshoot-power)
- [Meine SD-Karte scheint nicht mehr zu funktionieren.](#troubleshoot-sd)
- [Ich habe eine SD-Karte mit Raspberry Pi OS/NOOBS erstellt, aber wenn ich sie mit meinem Windows-PC betrachte, ist nicht alles da!](#troubleshoot-fs)

---

<a name="introduction"></a>
## Was ist ein Raspberry Pi?

Raspberry Pi ist die drittgrößte Computermarke der Welt. Der Raspberry Pi ist ein kreditkartengroßer Computer, der an Ihren Fernseher oder Bildschirm angeschlossen wird, sowie eine Tastatur und eine Maus. Sie können es verwenden, um Programmieren zu lernen und Elektronikprojekte zu erstellen, und für viele der Dinge, die Ihr Desktop-PC tut, wie Tabellenkalkulationen, Textverarbeitung, Surfen im Internet und Spielen. Es spielt auch High-Definition-Videos ab. Der Raspberry Pi wird von Erwachsenen und Kindern auf der ganzen Welt verwendet, um Programmieren und digitales Machen zu lernen. Wie Sie Ihren Raspberry Pi einrichten und verwenden, erfahren Sie [hier](https://www.raspberrypi.org/help/).

<a name="buying"></a>
## Kaufe deinen ersten Raspberry Pi

<a name="kaufen-wo"></a>
### Wo kann ich einen Raspberry Pi kaufen und wie viel kostet er?

Gehen Sie zu unserer [Produktseite](https://www.raspberrypi.org/products/) und wählen Sie die Produkte aus, die Sie kaufen möchten. Wählen Sie dann Ihr Land aus dem Dropdown-Menü aus. Ihnen werden unsere zugelassenen Wiederverkäufer für Ihr Land vorgestellt.

Die folgenden Preise sind in US-Dollar und exklusive lokaler Steuern und Versand-/Bearbeitungsgebühren.

|Produkt|Preis|
|------|-----|
| [Raspberry Pi Model A+](https://www.raspberrypi.org/products/raspberry-pi-1-model-a-plus)| $20 |
| [Raspberry Pi Model B+](https://www.raspberrypi.org/products/raspberry-pi-1-model-b-plus) |$25 |
| [Raspberry Pi 2 Model B](https://www.raspberrypi.org/products/raspberry-pi-2-model-b)| $35 |
| [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/raspberry-pi-3-model-b) | $35 |
| [Raspberry Pi 3 Model A+](https://www.raspberrypi.org/products/raspberry-pi-3-model-a-plus) | $25 |
| [Raspberry Pi 3 Model B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus) | $35 |
| [Raspberry Pi 4 Model B 2/4/8GB](https://www.raspberrypi.org/products/raspberry-pi-4-model-b) | $35/$55/$75 |
| [Raspberry Pi 4 Model B offizielle Kits 2/4/8GB](https://www.raspberrypi.org/products/raspberry-pi-4-desktop-kit) | $100/$120/$140 |
| [Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero) | $5 |
| [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w) |$10 |
| [Raspberry Pi Zero WH](https://www.raspberrypi.org/products/raspberry-pi-zero-w) |$15 |
| [Raspberry Pi 400](https://www.raspberrypi.org/products/raspberry-pi-400-unit) |$70 |
| [Raspberry Pi 400 offizielles Kit](https://www.raspberrypi.org/products/raspberry-pi-400) |$100 |

<a name="kaufen-was"></a>
### Was bekomme ich, wenn ich einen kaufe?

Raspberry Pi-Computer sind einzeln oder als komplette Kits erhältlich, die alles enthalten, was Sie für den Einstieg benötigen, einschließlich Maus, HDMI-Kabel, SD-Karte, Netzteil sowie unserem offiziellen Einsteigerhandbuch. Der Raspberry Pi 400 ist ein Computer in einer Tastatur: Andere Kits werden mit einem separaten Gehäuse und einer separaten Tastatur geliefert.

Raspberry Pi verkauft auch alle Geräte, die Sie benötigen, um Ihren Raspberry Pi separat zum Laufen zu bringen. Wir empfehlen die Verwendung eines offiziellen Raspberry Pi Netzteils. Verwenden Sie für den Pi Zero, 1, 2 und 3 das [Raspberry Pi 1, 2 und 3 Netzteil](https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/), für die Pi 4B und Pi 400 verwenden das [Raspberry Pi USB-C Netzteil](https://www.raspberrypi.org/products/type-c-power-supply/).

Offizielle Hüllen für die Raspberry Pi-Reihe sind separat erhältlich [von unseren Distributoren](https://www.raspberrypi.org/products/). Community-Mitglieder haben viele tolle [3D-druckbare Hüllen](https://www.raspberrypi.org/blog/3d-printed-raspberry-pi-cases/) entworfen. 

Ausführliche Informationen zu all unseren Produkten finden Sie auf unserer [Produktseite](https://www.raspberrypi.org/products/).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 

<a name="buying-fake"></a>
### Ich mache mir Sorgen, dass ich einen gefälschten Raspberry Pi habe!

Keine Sorge, soweit wir wissen, gibt es keine gefälschten Raspberry Pis. Die in der Raspberry-Pi-Reihe verwendeten Prozessoren sind nur von einem Anbieter und nur in Stückzahlen von mehreren Millionen erhältlich, was zusammen mit dem niedrigen Preis des Raspberry Pi die Herstellung von Klonen nicht wirtschaftlich macht. Es gibt eine Reihe von Konkurrenzprodukten, die ähnliche Namen verwenden, aber keine tatsächlichen Klone oder Fälschungen.

<a name="commercial"></a>
## Kommerzielle und industrielle Anwendungen, Weiterverkauf

<a name="commercial-resell"></a>
### Ich möchte Reseller von Raspberry Pi werden.

Wir haben eine exklusive Herstellungs- und Vertriebsvereinbarung mit [RS](http://uk.rs-online.com/web/generalDisplay.html?id=raspberrypi) und [Farnell](https://www.element14.com/community/community/raspberry-pi). Wiederverkäufer kaufen den Raspberry Pi in großen Mengen bei ihnen (was die Versandkosten auf fast nichts reduziert) und verkaufen sie weiter. Für den Weiterverkauf benötigen Sie keine spezielle Lizenz, und die Distributoren verkaufen sehr gerne an Wiederverkäufer. Wenn Sie daran interessiert sind, an unserem [Zugelassener Wiederverkäufer](https://www.raspberrypi.org/blog/approved-reseller/)-Programm mit teilzunehmen, kontaktieren Sie uns per E-Mail an info@raspberrypi.com.

<a name="commercial-compliance"></a>
### Welche Fertigungsstandards etc. erfüllt der Raspberry Pi?

Wir haben die Raspberry Pi-Modelle umfangreichen Konformitätstests für Europa, die USA und andere Länder auf der ganzen Welt unterzogen. Viele der Berichte finden Sie [hier](../hardware/raspberrypi/conformity.md).

<a name="commercial-integrate"></a>
### Kann ich einen Raspberry Pi in einem kommerziellen Produkt verwenden?

Dies ist eine sehr häufige Frage, und die Antwort ist ja! Sobald Sie einen Raspberry Pi gekauft haben, können Sie ihn nach Belieben verwenden. Beachten Sie jedoch, dass ein Großteil der Software in der Raspberry Pi OS-Distribution GPL-lizenziert ist, was bestimmte Anforderungen mit sich bringt, vor allem, dass Sie auf Anfrage Zugriff auf den Quellcode gewähren müssen. Dies ist normalerweise ziemlich einfach zu tun.

<a name="kommerziell-geeignet"></a>
### Ist ein Raspberry Pi für industrielle Anwendungen geeignet?

Ja – es hängt von Ihrem Anwendungsfall ab. Im industriellen Umfeld werden Raspberry Pis erfolgreich eingesetzt, aber die endgültige Entscheidung, ob das Gerät für die gestellte Aufgabe geeignet ist, muss beim Endanwender liegen. Weitere Informationen zu unserem Raspberry Pi-Modell, das speziell für den Einsatz in kommerziellen und industriellen Produkten entwickelt wurde, finden Sie in unserer [Compute Module-Dokumentation](../hardware/computemodule/README.md).

<a name="hardware"></a>
## Die Computerhardware

<a name="hardware-compare"></a>
### Was sind die Unterschiede zwischen Raspberry Pi-Modellen?

Dies sind die derzeit verfügbaren [Modelle des Raspberry Pi](https://www.raspberrypi.org/products/): das Pi 3 Model B, das Pi 2 Model B, das Pi Zero, das Pi Zero W und das Pi 1 Modell B+ und A+.

| Produkt | SoC | Geschwindigkeit | RAM | USB-Anschlüsse | Ethernet | Drahtlos | Bluetooth |
|---------|-----|-------|-----|:--------:|:--------:|:--------:|:---------:|
| Raspberry Pi Modell A+ | BCM2835 | 700MHz | 512 MB | 1 | Nein | Nein | Nein |
| Raspberry Pi Modell B+ | BCM2835 | 700MHz | 512 MB | 4 |100Base-T | Nein | Nein |
| Raspberry Pi 2 Modell B | BCM2836/7 | 900MHz | 1GB | 4 |100Base-T| Nein | Nein |
| Raspberry Pi 3 Modell B | BCM2837A0/B0 | 1200MHz | 1GB | 4 |100Base-T| 802.11n| 4.1 |
| Raspberry Pi 3 Modell A+ | BCM2837B0 | 1400MHz | 512 MB | 1 | Nein | 802.11ac/n | 4.2 |
| Raspberry Pi 3 Modell B+ | BCM2837B0 | 1400MHz | 1GB | 4 |1000Base-T | 802.11ac/n | 4.2 |
| Raspberry Pi 4 Modell B | BCM2711 | 1500MHz | 2GB | 2xUSB2, 2xUSB3 |1000Base-T | 802.11ac/n | 5.0 |
| Raspberry Pi 4 Modell B | BCM2711 | 1500MHz | 4GB | 2xUSB2, 2xUSB3 |1000Base-T | 802.11ac/n | 5.0 |
| Raspberry Pi 4 Modell B | BCM2711 | 1500MHz | 8GB | 2xUSB2, 2xUSB3 |1000Base-T | 802.11ac/n | 5.0 |
| Raspberry Pi Zero | BCM2835 | 1000MHz | 512 MB | 1 | Nein | Nein | Nein |
| Raspberry Pi Zero W | BCM2835 | 1000MHz | 512 MB | 1 | Nein | 802.11n | 4.1 |
| Raspberry Pi Zero WH | BCM2835 | 1000MHz | 512 MB | 1 | Nein | 802.11n | 4.1 |
| Raspberry Pi 400 | BCM2711 | 1800MHz | 4GB | 1xUSB2, 2xUSB3 |1000Base-T | 802.11ac/n | 5.0 |

Das Model A+ ist die kostengünstige Variante des Raspberry Pi. Es verfügt über 512 MB RAM (Stand August 2016: frühere Modelle haben 256 MB), einen USB-Port, 40 GPIO-Pins und keinen Ethernet-Port.

Das Model B+ ist die letzte Überarbeitung des ursprünglichen Raspberry Pi. Es verfügt über 512 MB RAM, vier USB-Ports, 40 GPIO-Pins und einen Ethernet-Port.

Im Februar 2015 wurde es vom Raspberry Pi 2 Model B, der zweiten Generation des Raspberry Pi, abgelöst. Der Raspberry Pi 2 teilt viele Spezifikationen mit dem Raspberry Pi 1 B+ und verwendet ursprünglich eine 900-MHz-Quad-Core-Arm-Cortex-A7-CPU und verfügt über 1 GB RAM. Einige neuere Versionen des Raspberry Pi 2 (v1.2) verwenden jetzt eine 900-MHz-Arm-Cortex-A53-CPU.

Das Raspberry Pi 3 Model B wurde im Februar 2016 auf den Markt gebracht. Es verwendet eine 1,2-GHz-64-Bit-Quad-Core-Arm-Cortex-A53-CPU, verfügt über 1 GB RAM, integriertes 802.11n-WLAN und Bluetooth 4.1.

Das Raspberry Pi 3 Model B+ wurde im März 2018 auf den Markt gebracht. Es verwendet eine 1,4 GHz 64-Bit-Quad-Core-Arm-Cortex-A53-CPU, verfügt über 1 GB RAM, Gigabit-Ethernet, integriertes 802.11ac/n-WLAN und Bluetooth 4.2.

Das Raspberry Pi 4 Model B wurde im Juni 2019 auf den Markt gebracht. Es verwendet eine 1,5 GHz 64-Bit-Quad-Core-Arm-Cortex-A72-CPU, verfügt über drei RAM-Optionen (2 GB, 4 GB, 8 GB), Gigabit-Ethernet, integriertes 802.11ac/n-Wireless LAN und Bluetooth 5.0. Ursprünglich mit einer 1-GB-Option auf den Markt gebracht, wurde dies durch die 2 GB zum ursprünglichen 1-GB-Preis ersetzt. Das 1-GB-Gerät ist weiterhin als Sonderbestellung erhältlich.

Der Raspberry Pi 400 wurde im November 2020 auf den Markt gebracht. Er verwendet eine 1,8 GHz 64-Bit-Quad-Core-Arm-Cortex-A72-CPU mit 4 GB RAM, Gigabit-Ethernet, integriertem 802.11ac/n-WLAN und Bluetooth 5.0. Der Raspberry Pi 400 ist der erste Raspberry Pi, der in eine kompakte Tastatur integriert ist.

Der Raspberry Pi Zero und Raspberry Pi Zero W/WH sind halb so groß wie ein Model A+, mit einer 1 GHz Single-Core-CPU und 512 MB RAM, Mini-HDMI- und USB-On-The-Go-Anschlüssen und einem Kameraanschluss. Der Raspberry Pi Zero W hat außerdem 802.11n Wireless LAN und Bluetooth 4.1 integriert. Der Raspberry Pi Zero WH ist identisch mit dem Zero W, kommt aber mit einem vorgelöteten Header.

Das Model A/A+ hat einen USB-Port, das Model B hat zwei Ports und das Model B+, Raspberry Pi 2 Model B und Raspberry Pi 3 Model B haben vier Ports. Diese können verwendet werden, um die meisten USB 2.0-Geräte anzuschließen. Über einen USB-Hub können weitere USB-Geräte wie Mäuse, Tastaturen, Netzwerkadapter und externe Speicher angeschlossen werden. Der Raspberry Pi Zero und Raspberry Pi Zero W verfügen über einen einzigen Micro-USB-Anschluss, dies erfordert ein USB-OTG-Kabel, um Geräte wie Tastaturen oder Hubs anzuschließen.

Das endgültige Modell (in der obigen Tabelle nicht beschrieben) ist das [Compute Module](https://www.raspberrypi.org/products/compute-module-3/), das für industrielle Anwendungen gedacht ist. Es handelt sich um ein Gerät mit kleinem Formfaktor, das mit einer Trägerplatine verbunden wird, beispielsweise einer Leiterplatte in einem Industrieprodukt, und Herstellern eine einfache Möglichkeit bietet, das Raspberry Pi-Ökosystem in ihren eigenen Geräten zu verwenden.

Weitere Informationen zu aktuellen Boards finden Sie auf unseren [products](https://www.raspberrypi.org/products/)-Seiten. Es gibt auch einige Raspberry-Pi-Modelle, die nicht mehr produziert werden, aber möglicherweise gebraucht oder bei Wiederverkäufern erhältlich sind. Das Model A war die erste kostengünstige Variante des Raspberry Pi. Es wurde im November 2014 durch das kleinere, sauberere Model A+ ersetzt; es hat die gleichen Spezifikationen wie das A+, hat aber nur 26 GPIO-Pins und 128 MB RAM. Das Modell B war die vorherige Inkarnation des B+; Auch hier hat es die meisten der gleichen Spezifikationen, hat aber nur zwei USB-Anschlüsse und 26 GPIO-Pins. Die ursprüngliche Version des Raspberry Pi Zero hatte keinen Kameraanschluss, aber alle aktuellen Versionen haben den Anschluss standardmäßig.

<a name="hardware-doc"></a>
### Welche Hardware-Dokumentation ist verfügbar?

Die gesamte verfügbare Dokumentation befindet sich in unserem [Dokumentations-Repository](./README.md).

<a name="hardware-interfaces"></a>
### Welche Hardwareschnittstellen hat es?

Je nach Modell verfügt der Raspberry Pi über 40 oder 26 dedizierte Schnittstellenpins. Dazu gehören in allen Fällen ein UART, ein I2C-Bus, ein SPI-Bus mit zwei Chip-Selects, I2S-Audio, 3V3, 5V und Masse. Die maximale Anzahl von GPIOs kann theoretisch durch die Nutzung des I2C- oder SPI-Busses unbegrenzt erweitert werden.

Es gibt auch einen dedizierten CSI-2-Kamera-Port für das [Raspberry Pi Camera Module](https://www.raspberrypi.org/products/) und einen DSI-Display-Port für das [Raspberry Pi LCD-Touchscreen-Display](https://www.raspberrypi.org/products/raspberry-pi-touch-display/).

<a name="hardware-inputs"></a>
### Kann ich einen Raspberry Pi für die Audio- oder Videoeingabe verwenden?

Nicht an sich: Es gibt keine Audio- oder Video-(HDMI/Composite)-IN-Fähigkeit auf dem Raspberry Pi. Sie können Boards von Drittanbietern hinzufügen, um diese Art von Funktionalität hinzuzufügen. Der Raspberry Pi verfügt über eine Kameraschnittstelle, die Videos vom [Raspberry Pi Camera Module](https://www.raspberrypi.org/products/) aufnehmen kann – Sie finden den Abschnitt [FAQ zum Kameramodul hier](#cameramodule).

<a name="hardware-onoff"></a>
### Wo ist der Ein-/Ausschalter?

Es gibt keinen Ein-/Ausschalter! Zum Einschalten einfach einstecken. Zum Ausschalten, wenn Sie den Raspberry Pi Desktop verwenden, wählen Sie einfach die Option `Logout` aus dem Hauptmenü und dann `Shutdown`. Wenn Sie nicht den Desktop verwenden und sich an der Konsole befinden, können Sie den Raspberry Pi mit der Eingabe von `sudo halt -h` herunterfahren. Warten Sie, bis alle LEDs außer der Power-LED aus sind, und warten Sie dann eine weitere Sekunde, um sicherzustellen, dass die SD-Karte ihre Wear-Leveling-Aufgaben und Schreibaktionen beenden kann. Sie können den Raspberry Pi jetzt sicher vom Strom trennen. Wenn Sie den Raspberry Pi nicht ordnungsgemäß herunterfahren, kann Ihre SD-Karte beschädigt werden, was bedeuten würde, dass Sie sie neu abbilden müssen.

<a name="hardware-footprint"></a>
### Welche Abmessungen hat der Raspberry Pi?

Die Raspberry Pi Model B-Versionen messen 85,60 mm x 56 mm x 21 mm (oder ungefähr 3,37 "x 2,21" x 0,83"), mit einer kleinen Überlappung für die SD-Karte und die Anschlüsse, die über die Kanten hinausragen. Sie wiegen 45g. Der Raspberry Pi Zero und Raspberry Pi Zero W messen 65 mm x 30 mm x 5,4 mm (oder etwa 2,56 x 1,18 x 0,20 Zoll) und wiegen 9 g. Die mechanischen Umrisse finden Sie in der Dokumentation [hier](../hardware/raspberrypi/mechanical/README.md)

<a name="pi-performance"></a>

## Leistung

<a name="pi-performance-perf"></a>
### Wie stark ist es?

Alle Raspberry Pi-Modelle bis hin zum Raspberry Pi 3 verfügen über eine GPU, die OpenGL ES 2.0, hardwarebeschleunigtes OpenVG und 1080p30 H.264 High-Profile-Encodieren und -Decodieren bietet. Die GPU ist in der Lage, 1 Gpixel/s, 1,5 Gtexel/s oder 24 GFLOPs für Allzweck-Computing zu leisten und verfügt über eine Reihe von Texturfiltern und DMA-Infrastruktur. Dies bedeutet, dass die Grafikfähigkeiten ungefähr dem Leistungsniveau der ursprünglichen Xbox entsprechen. Die reale Gesamtleistung für Raspberry Pi 1 Model A, A+, B, B+, Raspberry Pi Zero/Zero W und CM1 ähnelt der eines 300MHz Pentium 2, jedoch mit viel besserer Grafik. Das Raspberry Pi 2 Model B entspricht ungefähr einem Athlon Thunderbird mit 1,1 GHz; Auch hier hat es die viel hochwertigere Grafik, die aus der Verwendung der gleichen GPU wie bei früheren Modellen stammt. Der Raspberry Pi 3 Model B ist, abhängig von den gewählten Benchmarks, etwa doppelt so schnell wie der Raspberry Pi 2 Model B.

Der Raspberry Pi 4 und Raspberry Pi 400 verwenden eine verbesserte GPU – den VideoCore VI. Dies ist etwa viermal schneller als der VideoCore IV, der für frühere Modelle verwendet wurde. Die neuen ARM-A72-Kerne auf dem BCM2711-Chip bieten eine viel bessere Leistung als die Vorgängermodelle, und ein neuer PCIe-Bus bietet schnellere USB 2.0- und neue USB 3.0-Funktionen. Die native Ethernet-Fähigkeit des Raspberry Pi 4 ermöglicht volle 1-Gbit-I/O. Diese Funktionen, kombiniert mit dem optionalen zusätzlichen RAM (der Raspberry Pi 4 kann mit 2 GB, 4 GB oder 8 GB RAM erworben werden), bedeuten, dass der Raspberry Pi 4 jetzt ein großartiges Desktop-Computing-Erlebnis bietet!

<a name="pi-performance-desktop"></a>
### Kann ich meinen Raspberry Pi als Desktop-Ersatz verwenden?

Es hängt von Ihrem Modell ab! Für viele alltägliche Aufgaben ist der Raspberry Pi 3 durchaus geeignet, da moderne Internetbrowser jedoch viel Speicher benötigen, kann das Surfen etwas langsam sein, wenn Sie zu viele Browser-Tabs öffnen. Obwohl 1 GB RAM viel zu sein scheint, sind moderne Browser echte Speicherfresser!

Der Raspberry Pi 4 – mit seinen schnelleren Kernen, zusätzlichem Speicher und stark verbesserter I/O – ist ein sehr guter Desktop-Ersatz.

Die beste Option für den Desktop-Einsatz ist der Pi 400. Dies entspricht einem Raspberry Pi 4 in einer kompakten Tastatur.

<a name="pi-performance-addram"></a>
### Kann ich zusätzlichen RAM hinzufügen?

Nein. Der RAM auf dem Raspberry Pi 1 Model A, A+, B, B+ und Raspberry Pi Zero/Zero W ist ein Package on Package (POP) auf dem SoC, was bedeutet, dass Sie es nicht entfernen oder austauschen können. Der RAM der Raspberry Pi 2 und 3 Model B-Versionen befindet sich auf einem separaten Chip auf der Unterseite der Platine, aber 1 GB ist der maximale RAM, den der von den Raspberry Pi 2 und 3 Model B verwendete SoC unterstützen kann. Der Raspberry Pi 4 unterstützt bis zu 8 GB RAM, ist aber wie die Vorgängermodelle nach dem Kauf nicht aufrüstbar.

<a name="pi-performance-cluster"></a>
### Kann ich mehrere Raspberry Pis miteinander verbinden, um einen schnelleren Computer zu machen?

Irgendwie, aber nicht so, wie Sie es vielleicht machen möchten. Sie können nicht einfach einen leistungsfähigeren Computer bauen, um beispielsweise Spiele schneller zu spielen, indem Sie kleinere zusammenschrauben. Sie können Computer vernetzen, um einen Cluster-Computer zu erstellen, aber Sie müssen Ihre Software ändern, damit sie auf diese verteilte Weise funktioniert. Wir haben in Zusammenarbeit mit GCHQ ein Tutorial für [How to build a Raspberry Pi cluster](https://projects.raspberrypi.org/en/projects/build-an-octapi) zusammengestellt.

<a name="pi-performance-underclock"></a>
### Warum läuft mein Raspberry Pi langsamer als angegeben?

Der Raspberry Pi (alle Modelle) läuft im Leerlauf mit einer niedrigeren Geschwindigkeit als angegeben. Steigt die Auslastung der CPU, dann erhöht sich die Taktrate, bis sie ihren Maximalwert erreicht, der je nach Modell variiert. Beginnt die CPU zu überhitzen, kommt die Komplexität hinzu: Je nach Modell wird bei Erreichen einer bestimmten Temperatur der Takt gedrosselt, um eine Überhitzung zu vermeiden. Dies wird als thermische Drosselung bezeichnet. Wenn der Raspberry Pi thermisch gedrosselt wird, sehen Sie oben rechts auf dem Desktop ein Warnsymbol (siehe [hier](../configuration/warning-icons.md)).

<a name="pi-performance-overclock"></a>
### Übertaktet es?

Übertakten wird offiziell nicht unterstützt und führt in einigen Fällen zum Erlöschen der Garantie. Allerdings übertakten manche Leute erfolgreich. Aufgrund der Art und Weise, wie Siliziumchips hergestellt werden, unterscheidet sich jedes einzelne Gerät darin, wie stark es übertaktet werden kann. Für ein vollständig unterstütztes, stabiles System empfehlen wir Ihnen, Ihren Raspberry Pi nicht zu übertakten.

<a name="pi-performance-temps"></a>
### Wie hoch ist seine Betriebstemperatur? Braucht es einen Kühlkörper?

Der Raspberry Pi besteht aus kommerziellen Chips, die für verschiedene Temperaturbereiche qualifiziert sind; der LAN9514 (LAN9512 bei älteren Modellen mit 2 USB-Ports) wird von den Herstellern als qualifiziert von 0 °C bis 70 °C spezifiziert, während der SoC von -40 °C bis 85 °C qualifiziert ist. Sie werden vielleicht feststellen, dass das Board auch außerhalb dieser Temperaturen funktioniert, aber wir qualifizieren das Board selbst nicht für diese Extreme.

Sie sollten keinen Kühlkörper verwenden, da der im Raspberry Pi verwendete Chip dem eines Mobiltelefons entspricht und nicht heiß genug werden sollte, um eine spezielle Kühlung zu erfordern. Je nach verwendetem Gehäuse und Übertaktungseinstellungen kann ein Kühlkörper jedoch von Vorteil sein. Wir empfehlen die Verwendung eines Kühlkörpers, wenn Sie den Raspberry Pi 3 Model B übertakten. Wenn Ihnen das Aussehen eines solchen gefällt, können Sie dem Raspberry Pi natürlich nicht schaden, wenn Sie einen entsprechend großen Kühlkörper darauf platzieren.

Der Raspberry Pi 400 verwendet einen großen Heatspreader im Gehäuse und benötigt keine zusätzliche Kühlung.

<a name="pi-software"></a>
## Software

<a name="pi-software-os"></a>
### Welches Betriebssystem verwendet es?
Die empfohlene Distribution (Distro) ist Raspberry Pi OS, das speziell für den Raspberry Pi entwickelt wurde und von unseren Ingenieuren ständig optimiert wird. Es ist jedoch ein einfacher Vorgang, die Root-Partition auf der SD-Karte durch eine andere Arm Linux-Distribution zu ersetzen, daher empfehlen wir Ihnen, mehrere Distributionen auszuprobieren, um zu sehen, welche Ihnen am besten gefällt. Auf unserer Seite [Downloads](https://www.raspberrypi.org/downloads) sind mehrere andere Distributionen verfügbar. Das Betriebssystem wird auf der SD-Karte gespeichert.

<a name="pi-software-win"></a>
### Kann ich Windows 10 auf dem Raspberry Pi ausführen?
Auf dem Raspberry Pi kann Windows 10 nicht ausgeführt werden. Es gibt jedoch eine spezielle "Internet of Things"-Version von Windows 10, die auf dem Raspberry Pi 3B und 3B+ läuft, genannt Windows 10 IoT. Windows 10 IoT hat keinen grafischen Desktop und ist für die Verwendung in eingebetteten Geräten vorgesehen.

Möglicherweise sehen Sie online einen Hinweis auf Windows 10, das auf dem Raspberry Pi ausgeführt wird. Dies liegt daran, dass die Community eine Möglichkeit entwickelt hat, normales Windows 10 auf dem Raspberry Pi auszuführen. Obwohl es läuft, läuft es extrem langsam, so dass es keinen wirklichen Nutzen hat. Es ist vielmehr ein Proof-of-Concept. Microsoft sanktioniert nicht die Ausführung von Windows 10 auf dem Raspberry Pi.

<a name="pi-software-update"></a>
### Aktualisierung? Upgrades? Was mache ich?

Es ist wichtig, Ihr System mit den neuesten Sicherheitsupdates sowie Fehlerbehebungen für alle Anwendungen, die Sie möglicherweise verwenden, auf dem neuesten Stand zu halten. Sie können dies ganz einfach tun, indem Sie ein Terminalfenster öffnen und die folgenden beiden Befehle ausführen:

+ `sudo apt update` lädt Paketinformationen von allen konfigurierten Quellen herunter, damit das System die neuesten Updates kennt.

+ `sudo apt full-upgrade` lädt dann alle Updates herunter und installiert sie.

Wir empfehlen, diesen Vorgang etwa einmal pro Woche durchzuführen.

<a name="pi-software-rpiupdate"></a>
### Ich habe von etwas namens `rpi-update` gehört. Wann sollte ich das verwenden?

Verwenden Sie "rpi-update" nicht, es sei denn, Sie wurden von einem Raspberry Pi-Ingenieur dazu empfohlen. Dies liegt daran, dass der Linux-Kernel und die Raspberry Pi-Firmware auf die neueste Version aktualisiert werden, die derzeit getestet wird. Es kann daher Ihren Raspberry Pi instabil machen oder einen zufälligen Bruch verursachen.

<a name="pi-software-64"></a>
### Die Prozessoren der neuesten Raspberry Pi-Modelle sind 64-Bit, aber ich kann kein offizielles 64-Bit-Betriebssystem finden.

Raspberry Pi bietet derzeit aus verschiedenen Gründen kein offizielles 64-Bit-Betriebssystem. Erstens, da wir immer noch 32-Bit-Geräte verkaufen, müssten wir zwei separate Distributionen unterstützen, und im Moment haben wir nicht die Unterstützungskapazität. Zweitens würde der Aufbau eines vollständigen 64-Bit-Betriebssystems einen erheblichen Arbeitsaufwand erfordern, um beispielsweise die Schnittstelle zur 32-Bit-VideoCore-GPU zu reparieren. Es gibt 64-Bit-Betriebssysteme von Drittanbietern, die jedoch nicht die volle Unterstützung für die GPU bieten, die für eine offizielle Veröffentlichung erforderlich wäre.

<a name="pi-software-intelcompat"></a>
### Kann ich PC- oder Mac-Software auf dem Raspberry Pi ausführen?

Es ist nicht möglich, Mac-Software auf dem Raspberry Pi auszuführen.

Sie können Windows-Software nicht *direkt* auf dem Raspberry Pi ausführen. Es ist manchmal möglich, Emulationssoftware zu verwenden, um Windows-Anwendungen auf dem Raspberry Pi auszuführen, aber die Verwendung der Emulation macht es viel langsamer. Beispielsweise läuft Windows 98 auf dem Raspberry Pi mit einem Emulator namens QEMU recht gut, neuere Windows-Software läuft jedoch zu langsam, um auf dem Raspberry Pi nützlich zu sein.

Es gibt eine Fülle von Linux-Software, die direkt auf dem Raspberry Pi selbst verfügbar ist. Standardmäßig wird Raspberry Pi OS mit den am häufigsten verwendeten Anwendungen installiert. Wenn Sie etwas anderes installieren müssen, verwenden Sie die Anwendung "Software hinzufügen / entfernen". Sie können auch den Befehl `apt` verwenden - siehe [APT](../linux/software/apt.md). An anderer Stelle verfügbare Linux-Software-Binärdateien werden normalerweise für die x86- und x64-Architekturen kompiliert, können also nicht auf dem Raspberry Pi verwendet werden, da er die ARM-Architektur verwendet. Wenn der Quellcode jedoch verfügbar ist, können Sie selbst eine ARM-spezifische Version kompilieren.

<a name="pi-software-androidcompat"></a>
### Wird es Android oder Android Things ausführen?

Raspberry Pi selbst unterstützt nicht die Consumer-Version von Android, die Sie möglicherweise von Ihrem Mobiltelefon kennen. Es gibt Bemühungen der Community, eine Version zur Verfügung zu stellen, die online zu finden ist.

Google unterstützt Android Things auf dem Raspberry Pi 3 als Entwicklungsplattform. Android Things ist eine Variante der Android-Plattform, die es Entwicklern ermöglicht, mit dem Android SDK Software für eingebettete Geräte und Geräte für das Internet der Dinge (IoT) zu erstellen. Um mehr über die Plattform und die ersten Schritte zu erfahren, besuchen Sie [developer.android.com/things](https://developer.android.com/things/index.html).

<a name="pi-software-oldcompat"></a>
### Kann alte Software ausgeführt werden?

Generell müssen Sie prüfen, ob das gewünschte Programm für die Architektur Armv6 (Raspberry Pi 1/Zero/Zero W/CM), Armv7 (Raspberry Pi 2) oder Armv8 (Raspberry Pi 3) unter Linux kompiliert werden kann. In den meisten Fällen wird die Antwort ja sein. Spezifische Programme werden in [unseren Foren](https://www.raspberrypi.org/forums/) diskutiert, daher sollten Sie dort nach einer Antwort suchen. Letztendlich gibt es nichts Besseres, als sich einen Raspberry Pi zu schnappen und die Antwort durch direktes Testen herauszufinden!

<a name="pi-software-exefail"></a>
### Meine `.exe`-Datei wird nicht ausgeführt!

Die meisten `.exe`-Dateien stammen von Windows und sind für die x86-Prozessorarchitektur kompiliert. Diese laufen nicht auf dem Raspberry Pi, der eine ARM-Prozessorarchitektur verwendet. Eine Minderheit von '.exe'-Dateien, die aus C#-Code oder ähnlichem kompiliert wurden, verwenden tatsächlich einen Byte-Code anstelle eines prozessorspezifischen Befehlssatzes und können daher auf dem Raspberry Pi funktionieren, wenn die richtige Mono-Interpreter-Software installiert ist.

<a name="pi-software-fileshare"></a>
### Kann ich Dateien von meinem Raspberry Pi mit meinen Windows-Rechnern teilen?

Ja, es gibt eine Reihe von Möglichkeiten, dies zu tun, und die gebräuchlichste ist die Verwendung sogenannter Samba-Freigaben. Wir haben noch keine spezifische Dokumentation zu Samba-Freigaben in unseren offiziellen Dokumenten, aber [hier](https://www.raspberrypi.org/magpi/samba-file-server/) finden Sie einige aus unserem Magazin [The MagPi](https://www.raspberrypi.org/magpi).

Es ist auch einfach, Dateien auf und von Windows-Geräten zu kopieren, anstatt Ordner freizugeben. Es gibt reichlich Dokumentation [hier](../remote-access/README.md).

<a name="pi-software-cpuinfo"></a>
### Warum meldet `cpuinfo`, dass ich einen BCM2835 habe?

Die Entwickler des Upstream-Linux-Kernel hatten beschlossen, dass alle Modelle des Raspberry Pi bcm2835 als SoC-Namen zurückgeben. Bei Raspberry Pi verwenden wir gerne so viel Upstream-Kernel-Code wie möglich, da dies die Softwarewartung erheblich erleichtert, daher verwenden wir diesen Code. Leider bedeutet dies, dass `cat /proc/cpuinfo` für Raspberry Pi 2, Raspberry Pi 3 und Raspberry Pi 4, die die bcm2836/bcm2837, bcm2837 bzw. bcm2711 verwenden, ungenau ist. Sie können `cat /proc/device-tree/model` verwenden, um eine genaue Beschreibung des SoC auf Ihrem Raspberry Pi-Modell zu erhalten.

<a name="pi-software-autostart"></a>
### Wie führe ich ein Programm beim Start aus?

Dafür gibt es mehrere Möglichkeiten — [hier ist einer](../linux/usage/rc-local.md).

<a name="pi-software-timedstart"></a>
### Wie führe ich ein Programm zu einer bestimmten Zeit aus?

Mit Cron! [So geht's](../linux/usage/cron.md).

<a name="pi-video"></a>
## Video

<a name="pi-video-displays"></a>
### Welche Displays kann ich verwenden?

Die Platine verfügt über einen Composite- und HDMI-Ausgang, sodass Sie ihn über den Composite- oder über einen Composite-auf-SCART-Anschluss an einen alten analogen Fernseher, an einen digitalen Fernseher oder an einen DVI-Monitor anschließen können (mit einem billigen, passiven HDMI-auf-DVI Kabel für DVI). Beim Model B+, Raspberry Pi 2 und Raspberry Pi 3 wurde die RCA-Composite-Buchse durch eine 3,5-mm-Buchse ersetzt, die Audio und Video in einem vereint. Sie benötigen ein 3,5-mm-zu-3RCA-Adapterkabel, um es an einen älteren Fernseher anzuschließen. Es gibt viele verschiedene Arten dieses Kabels, aber Sie möchten eines kaufen, das mit dem iPod Video kompatibel ist (der iPod hat die linken und rechten Audiokanäle vertauscht, aber die in NOOBS enthaltene Version von Raspberry Pi OS kann dies austauschen für dich). Der Raspberry Pi Zero verwendet einen Mini-HDMI-Anschluss.

Der Raspberry Pi 4 unterstützt zwei HDMI-Monitore, die über Micro-HDMI-Anschlüsse angeschlossen werden. Es ist auch in der Lage, auf einem 4K-Monitor oder Fernseher in voller Auflösung anzuzeigen. Beachten Sie, dass für die beste HDMI-Leistung bei 4K ein hochwertiges HDMI-Kabel erforderlich ist. Wir verkaufen ein komplettes Set an Zusatzkomponenten, einschließlich HDMI-Kabel.

<a name="pi-video-hdmicec"></a>
### Unterstützt der HDMI-Port CEC?

Ja, der HDMI-Port des Raspberry Pi unterstützt den CEC-Standard. CEC wird vom Hersteller Ihres Fernsehgeräts möglicherweise anders genannt; Weitere Informationen finden Sie im [Wikipedia-Eintrag zu CEC](http://en.wikipedia.org/wiki/Consumer_Electronics_Control#CEC).

<a name="pi-video-vga"></a>
### Warum gibt es keine VGA-Unterstützung?

Es gibt zwar keine native VGA-Unterstützung, aber aktive Adapter sind verfügbar. Passive HDMI-zu-VGA-Kabel funktionieren nicht mit dem Raspberry Pi. Achten Sie beim Kauf eines aktiven VGA-Adapters darauf, dass er mit einem externen Netzteil geliefert wird. HDMI-zu-VGA-Adapter ohne externe Stromversorgung funktionieren oft nicht.

<a name="pi-video-touch"></a>
### Kann ich einen Touchscreen hinzufügen?

Die Raspberry Pi Foundation stellt einen 7" kapazitiven [Touchscreen](https://www.raspberrypi.org/products/raspberry-pi-touch-display) zur Verfügung, der den DSI-Port des Raspberry Pi nutzt. Dieser ist über die üblichen Distributoren erhältlich. Alternativ , bieten mehrere Drittanbieter eine Reihe von Touchscreens für den Raspberry Pi an.

<a name="pi-video-codecs"></a>
### Welche Codecs kann es abspielen?

Der Raspberry Pi kann H.264 (MP4/MKV) out of the box kodieren (aufzeichnen) und dekodieren (wiedergeben). Es gibt auch zwei zusätzliche Codecs, die Sie [über unseren Swag Store kaufen](http://swag.raspberrypi.org/collections/software) können, mit denen Sie [MPEG-2](http://swag.raspberrypi.org/collections/software/products/mpeg-2-license-key) entschlüsseln können, ein sehr beliebtes und weit verbreitetes Format zum Kodieren von DVDs, Videokameraaufnahmen, TV und vielen anderen, und [VC-1](http://swag.raspberrypi.org/collections/software/products/vc-1-license-key), ein Microsoft-Format, das auf Blu-ray-Discs, Windows Media, Slingbox und HD-DVDs zu finden ist.

Auf dem Raspberry Pi 4 ist die zusätzliche Hardware-CODEC-Unterstützung für MPEG-2 und VC-1 nicht verfügbar: Da die Prozessoren des Raspberry Pi 4 leistungsstark genug sind, um diese in Software zu entschlüsseln, ist keine CODEC-Lizenz erforderlich. Darüber hinaus verfügt der Raspberry Pi 4 auch über Hardware-Unterstützung für die Dekodierung von H265/HEVC.

<a name="pi-audio"></a>

## Audio

<a name="pi-audio-hdmi"></a>
### Wird Ton über HDMI unterstützt?

Jawohl.

<a name="pi-audio-analog"></a>
### Wie sieht es mit Standard-Audio-Ein- und -Ausgang aus?

Es gibt eine Standard-3,5-mm-Buchse für den Audioausgang an einen Verstärker (nicht bei Zero-Modellen). Sie können jedes unterstützte USB-Mikrofon für den Audioeingang hinzufügen oder über die I2S-Schnittstelle einen Codec für zusätzliche Audio-E/A hinzufügen.

<a name="pi-power"></a>

## Leistung

<a name="pi-power-kill"></a>
### Ist es sicher, einfach den Strom zu ziehen?

Nein, nicht wirklich – Sie können Ihre SD-Karte beschädigen, wenn Sie dies tun. Wir empfehlen, den Befehl `sudo halt` oder `sudo shutdown` auszugeben, bevor Sie den Strom ziehen. Dadurch wird sichergestellt, dass alle ausstehenden Dateitransaktionen auf die SD-Karte geschrieben werden und die SD-Karte nicht mehr „aktiv“ ist. Das Ziehen der Stromversorgung während einer SD-Kartentransaktion kann gelegentlich die Karte beschädigen.

<a name="pi-power-events"></a>
### Was ist mit ungeplanten Stromunterbrechungen?

Stromunterbrechungen können bei allen Arten von elektronischen Geräten zu Problemen führen, und der Pi ist nicht anders. Eine Beschädigung der SD-Karte kann verursacht werden, wenn das Gerät einfach ausgeschaltet wird, ohne dass es normal heruntergefahren wird. Dies liegt daran, dass das System zum Zeitpunkt des Stromausfalls möglicherweise auf die SD-Karte schreibt, wodurch das Dateisystem der SD-Karte in einem ungültigen Zustand verbleibt. Wenn Sie also Stromunterbrechungen nicht verhindern können, besteht eine Möglichkeit, das System robuster zu machen, darin, den Schreibvorgang auf die SD-Karte zu begrenzen oder sogar ganz zu stoppen. Sie können Swap deaktivieren, die Protokollierung im RAM aktivieren und systemd-timesyncd deaktivieren, wodurch die Anzahl der Zugriffe auf die SD-Karte erheblich reduziert wird. Sie können auch die gesamte Raspberry Pi OS-Installation schreibgeschützt machen, um jegliches Schreiben auf die Karte zu verhindern. Eine Internetrecherche gibt Hinweise zur Umsetzung dieser Maßnahmen.

<a name="pi-power-specs"></a>
### Was sind die Stromanforderungen?

Das Gerät muss mit einem 5V-Netzteil mit USB-Anschluss versorgt werden; USB-C für den Raspberry Pi 4 und Micro-USB für alle anderen Modelle. Wie viel Strom (mA) der Raspberry Pi genau benötigt, hängt davon ab, welches Modell Sie verwenden und was Sie daran anschließen. Wir empfehlen unsere eigenen Netzteile und verkaufen ein [2.5A (2500mA)](https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/) Netzteil für Modelle bis Pi 3, und eine [3,0A (3000mA) Versorgung](https://www.raspberrypi.org/products/type-c-power-supply/) für den Pi 4. Diese versorgen Sie mit genügend Strom, um Ihren Raspberry Pi zu betreiben die meisten Anwendungen, einschließlich der Verwendung der 4 USB-Ports. USB-Geräte mit sehr hoher Nachfrage erfordern jedoch möglicherweise die Verwendung eines Hubs mit eigener Stromversorgung. In der folgenden Tabelle sind die spezifischen Leistungsanforderungen jedes Modells aufgeführt.

| Produkt | Empfohlene Stromkapazität des Netzteils | Maximale Gesamtstromaufnahme von USB-Peripheriegeräten | Typischer aktiver Stromverbrauch des Bare-Boards |
|-|-|-|-|
| Raspberry-Pi-Modell A | 700mA | 500mA | 200mA |
| Raspberry Pi-Modell B |1,2A | 500mA | 500mA |
| Raspberry Pi Modell A+ | 700mA | 500mA | 180mA
| Raspberry Pi Modell B+ | 1,8A | 1,2A | 330mA |
| Raspberry Pi 2 Modell B | 1,8A | 1,2A | 350mA |
| Raspberry Pi 3 Modell B | 2,5A | 1,2A | 400mA |
| Raspberry Pi 3 Modell A+ | 2,5A | Begrenzt nur durch Netzteil-, Platinen- und Anschlusswerte. | 350mA |
| Raspberry Pi 3 Modell B+ | 2,5A | 1,2A | 500mA |
| Raspberry Pi 4 Modell B | 3,0A | 1,2A | 600mA |
| Raspberry Pi Zero W/WH | 1,2A | Begrenzt nur durch Netzteil-, Platinen- und Anschlusswerte.| 150mA |
| Raspberry Pi Zero | 1,2A | Begrenzt nur durch Netzteil-, Platinen- und Anschlusswerte | 100mA |

Der spezifische Strombedarf jedes Modells hängt vom Anwendungsfall ab: Die Netzteilempfehlungen basieren auf dem **typischen maximalen** Stromverbrauch, der typische Stromverbrauch gilt für jede Platine in einer *Desktop-Computer*-Konfiguration. Die Raspberry Pi Modelle A, A+ und B können maximal 500 mA an nachgeschaltete USB-Peripheriegeräte liefern. Wenn Sie ein Hochleistungs-USB-Gerät anschließen möchten, wird empfohlen, einen USB-Hub mit eigener Stromversorgung an den Raspberry Pi anzuschließen und Ihre Peripheriegeräte an den USB-Hub anzuschließen.

Ab dem Raspberry Pi B+ wird 1,2A an nachgeschaltete USB-Peripheriegeräte geliefert. Damit können die allermeisten USB-Geräte direkt an diese Modelle angeschlossen werden, sofern das vorgeschaltete Netzteil ausreichend Strom zur Verfügung hat.

Geräte mit sehr hoher Stromstärke oder Geräte, die einen Stoßstrom aufnehmen können, wie bestimmte Modems und USB-Festplatten, benötigen dennoch einen externen USB-Hub mit Stromversorgung. Der Strombedarf des Raspberry Pi steigt, wenn Sie die verschiedenen Schnittstellen des Raspberry Pi nutzen. Die GPIO-Pins können sicher 50 mA ziehen (beachten Sie, dass 50 mA auf alle Pins verteilt sind: ein einzelner GPIO-Pin kann nur 16 mA sicher ziehen), der HDMI-Anschluss verwendet 50 mA, das Kameramodul benötigt 250 mA und Tastaturen und Mäuse können so wenig vertragen als 100mA oder so viel wie 1000mA! Überprüfen Sie die Nennleistung der Geräte, die Sie an den Raspberry Pi anschließen möchten, und kaufen Sie ein entsprechendes Netzteil. Wenn Sie sich nicht sicher sind, empfehlen wir Ihnen, einen USB-Hub mit eigener Stromversorgung zu kaufen.

Dies ist die typische Strommenge (in Ampere), die von verschiedenen Raspberry Pi-Modellen während Standardprozessen verbraucht wird:

| | | Raspberry Pi 1B+ | Raspberry Pi 2B | Raspberry Pi 3B | Raspberry Pi Zero | Raspberry Pi 4B |
|-|-|----------|-------|-------------|------------ |-----|
| Booten | Max |0,26 | 0,40| 0,75| 0,20 | 0,85 |
| | Durchschn. | 0,22 | 0,22 | 0,35 | 0,15 | 0,7 |
| Leerlauf |Durchschn. | 0,20 | 0,22 | 0,30 | 0,10 | 0,6 |
| Videowiedergabe (H.264) | Max | 0,30 | 0,36 |0,55 |0,23 | 0,85 |
| | Durchschn. | 0,22 | 0,28 | 0,33 | 0,16 | 0,78 |
| Stress | Max | 0,35 | 0,82 | 1,34 | 0,35 | 1,25 |
| | Durchschn. | 0,32 | 0,75 | 0,85 | 0,23 | 1.2 |
| Strom anhalten | | | | 0,10 | 0,055 | 0,023 |

**Testbedingungen:** Wir haben ein standardmäßiges Raspberry Pi OS-Image (Stand 26. Februar 2016 oder Juni 2019 für den Raspberry Pi 4) bei Raumtemperatur verwendet, wobei der Raspberry Pi an einen HDMI-Monitor, eine USB-Tastatur, und USB-Maus. Der Raspberry Pi 3 Model B wurde mit einem Wireless LAN Access Point verbunden, der Raspberry Pi 4 wurde mit Ethernet verbunden. Alle diese Leistungsmessungen sind ungefähre Angaben und berücksichtigen nicht den Stromverbrauch von zusätzlichen USB-Geräten; Der Stromverbrauch kann diese Werte leicht überschreiten, wenn mehrere zusätzliche USB-Geräte oder ein HAT an den Raspberry Pi angeschlossen werden.

<a name="pi-power-usbhub"></a>
### Kann ich den Raspberry Pi über einen USB-Hub mit Strom versorgen?

Es hängt von dem Hub ab. Einige Hubs entsprechen dem USB 2.0-Standard und bieten nur 500 mA pro Port, was möglicherweise nicht ausreicht, um Ihren Raspberry Pi mit Strom zu versorgen. Andere Hubs betrachten die USB-Standards eher als Richtlinien und liefern von jedem Port so viel Strom, wie Sie möchten. Bitte beachten Sie auch, dass einige Hubs dafür bekannt sind, den Raspberry Pi *zurückzuspeisen*. Dies bedeutet, dass die Hubs den Raspberry Pi über sein USB-Eingangskabel mit Strom versorgen, ohne dass ein separates Micro-USB-Stromkabel erforderlich ist, und den Spannungsschutz umgehen. Wenn Sie einen Hub verwenden, der zum Raspberry Pi *zurückgespeist* wird und der Hub einen Stromstoß erfährt, könnte Ihr Raspberry Pi möglicherweise beschädigt werden.

<a name="pi-power-battbackup"></a>
### Kann ich den Raspberry Pi sowohl über Batterien als auch über eine Steckdose mit Strom versorgen?

Der Betrieb des Raspberry Pi direkt mit Batterien erfordert besondere Sorgfalt und kann zur Beschädigung oder Zerstörung Ihres Raspberry Pi führen. Wenn Sie sich jedoch als fortgeschrittener Benutzer betrachten, können Sie es versuchen. Zum Beispiel würden vier der gängigsten AA-Akkus bei voller Ladung 4,8 V liefern. 4,8 V wären technisch nur im Toleranzbereich des Raspberry Pi, aber das System würde schnell instabil werden, da die Batterien ihre volle Ladung verloren. Umgekehrt führt die Verwendung von vier AA-Alkalibatterien (nicht wiederaufladbar) zu 6 V. 6V liegen außerhalb des akzeptablen Toleranzbereichs und würden Ihren Raspberry Pi möglicherweise beschädigen oder im schlimmsten Fall zerstören. Es ist möglich, konstante 5 V von Batterien bereitzustellen, indem eine Abwärts- und/oder Boost-Schaltung verwendet wird oder indem ein Ladegerät verwendet wird, das speziell entwickelt wurde, um konstante 5 V von einigen Batterien auszugeben; Diese Geräte werden in der Regel als Notladegeräte für Mobiltelefone vermarktet.

<a name="pi-power-ethernet"></a>
### Ist Power over Ethernet möglich?

Ja: Wenn Sie ein 3B+ oder 4B besitzen, können Sie unseren [offiziellen Raspberry Pi PoE HAT](https://www.raspberrypi.org/products/poe-hat/) verwenden, um diesen Raspberry Pi über Ethernet mit Strom zu versorgen. Für andere Modelle gibt es Adapter, die die Spannung von der Ethernet-Leitung abtrennen, bevor sie an den Raspberry Pi angeschlossen werden. Wir haben jedoch keine PoE-Lösungen von Drittanbietern getestet, daher können wir keine davon empfehlen.

<a name="pi-power-gpioout"></a>
### Welche Spannungsgeräte kann ich an die GPIO-Pins anschließen und wie viel Strom kann ich ziehen?

Die GPIO-Pins sind nativ 3,3 V, daher MÜSSEN 5-V-Geräte ** NICHT ** direkt ohne irgendeine Art von Spannungsumwandlung angeschlossen werden. Die Pins können bis zu 16mA Strom liefern. Weitere Informationen finden Sie auf der [GPIO-Dokumentenseite](../hardware/raspberrypi/gpio/README.md).

<a name="sd-cards"></a>

## SD-Karten und Speicher

<a name="sd-cards-need"></a>
### Welche SD-Kartengröße benötige ich?

Ob Sie das [offizielle Raspberry Pi OS Betriebssystem](https://www.raspberrypi.org/downloads/raspbian/) (oder den [NOOBS Installer für Raspberry Pi OS](https://www.raspberrypi.org/downloads/noobs/) oder ein anderes eigenständiges Betriebssystem-Image (siehe [empfohlenes Betriebssystem von Drittanbietern](https://www.raspberrypi.org/downloads/)), **die von uns empfohlene SD-Karte mit minimaler Größe ist 8 GB**. Dies gibt Ihnen den freien Speicherplatz, den Sie benötigen, um zusätzliche Pakete zu installieren oder eigene Programme zu erstellen. Die ursprünglichen Raspberry Pi Modell A und Modell B erfordern SD-Karten in voller Größe. Das neuere Raspberry Pi 1 Modell A+, Modell 1 B+, 2B, 3B, 3B+, 3A+, 4B, Zero, Zero W und Zero WH erfordern microSD-Karten.

<a name="sd-cards-want"></a>
### Welche SD-Kartengröße wird unterstützt?

Während das empfohlene Minimum von 8 GB für die meisten Leute ausreichen sollte, haben wir Karten mit bis zu 128 GB ausprobiert und die meisten Karten scheinen in Ordnung zu sein. Sie können auch einen USB-Stick oder eine USB-Festplatte anschließen, um zusätzlichen Speicherplatz bereitzustellen.

<a name="sd-cards-usbboot"></a>
### Kann ich einen Raspberry Pi von einer USB-angeschlossenen Festplatte statt von der SD-Karte booten?

USB-Boot ist nur auf dem Raspberry Pi 2B v1.2, 3B, 3B+ und 3A+ möglich. Das Booten von einem USB-angeschlossenen Laufwerk (entweder einer SSD oder einer tatsächlichen Festplatte) kann den Raspberry Pi booten und schneller arbeiten lassen. Eine ausführliche Anleitung dazu haben wir [hier](../hardware/raspberrypi/bootmodes/msd.md).

<a name="sd-cards-recovery"></a>
### Was passiert, wenn ich das Gerät blockiere?

Wenn Sie das Gerät blockieren, können Sie es wiederherstellen, indem Sie die SD-Karte neu flashen.

<a name="networking"></a>

## Netzwerk und drahtlose Konnektivität

<a name="networking-support"></a>
### Unterstützt das Gerät das Netzwerken?

Die Versionen Raspberry Pi 1 Model B und B+, Raspberry Pi 2 und Raspberry Pi 3 Model B des Geräts verfügen über ein integriertes 10/100 verkabeltes Ethernet. Der Raspberry Pi 3B+ und Raspberry Pi 4 verfügen über 1000BaseT verkabeltes Ethernet, aber beim 3B+ ist der Durchsatz durch die USB 2.0-Verbindung zum SoC begrenzt. Beim Raspberry Pi 1 Model A und A+ und beim Raspberry Pi Zero/Zero W gibt es kein Ethernet.

<a name="networking-builtinwifi"></a>
### Gibt es ein integriertes drahtloses Netzwerk?

Die Raspberry Pi 3, 3+, 4 und Raspberry Pi Zero W verfügen über eine integrierte WLAN-Konnektivität. Sie können jedem Raspberry Pi-Modell auch einen USB-WLAN-Dongle hinzufügen.

Die Raspberry Pi-Modelle 3B+ und 4B unterstützen 802.11ac und alle früheren Modelle unterstützen bis zu 802.11n.

<a name="networking-builtinbt"></a>
### Ist Bluetooth integriert?

Ja, der Raspberry Pi 3, Raspberry Pi 4 und Raspberry Pi Zero W verfügen über integriertes Bluetooth.

<a name="networking-gigaperf"></a>
### Ich scheine auf meinem Raspberry Pi 3B+ kein Gigabit-Netzwerk mit voller Geschwindigkeit zu erhalten.

Obwohl der Ethernet-Chip des Raspberry Pi 3B+ Gigabit-fähig ist, erfolgt die Verbindung vom Chip zum SoC weiterhin über USB 2.0, was die verfügbare Gesamtbandbreite in der realen Welt auf ca. 220–250 Mbit/s begrenzt. Obwohl es kein Gigabit ist, ist dies ein gesunder Anstieg über die 100 Mbit/s-Höchstgeschwindigkeit des 3B-Modells. Um die beste Leistung zu erzielen, sollten Sie sicherstellen, dass die Ethernet-Flusssteuerung an Ihrem Router eingeschaltet ist.

<a name="networking-netboot"></a>
### Unterstützt das Gerät jede Form von Netbooting oder PXE?

Der Raspberry Pi 3 kann für den Netzwerkstart ohne vorhandene SD-Karte eingerichtet werden; frühere Modelle können PXE/Netboot mit einer entsprechend eingerichteten SD-Karte. Unsere Netbooting-Dokumentation finden Sie [hier](../hardware/raspberrypi/bootmodes/net.md).

Der Raspberry Pi 4 unterstützt derzeit kein Booten über das Netzwerk ohne eine vorhandene SD-Karte. Ein Bootloader-Update zur Unterstützung des Netzwerkbootens ist geplant, aber noch nicht verfügbar.

Wir haben auch [PiServer](https://www.raspberrypi.org/blog/piserver/) entwickelt, eine Software, mit der Sie ganz einfach ein Netzwerk von Client-Raspberry Pis einrichten können, die über Ethernet mit einem einzelnen x86-basierten Server verbunden sind . Mit PiServer benötigen Sie keine SD-Karten, Sie können alle Clients über den Server steuern und Benutzerkonten hinzufügen und konfigurieren – ideal für den Unterricht, Ihr Zuhause oder eine industrielle Umgebung.

Eine weitere Option ist [PiNet](http://pinet.org.uk), ein kostenloses Open-Source-Community-basiertes Projekt, das ursprünglich für Schulen entwickelt wurde. Jeder Raspberry Pi bootet einen kleinen Satz Startup-Dateien auf einer SD-Karte und ruft den Rest der benötigten Daten vom PiNet-Server ab, sodass Sie ein einziges Betriebssystem-Image für alle Raspberry Pis verwalten können. PiNet fügt auch Netzwerkbenutzerkonten, freigegebene Ordner und automatisierte Backups hinzu.

<a name="cameramodule"></a>

## Kameramodul

<a name="cameramodule-what"></a>
### Was ist das Kameramodul?

Die Raspberry Pi Kameramodule sind kleine PCBs, die mit einem kurzen Flachbandkabel an den CSI-2-Kameraanschluss des Raspberry Pi angeschlossen werden. Sie bieten Anschlussmöglichkeiten für eine Kamera, die Standbilder oder Videoaufnahmen aufnehmen kann. Die Kameramodule verbinden sich mit der Image System Pipeline (ISP) im SoC des Raspberry Pi, wo die eingehenden Kameradaten verarbeitet und schließlich in ein Bild oder Video auf der SD-Karte (oder einem anderen Speicher) umgewandelt werden. Sie können [hier mehr über die Kameramodule lesen](https://www.raspberrypi.org/products/).

<a name="cameramodule-model"></a>
### Welches Kameramodell verwendet das Kameramodul?

Das ursprüngliche Kameramodul ist ein Omnivision OV5647, das V2, das es ersetzt hat, ist ein Sony IMX219. Es ist jetzt ein hochwertiges Kameramodul mit austauschbarem Objektivanschluss erhältlich, das den Sony IMX447-Sensor verwendet.

<a name="cameramodule-res"></a>
### Welche Auflösungen werden unterstützt?

Das ursprüngliche Kameramodul kann Fotos mit bis zu 5 Megapixeln aufnehmen und Videos mit Auflösungen von bis zu 1080p30 aufnehmen. Das Kameramodul V2 kann Fotos mit bis zu 8 Megapixel (8MP) aufnehmen. Es unterstützt die Videomodi 1080p30, 720p60 und VGA90 sowie Standbildaufnahmen. Das High Quality-Kameramodul hat 12,3 MP und die gleiche Videoleistung wie die anderen Modelle.

<a name="cameramodule-format"></a>
### Welche Bildformate werden unterstützt?

Die Raspberry Pi Kameramodule unterstützen Raw-Capturing (Bayer-Daten direkt vom Sensor) oder Encoding als JPEG, PNG, GIF und BMP, unkomprimiertes YUV und unkomprimierte RGB-Fotos. Sie können Videos als H.264-, Baseline-, Haupt- und High-Profile-Formate aufnehmen.

<a name="cameramodule-how"></a>
### Wie verwende ich das Kameramodul?

Dafür haben wir [eine Anleitung](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera) zusammengestellt!

Es gibt auch eine Reihe von Befehlszeilenanwendungen für Standbilder und Videoausgabe. Diese Anwendungen bieten die typischen Funktionen einer Kompaktkamera, wie z. B. die Einstellung von Bildgröße, Komprimierungsqualität, Belichtungsmodus und ISO. Weitere Informationen finden Sie in der [Dokumentation](https://www.raspberrypi.org/documentation/hardware/camera/README.md).

<a name="cameramodule-cable"></a>
### Kann ich das Flachbandkabel verlängern?

Jawohl. Wir haben Berichte von Leuten, die Kabel mit einer Länge von bis zu vier Metern verwenden und immer noch akzeptable Bilder erhalten, obwohl Ihre Erfahrungen davon abweichen können.

<a name="cameramodule-power"></a>
### Wie viel Strom verbraucht das Kameramodul?

Die Raspberry Pi Kameramodule benötigen zum Betrieb 250 mA. Stellen Sie sicher, dass Ihr Netzteil ausreichend Strom für das angeschlossene Kameramodul sowie für den Raspberry Pi selbst und direkt daran angeschlossene Peripheriegeräte liefern kann.

<a name="troubleshoot"></a>

## Fehlerbehebung

<a name="troubleshoot-defpasswd"></a>
### Wie lautet der Benutzername und das Passwort für den Raspberry Pi?

Der Standardbenutzername für Raspberry Pi OS ist `pi` (ohne Anführungszeichen) und das Standardpasswort ist `raspberry` (ohne Anführungszeichen). Wenn dies nicht funktioniert, überprüfen Sie die Informationen zu Ihrer spezifischen Distribution auf der [Downloadseite](https://www.raspberrypi.org/downloads).

<a name="troubleshoot-inputpasswd"></a>
### Warum passiert nichts, wenn ich mein Passwort eingebe?

Um Ihre Informationen zu schützen, zeigt Linux nichts an, wenn Sie Passwörter in der Bash-Eingabeaufforderung oder im Terminal eingeben. Solange Sie den eingegebenen Benutzernamen sehen konnten, funktioniert Ihre Tastatur ordnungsgemäß.

<a name="troubleshoot-boot"></a>
### Warum startet/bootet mein Raspberry Pi nicht?

Die wohl am häufigsten gestellte Frage! Wir haben eine vollständige Anleitung zum Einrichten Ihres Raspberry Pi [hier](../setup/), aber wenn er immer noch nicht bootet, finden Sie im [Troubleshooting-Beitrag in unserem Forum](https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=58151).

<a name="troubleshoot-temp"></a>
### Warum ist mein Raspberry Pi heiß?

Die gesamte Elektronik gibt Wärme ab, und der Raspberry Pi ist keine Ausnahme. Der Raspberry Pi 3 Model B+ verfügt über eine Wärmeverteilungstechnologie, um die gesamte Leiterplatte und die Anschlüsse als Kühlkörper zu verwenden, um überschüssige Energie abzuleiten. Dies bedeutet, dass Sie außer in Ausnahmefällen wahrscheinlich keinen Kühlkörper auf dem SoC oder dem Ethernet-Hub-Chip benötigen.

Der Raspberry Pi 4 Model B verwendet die gleiche Heat-Spreading-Technologie, ist aber aufgrund der viel leistungsstärkeren CPU-Kerne in der Lage, einen höheren Spitzenstromverbrauch als ein Model 3B+ zu erzielen. Unter einer kontinuierlich hohen Prozessorauslastung drosselt das Model 4B eher als ein Model 3B+.

Sie können einen Kühlkörper hinzufügen, wenn Sie möchten, und dies kann thermisches Throttling verhindern, indem die Chips unter der Throttling-Temperatur gehalten werden (siehe Abschnitt Taktrate im Abschnitt [Performance](#pi-performance)).

<a name="troubleshoot-power"></a>
### Ich erhalte immer wieder ein Blitzsymbol und Nachrichten zum Thema Strom.

Die meisten Raspberry Pi-Modelle verfügen über eine Schaltung, um Einbrüche der eingehenden Stromversorgungsspannung unter etwa 4,65 V zu erkennen. In einem solchen Fall erscheint das Blitz-Warnsymbol (siehe [hier](../configuration/warning-icons.md)) und eine Nachricht wird an das Systemprotokoll gesendet. Unterhalb dieser Spannung gibt es keine Garantie, dass der Raspberry Pi richtig funktioniert; Dies kann dazu führen, dass das Gerät blockiert oder die SD-Karte schlecht schreibt, ein USB-Gerät ausfällt, das Ethernet ausfällt usw. Wir empfehlen ein hochwertiges 5V-Netzteil, 2,5A für den Raspberry Pi 3B+, mit einem dicken Kupferkabel. wie [unser offizielles Netzteil](https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/). Das Kabel selbst kann sehr wichtig sein: Oft verwenden die billigeren Kabel sehr dünne Kupferdrähte, die einen erheblichen Spannungsabfall verursachen können.

<a name="troubleshoot-sd"></a>
### Meine SD-Karte scheint nicht mehr zu funktionieren.

SD-Karten haben aufgrund ihrer Funktionsweise eine begrenzte Lebensdauer. In den meisten Fällen bieten sie eine Nutzungsdauer von einigen Jahren, aber ein starker Dateizugriff oder die Verwendung als Swap-Laufwerk kann die Lebensdauer der SD-Karte erheblich verkürzen. Beachten Sie, dass auch SD-Karten mit gefälschter Kapazität verkauft werden, die wahrscheinlich unzuverlässig sind.

<a name="troubleshoot-fs"></a>
### Ich habe eine SD-Karte mit Raspberry Pi OS/NOOBS erstellt, aber wenn ich sie mit meinem Windows-PC betrachte, ist nicht alles da!

Dies hat mit den Fähigkeiten von Windows zu tun, Linux-formatierte Partitionen zu lesen. Wenn Sie die SD-Karte abbilden, wird sie automatisch in mehrere Partitionen aufgeteilt. Die erste Partition verwendet ein Format, das Windows lesen kann, aber die anderen Partitionen verwenden ein Linux-spezifisches Dateisystem, das Windows einfach nicht erkennt. Das bedeutet, wenn Sie eine SD-Karte in einen Windows-Rechner einlegen, wird nur die erste Partition angezeigt, und es kann durchaus sein, dass die anderen Partitionen beschädigt sind und formatiert werden müssen - **nicht formatieren**! Hier sind einige Informationen darüber, was in dieser ersten [Partition](../configuration/boot_folder.md) enthalten ist. Wenn Sie die SD-Karte in einen Computer mit Linux einlegen, werden alle Partitionen korrekt angezeigt.

## Ich habe noch weitere Fragen!

Lesen Sie die wichtigen Themen im [Unterforum für Anfänger](https://www.raspberrypi.org/forums/viewforum.php?f=91) und überprüfen Sie die [Hilfeseiten](https://www.raspberrypi.org/help/) für mehr Informationen. Wenn die Antwort nicht dabei ist, frag in [den Foren](https://www.raspberrypi.org/forums), wo es viele hilfreiche Raspberry Pi-Besitzer, -Benutzer und -Fans gibt, die dir gerne weiterhelfen aus.


