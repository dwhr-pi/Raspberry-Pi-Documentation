# Installation einer Raspberry Pi-Kamera

## Kamera anschließen

Das Flexkabel wird in den Anschluss mit der Bezeichnung CAMERA am Raspberry Pi eingesteckt, der sich zwischen den Ethernet- und HDMI-Ports befindet. Das Kabel muss so eingesteckt werden, dass die silbernen Kontakte zum HDMI-Port zeigen. Um den Anschluss zu öffnen, ziehen Sie die Laschen an der Oberseite des Anschlusses nach oben und dann in Richtung des Ethernet-Ports. Das Flexkabel sollte fest in den Stecker eingesteckt werden, wobei darauf zu achten ist, dass das Flexkabel nicht zu spitz wird. Um den Anschluss zu schließen, drücken Sie den oberen Teil des Anschlusses in Richtung des HDMI-Anschlusses und nach unten, während Sie das Flexkabel festhalten.

Wir haben ein Video erstellt, um den Vorgang des Anschließens der Kamera zu veranschaulichen. Obwohl das Video die Originalkamera auf dem Original Raspberry Pi 1 zeigt, ist das Prinzip für alle Kameraboards gleich:

[![Screenshot der Kameraverbindung](https://img.youtube.com/vi/GImeVqHQzsE/0.jpg)](http://www.youtube.com/watch?v=GImeVqHQzsE)

Je nach Modell kann die Kamera mit einem kleinen Stück durchscheinender blauer Plastikfolie geliefert werden, die das Objektiv bedeckt. Dieser dient nur zum Schutz der Linse während des Versands und muss durch vorsichtiges Abziehen entfernt werden.

## Kamera aktivieren

### Den Desktop verwenden

Wählen Sie im Desktop-Menü „Preferences“ und „Raspberry Pi Configuration“ aus: Ein Fenster wird angezeigt. Wählen Sie die Registerkarte "Schnittstellen" und klicken Sie dann auf die Option "Kamera aktivieren". Klicken Sie auf `OK`. Sie müssen neu starten, damit die Änderungen wirksam werden.

### Verwenden der Befehlszeile

Öffnen Sie das `raspi-config`-Tool vom Terminal aus:

```bash
sudo raspi-config
```

Wählen Sie `Schnittstellenoptionen`, dann `Kamera` und drücken Sie `Enter`. Wählen Sie `Ja` und dann `Ok`. Gehen Sie zu `Finish` und Sie werden zum Neustart aufgefordert.