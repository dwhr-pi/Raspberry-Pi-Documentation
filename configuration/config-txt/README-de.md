# config.txt

Der Raspberry Pi verwendet eine Konfigurationsdatei anstelle des [BIOS](https://en.wikipedia.org/wiki/BIOS), das Sie auf einem herkömmlichen PC erwarten würden. Die Systemkonfigurationsparameter, die traditionell mit einem BIOS bearbeitet und gespeichert würden, werden stattdessen in einer optionalen Textdatei namens `config.txt` gespeichert. Dieser wird von der GPU gelesen, bevor die ARM-CPU und Linux initialisiert werden. Sie muss sich daher auf der ersten (Boot-)Partition Ihrer SD-Karte neben `bootcode.bin` und `start.elf` befinden. Diese Datei ist normalerweise unter Linux als `/boot/config.txt` zugänglich und muss als [root](../../linux/usage/root.md) bearbeitet werden. Unter Windows oder OS X ist sie als Datei im einzigen zugänglichen Teil der Karte sichtbar. Wenn Sie einige der folgenden Konfigurationseinstellungen anwenden müssen, aber noch keine `config.txt` auf Ihrer Bootpartition haben, erstellen Sie sie einfach als neue Textdatei.

Alle Änderungen werden erst wirksam, nachdem Sie Ihren Raspberry Pi neu gestartet haben. Nachdem Linux gebootet hat, können Sie die aktuellen aktiven Einstellungen mit den folgenden Befehlen anzeigen:

- `vcgencmd get_config <config>`: Zeigt einen bestimmten Konfigurationswert an, z.B. `vcgencmd get_config arm_freq`.

- `vcgencmd get_config int`: Listet alle Integer-Konfigurationsoptionen auf, die gesetzt sind (nicht null).

- `vcgencmd get_config str`: Listet alle gesetzten String-Konfigurationsoptionen auf (nicht null).

Beachten Sie, dass einige Konfigurationseinstellungen nicht mit `vcgencmd` abgerufen werden können.

## Datei Format

Die Datei `config.txt` wird von der frühen Boot-Firmware gelesen, hat also ein sehr einfaches Dateiformat. Das Format ist eine einzelne `property=value`-Anweisung in jeder Zeile, wobei `value` entweder eine ganze Zahl oder ein String ist. Kommentare können hinzugefügt oder vorhandene Konfigurationswerte auskommentiert und deaktiviert werden, indem eine Zeile mit dem Zeichen `#` beginnt.

Es gibt eine Zeilenlänge von 80 Zeichen für Einträge, alle Zeichen, die diese Grenze überschreiten, werden ignoriert.

Hier ist eine Beispieldatei:

```
# Erzwingen Sie den Monitor in den HDMI-Modus, damit der Ton über das HDMI-Kabel gesendet wird
hdmi_drive=2
# Stellen Sie den Monitormodus auf DMT . ein
hdmi_group=2
# Monitorauflösung auf 1024x768 XGA 60Hz einstellen (HDMI_DMT_XGA_60)
hdmi_mode=16
# Verkleinern Sie das Display, um zu verhindern, dass Text über den Bildschirm läuft
overscan_left=20
overscan_right=12
overscan_top=10
overscan_bottom=10
```

## config.txt-Optionen

Mithilfe der Datei config.txt kann eine Reihe von Optionen angegeben werden. Diese sind in verschiedene Abschnitte unterteilt, die unten indiziert sind:

- [Speicher](memory.md)
- [Lizenzschlüssel/Codecs](codeclicence.md)
- [Video/Anzeige](video.md)
- [Audio](audio.md)
- [Kamera](camera.md)
- [Boot](boot.md)
- [Ports und Gerätebaum](../device-tree.md#part4.6)
- [GPIOs](gpio.md)
- [Übertaktung](overclocking.md)
- [Bedingte Filter](conditional.md)
- [Sonstiges](Misc.md)




*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses/bis-sa/3.0/) geteilt wird.*