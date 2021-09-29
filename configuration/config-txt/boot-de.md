# Bootoptionen in config.txt

## start_file, fixup_file

Diese Optionen geben die Firmware-Dateien an, die vor dem Booten an die VideoCore-GPU übertragen werden.

`start_file` gibt die zu verwendende VideoCore-Firmwaredatei an.
`fixup_file` gibt die Datei an, die verwendet wird, um Speicherorte zu reparieren, die in der `start_file` verwendet werden, um der GPU-Speicheraufteilung zu entsprechen. Beachten Sie, dass "start_file" und "fixup_file" ein übereinstimmendes Paar sind - die Verwendung nicht übereinstimmender Dateien verhindert das Booten des Boards. Dies ist eine erweiterte Option, daher empfehlen wir Ihnen, `start_x` und `start_debug` statt dieser Option zu verwenden.

## start_x, start_debug

Diese bieten eine Verknüpfung zu einigen alternativen Einstellungen für `start_file` und `fixup_file` und sind die empfohlenen Methoden zur Auswahl von Firmware-Konfigurationen.

`start_x=1` impliziert
   `start_file=start_x.elf`
   `fixup_file=fixup_x.dat`
   
 Wenn auf dem Pi 4 die Dateien `start4x.elf` und `fixup4x.dat` vorhanden sind, werden stattdessen diese Dateien verwendet.
   
`start_debug=1` impliziert
   `start_file=start_db.elf`
   `fixup_file=fixup_db.dat`

Es gibt keine spezielle Handhabung für den Pi 4, wenn Sie also die Pi 4 Debug-Firmware-Dateien verwenden möchten, müssen Sie `start_file` und `fixup_file` manuell angeben.

Bei Verwendung des Kameramoduls sollte `start_x=1` angegeben werden. Durch Aktivieren der Kamera über `raspi-config` wird dies automatisch eingestellt.

##disable_commandline_tags

Setzen Sie den Befehl `disable_commandline_tags` auf `1`, um zu verhindern, dass `start.elf` ATAGS (Speicher von `0x100`) ausfüllt, bevor der Kernel gestartet wird.

##Befehlszeile

`cmdline` ist der alternative Dateiname auf der Bootpartition, aus dem die Kernel-Befehlszeilenzeichenfolge gelesen werden soll; der Standardwert ist `cmdline.txt`.

## Kernel

`kernel` ist der alternative Dateiname auf der Bootpartition, der beim Laden des Kernels verwendet wird. Der Standardwert auf dem Pi 1, Pi Zero und Compute Module ist `kernel.img` und auf dem Pi 2, Pi 3 und Compute Module 3 ist es `kernel7.img`. Auf dem Pi4 ist es `kernel7l.img`.

##arm_64bit

Wenn auf einen Wert ungleich Null gesetzt, zwingt das Kernel-Ladesystem, einen 64-Bit-Kernel anzunehmen, startet die Prozessoren im 64-Bit-Modus und setzt `kernel8.img` als geladenes Kernel-Image, es sei denn, es gibt ein explizites ` Kernel`-Option definiert, in diesem Fall wird diese stattdessen verwendet. Der Standardwert ist auf allen Plattformen 0. **HINWEIS**: 64-Bit-Kernel müssen unkomprimierte Bilddateien sein.

Beachten Sie, dass der 64-Bit-Kernel nur auf den Boards Pi4, Pi3 und Pi2B rev1.2 mit der neuesten Firmware funktioniert.

## arm_control

**VERALTET – verwenden Sie arm_64bit, um 64-Bit-Kernel zu aktivieren**.

Setzt kartenspezifische Steuerbits.

##Armwanne

`armstub` ist der Dateiname auf der Bootpartition, von der der ARM-Stub geladen werden soll. Der Standard-ARM-Stub ist in der Firmware gespeichert und wird basierend auf dem Pi-Modell und verschiedenen Einstellungen automatisch ausgewählt.

Der Stub ist ein kleines Stück ARM-Code, der vor dem Kernel ausgeführt wird. Seine Aufgabe besteht darin, Low-Level-Hardware wie den Interrupt-Controller einzurichten, bevor die Kontrolle an den Kernel übergeben wird.

##arm_peri_high

Setzen Sie `arm_peri_high` auf `1`, um den "High Peripheral"-Modus auf dem Pi 4 zu aktivieren. Er wird automatisch gesetzt, wenn ein passender DTB geladen wird.

**HINWEIS**: Das Aktivieren des "High Peripheral"-Modus ohne einen kompatiblen Gerätebaum führt dazu, dass Ihr System nicht startet. Derzeit fehlt die Unterstützung für ARM-Stubs, daher müssen Sie auch eine geeignete Datei mit `armstub` laden.

## Kernel_Adresse

