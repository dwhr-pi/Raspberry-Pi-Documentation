## Fernsehdienst

`tvservice` ist eine Befehlszeilenanwendung zum Abrufen und Einstellen von Informationen über das Display, die hauptsächlich auf HDMI-Video und -Audio ausgerichtet sind.

Wenn Sie `tvservice` allein eingeben, wird eine Liste der verfügbaren Befehlszeilenoptionen angezeigt.

### Optionen

#### -p, --bevorzugt

Schalten Sie den HDMI-Ausgang mit den bevorzugten Einstellungen ein.

#### -o, --off

Schaltet die Displayausgabe aus.

**Wichtiger Hinweis:** Wenn Sie die Ausgabe mit diesem Befehl ausschalten, werden auch alle mit der Anzeige verknüpften Framebuffer/Dispmanx-Layer zerstört. Diese werden bei einem nachfolgenden Einschalten NICHT wiederhergestellt, sodass ein leerer Bildschirm angezeigt wird.

Eine bessere Option ist die Verwendung der Option [vcgencmd display_power](vcgencmd.md), da dadurch alle Framebuffer erhalten bleiben, so dass das Display beim Wiedereinschalten in den vorherigen Einschaltzustand zurückkehrt.

#### -e, --explicit="Gruppe Modus Laufwerk"

Schalten Sie HDMI mit den angegebenen Einstellungen ein

Die Gruppe kann `CEA`, `DMT`, `CEA_3D_SBS`, `CEA_3D_TB`, `CEA_3D_FP`, `CEA_3D_FS` sein.
Modus ist einer der Modi, die von der Option `-m, --modes` zurückgegeben werden.
Das Laufwerk kann eines von `HDMI`, `DVI` sein.

#### -t, --ntsc

Verwenden Sie 59,94 Hz (NTSC-Frequenz) anstelle von 60 Hz für den HDMI-Modus.

#### -c, --sdtvon="Modus-Seitenverhältnis [P]"

Schalten Sie das SDTV (Composite-Ausgabe) mit dem angegebenen Modus „PAL“ oder „NSTC“ und dem angegebenen Seitenverhältnis „4:3“, „14:9“, „16:9“ ein. Der optionale Parameter `P` kann verwendet werden, um den progressiven Modus anzugeben.

#### -m, --modes=Gruppe

wobei Gruppe "CEA" oder "DMT" ist.

Zeigt eine Liste der in der angegebenen Gruppe verfügbaren Anzeigemodi an.

#### -M, --monitor

Überwacht alle HDMI-Ereignisse, z. B. Trennen oder Anschließen.

#### -s, --status

Zeigt die aktuellen Einstellungen für den Anzeigemodus an, einschließlich Modus, Auflösung und Frequenz.

#### -a, --audio

Zeigt die aktuellen Einstellungen für den Audiomodus an, einschließlich Kanäle, Abtastrate und Abtastgröße.

#### -d, --dumpid=Dateiname

Speichern Sie die aktuelle EDID unter dem angegebenen Dateinamen. Sie können dann `edidparser <Dateiname>` verwenden, um die Daten in einer für Menschen lesbaren Form anzuzeigen.

#### -j, --json

In Kombination mit den `--modes`-Optionen werden die Modusinformationen im JSON-Format angezeigt.

#### -n, --name

Extrahiert den Anzeigenamen aus den EDID-Daten und zeigt ihn an.

