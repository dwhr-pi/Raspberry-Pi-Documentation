# Lizenzschlüssel und Codec-Optionen in config.txt

Die Hardware-Decodierung zusätzlicher Codecs auf dem Pi 3 und früheren Modellen kann durch [Kauf einer Lizenz] (http://swag.raspberrypi.org/collections/software) aktiviert werden, die an die CPU-Seriennummer Ihres Raspberry Pi gebunden ist.

Auf dem Raspberry Pi 4 sind die Hardware-Codecs für MPEG2 oder VC1 dauerhaft deaktiviert und können auch mit einem Lizenzschlüssel nicht aktiviert werden; auf dem Pi 4 können MPEG2 und VC1 dank seiner im Vergleich zu früheren Modellen erhöhten Verarbeitungsleistung in Software über Anwendungen wie VLC dekodiert werden. Daher ist kein Hardware-Codec-Lizenzschlüssel erforderlich, wenn Sie einen Pi 4 verwenden.

## decode_MPG2

`decode_MPG2` ist ein Lizenzschlüssel um Hardware MPEG-2 Dekodierung zu ermöglichen, z.B. `decode_MPG2=0x12345678`.

##decode_WVC1

`decode_WVC1` ist ein Lizenzschlüssel, um Hardware-VC-1-Decodierung zu ermöglichen, z.B. `decode_WVC1=0x12345678`.

Wenn Sie mehrere Raspberry Pis haben und für jeden eine Codec-Lizenz gekauft haben, können Sie bis zu acht Lizenzschlüssel in einer einzigen `config.txt` auflisten, zum Beispiel `decode_MPG2=0x12345678,0xabcdabcd,0x87654321`. Dies ermöglicht es Ihnen, dieselbe SD-Karte zwischen den verschiedenen Pis auszutauschen, ohne jedes Mal die `config.txt` bearbeiten zu müssen.




*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses .) geteilt wird /bis-sa/3.0/)*