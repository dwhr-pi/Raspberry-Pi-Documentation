# Bedingte Filter in config.txt

Wenn eine einzelne SD-Karte (oder ein Karten-Image) mit einem Pi und einem Monitor verwendet wird, ist es einfach, `config.txt` so einzustellen, wie es für diese spezielle Kombination erforderlich ist, und es so zu belassen, es nur zu ändern, wenn sich etwas ändert.


Wenn jedoch ein Pi zwischen verschiedenen Monitoren ausgetauscht wird oder wenn die SD-Karte (oder das Kartenbild) zwischen mehreren Pis ausgetauscht wird, reicht ein einzelner Satz von Einstellungen möglicherweise nicht mehr aus. Bedingte Filter ermöglichen es Ihnen, bestimmte Abschnitte der Konfigurationsdatei zu definieren, die nur in bestimmten Fällen verwendet werden, sodass eine einzelne `config.txt` unterschiedliche Konfigurationen erstellen kann, wenn sie von unterschiedlicher Hardware gelesen wird.

## Der `[alle]`-Filter

Dies ist der einfachste Filter. Es setzt alle zuvor eingestellten Filter zurück und ermöglicht, dass alle darunter aufgeführten Einstellungen auf die gesamte Hardware angewendet werden.


    [alle]

Es ist normalerweise eine gute Idee, am Ende von Gruppen von gefilterten Einstellungen einen `[all]`-Filter hinzuzufügen, um ein unbeabsichtigtes Kombinieren von Filtern zu vermeiden (siehe unten).

## Die Modellfilter `[pi1]` und `[pi2]` (usw.)

Die bedingten Modellfilter werden gemäß der folgenden Tabelle angewendet.

| Filter | Anwendbare(s) Modell(e) |
|--------|-----------------|
| [pi1] | Modell A, Modell B, Rechenmodul |
| [pi2] | Modell 2B (BCM2836- oder BCM2837-basiert) |
| [pi3] | Modell 3B, Modell 3B+, Modell 3A+, Rechenmodul 3 |
| [pi3+]| Modell 3A+, Modell 3B+ |
| [pi4]| Modell 4B |
| [pi0] | Null, Null W, Null WH |
| [pi0w]| Null W, Null WH |

Diese sind besonders nützlich, um verschiedene Einstellungen für `kernel`, `initramfs` und `cmdline` zu ​​definieren, da Pi 1 und Pi 2 unterschiedliche Kernel benötigen. Sie können auch nützlich sein, um unterschiedliche Übertaktungseinstellungen zu definieren, da Pi 1 und Pi 2 unterschiedliche Standardgeschwindigkeiten haben. Um beispielsweise separate `initramfs`-Bilder für jedes zu definieren:

    [pi1]
    initramfs initrd.img-3.18.7+ followkernel
    [pi2]
    initramfs initrd.img-3.18.7-v7+ followkernel
    [alle]

Denken Sie daran, den Filter `[all]` am Ende zu verwenden, damit alle nachfolgenden Einstellungen nicht nur auf die Pi 2-Hardware beschränkt sind.

Es ist wichtig zu beachten, dass der Raspberry Pi Zero W den Inhalt von [pi0w] UND [pi0] sieht. Ebenso sieht ein Raspberry Pi 3B Plus [pi3+] UND [pi3]. Wenn Sie möchten, dass eine Einstellung nur für Pi Zero oder Pi 3B gilt, müssen Sie ihr (Reihenfolge ist wichtig) mit einer Einstellung im Abschnitt [pi0w] oder [pi3+] folgen, die sie umkehrt.


## Der `[keine]`-Filter

Der Filter `[none]` verhindert, dass alle nachfolgenden Einstellungen auf jegliche Hardware angewendet werden. Obwohl es nichts gibt, was Sie ohne `[none]` nicht tun können, kann es eine nützliche Möglichkeit sein, Gruppen nicht verwendeter Einstellungen in der config.txt zu behalten, ohne jede Zeile auskommentieren zu müssen.

    [keiner]

## Der `[EDID=*]`-Filter

Wenn Sie zwischen mehreren Monitoren wechseln, während Sie eine einzelne SD-Karte in Ihrem Pi verwenden und eine leere Konfiguration nicht ausreicht, um automatisch die gewünschte Auflösung für jeden einzelnen auszuwählen, können bestimmte Einstellungen basierend auf den EDID-Namen der Monitore ausgewählt werden.

Führen Sie den folgenden Befehl aus, um den EDID-Namen eines angeschlossenen Monitors anzuzeigen:

    Fernsehdienst -n

Dies wird etwa so gedruckt:

    Gerätename=VSC-TD2220

Sie können dann Einstellungen festlegen, die nur für diesen Monitor gelten:

    [EDID=VSC-TD2220]
    hdmi_group=2
    hdmi_mode=82
    [alle]

