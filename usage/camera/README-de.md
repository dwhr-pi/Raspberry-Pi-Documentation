# Kameramodule

Raspberry Pi verkauft derzeit zwei Arten von Kameraplatinen: ein [8MP-Gerät](https://www.raspberrypi.org/products/camera-module-v2/) und ein [12MP High Quality (HQ)](https:// www.raspberrypi.org/products/raspberry-pi-high-quality-camera/) Kamera. Das 8MP-Gerät ist auch in [NoIR-Form](https://www.raspberrypi.org/products/pi-noir-camera-v2/) ohne IR-Filter erhältlich. Das ursprüngliche 5MP-Gerät ist vom Raspberry Pi nicht mehr erhältlich. Die Spezifikationen aller Geräte finden Sie [hier](../../hardware/camera/README.md).

Alle Raspberry Pi-Kameras können hochauflösende Fotos zusammen mit Full-HD-1080p-Videos aufnehmen und können vollständig programmgesteuert gesteuert werden. Diese Dokumentation beschreibt die Verwendung der Kamera in verschiedenen Szenarien und die Verwendung der verschiedenen Softwaretools.

Informationen zur Installation finden Sie auf [dieser Seite](./installing.md).


## Grundlegende Kameranutzung

Nach der Installation gibt es verschiedene Möglichkeiten, die Kameras zu verwenden. Die einfachste Möglichkeit besteht darin, eine der bereitgestellten Kameraanwendungen zu verwenden. Standardmäßig sind vier Linux-Befehlszeilenanwendungen installiert (z. B. `raspistill`): deren Verwendung wird auf [dieser Seite](raspicam/README.md) beschrieben.

Sie können auch programmgesteuert auf die Kamera zugreifen, indem Sie die [Python-Programmiersprache](python/README.md) verwenden, indem Sie die [`picamera`-Bibliothek](https://projects.raspberrypi.org/en/projects/getting-started-with -Pikamera).


## Erweiterte Kameranutzung

Erweiterte Funktionen sowie einige Hinweise und Tipps werden auf den folgenden Seiten beschrieben:

- [Verwenden von RAW](./raspicam/raw.md)
- [Langzeitbelichtungen](./raspicam/longexp.md)
- [Direktzugriff auf Sensoren](./raspicam/direct.md)
- [Verwenden von V4L2 zum Zugriff auf die Kamera (z. B. Verwenden von Pi-Kameras als Webcams)](./raspicam/v4l2.md)
- [Entfernen des IR-Filters der HQ-Kamera](../../hardware/camera/hqcam_filter_removal.md)

## libcamera - Die Zukunft der Raspberry Pi-Kamerasoftware

libcamera ist eine neue Linux-API für die Schnittstelle zu Kameras. Raspberry Pi war an der Entwicklung von libcamera beteiligt und nutzt dieses ausgeklügelte System nun für neue Kamerasoftware. Dies bedeutet, dass sich Raspberry Pi von der Firmware-basierten Kamera-Bildverarbeitungspipeline (ISP) zu einem offeneren System bewegt.

- [Hauptwebsite von Libcamera](http://libcamera.org/).
- [Raspberry Pi libcamera-Implementierung](../../linux/software/libcamera/README.md).
- [Tuning-Anleitung für die Raspberry Pi-Kameras und libcamera](../../linux/software/libcamera/rpi_SOFT_libcamera_1p0.pdf)
- [Schreiben eines Kernelmoduls zur Unterstützung einer neuen Kamera oder eines Aufnahmechips](../../linux/software/libcamera/csi-2-usage.md)