`kernel_address` ist die Speicheradresse, in die das Kernel-Image geladen werden soll. 32-Bit-Kernel werden standardmäßig auf die Adresse `0x8000` geladen und 64-Bit-Kernel auf die Adresse `0x80000`. Wenn `kernel_old` gesetzt ist, werden Kernel auf die Adresse `0x0` geladen.

##kernel_old

Setzen Sie `kernel_old` auf `1`, um den Kernel auf die Speicheradresse `0x0` zu laden.

## ramfsfile

`ramfsfile` ist der optionale Dateiname auf der Bootpartition eines zu ladenden ramfs. Hinweis Ausreichend neue Firmware unterstützt das Laden mehrerer ramfs-Dateien - trennen Sie die mehreren Dateinamen durch Kommas und achten Sie darauf, dass die Zeilenlänge von 80 Zeichen nicht überschritten wird. Alle geladenen Dateien werden im Speicher verkettet und als einzelner Ramfs-Blob behandelt. Weitere Informationen finden Sie [hier](https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=10532).

## ramfsaddr

`ramfsaddr` ist die Speicheradresse, in die die `ramfsfile` geladen werden soll.

##initramfs

Der Befehl `initramfs` gibt sowohl den ramfs-Dateinamen **als auch** die Speicheradresse an, an die sie geladen werden soll. Es führt die Aktionen von `ramfsfile` und `ramfsaddr` in einem Parameter aus. Die Adresse kann auch `followkernel` (oder `0`) sein, um sie nach dem Kernel-Image im Speicher zu platzieren. Beispielwerte sind: `initramfs initramf.gz 0x00800000` oder `initramfs init.gz followkernel`. Wie bei `ramfsfile` erlauben neuere Firmwares das Laden mehrerer Dateien durch Komma-Trennung ihrer Namen. **HINWEIS:** Diese Option verwendet eine andere Syntax als alle anderen Optionen, und Sie sollten hier kein `=`-Zeichen verwenden.

##init_uart_baud

`init_uart_baud` ist die anfängliche UART-Baudrate. Der Standardwert ist '115200'.

##init_uart_clock

`init_uart_clock` ist die anfängliche UART-Taktfrequenz. Der Standardwert ist '48000000' (48MHz). Beachten Sie, dass dieser Takt nur für UART0 (ttyAMA0 in Linux) gilt und dass die maximale Baudrate für den UART auf 1/16 des Takts begrenzt ist. Der Standard-UART auf dem Pi 3 und Pi Zero ist UART1 (ttyS0 in Linux) und sein Takt ist der Kern-VPU-Takt - mindestens 250 MHz.

## bootcode_delay

Der Befehl `bootcode_delay` verzögert eine bestimmte Anzahl von Sekunden in `bootcode.bin`, bevor `start.elf` geladen wird: der Standardwert ist `0`.

Dies ist besonders nützlich, um eine Verzögerung vor dem Lesen der EDID des Monitors einzufügen, zum Beispiel wenn der Pi und der Monitor von derselben Quelle gespeist werden, der Monitor jedoch länger zum Starten braucht als der Pi. Versuchen Sie, diesen Wert einzustellen, wenn die Anzeigeerkennung beim ersten Start falsch ist, aber richtig ist, wenn Sie den Pi sanft neu starten, ohne die Stromversorgung des Monitors zu trennen.

## boot_delay

Der Befehl `boot_delay` weist an, eine bestimmte Anzahl von Sekunden in `start.elf` zu warten, bevor der Kernel geladen wird: Der Standardwert ist `1`. Die Gesamtverzögerung in Millisekunden wird berechnet als `(1000 x boot_delay) + boot_delay_ms`. Dies kann nützlich sein, wenn Ihre SD-Karte eine Weile braucht, um sich vorzubereiten, bevor Linux davon booten kann.

## boot_delay_ms

Der Befehl `boot_delay_ms` bedeutet, eine bestimmte Anzahl von Millisekunden in `start.elf` zusammen mit `boot_delay` zu warten, bevor der Kernel geladen wird. Der Standardwert ist `0`.

##disable_splash

Wenn `disable_splash` auf `1` gesetzt ist, wird der Rainbow Splash Screen beim Booten nicht angezeigt. Der Standardwert ist `0`.

## enable_gic (nur Pi 4B)

Wenn dieser Wert auf dem Raspberry Pi 4B auf „0“ gesetzt ist, werden die Interrupts über den Legacy-Interrupt-Controller und nicht über den GIC-400 an die ARM-Kerne geleitet. Der Standardwert ist '1'.

## force_eeprom_read

Setzen Sie diese Option auf „0“, um zu verhindern, dass die Firmware beim Einschalten versucht, ein I2C HAT EEPROM (verbunden mit den Pins ID_SD & ID_SC) zu lesen.

## os_prefix
<a name="os_prefix"></a>