Dies erzwingt den 1920x1080 DVT-Modus für den angegebenen Monitor, ohne andere Monitore zu beeinträchtigen.

Beachten Sie, dass diese Einstellungen nur beim Booten gelten, daher muss der Monitor beim Booten angeschlossen sein und der Pi muss seine EDID-Informationen lesen können, um den richtigen Namen zu finden. Wenn Sie nach dem Booten einen anderen Monitor an den Pi anschließen, werden keine anderen Einstellungen ausgewählt.

Wenn auf dem Pi 4 beide HDMI-Anschlüsse verwendet werden, wird die EDID mit beiden verglichen und die nachfolgende Konfiguration wird nur auf das erste übereinstimmende Gerät angewendet. Sie können die EDID-Namen für beide Ports ermitteln, indem Sie zuerst `tvservice -l` in einem Terminalfenster ausführen, um alle angeschlossenen Geräte aufzulisten, und dann die zurückgegebenen numerischen IDs in `tvservice -v <id> -n` verwenden, um den EDID-Namen für zu finden eine bestimmte Display-ID.

## Der Seriennummernfilter

Manchmal sollten Einstellungen nur auf einen einzelnen bestimmten Pi angewendet werden, auch wenn Sie die SD-Karte gegen eine andere austauschen. Beispiele hierfür sind Lizenzschlüssel und Übertaktungseinstellungen (obwohl die Lizenzschlüssel bereits den Austausch von SD-Karten auf andere Weise unterstützen). Sie können damit auch verschiedene Anzeigeeinstellungen auswählen, auch wenn die obige EDID-Identifikation nicht möglich ist, vorausgesetzt, Sie tauschen nicht die Monitore zwischen Ihren Pis aus. Zum Beispiel, wenn Ihr Monitor keinen verwendbaren EDID-Namen liefert oder wenn Sie Composite-Ausgabe verwenden (für die EDID nicht gelesen werden kann).

Führen Sie den folgenden Befehl aus, um die Seriennummer Ihres Pi anzuzeigen:

    Katze /proc/cpuinfo

Die Seriennummer wird unten als 16-stelliger Hex-Wert angezeigt. Wenn Sie beispielsweise Folgendes sehen:

    Seriennummer: 0000000012345678

dann können Sie Einstellungen definieren, die nur auf diesen bestimmten Pi angewendet werden:

    [0x12345678]
    # Einstellungen hier werden nur auf den Pi mit dieser Seriennummer angewendet
    [alle]
    # Einstellungen hier werden auf alle Hardware angewendet

## Der GPIO-Filter

Sie können auch nach dem Status eines GPIO filtern. Zum Beispiel

    [gpio4=1]
    # Einstellungen hier werden angewendet, wenn GPIO 4 hoch ist
    
    [gpio2=0]
    # Einstellungen hier werden angewendet, wenn GPIO 2 niedrig ist
    
    [alle]
    # Einstellungen hier werden auf alle Hardware angewendet

## Der `[HDMI:*]`-Filter – nur Pi 4

Der Raspberry Pi 4 hat zwei HDMI-Anschlüsse, und für viele `config.txt`-Befehle im Zusammenhang mit HDMI muss angegeben werden, auf welchen HDMI-Anschluss Bezug genommen wird. Die HDMI-Bedingung filtert nachfolgende HDMI-Konfigurationen auf den spezifischen Port.

    [HDMI:0]
      hdmi_group=2
      hdmi_mode=45
    [HDMI:1]
      hdmi_group=2
      hdmi_mode=67
    
Eine alternative `variable:index`-Syntax ist für alle portspezifischen HDMI-Befehle verfügbar. Sie könnten Folgendes verwenden, das dem vorherigen Beispiel entspricht:

    hdmi_group:0=2
    hdmi_mode:0=45
    hdmi_group:1=2
    hdmi_mode:1=67

## Bedingte Filter kombinieren

Filter desselben Typs ersetzen sich gegenseitig, daher überschreibt `[pi2]` `[pi1]`, da nicht beide gleichzeitig wahr sein können.

Filter unterschiedlicher Typen lassen sich einfach kombinieren, indem man sie nacheinander auflistet, zum Beispiel:

    # Einstellungen hier werden auf alle Hardware angewendet
    [EDID=VSC-TD2220]
    # Einstellungen hier werden nur angewendet, wenn der Monitor VSC-TD2220 angeschlossen ist
    [pi2]
    # Einstellungen hier werden nur angewendet, wenn der Monitor VSC-TD2220 angeschlossen ist *und* an einem Pi 2
    [alle]
    # Einstellungen hier werden auf alle Hardware angewendet

Verwenden Sie den Filter `[all]`, um alle vorherigen Filter zurückzusetzen und ein unbeabsichtigtes Kombinieren verschiedener Filtertypen zu vermeiden.





*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses .) geteilt wird /bis-sa/3.0/)*