# Speicheroptionen in config.txt

## gpu_mem

Gibt an, wie viel Speicher in Megabyte für die ausschließliche Verwendung der GPU reserviert werden soll: Der verbleibende Speicher wird der ARM-CPU zur Verwendung durch das Betriebssystem zugewiesen. Für Pis mit weniger als 1 GB Speicher ist die Standardeinstellung `64`; für Pis mit 1 GB oder mehr Speicher ist die Standardeinstellung `76`.

Der der GPU zugewiesene Speicher wird für Anzeige-, 3D-, Codec- und Kamerazwecke sowie für einige grundlegende Firmware-Verwaltung verwendet. Bei den unten angegebenen Höchstwerten wird davon ausgegangen, dass Sie alle diese Funktionen verwenden. Ist dies nicht der Fall, können kleinere Werte von gpu_mem verwendet werden.

Um die beste Leistung von Linux zu gewährleisten, sollten Sie `gpu_mem` auf den niedrigstmöglichen Wert setzen. Wenn eine bestimmte Grafikfunktion nicht richtig funktioniert, versuchen Sie, den Wert von `gpu_mem` zu erhöhen und beachten Sie dabei die unten aufgeführten empfohlenen Höchstwerte. Im Gegensatz zu GPUs auf x86-Rechnern, bei denen eine Erhöhung des Arbeitsspeichers die 3D-Leistung verbessern kann, bedeutet die Architektur des VideoCore **keinen Leistungsvorteil durch die Angabe größerer Werte als notwendig**, und tatsächlich kann dies die Leistung beeinträchtigen.

Auf dem Raspberry Pi 4 verfügt die 3D-Komponente der GPU über eine eigene Memory Management Unit (MMU) und verwendet keinen Speicher aus der `gpu_mem`-Belegung. Stattdessen wird Speicher dynamisch innerhalb von Linux zugewiesen. Dadurch kann beim Pi 4 ein kleinerer Wert für `gpu_mem` angegeben werden, verglichen mit früheren Modellen.

Die empfohlenen Höchstwerte sind wie folgt:

| Gesamt-RAM | `gpu_mem` empfohlenes Maximum |
|-----------|-------------------------------|
| 256 MB | `128` |
| 512 MB | `384` |
| 1 GB oder mehr | `512`, `256` auf dem Pi4 |

Es ist möglich, `gpu_mem` auf größere Werte zu setzen, dies sollte jedoch vermieden werden, da dies zu Problemen führen kann, z. B. das Booten von Linux verhindern. Der Mindestwert ist '16', dies deaktiviert jedoch bestimmte GPU-Funktionen.

Sie können auch `gpu_mem_256`, `gpu_mem_512` und `gpu_mem_1024` verwenden, um den Austausch derselben SD-Karte zwischen Pis mit unterschiedlicher RAM-Größe zu ermöglichen, ohne jedes Mal `config.txt` bearbeiten zu müssen:

## gpu_mem_256

Der Befehl `gpu_mem_256` setzt den GPU-Speicher in Megabyte für Raspberry Pis mit 256 MB Speicher. (Wird ignoriert, wenn die Speichergröße nicht 256 MB beträgt). Dies überschreibt `gpu_mem`.

## gpu_mem_512

Der Befehl `gpu_mem_512` setzt den GPU-Speicher in Megabyte für Raspberry Pis mit 512 MB Speicher. (Wird ignoriert, wenn die Speichergröße nicht 512 MB beträgt). Dies überschreibt `gpu_mem`.

## gpu_mem_1024

Der Befehl `gpu_mem_1024` setzt den GPU-Speicher in Megabyte für Raspberry Pis mit 1GB oder mehr Speicher. (Wird ignoriert, wenn die Speichergröße kleiner als 1 GB ist). Dies überschreibt `gpu_mem`.

## total_mem

Dieser Parameter kann verwendet werden, um einen Raspberry Pi zu zwingen, seine Speicherkapazität zu begrenzen: Geben Sie die Gesamtmenge an RAM in Megabyte an, die der Pi verwenden soll. Damit sich beispielsweise ein Raspberry Pi 4B mit 4 GB wie ein 1-GB-Modell verhält, verwenden Sie Folgendes:

```
total_mem=1024
```

Dieser Wert wird zwischen einem Minimum von 128 MB und einem Maximum des gesamten auf der Platine installierten Speichers festgelegt.

##disable_l2cache

Das Setzen auf `1` deaktiviert den Zugriff der CPU auf den L2-Cache der GPU und erfordert einen entsprechenden deaktivierten L2-Kernel. Der Standardwert beim BCM2835 ist '0'. Bei BCM2836, BCM2837 und BCM2711 haben die ARMs ihren eigenen L2-Cache und daher ist der Standardwert "1". Die standardmäßigen Pi-Builds kernel.img und kernel7.img spiegeln diesen Unterschied in den Cache-Einstellungen wider.

*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses/bis-sa/3.0/) geteilt wird.*