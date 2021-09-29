# Übertaktungsoptionen in config.txt

**HINWEIS:** Das Setzen von Übertaktungsparametern auf andere als die von [raspi-config](../raspi-config.md#overclock) verwendeten Werte kann ein permanentes Bit im SoC setzen, sodass erkannt werden kann, dass Ihr Pi wurde übertaktet. Die spezifischen Umstände, unter denen das Übertaktungsbit gesetzt wird, sind, wenn 'force_turbo' auf '1' gesetzt ist und eine der 'over_voltage_*'-Optionen auf einen Wert > '0' gesetzt ist. Weitere Informationen finden Sie im [Blog-Post zum Turbo-Modus](https://www.raspberrypi.org/blog/introducing-turbo-mode-up-to-50-more-performance-for-free/). **Pi 4**-Übertaktungsoptionen können sich in Zukunft ändern.

Der neueste Kernel hat einen [cpufreq](http://www.pantz.org/software/cpufreq/usingcpufreqonlinux.html) Kernel-Treiber mit standardmäßig aktiviertem "ondemand"-Governor. Es hat keine Auswirkung, wenn Sie keine Übertaktungseinstellungen haben, aber wenn Sie übertakten, variiert die CPU-Frequenz mit der Prozessorlast. Von den Standardwerten abweichende Werte werden nach Angaben des Gouverneurs nur bei Bedarf verwendet. Sie können die Mindestwerte mit den Konfigurationsoptionen `*_min` anpassen (es werden nur Werte niedriger als die Standardeinstellung verwendet) oder dynamisches Takten deaktivieren (und Übertakten erzwingen) mit `force_turbo=1`. Für weitere Informationen [siehe diesen Abschnitt der Dokumentation](../../hardware/raspberrypi/frequency-management.md).

Übertaktung und Überspannung werden zur Laufzeit deaktiviert, wenn der SoC 85 °C erreicht, um den SoC abzukühlen. Bei Raspberry Pi Model 1 oder 2 sollten Sie diese Grenze nicht erreichen, eher bei Raspberry Pi 3 und Raspberry Pi 4B. Für weitere Informationen [siehe diesen Abschnitt der Dokumentation](../../hardware/raspberrypi/frequency-management.md). Übertaktung und Überspannung werden ebenfalls deaktiviert, wenn eine Unterspannungssituation erkannt wird.

## Übertaktungsoptionen

| Option | Beschreibung |
| --- | --- |
| arm_freq | Frequenz der ARM-CPU in MHz. |
| gpu_freq | Setzt `core_freq`, `h264_freq`, `isp_freq`, `v3d_freq` und `hevc_freq` zusammen |
| core_freq | Die Frequenz des GPU-Prozessorkerns in MHz beeinflusst die CPU-Leistung, da sie den L2-Cache und den Speicherbus antreibt; der L2-Cache profitiert nur von Pi Zero/Pi Zero W/ Pi 1, es gibt einen kleinen Vorteil für SDRAM auf Pi 2/Pi 3. Siehe Abschnitt unten für die Verwendung auf dem Pi 4.|
| h264_freq | Frequenz des Hardware-Videoblocks in MHz; individuelle Überschreibung der `gpu_freq`-Einstellung |
| isp_freq | Frequenz des Bildsensor-Pipeline-Blocks in MHz; individuelle Überschreibung der `gpu_freq`-Einstellung |
| v3d_freq | Frequenz des 3D-Blocks in MHz; individuelle Überschreibung der `gpu_freq`-Einstellung |
| hevc_freq | Frequenz des High Efficiency Video Codec Blocks in MHz; individuelle Überschreibung der `gpu_freq`-Einstellung. Nur Pi4. |
| sdram_freq | Frequenz des SDRAM in MHz. SDRAM-Übertaktung auf Pi 4B wird derzeit nicht unterstützt|
| Überspannung | Anpassung der CPU/GPU-Kernspannung. Der Wert sollte im Bereich [-16, 8] liegen, was dem Bereich [0,8V, 1,4V] mit 0,025V Schritten entspricht. Mit anderen Worten, die Angabe von -16 ergibt 0,8 V als GPU/Kernspannung und die Angabe von 8 ergibt 1,4 V. Für Standardwerte siehe Tabelle unten. Werte über 6 sind nur bei Angabe von `force_turbo` erlaubt: dies setzt das Garantiebit, wenn auch `over_voltage_*` gesetzt ist. |
| over_voltage_sdram | Setzt `over_voltage_sdram_c`, `over_voltage_sdram_i` und `over_voltage_sdram_p` zusammen. |
| over_voltage_sdram_c | Spannungsanpassung des SDRAM-Controllers. [-16,8] entspricht [0.8V,1.4V] mit 0.025V Schritten. |
| over_voltage_sdram_i | Anpassung der SDRAM-E/A-Spannung. [-16,8] entspricht [0.8V,1.4V] mit 0.025V Schritten. |
| over_voltage_sdram_p | SDRAM-Phy-Spannungsanpassung. [-16,8] entspricht [0.8V,1.4V] mit 0.025V Schritten. |
| force_turbo | Erzwingt Turbomodus-Frequenzen, auch wenn die ARM-Kerne nicht ausgelastet sind. Die Aktivierung kann das Garantiebit setzen, wenn auch `over_voltage_*` gesetzt ist. |
| initial_turbo | Aktiviert den Turbo-Modus vom Booten für den angegebenen Wert in Sekunden oder bis cpufreq eine Frequenz festlegt. Weitere Informationen [siehe hier](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=6201&start=425#p180099). Der Maximalwert ist '60'. |
| arm_freq_min | Mindestwert von `arm_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| core_freq_min | Mindestwert von `core_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| gpu_freq_min | Mindestwert von `gpu_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| h264_freq_min | Mindestwert von `h264_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| isp_freq_min | Mindestwert von `isp_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| v3d_freq_min | Mindestwert von `v3d_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| hevc_freq_min | Mindestwert von `hevc_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| sdram_freq_min | Mindestwert von `sdram_freq`, der für die dynamische Frequenztaktung verwendet wird. |
| Überspannung_min | Minimalwert von `over_voltage`, der für die dynamische Frequenztaktung verwendet wird. |
| temp_limit | Überhitzungsschutz. Dadurch werden die Takte und Spannungen auf Standard gesetzt, wenn der SoC diesen Wert in Celsius erreicht. Werte über 85 werden auf 85 geklemmt. |
| temp_soft_limit | **Nur 3A+/3B+**. Steuerung der CPU-Geschwindigkeitsdrosselung. Dies legt die Temperatur fest, bei der das Drosselungssystem für den CPU-Takt aktiviert wird. Bei dieser Temperatur wird die Taktfrequenz von 1400MHz auf 1200MHz reduziert. Der Standardwert ist '60', kann auf maximal '70' erhöht werden, dies kann jedoch zu Instabilität führen. |

In dieser Tabelle sind die Standardwerte für die Optionen bei verschiedenen Raspberry Pi Modellen angegeben, alle Frequenzen sind in MHz angegeben.

| Option | Pi 0/W | Pi1 | Pi2 | Pi3 | Pi3A+/Pi3B+ | Pi4 |
| --- | :---: | :---: | :---: | :----: | :-----: | :----: |
| arm_freq | 1000 | 700 | 900 | 1200 | 1400 | 1500 |
| core_freq | 400 | 250 | 250 | 400 | 400 | 500/550/360 |
| h264_freq | 300 | 250 | 250 | 400 | 400 | 500/550/360 |
| isp_freq | 300 | 250 | 250 | 400 | 400 | 500/550/360 |
| v3d_freq | 300 | 250 | 250 | 400 | 400 | 500/550/360 |
| hevc_freq | Nicht zutreffend | k.A. | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend | 500/550/360 |
| sdram_freq | 450 | 400 | 450 | 450 | 500 | 3200 |
| arm_freq_min | 700 | 700 | 600 | 600 | 600 | 600 |
| core_freq_min| 250 | 250 | 250 | 250 | 250 | 250/275 |
| gpu_freq_min | 250 | 250 | 250 | 250 | 250 | 500 |
| h264_freq_min| 250 | 250 | 250 | 250 | 250 | 500 |
| isp_freq_min | 250 | 250 | 250 | 250 | 250 | 500 |
| v3d_freq_min | 250 | 250 | 250 | 250 | 250 | 500 |
| sdram_freq_min |400 | 400 | 400 | 400 | 400 | 400 |

Diese Tabelle enthält Standardwerte für Optionen, die für alle Modelle gleich sind.

| Option | Standard |
| --- | :---: |
| initial_turbo | 0 |
| Überspannung_min | 0 |
| temp_limit | 85 |
| over_voltage_sdram_c | 0 (1,2 V) |
| over_voltage_sdram_i | 0 (1,2 V) |
| over_voltage_sdram_p | 0 (1,2 V) |

Diese Tabelle listet die Standardeinstellungen von `over_voltage` für die verschiedenen Pi-Modelle auf. Die Firmware verwendet Adaptive Voltage Scaling (AVS), um die optimale einzustellende Spannung zu bestimmen. Beachten Sie, dass für jeden ganzzahligen Anstieg von "over_voltage" die Spannung um 25 mV höher ist.

| Modell | Standard | Resultierende Spannung |
| --- | --- | --- |
| Pi 1 | 0 | 1,2V |
| Pi 2 | 0 | 1,2-1.3125V |
| Pi 3 | 0 | 1,2-1.3125V |
| Pi Null | 6 | 1,35V |

#### Spezifisch für Pi 4B

Die `core_freq` des Raspberry Pi 4 kann sich von der Standardeinstellung ändern, wenn entweder `hdmi_enable_4kp60` oder `enable_tvout` verwendet wird, aufgrund der Beziehung zwischen den internen Uhren und den besonderen Anforderungen der angeforderten Anzeigemodi.

| Anzeigeoption | Frequenz |
| -------------- | --------: |
| Standard | 500 |
| enable_tvout | 360 |
| hdmi_enable_4kp60 | 550 |

Das Ändern von `core_freq` in `config.txt` wird auf dem Pi 4 nicht unterstützt, jede Änderung gegenüber der Standardeinstellung führt mit ziemlicher Sicherheit zu einem Fehler beim Booten.

Es wird empfohlen beim Übertakten die individuellen Frequenzeinstellungen (`isp_freq`, `v3d_freq` etc) statt `gpu_freq` zu verwenden, da es versucht, `core_freq` einzustellen (was beim Pi 4 nicht geändert werden kann) wahrscheinlich den gewünschten Effekt haben.

### force_turbo

Standardmäßig (`force_turbo=0`) erhöht der "On Demand" CPU-Frequenztreiber die Takte auf ihre maximalen Frequenzen, wenn die ARM-Kerne beschäftigt sind, und senkt sie auf die minimalen Frequenzen, wenn die ARM-Kerne im Leerlauf sind.

`force_turbo=1` überschreibt dieses Verhalten und erzwingt maximale Frequenzen, auch wenn die ARM-Kerne nicht ausgelastet sind.

### never_over_voltage

Setzt ein Bit im OTP-Speicher (einmalig programmierbar), das eine Überspannung des Geräts verhindert. Damit soll das Gerät gesperrt werden, damit das Garantiebit weder versehentlich noch böswillig durch eine ungültige Überspannung gesetzt werden kann.

###disable_auto_turbo

Auf Pi 2/Pi 3 verhindert das Setzen dieses Flags, dass die GPU in den Turbo-Modus wechselt, was in bestimmten Lastfällen möglich ist.

## Uhren Beziehung

GPU-Kern, CPU, SDRAM und GPU haben jeweils ihre eigenen PLLs und können unabhängige Frequenzen haben. Die Blöcke h264, v3d und ISP teilen sich eine PLL. Weitere Informationen [siehe hier](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=6201&start=275#p168042).

Um die aktuelle Frequenz des Pi in KHz anzuzeigen, geben Sie Folgendes ein: `cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq`. Teilen Sie das Ergebnis durch 1000, um den Wert in MHz zu ermitteln. Beachten Sie, dass diese Frequenz die vom Kernel *angeforderte* Frequenz ist, und es ist möglich, dass jede Drosselung (zum Beispiel bei hohen Temperaturen) bedeuten kann, dass die CPU tatsächlich langsamer läuft als angegeben. Eine momentane Messung der aktuellen ARM-CPU-Frequenz kann mit dem vcgencmd `vcgencmd measure_clock arm` abgerufen werden. Diese wird in Hertz angezeigt.

## Überwachung der Kerntemperatur

Um die Temperatur des Pi anzuzeigen, geben Sie `cat /sys/class/thermal/thermal_zone0/temp` ein. Teilen Sie das Ergebnis durch 1000, um den Wert in Grad Celsius zu erhalten. Alternativ gibt es einen vcgencmd, `vcgencmd measure_temp`, der die GPU direkt nach ihrer Temperatur abfragt.

Das Erreichen der Temperaturgrenze schadet dem SoC zwar nicht, führt jedoch zu CPU-Throttling. Ein Kühlkörper kann helfen, die Kerntemperatur und damit die Leistung zu kontrollieren. Dies ist besonders nützlich, wenn der Pi in einem Gehäuse läuft. Der Luftstrom über dem Kühlkörper sorgt für eine effizientere Kühlung.

Mit Firmware vom 12. September 2016 oder höher wird bei einer Kerntemperatur zwischen 80'C und 85'C ein Warnsymbol mit einem roten halbgefüllten Thermometer angezeigt und die ARM-Kerne werden gedrosselt. Wenn die Temperatur 85 °C überschreitet, wird ein Symbol mit einem vollständig gefüllten Thermometer angezeigt und sowohl die ARM-Kerne als auch die GPU werden gedrosselt.

Beim Raspberry Pi 3 Model B+ wurde die PCB-Technologie geändert, um eine bessere Wärmeableitung und eine erhöhte thermische Masse zu bieten. Darüber hinaus wurde ein weiches Temperaturlimit eingeführt, mit dem Ziel, die Zeit zu maximieren, die ein Gerät "sprinten" kann, bevor es das harte Limit bei 85°C erreicht. Bei Erreichen des Soft-Limits wird die Taktfrequenz von 1,4 GHz auf 1,2 GHz reduziert und die Betriebsspannung leicht reduziert. Dies reduziert die Temperaturanstiegsrate: Wir handeln einen kurzen Zeitraum mit 1,4 GHz für einen längeren Zeitraum mit 1,2 GHz. Standardmäßig beträgt das Softlimit 60°C und kann über die Einstellung `temp_soft_limit` in der config.txt geändert werden.

Weitere Informationen finden Sie auf der Seite [warning icons](../warning-icons.md).

## Überwachungsspannung

Für eine zuverlässige Leistung ist es wichtig, die Versorgungsspannung über 4,8 V zu halten. Beachten Sie, dass die Spannung von einigen USB-Ladegeräten/Netzteilen bis auf 4,2 V sinken kann. Dies liegt daran, dass sie normalerweise dafür ausgelegt sind, einen 3,7-V-LiPo-Akku aufzuladen, und nicht, um einen Computer mit 5 V zu versorgen.

Um die Netzteilspannung des Pi zu überwachen, müssen Sie ein Multimeter verwenden, um zwischen den VCC- und GND-Pins am GPIO zu messen. Weitere Informationen finden Sie in [power](../../hardware/raspberrypi/power/README.md).

Wenn die Spannung unter 4,63 V (+-5 %) sinkt, zeigen neuere Versionen der Firmware ein gelbes Blitzsymbol auf dem Display an, um auf fehlenden Strom hinzuweisen, und eine Meldung über den Unterspannungszustand wird dem Kernel hinzugefügt Protokoll.

Weitere Informationen finden Sie auf der Seite [warning icons](../warning-icons.md).

## Übertaktungsprobleme

Die meisten Übertaktungsprobleme treten sofort mit einem Fehler beim Booten auf. Halten Sie in diesem Fall beim nächsten Booten die Umschalttaste gedrückt. Dadurch werden alle Übertaktungen vorübergehend deaktiviert, sodass Sie erfolgreich booten und dann Ihre Einstellungen bearbeiten können.




*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses .) geteilt wird /bis-sa/3.0/)*