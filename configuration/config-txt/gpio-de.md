# GPIO-Steuerung in config.txt

## `GPIO`
Die `gpio`-Direktive ermöglicht es, GPIO-Pins beim Booten auf bestimmte Modi und Werte zu setzen, so dass es
zuvor eine benutzerdefinierte `dt-blob.bin`-Datei benötigt haben. Jede Zeile wendet die gleichen Einstellungen an (oder macht zumindest die gleichen
Änderungen) in einen Satz von Pins, entweder einen einzelnen Pin (`3`), eine Reihe von Pins (`3-4`) oder eine durch Kommas getrennte Liste von beiden (`3-4,6,8`).
Dem Pin-Set folgen ein `=` und ein oder mehrere durch Kommas getrennte Attribute aus dieser Liste:

* `ip` - Eingabe
* `op` - Ausgabe
* `a0-a5` - Alt0-Alt5
* `dh` - High fahren (für Ausgänge)
* `dl` - Low fahren (für Ausgänge)
* `pu` - Hochziehen
* `pd` - Herunterziehen
* `pn/np` - Kein Ziehen

`gpio`-Einstellungen werden der Reihe nach angewendet, sodass diejenigen, die später erscheinen, die früher erscheinenden überschreiben.

Beispiele:
```
# Wählen Sie Alt2 für die GPIO-Pins 0 bis 27 (für DPI24)
gpio=0-27=a2

# Setzen Sie GPIO12 als Ausgang auf 1
gpio=12=op,dh

# Wechseln Sie die Anzieh-(Eingangs-)Pins 18 und 20
gpio=18,20=pu

# Machen Sie die Pins 17 bis 21 Eingänge
gpio=17-21=ip
```

Die `gpio`-Direktive respektiert die Abschnittsüberschriften "[...]" in der `config.txt`, daher ist es möglich, verschiedene Einstellungen zu verwenden
basierend auf Modell, Seriennummer und EDID.

GPIO-Änderungen, die über diesen Mechanismus vorgenommen werden, haben keine direkten Auswirkungen auf den Kernel — sie führen nicht dazu, dass GPIO-Pins
in die sysfs-Schnittstelle exportiert werden, und sie können durch pinctrl-Einträge im Gerätebaum überschrieben werden sowie
Dienstprogramme wie `raspi-gpio`.

Beachten Sie auch, dass es eine Verzögerung von einigen Sekunden zwischen dem Anlegen der Stromversorgung und dem Wirksamwerden der Änderungen gibt – länger
beim Booten über das Netzwerk oder von einem USB-Massenspeichergerät.

## `enable_jtag_gpio`

Die Einstellung `enable_jtag_gpio=1` wählt den Alt4-Modus für die GPIO-Pins 22-27 und richtet einige interne SoC-Verbindungen ein, wodurch die JTAG-Schnittstelle für die ARM-CPU aktiviert wird. Es funktioniert auf allen Raspberry-Pi-Modellen.

| Pin # | Funktion |
| ------ | -------- |
| GPIO22 | ARM_TRST |
| GPIO23 | ARM_RTCK |
| GPIO24 | ARM_TDO |
| GPIO25 | ARM_TCK |
| GPIO26 | ARM_TDI |
| GPIO27 | ARM_TMS |