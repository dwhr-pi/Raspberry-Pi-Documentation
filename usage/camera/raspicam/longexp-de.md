# Langzeitbelichtungen

Die verschiedenen Kameramodule haben unterschiedliche Fähigkeiten hinsichtlich der Belichtungszeiten:

| Modul | Max. Belichtung (Sekunden) |
| - | :-: |
|V1 (OMx5647) | 6 |
|V2 (IMX219) | 10 |
|HQ (IMX417) | 230 |



Aufgrund der Funktionsweise des ISP kann die Anforderung einer langen Belichtung standardmäßig dazu führen, dass der Aufnahmeprozess bis zu 7-mal so lange dauert wie die Belichtungszeit, sodass eine 200-Sekunden-Belichtung mit der HQ-Kamera 1400 Sekunden dauern kann, um tatsächlich ein Bild zurückzugeben. Dies liegt an der Art und Weise, wie das Kamerasystem die richtigen Belichtungen und Verstärkungen für das Bild ermittelt, indem es seine Algorithmen AGC (automatische Verstärkungsregelung) und AWB (automatischer Weißabgleich) verwendet. Das System benötigt einige Frames, um diese Zahlen zu berechnen, um ein anständiges Bild zu produzieren. In Kombination mit dem Verwerfen von Frames zu Beginn der Verarbeitung (falls sie beschädigt sind) und dem Wechsel zwischen Vorschau- und Aufnahmemodus kann dies dazu führen, dass bis zu 7 Frames benötigt werden, um ein endgültiges Bild zu erstellen. Bei Langzeitbelichtungen kann das lange dauern.

Glücklicherweise können die Kameraparameter geändert werden, um die Bildzeit drastisch zu reduzieren. Dies bedeutet jedoch, die automatischen Algorithmen abzuschalten und manuell Werte für die AGC und ggf. AWB bereitzustellen. Darüber hinaus kann ein Burst-Modus verwendet werden, um die Auswirkungen des Wechsels zwischen Vorschau- und Aufnahmemodus zu mildern.

Für die HQ-Kamera nimmt das folgende Beispiel eine 100-Sekunden-Belichtung auf.

`raspistill -t 10 -bm -ex off -ag 1 -ss 100000000 -st -o long_exposure.jpg`

In diesem Beispiel wird der Burst-Modus (`-bm`) aktiviert, wodurch das Umschalten der Vorschau deaktiviert, die automatische Verstärkungsregelung deaktiviert und manuell auf 1 (`-ag 1`) gesetzt wird. Die Option `-st` erzwingt die Berechnung von Statistiken wie AWB aus dem erfassten Frame, wodurch die Notwendigkeit entfällt, spezifische Werte anzugeben, obwohl diese bei Bedarf eingegeben werden können.


