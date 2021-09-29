# Verschiedene Optionen in config.txt

## vermeiden_warnungen

Die [Warnsymbole](../warning-icons.md) können mit dieser Option deaktiviert werden, dies wird jedoch nicht empfohlen.

`avoid_warnings=1` deaktiviert die Warn-Overlays.
`avoid_warnings=2` deaktiviert die Warn-Overlays, erlaubt aber zusätzlich den Turbo-Modus auch bei anliegender Unterspannung.

##logging_level

Legt die VideoCore-Protokollierungsebene fest. Der Wert ist eine VideoCore-spezifische Bitmaske.

## enthalten

Bewirkt, dass der Inhalt der angegebenen Datei in die aktuelle Datei eingefügt wird.

Wenn Sie beispielsweise die Zeile `include extraconfig.txt` zu `config.txt` hinzufügen, wird der Inhalt der `extraconfig.txt`-Datei in die `config.txt`-Datei aufgenommen.

**Include-Direktiven werden von bootcode.bin oder dem EEPROM-Bootloader nicht unterstützt**

##max_usb_current

**Dieser Befehl ist jetzt veraltet und hat keine Auswirkung.** Ursprünglich waren die USB-Ports bei bestimmten Raspberry Pi-Modellen auf maximal 600 mA beschränkt. Die Einstellung `max_usb_current=1` hat diese Vorgabe auf 1200mA geändert. Bei allen Firmwares ist dieses Flag jetzt jedoch standardmäßig gesetzt, sodass es nicht mehr erforderlich ist, diese Option zu verwenden.