# Der Boot-Ordner

Bei einer einfachen Installation von [Raspberry Pi OS](../raspbian/README.md) werden die Bootdateien auf der ersten Partition der SD-Karte gespeichert, die mit dem FAT-Dateisystem formatiert ist. Dies bedeutet, dass es auf Windows-, macOS- und Linux-Geräten gelesen werden kann.

Wenn der Raspberry Pi eingeschaltet ist, lädt er verschiedene Dateien von der Boot-Partition/-Ordner, um die verschiedenen Prozessoren zu starten, dann bootet er den Linux-Kernel.

Nachdem Linux gebootet hat, wird die Bootpartition als `/boot` gemountet.

## Inhalt des Boot-Ordners

### bootcode.bin

Dies ist der Bootloader, der vom SoC beim Booten geladen wird, einige sehr grundlegende Einstellungen vornimmt und dann eine der `start*.elf`-Dateien lädt. `bootcode.bin` wird auf dem Raspberry Pi 4 nicht verwendet, da es durch Bootcode im [onboard EEPROM](../hardware/raspberrypi/booteeprom.md) ersetzt wurde.

### start.elf, start_x.elf, start_db.elf, start_cd.elf, start4.elf, start4x.elf, start4cd.elf, start4db.elf

Das sind binäre Blobs (Firmware), die auf den VideoCore im SoC geladen werden, die dann den Bootvorgang übernehmen.
`start.elf` ist die Basis-Firmware, `start_x.elf` beinhaltet Kameratreiber und Codec, `start_db.elf` ist eine Debug-Version der Firmware und `start_cd.elf` ist eine abgespeckte Version ohne unterstützende Hardware Blöcke wie Codecs und 3D und zur Verwendung, wenn `gpu_mem=16` in `config.txt` angegeben ist. Weitere Informationen zu deren Verwendung finden Sie in [the `config.txt` section](./config-txt/boot.md).

`start4.elf`, `start4x.elf`, `start4cd.elf` und `start4db.elf` sind spezielle Firmware-Dateien für den Pi 4.

###fixup*.dat

Dies sind Linker-Dateien, die paarweise mit den `start*.elf`-Dateien übereinstimmen, die im vorherigen Abschnitt aufgelistet wurden.

###cmdline.txt

Die Kernel-Befehlszeile wird beim Booten an den Kernel übergeben.

### config.txt

Enthält viele Konfigurationsparameter zum Einrichten des Pi. Siehe [den Abschnitt `config.txt`](./config-txt/README.md).

### Problem.txt

Einige textbasierte Housekeeping-Informationen, die das Datum und die Git-Commit-ID der Verteilung enthalten.

### ssh oder ssh.txt

Wenn diese Datei vorhanden ist, wird SSH beim Booten aktiviert. Der Inhalt spielt keine Rolle, er kann leer sein. SSH ist ansonsten standardmäßig deaktiviert.

### wpa_supplicant.conf

Dies ist die Datei zum Konfigurieren der drahtlosen Netzwerkeinstellungen (sofern die Hardware dazu in der Lage ist). Passen Sie den Ländercode und den Netzwerkteil an Ihren Fall an. Weitere Informationen zur Verwendung dieser Datei finden Sie in [the `wireless/headless` section](./wireless/headless.md).

###Gerätebaumdateien

Es gibt verschiedene Gerätebaum-Blobdateien, die die Erweiterung `.dtb` haben. Diese enthalten die Hardwaredefinitionen der verschiedenen Raspberry Pi-Modelle und werden beim Booten verwendet, um den Kernel einzurichten, nach dem das Pi-Modell erkannt wird. Mehr [Details hier](device-tree.md#part3.1).

###Kernel-Dateien

Der Boot-Ordner enthält verschiedene [kernel](../linux/kernel/README.md) Image-Dateien, die für die verschiedenen Raspberry Pi-Modelle verwendet werden:

| Dateiname | Prozessor | Raspberry Pi-Modell | Anmerkungen |
| ---------| ----------|-----------------|-------|
| Kernel.img | BCM2835 | Pi Null, Pi 1 | |
| kernel7.img| BCM2836, BCM2837 | Pi 2, Pi 3 | Später verwendet Pi 2 den BCM2837 |
| Kernel7l.img | BCM2711 | Pi 4 | Large Physical Address Extension (LPAE)|
| Kernel8.img | BCM2837, BCM2711 | Pi 2, Pi 3, Pi 4 | Beta-64-Bit-Kernel<sup>1</sup>. Frühere Pi 2 mit BCM2836 unterstützen 64-Bit nicht. |

<sup>1</sup> Informationen zum Booten eines 64-Bit-Kernels finden Sie [hier](config-txt/boot.md).

Hinweis: Die von `lscpu` gemeldete Architektur ist `armv7l` für 32-Bit-Systeme (d.h. alles außer kernel8.img) und `aarch64` für 64-Bit-Systeme. Das „l“ im Fall „armv7l“ bezieht sich darauf, dass die Architektur Little-Endian ist, nicht „LPAE“, wie durch das „l“ im Dateinamen „kernel7l.img“ angezeigt.

## Gerätebaum-Overlays

Der Unterordner `Overlays` enthält Gerätebaum-Overlays. Diese werden verwendet, um verschiedene Hardwaregeräte zu konfigurieren, die an das System angeschlossen werden können, beispielsweise das Raspberry Pi Touch Display oder Soundboards von Drittanbietern. Diese Overlays werden mit Hilfe von Einträgen in `config.txt` ausgewählt — siehe ['Device Trees, Overlays and Parameters, part 2' for more info](device-tree.md#part2).