# Die Kernel-Befehlszeile

Der Linux-Kernel akzeptiert während des Bootens eine Befehlszeile mit Parametern. Auf dem Raspberry Pi ist diese Befehlszeile in einer Datei in der Bootpartition namens cmdline.txt definiert. Dies ist eine einfache Textdatei, die mit einem beliebigen Texteditor bearbeitet werden kann, z. Nano.
```
sudo nano /boot/cmdline.txt
```
Beachten Sie, dass wir `sudo` verwenden müssen, um alles in der Bootpartition zu bearbeiten, und dass alle Parameter in `cmdline.txt` in derselben Zeile stehen müssen (kein Wagenrücklauf).

Die beim Booten an den Kernel übergebene Kommandozeile kann mit `cat /proc/cmdline` angezeigt werden. Es wird nicht genau das gleiche sein wie in `cmdline.txt`, da die Firmware Änderungen daran vornehmen kann, bevor der Kernel gestartet wird.

## Befehlszeilenoptionen

Es gibt viele Kernel-Befehlszeilenparameter, von denen einige vom Kernel definiert werden. Andere werden durch Code definiert, den der Kernel möglicherweise verwendet, wie beispielsweise das Plymouth-Splash-Screen-System.

#### Standardeinträge

 - Konsole: Definiert die serielle Konsole. Normalerweise gibt es zwei Einträge:
     - `Konsole=Seriell0,115200`
     - `Konsole=tty1`
 - root: Definiert den Ort des Root-Dateisystems, z.B. `root=/dev/mmcblk0p2` bedeutet Multimediakartenblock 0 Partition 2.
 - rootfstype: definiert, welche Art von Dateisystem das rootfs verwendet, z.B. `rootfstype=ext4`
 - Aufzug: Gibt den zu verwendenden E/A-Scheduler an. `elevator=deadline` bedeutet, dass der Kernel allen I/O-Operationen eine Frist auferlegt, um das Verhungern von Anfragen zu verhindern.
 - quiet: setzt den Standard-Log-Level des Kernels auf `KERN_WARNING`, der alle bis auf sehr schwerwiegende Log-Meldungen während des Bootens unterdrückt.

#### Einträge im FKMS- und KMS-Modus anzeigen

Die Firmware fügt automatisch eine bevorzugte Auflösung und Overscan-Einstellungen über einen Eintrag hinzu, wie zum Beispiel:

```video=HDMI-A-1:1920x1080M@60,margin_left=0,margin_right=0,margin_top=0,margin_bottom=0```

Dieser Standardeintrag kann geändert werden, indem der obige Eintrag manuell in /boot/cmdline.txt dupliziert und die erforderlichen Änderungen an den Randparametern vorgenommen werden. Darüber hinaus ist es möglich, Rotations- und Spiegelungsparameter hinzuzufügen, wie im Standard [Linux-Framebuffer-Dokumentation] (https://github.com/raspberrypi/linux/blob/rpi-5.4.y/Documentation/fb/modedb.rst .) dokumentiert ). Standardmäßig werden die `margin_*`-Optionen aus den `overscan`-Einträgen in der config.txt gesetzt, falls vorhanden. Die Firmware kann daran gehindert werden, KMS-spezifische Änderungen an der Befehlszeile vorzunehmen, indem Sie `disable_fw_kms_setup=1` zu `config.txt` hinzufügen

Ein Beispieleintrag könnte wie folgt lauten:

```video=HDMI-A-1:1920x1080M@60,margin_left=0,margin_right=0,margin_top=0,margin_bottom=0,rotate=90,reflect_x```

Mögliche Optionen für den Anzeigetyp, den ersten Teil des `video=`-Eintrags, sind wie folgt:

| Videooption | Anzeige |
|:---:|:---|
| HDMI-A-1 | HDMI 1 (HDMI 0 auf Siebdruck von Pi4B, HDMI auf einzelnen HDMI-Boards) |
| HDMI-A-2 | HDMI 2 (HDMI 1 auf Siebdruck von Pi4B) |
| DSI-1 | DSI oder DPI |
| Komposit-1 | Verbund |

#### Sonstige Einträge (nicht erschöpfend)

 - splash: weist den Boot über das Plymouth-Modul an, einen Begrüßungsbildschirm zu verwenden.
 - plymouth.ignore-serial-consoles: Wenn das Plymouth-Modul aktiviert ist, verhindert es normalerweise, dass Boot-Meldungen auf einer eventuell vorhandenen seriellen Konsole angezeigt werden. Dieses Flag weist Plymouth an, alle seriellen Konsolen zu ignorieren, wodurch Boot-Meldungen wieder sichtbar werden, als ob Plymouth nicht ausgeführt würde.
 - dwc_otg.lpm_enable: deaktiviert Link Power Management (LPM) im dwc_otg-Treiber; Der dwc_otg-Treiber ist der Treiber für den im Raspberry Pi integrierten USB-Controller.
 - dwc_otg.speed: legt die Geschwindigkeit des USB-Controllers fest. `dwc_otg.speed=1` setzt es auf volle Geschwindigkeit (USB 1.0), was langsamer ist als hohe Geschwindigkeit (USB 2.0). Diese Option sollte nur während der Fehlerbehebung bei Problemen mit USB-Geräten gesetzt werden.
 - smsc95xx.turbo_mode: aktiviert/deaktiviert den Turbomodus des kabelgebundenen Netzwerktreibers. `smsc95xx.turbo_mode=N` schaltet den Turbo-Modus aus.
 - usbhid.mousepoll: Gibt das Maus-Polling-Intervall an. Wenn Sie Probleme mit einer langsamen oder unregelmäßigen drahtlosen Maus haben, kann es hilfreich sein, dies auf 0 zu setzen: `usbhid.mousepoll=0`.