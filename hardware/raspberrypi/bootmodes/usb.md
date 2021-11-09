# USB-Boot-Modi

Es gibt zwei separate Boot-Modi für USB (nur bei bestimmten Modellen verfügbar):

* [USB-Gerätestart](device.md)
* [USB-Host-Boot](host.md) mit Boot-Optionen:
  * [USB-Massenspeicherboot](msd.md)
  * [Netzwerkstart](net.md)

Die Wahl zwischen den beiden Boot-Modi trifft die Firmware beim Booten, wenn sie die OTP-Bits liest. Es gibt zwei Bits zur Steuerung des USB-Boots: Das erste aktiviert das Booten von USB-Geräten und ist standardmäßig aktiviert. Die zweite aktiviert den USB-Host-Boot; Wenn das USB-Host-Boot-Modus-Bit gesetzt ist, liest der Prozessor den OTGID-Pin, um zu entscheiden, ob er als Host (auf Null gefahren wie beim Raspberry Pi Model B) oder als Gerät (links schwebend) gestartet werden soll. Der Pi Zero hat Zugriff auf diesen Pin über den OTGID-Pin am USB-Anschluss, und das Compute-Modul hat Zugriff auf diesen Pin am Edge-Anschluss.

Es gibt auch OTP-Bits, die es ermöglichen, bestimmte GPIO-Pins zu verwenden, um auszuwählen, welche Boot-Modi der Pi verwenden soll.