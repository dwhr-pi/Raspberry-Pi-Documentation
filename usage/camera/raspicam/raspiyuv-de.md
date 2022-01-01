# raspiyuv

`raspiyuv` hat die gleichen Funktionen wie `raspistill`, aber anstatt Standard-Bilddateien wie `.jpg`s auszugeben, generiert es YUV420- oder RGB888-Bilddateien aus der Ausgabe des Kamera-ISPs.

In den meisten Fällen ist die Verwendung von "raspistill" die beste Option für die Standardbildaufnahme, aber die Verwendung von YUV kann unter bestimmten Umständen von Vorteil sein. Wenn Sie beispielsweise nur ein unkomprimiertes Schwarzweißbild für Computer Vision-Anwendungen benötigen, können Sie einfach den Y-Kanal einer YUV-Aufnahme verwenden.

Es gibt einige spezifische Punkte zu den YUV420-Dateien, die erforderlich sind, um sie richtig zu verwenden. Der Zeilenabstand (oder Pitch) ist ein Vielfaches von 32, und jede YUV-Ebene ist ein Vielfaches von 16 in der Höhe. Dies kann bedeuten, dass je nach Auflösung des aufgenommenen Bildes zusätzliche Pixel am Zeilenende oder Lücken zwischen den Ebenen vorhanden sein können. Diese Lücken sind ungenutzt.

Siehe [diese Seite](./raw.md) für weitere Details.