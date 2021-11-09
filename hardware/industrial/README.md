# Industrieller Einsatz des Raspberry Pi

Der Raspberry Pi wird oft als Teil eines anderen Produkts verwendet. Diese Dokumentation beschreibt einige zusätzliche Funktionen, die zur Verwendung anderer Funktionen des Pi verfügbar sind.

## Kunden-OTP-Einstellungen

Es gibt eine Reihe von OTP-Werten, die verwendet werden können. Um eine Liste aller [OTP-Werte] (../raspberrypi/otpbits.md) anzuzeigen, können Sie Folgendes verwenden:

```
pi@raspberrypi:~ $ vcgencmd otp_dump
```

Einige interessante Zeilen aus diesem Dump sind:

* 28 - Seriennummer
* 29 - Einer-Ergänzung der Seriennummer
* 30 - Revisionsnummer

Außerdem stehen dem Kunden von 36 bis 43 (einschließlich) acht Zeilen mit 32 Bit zur Verfügung

Um diese Bits zu programmieren, müssen Sie die vcmailbox verwenden. Dies ist eine Linux-Treiberschnittstelle zur Firmware, die die Programmierung der Zeilen übernimmt. Siehe dazu die Dokumentation [hier](https://github.com/raspberrypi/firmware/wiki/Mailbox-property-interface) und die vcmailbox-Beispielanwendung [hier](https://github.com /raspberrypi/userland/blob/master/host_applications/linux/apps/vcmailbox/vcmailbox.c).

Die vcmailbox-Anwendung kann direkt über die Befehlszeile eines Raspberry Pi Raspberry Pi OS-Builds verwendet werden. Ein Anwendungsbeispiel wäre:

```
pi@raspberrypi:~ $ /opt/vc/bin/vcmailbox 0x00010004 8 8 0 0
0x00000020 0x80000000 0x00010004 0x00000008 0x800000008 0xnnnnnnnn 0x00000000 0x00000000
```

Das Obige verwendet die [Mailbox-Eigenschaftenschnittstelle](https://github.com/raspberrypi/firmware/wiki/Mailbox-property-interface) `GET_BOARD_SERIAL` mit einer Anfragegröße von 8 Byte und einer Antwortgröße von 8 Byte (sendet zwei Ganzzahlen für die Anfrage 0, 0). Die Antwort darauf besteht aus zwei ganzen Zahlen (0x00000020 und 0x80000000), gefolgt vom Tag-Code, der Anforderungslänge, der Antwortlänge (mit dem 31. 32bit sind immer 0).

Um die Kunden-OTP-Werte festzulegen, müssen Sie das Tag `SET_CUSTOMER_OTP` (0x38021) wie folgt verwenden:
```
pi@raspberrypi:~ $ /opt/vc/bin/vcmailbox 0x00038021 [8 + Zahl * 4] [8 + Zahl * 4] [Startnummer] [Zahl] [Wert] [Wert] [Wert] ...
```

- `start_num` = die erste Zeile zum Programmieren von 0-7
- `number` = Anzahl der zu programmierenden Zeilen
- `value` = jeder zu programmierende Wert

Um also die OTP-Kundenzeilen 4, 5 und 6 auf 0x11111111, 0x22222222 bzw. 0x33333333 zu programmieren, verwenden Sie:

```
pi@raspberrypi:~ $ /opt/vc/bin/vcmailbox 0x00038021 20 20 4 3 0x11111111 0x22222222 0x33333333
```

Dadurch werden die Zeilen 40, 41 und 42 programmiert.

Um die Werte zurückzulesen, können Sie verwenden:

```
pi@raspberrypi:~ $ /opt/vc/bin/vcmailbox 0x00030021 20 20 4 3 0 0 0
0x0000002c 0x80000000 0x00030021 0x00000014 0x80000014 0x00000000 0x00000003 0x11111111 0x22222222 0x33333333
```

Wenn Sie diese Funktionalität in Ihren eigenen Code integrieren möchten, sollten Sie dies am Beispiel des Codes vcmailbox.c erreichen.

## Sperren der OTP-Änderungen

Es ist möglich, die OTP-Änderungen zu sperren, um eine erneute Bearbeitung zu vermeiden. Dies kann über ein spezielles Argument mit dem OTP-Schreibpostfach erfolgen:

```
pi@raspberrypi:~ $ /opt/vc/bin/vcmailbox 0x00038021 8 8 0xffffffff 0xaffe0000
```

Einmal gesperrt, können die OTP-Werte des Kunden nicht mehr geändert werden. Beachten Sie, dass dieser Sperrvorgang irreversibel ist.

## Kunden-OTP-Bits unlesbar machen

Es ist möglich zu verhindern, dass die Kunden-OTP-Bits überhaupt gelesen werden. Dies kann über ein spezielles Argument mit dem OTP-Schreibpostfach erfolgen:

```
pi@raspberrypi:~ $ /opt/vc/bin/vcmailbox 0x00038021 8 8 0xffffffff 0xaffebabe
```

 Dieser Vorgang ist für die überwiegende Mehrheit der Benutzer unwahrscheinlich und kann nicht rückgängig gemacht werden.