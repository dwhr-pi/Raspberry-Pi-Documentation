# Raspberry Pi-Revisionscodes

Jede unterschiedliche Raspberry Pi-Modellrevision hat einen eindeutigen Revisionscode. Sie können den Revisionscode eines Raspberry Pi nachschlagen, indem Sie Folgendes ausführen:

```bash
cat /proc/cpuinfo
```

Die letzten drei Zeilen zeigen den Hardwaretyp, den Revisionscode und die eindeutige Seriennummer des Pi. Zum Beispiel:

```
Hardware    : BCM2835
Revision    : a02082
Serial      : 00000000765fc593
```

*Hinweis: Ab dem 4.9-Kernel melden alle Pis `BCM2835`, auch solche mit BCM2836-, BCM2837- und BCM2711-Prozessoren. Sie sollten diese Zeichenfolge nicht verwenden, um den Prozessor zu erkennen. Entschlüsseln Sie den Revisionscode mit den folgenden Informationen oder `cat /sys/firmware/devicetree/base/model`*

## Revisionscodes im alten Stil

Der erste Satz von Raspberry Pi-Modellen erhielt sequentielle Hex-Revisionscodes von „0002“ bis „0015“:

| Code | Modell | Revision | RAM            | Manufacturer |
| ---- | ------ | -------- | -------------- | ------------ |
| 0002 | B      | 1.0      | 256MB          | Egoman       |
| 0003 | B      | 1.0      | 256MB          | Egoman       |
| 0004 | B      | 2.0      | 256MB          | Sony UK      |
| 0005 | B      | 2.0      | 256MB          | Qisda        |
| 0006 | B      | 2.0      | 256MB          | Egoman       |
| 0007 | A      | 2.0      | 256MB          | Egoman       |
| 0008 | A      | 2.0      | 256MB          | Sony UK      |
| 0009 | A      | 2.0      | 256MB          | Qisda        |
| 000d | B      | 2.0      | 512MB          | Egoman       |
| 000e | B      | 2.0      | 512MB          | Sony UK      |
| 000f | B      | 2.0      | 512MB          | Egoman       |
| 0010 | B+     | 1.2      | 512MB          | Sony UK      |
| 0011 | CM1    | 1.0      | 512MB          | Sony UK      |
| 0012 | A+     | 1.1      | 256MB          | Sony UK      |
| 0013 | B+     | 1.2      | 512MB          | Embest       |
| 0014 | CM1    | 1.0      | 512MB          | Embest       |
| 0015 | A+     | 1.1      | 256MB/512MB    | Embest       |

## Revisionscodes im neuen Stil

Mit der Einführung des Raspberry Pi 2 wurden Revisionscodes im neuen Stil eingeführt. Anstatt sequenziell zu sein, stellt jedes Bit des Hex-Codes eine Information über die Revision dar:

```
NOQuuuWuFMMMCCCCPPPPTTTTTTTTRRRR
```

| Teil | Steht für | Optionen |
| -------- | ------------ | -------------------------- |
| N | Überspannung | 0: Überspannung erlaubt |
| | | 1: Überspannung unzulässig |
| O | OTP-Programm<sup>1</sup> | 0: OTP-Programmierung erlaubt |
| | | 1: OTP-Programmierung unzulässig |
| Q | OTP-Lesen<sup>1</sup> | 0: OTP-Lesen erlaubt |
| | | 1: OTP-Lesen nicht zulässig |
| uuu | Unbenutzt | Unbenutzt |
| W | Garantie-Bit | 0: Garantie ist intakt |
| | | 1: Garantie wurde ungültig durch [overclocking](../../../configuration/config-txt/overclocking.md) |
| du | Unbenutzt | Unbenutzt |
| F | Neue Flagge | 1: Überarbeitung im neuen Stil |
| | | 0: Überarbeitung im alten Stil |
| MMM | Speichergröße | 0: 256 MB |
| | | 1: 512 MB |
| | | 2: 1GB |
| | | 3: 2GB |
| | | 4: 4GB |
| | | 5: 8GB |
| CCCC | Hersteller | 0: Sony UK |
| | | 1: Egoman |
| | | 2: Embest |
| | | 3: Sony Japan |
| | | 4: Embest |
| | | 5: Stadion |
| PPPP | Prozessor | 0: BCM2835 |
| | | 1: BCM2836 |
| | | 2: BCM2837 |
| | | 3: BCM2711 |
| TTTTTTTT | Typ | 0: A |
| | | 1: B |
| | | 2: A+ |
| | | 3: B+ |
| | | 4: 2B |
| | | 5: Alpha (früher Prototyp) |
| | | 6: CM1 |
| | | 8: 3B |
| | | 9: Zero |
| | | a: CM3 |
| | | c: Zero W |
| | | d: 3B+ |
| | | e: 3A+ |
| | | f: Nur zur internen Verwendung |
| | | 10: CM3+ |
| | | 11: 4B |
| | | 13: 400 |
| | | 14: CM4 |
| RRRR | Überarbeitung | 0, 1, 2 usw. |

<sup>1</sup> Informationen zur Programmierung der OTP-Bits finden Sie [hier](../../industrial/README.md) und [hier](../otpbits.md).


Im Einsatz befindliche Revisionscodes im neuen Stil:

| Code   | Modell            | Revision | RAM    | Manufacturer |
| ------ | ----------------- | -------- | -------| ------------ |
| 900021 | A+ | 1.1 | 512 MB | Sony DE |
| 900032 | B+ | 1.2 | 512 MB | Sony DE |
| 900092 | Zero | 1.2 | 512 MB | Sony DE |
| 900093 | Zero | 1,3 | 512 MB | Sony DE |
| 9000c1 | Zero W | 1.1 | 512 MB | Sony DE |
| 9020e0 | 3A+ | 1,0 | 512 MB | Sony DE |
| 920092 | Zero | 1.2 | 512 MB | Embest |
| 920093 | Zero | 1,3 | 512 MB | Embest |
| 900061 | CM | 1.1 | 512 MB | Sony DE |
| a01040 | 2B | 1,0 | 1GB | Sony DE |
| a01041 | 2B | 1.1 | 1GB | Sony DE |
| a02082 | 3B | 1.2 | 1GB | Sony DE |
| a020a0 | CM3 | 1,0 | 1GB | Sony DE |
| a020d3 | 3B+ | 1,3 | 1GB | Sony DE |
| a02042 | 2B (mit BCM2837) | 1.2 | 1GB | Sony DE |
| a21041 | 2B | 1.1 | 1GB | Embest |
| a22042 | 2B (mit BCM2837) | 1.2 | 1GB | Embest |
| a22082 | 3B | 1.2 | 1GB | Embest |
| a220a0 | CM3 | 1,0 | 1GB | Embest |
| a32082 | 3B | 1.2 | 1GB | Sony Japan |
| a52082 | 3B | 1.2 | 1GB | Stadion |
| a22083 | 3B | 1,3 | 1GB | Embest |
| a02100 | CM3+ | 1,0 | 1GB | Sony DE |
| a03111 | 4B | 1.1 | 1GB | Sony DE |
| b03111 | 4B | 1.1 | 2GB | Sony DE |
| b03112 | 4B | 1.2 | 2GB | Sony DE |
| b03114 | 4B | 1,4 | 2GB | Sony DE |
| c03111 | 4B | 1.1 | 4GB | Sony DE |
| c03112 | 4B | 1.2 | 4GB | Sony DE |
| d03114 | 4B | 1,4 | 8GB | Sony DE |
| c03130 | Pi 400 | 1,0 | 4GB | Sony DE |