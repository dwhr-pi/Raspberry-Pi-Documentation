# Glossar der Raspberry Pi-Dokumentation

- **A/mA** - Ampere/Milliampere, die [Basiseinheit des elektrischen Stroms](https://en.wikipedia.org/wiki/Ampere).
- **API** - [Application Programming Interface](https://en.wikipedia.org/wiki/Application_programming_interface), eine Reihe von Unterprogrammdefinitionen, Kommunikationsprotokollen und Tools zum Erstellen von Software.
- **Arm** - Eine [Reihe von Befehlssatzarchitekturen und Prozessor-Mikroarchitekturen](https://en.wikipedia.org/wiki/Arm_architecture), die hauptsächlich von Arm Holdings mit Sitz in Cambridge, Großbritannien, entwickelt wurden.
- **Armv6** - Die Befehlssatzarchitektur, die auf dem ersten Raspberry Pi (Pi 1) und der Pi Zero-Reihe verwendet wird.
- **Armv7** - Die Befehlssatzarchitektur, die auf der Raspberry Pi 2-Reihe verwendet wird.
- **Armv8** - Die Befehlssatzarchitektur, die auf der Raspberry Pi 3-Reihe verwendet wird; sehr ähnlich zu Armv7 im 32-Bit-Modus.
- **balenaEtcher** - Ein grafisches Tool zum [Programmieren von SD-Karten](../installation/installing-images/README.md) bereit zur Verwendung auf unseren Geräten; verfügbar für Linux, Windows und Mac. Jetzt abgelöst von Raspberry Pis eigenem Imaging-Tool, das [hier] (https://www.raspberrypi.org/downloads/) heruntergeladen werden kann.
- **Bare Metal** - Programmierung des Raspberry Pi ohne Verwendung eines Betriebssystems.
- **BCM2835** - Der SoC, der auf dem Raspberry Pi 1, Compute Module 1 und Raspberry Pi Zero verwendet wird; siehe unsere [offizielle Dokumentation](../hardware/raspberrypi/bcm2835/README.md).
- **BCM2836** - Der SoC, der auf dem ursprünglichen Raspberry Pi 2 verwendet wurde; siehe unsere [offizielle Dokumentation](../hardware/raspberrypi/bcm2836/README.md).
- **BCM2837** - Der SoC, der auf dem Raspberry Pi 3, Compute Module 3 und Raspberry Pi 2 Version 1.2 verwendet wird; siehe unsere [offizielle Dokumentation](../hardware/raspberrypi/bcm2837/README.md).
- **BCM2711** - Der auf dem Raspberry Pi 4B verwendete SoC; siehe unsere [offizielle Dokumentation](../hardware/raspberrypi/bcm2711/README.md).
- **BLE/BTLE** - Bluetooth Low Energy, eine stromsparende Version des drahtlosen Bluetooth-Kommunikationsprotokolls.
- **BT** - Bluetooth, ein drahtloses Kommunikationsprotokoll mit geringer Reichweite und geringer Bandbreite, das häufig für mobile Geräte verwendet wird.
- **CODEC** - Coder/Decoder, Hardware- oder Softwareblöcke, die Video- oder Audiodaten codieren und/oder decodieren.
- **Compute Module** - Eine [Pi-Variante für gewerbliche Kunden](../hardware/computemodule/README.md); verwendet einen anderen Formfaktor, was die Verwendung in anderen Produkten erleichtert.
- **`config.txt`** - Eine Datei auf der Boot-Partition der SD-Karte eines Raspberry Pi, die beim Booten analysiert wird, um Betriebssystemfunktionen zu aktivieren, zu deaktivieren und zu verwalten, die nach dem Booten nicht geändert werden können; siehe unsere [offizielle Dokumentation](../configuration/config-txt/README.md).
- **CSI** - Camera Serial Interface, eine Hardwareschnittstelle zum Anschluss von Kameras an SoCs.
- **DHCP** - Dynamic Host Configuration Protocol, ein Netzwerkverwaltungsprotokoll, das jedem Gerät in einem Netzwerk dynamisch eine IP-Adresse und andere Netzwerkkonfigurationsparameter zuweist.
- **DPI** - Parallel Display Interface, eine bis zu 24-Bit parallele RGB-Schnittstelle über die GPIO-Pins des Raspberry Pi.
- **DSI** - Display Serial Interface, eine Hardware-Schnittstelle zum Anschluss von LCD-Panels.
- **Device Tree (DT)** - Eine [Datenstruktur zum Definieren von Hardware](https://en.wikipedia.org/wiki/Device_tree).
- **Gerätebaumquelle (DTS)** - Eine menschenlesbare Quelldatei, die in einen DTB kompiliert werden soll.
- **Gerätebaum-Blob (DTB)** - Eine Binärdatei, die einen Gerätebaum enthält; das Ergebnis der Kompilierung von DTS-Dateien.
- **`dt-blob.bin`** - Eine Binärdatei mit der anfänglichen GPIO-Konfiguration, die von der Firmware beim Booten gelesen wird; trotz des Namens ist dies __nicht__ Device Tree Blob (obwohl es von den Device Tree Tools kompiliert wird).
- **DVI** - [Digital Visual Interface](https://en.wikipedia.org/wiki/Digital_Visual_Interface), eine Videoschnittstelle zum Anschließen eines Quellgeräts an einen Monitor oder ein Display; elektrisch kompatibel mit HDMI, so dass ein einfacher Adapter von einem zum anderen konvertieren kann (außer Audio, das DVI nicht unterstützt).
- **Etcher** - Siehe balenaEtcher.
- **FAQ** - Häufig gestellte Fragen! Siehe unsere [FAQ](../faqs/).
- **Firmware** - Software, die auf einem bestimmten Hardwaregerät ausgeführt wird, zum Beispiel einem drahtlosen Chip oder Dongle; unterscheidet sich von der üblichen Software, die auf dem Raspberry Pi läuft, dadurch, dass es sich normalerweise um einen festen binären Datenblock handelt, der beim Start auf das Gerät geladen wird; Auf dem Raspberry Pi muss die VC4-GPU beim Start mit Firmware geladen werden, damit das gesamte System gestartet und ausgeführt werden kann.
- **GNU** - Das [GNU Project](https://en.wikipedia.org/wiki/GNU_Project) ist ein Massenkollaborationsprojekt für freie Software, das riesige Mengen an Software produziert hat, die auf dem Raspberry Pi verwendet wird. einschließlich Linux selbst.
- **GPIO** - General Purpose Input/Output, die programmierbaren Pins auf dem Raspberry Pi.
- **GPU** - Graphical Processing Unit, ein Hardwaregerät zur Verarbeitung grafischer (und verwandter) Aufgaben mit hoher Geschwindigkeit; auf der Raspberry-Pi-Reihe bis hin zum Pi 3B+ heißt dies VideoCore4 (VC4) und ist im SoC integriert. Auf dem Pi 4 kommt eine neuere und schnellere Version namens VideoCore6 (VC6) zum Einsatz.
- **HAT** - Hardware Attached on Top, eine Spezifikation zum Entwerfen von Geräten zum Anschließen an den Raspberry Pi; siehe [Startankündigung](https://www.raspberrypi.org/blog/introducing-raspberry-pi-hats/).
- **HDMI** - [High-Definition Multimedia Interface](https://en.wikipedia.org/wiki/HDMI), eine Standardschnittstelle zum Übertragen von unkomprimiertem Video und komprimierten oder unkomprimierten Audiodaten von einem Quellgerät an ein Display Gerät.
- **HDCP** - [High-bandwidth Digital Content Protection](https://en.wikipedia.org/wiki/High-bandwidth_Digital_Content_Protection), ein optionaler Verschlüsselungsmechanismus für die HDMI- oder DVI-Übertragung.
- **Headless** - Ein kopfloser Raspberry Pi ist ein Raspberry Pi, bei dem kein Anzeigegerät, keine Tastatur oder Maus angeschlossen ist. Der Zugriff erfolgt normalerweise über eine Netzwerkverbindung mit einem Fernzugriffsprotokoll wie SSH oder VNC.
- **HVS** - Hardware Video Scaler, ein Hardwareblock in der VC4- oder VC6-GPU auf allen Raspberry Pi-Modellen, der zum Bearbeiten und Anzeigen von Bitmaps verwendet wird.
- **I<sup>2</sup>C/I2C/IIC** - [Inter-Integrated Circuit](https://en.wikipedia.org/wiki/I%C2%B2C) (ausgesprochen _I-squared -C_), ein elektrisches Protokoll, das zum Anschließen von Peripherie-ICs mit niedrigerer Geschwindigkeit an Prozessoren und Mikrocontroller in der Kurzstrecken-Intra-Board-Kommunikation verwendet wird.
- **I<sup>2</sup>S** - [Inter-Integrated Circuit Sound](https://en.wikipedia.org/wiki/I%C2%B2S), ein verwendeter elektrischer serieller Busschnittstellenstandard um digitale Audiogeräte miteinander zu verbinden.
- **ISP** - Imaging System Pipeline, eine Reihe von Hardware- (und manchmal Software-)Stufen, die Bilder von einer Kamera zu einem hochwertigen Ergebnis verarbeiten; Der Raspberry Pi verfügt über einen integrierten Hardware-ISP, der Bilder verarbeitet, die von einer an den CSI-Port angeschlossenen Kamera aufgenommen wurden.
- **ISP** - Internetdienstanbieter.
- **LAN** - Local Area Network, ein Ethernet- oder Wireless-basiertes Kommunikationsnetzwerk in einem lokalisierten Bereich, beispielsweise einem Haus oder Büro.
- **LED** - Light-Emitting Diode, ein Halbleiterbauelement, das mithilfe von Elektrizität Licht erzeugt.
- **Linux** - Das Hauptbetriebssystem des Raspberry Pi; die spezielle maßgeschneiderte Distribution für die Raspberry Pi-Reihe heißt Raspberry Pi OS; Dritte haben auch eigene Distributionen produziert.
- **MSD** - Massenspeichergerät (unter anderem), wie eine SD-Karte oder ein Festplattenlaufwerk.
- **MQTT** - MQTT steht für MQ Telemetry Transport, ein leichtes und einfaches Machine-to-Machine-Protokoll, das häufig für IoT-Geräte verwendet wird; siehe [die MQTT-Website](http://mqtt.org/).
- **NAND** - Ein [NAND-Gatter](https://en.wikipedia.org/wiki/NAND_Gate) ist ein Logikgatter, insbesondere ein invertiertes UND-Gatter; kann sich auch auf einen Flash-Speichertyp beziehen, der NAND-Logik verwendet.
- **NOOBS** - Neue Out-of-Box-Software, einfache [Installation des Betriebssystem-Images](../installation/noobs.md) für die Raspberry Pi-Reihe; **Hinweis:** NOOBS wird nicht mehr vom Raspberry Pi-Team entwickelt, siehe stattdessen PINN.
- **OTP** - Einmal programmierbar, ein Speichertyp, der nur einmal programmiert werden kann und die programmierten Daten nach dem Ausschalten behält; verwendet für Seriennummern etc.
- **Overlay** - Ein Bootzeit- oder Laufzeit-Patch für einen Gerätebaum, der ein gewisses Maß an Konfiguration durch den Benutzer ermöglicht; Weitere Informationen finden Sie in unserer [offiziellen Dokumentation](../configuration/device-tree.md) oder führen Sie `dtoverlay -a` in einem Terminalfenster aus, um eine Liste der verfügbaren Overlays zu erhalten.
- **Pink Pony/Unicorn** - Eine Funktionsanfrage für ein zukünftiges Raspberry Pi-Modell, die wahrscheinlich nicht passieren wird.
- **PINN** - Eine [Drittanbieter-Entwicklung](https://github.com/procount/pinn) des NOOBS-Systems, oft eine bessere Option als das offizielle NOOBS.
- **PoE** - Power over Ethernet, ein Mechanismus, um ein Gerät über seine Ethernet-Verbindung mit Strom zu versorgen; wir produzieren einen [PoE HAT](https://www.raspberrypi.org/products/poe-hat) für den Raspberry Pi 3 Model B+ und Raspberry Pi 4 Model B.
- **Polyfuse** - Eine selbstrückstellende elektrische Sicherung, die bei einigen Pi-Modellen zum Schutz vor Überstrom verwendet wird; Das Zurücksetzen kann Stunden oder sogar Tage dauern.
- **`raspi-config`** - Ein Befehlszeilentool zum Konfigurieren der Betriebssystemfunktionen von Raspberry Pi OS; siehe unsere [offizielle Dokumentation](../configuration/raspi-config.md).
- **Raspberry Pi Imager** - Das offizielle grafische Tool zum Herunterladen und Installieren von Betriebssystem-Images auf eine SD-Karte. Laden Sie es [hier] herunter (https://www.raspberrypi.org/downloads/).
- **Raspbian** - Ein armhf-Build der Debian-Quell-Repositorys.
- **Raspberry Pi OS (32bit)** - Eine Distribution der Raspbian-Repositorys, die auf die gesamte Raspberry Pi-Reihe ausgerichtet ist.
- **Raspberry Pi OS (64bit)** - Eine Distribution der Debian-Repositorys, die auf die Modelle Pi 3 und Pi 4 ausgerichtet ist.
- **Raspberry Pi Configuration Tool (`rcgui`)** - Ein grafisches Äquivalent zu `raspi-config`.
- **Raspberry Pi Desktop** - Eine speziell auf den Raspberry Pi zugeschnittene Version der LXDE-Desktop-Umgebung (früher PIXEL genannt).
- **raspivid, raspistill, raspiyuv, raspividyuv** - Eine Reihe von Anwendungen zum Ausführen des Raspberry Pi-Kameramoduls in verschiedenen Modi; siehe unsere [offizielle Dokumentation](../raspbian/applications/camera.md).
- **RPF** - Raspberry Pi Foundation, die ursprüngliche, im Vereinigten Königreich registrierte Wohltätigkeitsorganisation (1129409), die den Raspberry Pi auf den Markt gebracht hat.
- **RPT/RPF(T)** - Raspberry Pi Trading, eine hundertprozentige kommerzielle Tochtergesellschaft des RPF, die alle Raspberry Pi-Produkte, einschließlich Zeitschriften, entwirft und entwickelt; Alle Gewinne aus dem RPT gehen an das RPF zur Verwendung in ihren Bildungsprogrammen.
- **rpi-update** - Ein Programm, das Sie auf einem Raspberry Pi ausführen können, um die neueste Test-Firmware und den Linux-Kernel herunterzuladen; Es gibt keine Garantie dafür, dass die neueste Version korrekt funktioniert, daher sollten Sie rpi-update nur ausführen, wenn es von einem Raspberry Pi-Ingenieur empfohlen wird.
- **SoC** - System on a Chip, ein integrierter Schaltkreis, der alle Komponenten eines Computers umfasst.
- **SPI** - Serial Peripheral Interface Bus, eine synchrone serielle Kommunikationsschnittstellenspezifikation, die für die Kommunikation über kurze Distanzen verwendet wird.
- **SSH** - Secure Shell, ein kryptografisches Netzwerkprotokoll zum sicheren Betrieb von Netzwerkdiensten über ein ungesichertes Netzwerk; siehe unsere [offizielle Dokumentation](../remote-access/ssh/README.md) oder [den Wikipedia-Artikel zu SSH](https://en.wikipedia.org/wiki/Secure_Shell).
- **TLA** - Drei-Buchstaben-Abkürzung, fast der gesamte Grund für dieses Glossar.
- **TLS** - [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security), ein kryptografisches Protokoll, das zur Bereitstellung von Sicherheit über ein Computernetzwerk verwendet wird und häufig verwendet wird, um andere Protokolle wie MQTT abzusichern.
- **TP1/TP2** - Testpunkte.
- **UART** - Universal Asynchronous Receiver-Transmitter, ein Protokoll für die asynchrone serielle Kommunikation, bei dem das Datenformat und die Übertragungsgeschwindigkeiten konfigurierbar sind.
- **USB** - [Universal Serial Bus](https://en.wikipedia.org/wiki/USB), die wichtigsten Raspberry Pi-Modelle verfügen über vier USB-Buchsen zum Anschluss von USB-Geräten, zum Beispiel Mäusen oder Tastaturen.
- **VC4, VC6** - Die auf dem Raspberry Pi verwendete VideoCore4- oder VideoCore6-GPU; enthält eine große Anzahl von Hardwareblöcken, die Grafiken, Kameras, Display, CODECs usw. verarbeiten.
- **VNC** - Ein grafisches Desktop-Sharing-System, das das Remote Frame Buffer Protocol (RFB) verwendet, um einen anderen Computer fernzusteuern; siehe unsere [offizielle Dokumentation](../remote-access/vnc/README.md).
- **Volt (V)** - SI-abgeleitete Einheit für [elektrische Potentialdifferenz](https://en.wikipedia.org/wiki/Volt).
- **`vcgencmd`** - Ein Raspberry Pi-spezifisches Tool zur Kommunikation mit der VideoCore-GPU.
- **Watt** - Ein Maß für [Leistung](https://en.wikipedia.org/wiki/Watt), das häufig bei der Angabe von Netzteilen verwendet wird.