`os_prefix` ist eine optionale Einstellung, mit der Sie zwischen mehreren Versionen des Kernels und der Gerätebaumdateien wählen können, die auf derselben Karte installiert sind. Jeder Wert in `os_prefix` wird dem Namen aller Betriebssystemdateien vorangestellt, die von der Firmware geladen werden, wobei "Betriebssystemdateien" für Kernel, initramfs, cmdline.txt, .dtbs und Overlays definiert sind. Das Präfix wäre normalerweise ein Verzeichnisname, könnte aber auch ein Teil des Dateinamens sein, beispielsweise "test-". Aus diesem Grund müssen Verzeichnispräfixe das nachgestellte `/`-Zeichen enthalten.

Um die Wahrscheinlichkeit eines nicht bootfähigen Systems zu verringern, testet die Firmware zuerst den angegebenen Präfixwert auf Funktionsfähigkeit. "). Ein Sonderfall dieses Durchführbarkeitstests wird auf Overlays angewendet, die nur von `${os_prefix}${overlay_prefix}` geladen werden (wobei der Standardwert von [`overlay_prefix`](#overlay_prefix) "overlays/" ist, wenn `${os_prefix}${overlay_prefix}README` existiert, ansonsten ignoriert es `os_prefix` und behandelt Overlays als geteilt.

(Der Grund, warum die Firmware beim Prüfen von Präfixen auf das Vorhandensein von Schlüsseldateien statt von Verzeichnissen prüft, ist zweierlei - das Präfix ist möglicherweise kein Verzeichnis und nicht alle Bootmethoden unterstützen das Testen auf die Existenz eines Verzeichnisses.)

Hinweis Jede benutzerdefinierte Betriebssystemdatei kann alle Präfixe umgehen, indem sie einen absoluten Pfad (in Bezug auf die Bootpartition) verwendet - beginnen Sie einfach den Dateipfad mit einem `/`, z. `kernel=/my_common_kernel.img`.

Siehe auch [`overlay_prefix`](#overlay_prefix) und [`upstream_kernel`](#upstream_kernel).

## overlay_prefix
<a name="overlay_prefix"></a>

Gibt ein Unterverzeichnis/Präfix an, aus dem Overlays geladen werden sollen - standardmäßig ist `overlays/` (beachten Sie das nachgestellte `/`). In Verbindung mit [`os_prefix`](#os_prefix) kommt das `os_prefix` vor dem `overlay_prefix`, dh `dtoverlay=disable-bt` versucht, `${os_prefix}${overlay_prefix}disable-bt . zu laden .dtbo`.

Beachten Sie, dass Overlays mit dem Hauptbetriebssystem geteilt werden, sofern `${os_prefix}${overlay_prefix}README` nicht vorhanden ist (d. h. `os_prefix` wird ignoriert).

##uart_2ndstage

Die Einstellung `uart_2ndstage=1` bewirkt, dass der Second-Stage-Loader (`bootcode.bin` bei Geräten vor dem Raspberry Pi 4 oder der Bootcode im EEPROM bei Raspberry Pi 4 Geräten) und die Hauptfirmware (`start*.elf `), um Diagnoseinformationen an UART0 auszugeben.

Beachten Sie, dass die Ausgabe wahrscheinlich den Bluetooth-Betrieb stört, es sei denn, sie wird deaktiviert (`dtoverlay=disable-bt`) oder auf den anderen UART umgeschaltet (`dtoverlay=miniuart-bt`) und wenn gleichzeitig auf den UART zugegriffen wird, um von Unter Linux kann es zu Datenverlusten kommen, die zu einer beschädigten Ausgabe führen. Diese Funktion sollte nur erforderlich sein, wenn versucht wird, ein frühes Boot-Ladeproblem zu diagnostizieren.

##upstream_kernel
<a name="upstream_kernel"></a>

Wenn `upstream_kernel=1` verwendet wird, setzt die Firmware [`os_prefix`](#os_prefix) auf "upstream/", es sei denn, es wurde explizit auf etwas anderes gesetzt, aber wie andere `os_prefix`-Werte wird es ignoriert, wenn die Der erforderliche Kernel und die .dtb-Datei können nicht gefunden werden, wenn das Präfix verwendet wird.

Die Firmware wird auch Upstream-Linux-Namen für DTBs bevorzugen (zB `bcm2837-rpi-3-b.dtb` statt `bcm2710-rpi-3-b.dtb`). Wenn die Upstream-Datei nicht gefunden wird, lädt die Firmware stattdessen die Downstream-Variante und wendet automatisch das "Upstream"-Overlay an, um einige Anpassungen vorzunehmen. Beachten Sie, dass dieser Vorgang _nach_dem `os_prefix` abgeschlossen wurde.

*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses .) geteilt wird /bis-sa/3.0/)*