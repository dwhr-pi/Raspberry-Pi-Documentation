# Kernel-Header

Wenn Sie ein Kernel-Modul oder ähnliches kompilieren, benötigen Sie die Linux-Kernel-Header. Diese stellen die verschiedenen Funktions- und Strukturdefinitionen bereit, die beim Kompilieren von Code erforderlich sind, der mit dem Kernel verbunden ist.

Wenn Sie den gesamten Kernel von github geklont haben, sind die Header bereits im Quellbaum enthalten. Wenn Sie nicht alle zusätzlichen Dateien benötigen, können Sie nur die Kernel-Header aus dem Raspberry Pi OS-Repository installieren.

```
sudo apt install raspberrypi-kernel-headers
```
Beachten Sie, dass es eine Weile dauern kann, bis dieser Befehl abgeschlossen ist, da viele kleine Dateien installiert werden. Es gibt keine Fortschrittsanzeige.

Wenn eine neue Kernel-Version erstellt wird, benötigen Sie die Header, die dieser Kernel-Version entsprechen. Es kann mehrere Wochen dauern, bis das Repository auf die neueste Kernel-Version aktualisiert ist. In diesem Fall ist es am besten, den Kernel zu klonen, wie im [Build-Abschnitt] (building.md) beschrieben.
