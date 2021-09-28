# Ändern der Standard-Pin-Konfiguration

**Diese Funktion ist für fortgeschrittene Benutzer gedacht.**

Ab dem 15. Juli 2014 unterstützt die Raspberry Pi-Firmware benutzerdefinierte Standard-Pin-Konfigurationen über eine vom Benutzer bereitgestellte Gerätebaum-Blobdatei. Um herauszufinden, ob Ihre Firmware aktuell genug ist, führen Sie bitte `vcgencmd version` aus.

## Aktionen auf Geräte-Pins während der Boot-Sequenz

Während der Boot-Sequenz durchlaufen die GPIO-Pins verschiedene Aktionen.

1. Power-on – Pins sind standardmäßig Eingänge mit Standard-Pulls; die Standardzüge für jeden Pin sind im [datasheet](../hardware/raspberrypi/bcm2835/BCM2835-ARM-Peripherals.pdf) beschrieben.
1. Einstellung durch das Bootrom
1. Einstellung durch `bootcode.bin`
1. Einstellung durch `dt-blob.bin` (diese Seite)
1. Einstellung durch den GPIO-Befehl in `config.txt` (siehe [hier](config-txt/gpio.md))
1. Zusätzliche Firmware-Pins (z. B. UARTS)
1. Kernel-/Gerätebaum

Bei einem Soft-Reset gilt das gleiche Verfahren, mit Ausnahme von Standard-Pulls, die nur bei einem Power-On-Reset angewendet werden.

Beachten Sie, dass es einige Sekunden dauern kann, um von Stufe 1 zu Stufe 4 zu gelangen. Während dieser Zeit befinden sich die GPIO-Pins möglicherweise nicht in dem von angeschlossenen Peripheriegeräten erwarteten Zustand (wie in "dtblob.bin" oder "config.txt" definiert). . Da verschiedene GPIO-Pins unterschiedliche Standard-Pulls haben, sollten Sie **eine der folgenden Methoden** für Ihr Peripheriegerät ausführen:
* Wählen Sie einen GPIO-Pins, der beim Zurücksetzen standardmäßig so zieht, wie es vom Peripheriegerät erforderlich ist
* Start des Peripheriegeräts verzögern, bis Stufe 4/5 erreicht ist
* Fügen Sie einen geeigneten Pull-Up/-Down-Widerstand hinzu


## Bereitstellung eines benutzerdefinierten Gerätebaum-Blobs

Um eine Gerätebaum-Quelldatei (`.dts`) in eine Gerätebaum-Blobdatei (`.dtb`) zu kompilieren, muss der Gerätebaum-Compiler durch Ausführen von `sudo apt install device-tree-compiler` installiert werden. Der Befehl `dtc` kann dann wie folgt verwendet werden:

```
sudo dtc -I dts -O dtb -o /boot/dt-blob.bin dt-blob.dts
```

**HINWEIS:** Bei NOOBS-Installationen sollte die DTB-Datei stattdessen auf der Wiederherstellungspartition abgelegt werden.

Ebenso kann eine `.dtb`-Datei bei Bedarf wieder in eine `.dts`-Datei konvertiert werden.

```
dtc -I dtb -O dts -o dt-blob.dts /boot/dt-blob.bin
```

## Abschnitte des dt-Blobs

Die `dt-blob.bin` wird verwendet, um den binären Blob (VideoCore) beim Booten zu konfigurieren. Es wird derzeit nicht vom Linux-Kernel verwendet, aber ein Kernel-Abschnitt wird zu einem späteren Zeitpunkt hinzugefügt, wenn wir den Raspberry Pi-Kernel neu konfigurieren, um einen dt-Blob für die Konfiguration zu verwenden. Der dt-blob kann alle Versionen des Raspberry Pi, einschließlich des Compute-Moduls, konfigurieren, um die alternativen Einstellungen zu verwenden. Die folgenden Abschnitte sind im dt-blob gültig:

1. `Videocore`

   Dieser Abschnitt enthält alle VideoCore-Blob-Informationen. Alle nachfolgenden Abschnitte müssen in diesem Abschnitt eingeschlossen werden.

2. `pins_*`

 Es gibt eine Reihe von separaten `pins_*`-Abschnitten, die auf bestimmten Raspberry Pi-Modellen basieren, nämlich:
   
 - **pins_rev1** Rev1-Pin-Setup. Aufgrund der verschobenen I2C-Pins gibt es einige Unterschiede.
 - **pins_rev2** Rev2-Pin-Setup. Dazu gehören die zusätzlichen Codec-Pins auf P5.
 - **pins_bplus1** Modell B+ Rev. 1.1, einschließlich des vollständigen 40-Pin-Anschlusses.
 - **pins_bplus2** Model B+ Rev 1.2, Vertauschen der Low-Power- und Lan-Run-Pins.
 - **pins_aplus** Modell A+, kein Ethernet.
 - **pins_2b1** Pi 2 Modell B Rev. 1.0; steuert das SMPS über I2C0.
 - **pins_2b2** Pi 2 Modell B Rev. 1.1; steuert das SMPS über die Software I2C auf 42 und 43.
 - **pins_3b1** Pi 3 Modell B Rev 1.0
 - **pins_3b2** Pi 3 Modell B Rev 1,2
 - **pins_3bplus** Pi 3 Modell B+
 - **pins_3aplus** Pi 3 Modell A+
 - **pins_pi0** Der Pi Zero
 - **pins_pi0w** Der Pi Zero W
 - **pins_cm** Das Rechenmodul. Die Vorgabe dafür ist die Vorgabe für den Chip, also eine nützliche Informationsquelle über die Vorgabe-Pull-Ups/-Downs auf dem Chip.
 - **pins_cm3** Das Rechenmodul Version 3
  
   Jeder `pins_*`-Abschnitt kann `pin_config`- und `pin_defines`-Abschnitte enthalten.

