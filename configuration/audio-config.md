# Audiokonfiguration

Der Raspberry Pi verfügt über bis zu drei Audioausgabemodi: HDMI 1 und 2, falls vorhanden, und eine Kopfhörerbuchse. Sie können jederzeit zwischen diesen Modi wechseln.

Wenn Ihr HDMI-Monitor oder Fernseher über integrierte Lautsprecher verfügt, kann der Ton über das HDMI-Kabel wiedergegeben werden, Sie können ihn jedoch auf einen Kopfhörer oder andere Lautsprecher umschalten, die an die Kopfhörerbuchse angeschlossen sind. Wenn Ihr Display Lautsprecher behauptet, wird der Ton standardmäßig über HDMI ausgegeben; falls nicht, erfolgt die Ausgabe über die Kopfhörerbuchse. Dies ist möglicherweise nicht die gewünschte Ausgangskonfiguration oder die automatische Erkennung ist ungenau. In diesem Fall können Sie den Ausgang manuell umschalten.

## Audioausgang ändern

Es gibt zwei Möglichkeiten, die Audioausgabe einzustellen.

### Desktop-Lautstärkeregelung

Wenn Sie mit der rechten Maustaste auf das Lautstärkesymbol in der Desktop-Taskleiste klicken, wird die Audioausgabeauswahl angezeigt. Auf diese Weise können Sie zwischen den internen Audioausgängen wählen. Außerdem können Sie beliebige externe Audiogeräte wie USB-Soundkarten und Bluetooth-Audiogeräte auswählen. Ein grünes Häkchen wird neben dem aktuell ausgewählten Audioausgabegerät angezeigt – klicken Sie einfach mit der linken Maustaste auf die gewünschte Ausgabe im Popup-Menü, um dies zu ändern. Der Lautstärkeregler und die Stummschaltung funktionieren auf dem aktuell ausgewählten Gerät.

### raspi-config

Öffnen Sie [raspi-config](raspi-config.md), indem Sie Folgendes in die Befehlszeile eingeben:

```
sudo raspi-config
```

Dies öffnet den Konfigurationsbildschirm:

Wählen Sie `System Options` (derzeit Option 1, aber Ihre können anders sein) und drücken Sie `Enter`.

Wählen Sie nun die Option mit dem Namen `Audio` (derzeit Option S2, aber Ihre kann anders sein) und drücken Sie `Enter`:

Wählen Sie Ihren gewünschten Modus, drücken Sie 'Enter' und drücken Sie die rechte Pfeiltaste, um die Optionsliste zu verlassen, und wählen Sie dann 'Finish', um das Konfigurationstool zu verlassen.

Nachdem Sie Ihre Audioeinstellungen geändert haben, müssen Sie Ihren Raspberry Pi neu starten, damit Ihre Änderungen wirksam werden.


## Wenn Sie immer noch keinen Ton über HDMI erhalten

In einigen seltenen Fällen ist es notwendig, `config.txt` zu bearbeiten, um den HDMI-Modus zu erzwingen (im Gegensatz zum DVI-Modus, der keinen Ton sendet). Sie können dies tun, indem Sie `/boot/config.txt` bearbeiten und `hdmi_drive=2` setzen und dann neu starten, damit die Änderung wirksam wird.