## So drehen Sie Ihr Display

Die Optionen zum Drehen des Displays Ihres Raspberry Pi hängen davon ab, welche Bildschirmtreibersoftware darauf ausgeführt wird, was auch davon abhängen kann, welchen Raspberry Pi Sie verwenden.

### Gefälschter oder vollständiger KMS-Grafiktreiber (Standard auf Pi4)

Wenn Sie den Raspberry Pi-Desktop ausführen, wird die Rotation mit dem "Screen Configuration Utility" aus dem Desktop-Menü "Preferences" erreicht. Dadurch wird eine grafische Darstellung des Displays oder der Displays angezeigt, die mit dem Raspberry Pi verbunden sind. Klicken Sie mit der rechten Maustaste auf die Anzeige, die Sie drehen möchten, und wählen Sie die gewünschte Option aus.

Es ist auch möglich, diese Einstellungen mit der Befehlszeilenoption `xrandr` zu ändern. Die folgenden Befehle geben jeweils 0°, -90°, +90° und 180° Drehungen.

```
xrandr --output HDMI-1 --normal drehen
xrandr --output HDMI-1 --nach links drehen
xrandr --output HDMI-1 --nach rechts drehen
xrandr --output HDMI-1 --invertiert drehen
```

Beachten Sie, dass der Eintrag `--output` angibt, für welches Gerät die Rotation gilt. Sie können den Gerätenamen bestimmen, indem Sie einfach `xrandr` in die Befehlszeile eingeben, die Informationen einschließlich des Namens für alle angeschlossenen Geräte anzeigt.

Sie können auch die Befehlszeile verwenden, um die Anzeige mit der Option `--reflect` zu spiegeln. Die Reflexion kann 'normal', 'x', 'y' oder 'xy' sein. Dadurch wird der Ausgabeinhalt über die angegebenen Achsen gespiegelt. Z.B.

```
bash
xrandr --Ausgang HDMI-1 --reflektieren x
```

Wenn Sie nur die Konsole verwenden (kein grafischer Desktop), müssen Sie die entsprechenden Kernel-Befehlszeilen-Flags setzen. Ändern Sie die Konsoleneinstellungen wie auf [dieser Seite](./cmdline-txt.md) beschrieben.

### Legacy-Grafiktreiber (Standard bei Modellen vor dem Pi4)

Es gibt `config.txt`-Optionen zum Rotieren, wenn die älteren Anzeigetreiber verwendet werden.

`display_hdmi_rotate` wird verwendet, um das HDMI-Display zu drehen, `display_lcd_rotate` wird verwendet, um jedes angeschlossene LCD-Panel zu drehen (unter Verwendung der DSI- oder DPI-Schnittstelle). Diese Optionen drehen sowohl den Desktop als auch die Konsole. Jede Option benötigt einen der folgenden Parameter:

| display_*_rotate | Ergebnis |
| --- | --- |
| 0 | keine Drehung |
| 1 | 90 Grad im Uhrzeigersinn drehen* |
| 2 | 180 Grad im Uhrzeigersinn drehen |
| 3 | 270 Grad im Uhrzeigersinn drehen* |
| 0x10000 | horizontaler Flip |
| 0x20000 | vertikaler Flip |

*) Beachten Sie, dass die 90- und 270-Grad-Rotationsoptionen zusätzlichen Speicher auf der GPU erfordern, sodass diese nicht mit der 16-MB-GPU-Aufteilung funktionieren.

Sie können die Rotationseinstellungen mit den Flips kombinieren, indem Sie sie zusammenfügen. Sie können auf die gleiche Weise auch horizontale und vertikale Flips durchführen. Z.B. Eine 180-Grad-Drehung mit vertikalem und horizontalem Flip ist 0x20000 + 0x10000 + 2 = 0x30002.