3. `pin_config`

   Der Abschnitt `pin_config` dient der Konfiguration der einzelnen Pins. Jedes Element in diesem Abschnitt muss ein benannter Pin-Abschnitt sein, z. B. "pin@p32", was GPIO32 bedeutet. Es gibt einen speziellen Abschnitt `pin@default`, der die Standardeinstellungen für alles enthält, was im Abschnitt pin_config nicht speziell genannt wird.
   
4. `pin@pinname`

   Dieser Abschnitt kann eine beliebige Kombination der folgenden Elemente enthalten:
   
   1. `Polarität`
      * `aktiv_hoch`
      * `active_low`
   2. `Kündigung`
      * `pull_up`
      * `pull_down`
      * `kein_ziehen`
   3. `startup_state`
      * `aktiv`
      * `inaktiv`
   4. `Funktion`
      * `Eingabe`
      * `Ausgabe`
      * `SD-Karte`
      * `i2c0`
      * `i2c1`
      * `spi`
      * `spi1`
      * `spi2`
      * `smi`
      * `dpi`
      * `pcm`
      * `pwm`
      * `uart0`
      * `uart1`
      * `gp_clk`
      * `emmc`
      * `arm_jtag`
   5. `drive_strength_mA`
      Die Antriebsstärke wird verwendet, um eine Stärke für die Stifte einzustellen. Bitte beachten Sie, dass Sie nur eine einzelne Antriebsstärke für die Bank angeben können. <8> und <16> sind gültige Werte.

5. `pin_defines`

   Dieser Abschnitt wird verwendet, um bestimmte VideoCore-Funktionen auf bestimmte Pins einzustellen. Dadurch kann der Benutzer den Power-Enable-Pin der Kamera an einen anderen Ort oder die HDMI-Hotplug-Position verschieben: Dinge, die Linux nicht kontrolliert. Bitte beachten Sie die Beispiel-DTS-Datei unten.

## Uhrkonfiguration

Es ist möglich, die Konfiguration der Uhren über diese Schnittstelle zu ändern, obwohl es schwierig sein kann, die Ergebnisse vorherzusagen! Die Konfiguration des Taktsystems ist sehr komplex. Es gibt fünf separate PLLs, und jeder hat seine eigene feste (oder im Fall von PLLC variable) VCO-Frequenz. Jeder VCO hat dann eine Anzahl verschiedener Kanäle, die mit einer anderen Aufteilung der VCO-Frequenz eingerichtet werden können. Jedes der Clock-Ziele kann so konfiguriert werden, dass es von einem der Clock-Kanäle kommt, obwohl es eine eingeschränkte Zuordnung von Quelle zu Ziel gibt, sodass nicht alle Kanäle an alle Clock-Ziele geroutet werden können.

Hier sind einige Beispielkonfigurationen, die Sie verwenden können, um bestimmte Uhren zu ändern. Wir werden diese Ressource erweitern, wenn Anfragen nach Uhrenkonfigurationen gestellt werden.

```
clock_routing {
   vco@PLLA { Häufigkeit = <1966080000>; };
   chan@APER { div = <4>; };
   clock@GPCLK0 { pll = "PLLA"; chan = "APER"; };
};

clock_setup {
   clock@PWM { freq = <2400000>; };
   clock@GPCLK0 { freq = <12288000>; };
   clock@GPCLK1 { freq = <25000000>; };
};
```

Das Obige stellt den PLLA auf einen Quell-VCO mit 1,96608 GHz ein (die Grenzen für diesen VCO sind 600 MHz - 2,4 GHz), ändern den APER-Kanal auf /4 und konfigurieren GPCLK0 so, dass er von PLLA über APER bezogen wird. Dies wird verwendet, um einem Audio-Codec die 12288000 Hz zu geben, die er benötigt, um den 48000-Frequenzbereich zu erzeugen.

## Beispiel-Gerätebaum-Quelldatei

Diese Beispieldatei stammt aus dem Firmware-Repository. Es ist der Master Raspberry Pi Blob, von dem andere normalerweise abgeleitet werden.

https://github.com/raspberrypi/firmware/blob/master/extra/dt-blob.dts