## Der V4L2-Treiber

Der V4L2-Treiber bietet einen Standard-Linux-Treiber für den Zugriff auf Kamerafunktionen: Dies ist der Treiber, der benötigt wird, um eine Raspberry Pi-Kamera beispielsweise als Webcam zu verwenden. Der V4L2-Treiber bietet zusätzlich zum Firmware-basierten Kamerasystem eine Standard-API.

### Installieren des V4L2-Treibers

Die Installation des V4L2-Treibers erfolgt automatisch. Es wird als untergeordnetes Element des VCHIQ-Treibers geladen und überprüft nach dem Laden, wie viele Kameras angeschlossen sind, und erstellt dann die richtige Anzahl von Geräteknoten.

| \dev\videoX | Standardaktion |
|-------------|:--------------:|
| video10 | Entschlüsseln |
| video11 | Codieren |
| video12 | Einfacher ISP |
| video13 | Vollständiger ISP-Eingang |
| video14 | Full ISP Hi-Res Out |
| video15 | Vollständiger ISP-Ausgang mit niedriger Auflösung | |
| video16 | Vollständige ISP-Statistiken |
| video19 | HEVC-Dekodierung |

### Treiber testen

Es gibt viele Linux-Anwendungen, die die V4L2-API verwenden. Die Kernel-Maintainer stellen ein Testtool namens `Qv4l2` bereit, das wie folgt aus den Raspberry Pi OS-Repositorys installiert werden kann:

`sudo apt install Qv4l2`

### Den Treiber verwenden

Weitere Informationen zur Verwendung dieses Treibers finden Sie in der [V4L2-Dokumentation](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/v4l2.html).