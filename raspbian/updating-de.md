# Aktualisieren und Aktualisieren von Raspberry Pi OS

In diesem Abschnitt wird beschrieben, wie Sie Softwareupdates auf Geräten bereitstellen, auf denen Raspberry Pi OS ausgeführt wird.

Bevor wir fortfahren, untersuchen wir, warum es wichtig ist, unsere Geräte auf dem neuesten Stand zu halten.

Der erste und wahrscheinlich wichtigste Grund ist die Sicherheit. Ein Gerät mit Raspberry Pi OS enthält Millionen Codezeilen, auf die Sie sich verlassen. Im Laufe der Zeit werden diese Millionen Codezeilen bekannte Schwachstellen aufdecken, die als [Common Vulnerabilities and Exposures (CVE)] (https://cve.mitre.org/index.html) bekannt sind und in öffentlich zugänglichen Datenbanken dokumentiert sind sie sind leicht auszunutzen. [Hier ist ein Beispiel](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-8831) eines kürzlich in KODI gefundenen CVE, das einen etwas mehr Einblick in die Informationen bietet in der Datenbank verfügbar sind und wie CVEs verfolgt werden. Die einzige Möglichkeit, diese Exploits als Benutzer von Raspberry Pi OS abzuschwächen, besteht darin, Ihre Software auf dem neuesten Stand zu halten, da die vorgelagerten Repositorys CVEs genau verfolgen und versuchen, sie schnell zu entschärfen.

Der zweite Grund, der mit dem ersten zusammenhängt, ist, dass die Software, die Sie auf Ihrem Gerät ausführen, mit Sicherheit Fehler enthält. Einige Fehler sind CVEs, aber Fehler können auch die gewünschte Funktionalität beeinträchtigen, ohne mit der Sicherheit zu tun zu haben. Indem Sie Ihre Software auf dem neuesten Stand halten, verringern Sie die Wahrscheinlichkeit, dass diese Fehler auftreten.

## APT (Erweitertes Verpackungstool)

Um Software in Raspberry Pi OS zu aktualisieren, können Sie das Tool [apt](../linux/software/apt.md) in einem Terminal verwenden. Öffnen Sie ein Terminalfenster über die Taskleiste oder das Anwendungsmenü:

![Terminal](../usage/terminal/images/terminal.png)

**Aktualisieren** Sie zuerst die Paketliste Ihres Systems, indem Sie den folgenden Befehl eingeben:

```bash
sudo apt-Update
```

**Aktualisieren** Sie als Nächstes alle Ihre installierten Pakete mit dem folgenden Befehl auf die neueste Version:

```bash
sudo apt Voll-Upgrade
```

Beachten Sie, dass `full-upgrade` einem einfachen `upgrade` vorgezogen wird, da es auch eventuell vorgenommene Abhängigkeitsänderungen aufnimmt.

Im Allgemeinen hält dies regelmäßig Ihre Installation für die von Ihnen verwendete Hauptversion des Raspberry Pi-Betriebssystems (z. B. Stretch) auf dem neuesten Stand. Es wird nicht von einer Hauptversion auf eine andere aktualisiert, zum Beispiel Stretch to Buster.

Im Raspberry Pi OS-Image der Foundation werden jedoch gelegentlich Änderungen vorgenommen, die einen manuellen Eingriff erfordern, beispielsweise ein neu eingeführtes Paket. Diese werden bei einem Upgrade nicht installiert, da dieser Befehl nur die bereits installierten Pakete aktualisiert.

### Aktualisieren des Kernels und der Firmware

Der Kernel und die Firmware werden als Debian-Paket installiert und erhalten daher auch Updates, wenn Sie das obige Verfahren verwenden. Diese Pakete werden selten und nach umfangreichen Tests aktualisiert.

### Kein Platz mehr

Beim Ausführen von `sudo apt full-upgrade` wird angezeigt, wie viele Daten heruntergeladen werden und wie viel Speicherplatz sie auf der SD-Karte einnehmen. Es lohnt sich, mit `df -h` zu überprüfen, ob Sie über genügend freien Speicherplatz verfügen, da `apt` dies leider nicht für Sie erledigt. Beachten Sie auch, dass heruntergeladene Paketdateien (`.deb`-Dateien) in `/var/cache/apt/archives` aufbewahrt werden. Sie können diese entfernen, um mit `sudo apt clean` (`sudo apt-get clean` in älteren Versionen von apt) Speicherplatz freizugeben.

### Upgrade von Stretch auf Buster

**Warnung**: Das Aktualisieren eines vorhandenen Stretch-Bildes ist möglich, aber es kann nicht garantiert werden, dass es unter allen Umständen funktioniert, und wir empfehlen es nicht. Wenn Sie versuchen möchten, ein Stretch-Image auf Buster zu aktualisieren, empfehlen wir dringend, zuerst ein Backup zu erstellen – wir übernehmen keine Verantwortung für Datenverluste durch ein fehlgeschlagenes Update.

Um ein Upgrade durchzuführen, ändern Sie zuerst die Dateien `/etc/apt/sources.list` und `/etc/apt/sources.list.d/raspi.list`. Ändern Sie in beiden Dateien jedes Vorkommen des Wortes `stretch` in `buster`. (Beide Dateien benötigen zum Bearbeiten sudo.)

Öffnen Sie dann ein Terminalfenster und führen Sie Folgendes aus:

```bash
sudo apt-Update
sudo apt -y dist-upgrade
```
Beantworten Sie alle Eingabeaufforderungen mit „Ja“. Es kann auch vorkommen, dass die Installation an einer Stelle pausiert, während eine Seite mit Informationen auf dem Bildschirm angezeigt wird – halten Sie die <kbd>Leertaste</kbd> gedrückt, um durch all dies zu scrollen, und drücken Sie dann <kbd>q</kbd> weitermachen.

Wenn Sie PulseAudio schließlich nur für Bluetooth-Audio verwenden, entfernen Sie es aus dem Bild, indem Sie Folgendes eingeben:

```bash
sudo apt -y purge "pulseaudio*"
```

Wenn Sie zu einem neuen Pi-Modell (z. B. dem Pi 3B+) wechseln, müssen Sie möglicherweise auch den Kernel und die Firmware gemäß den obigen Anweisungen aktualisieren.

## Lösungen von Drittanbietern

In diesem Abschnitt wird erläutert, warum Lösungen von Drittanbietern von Interesse sein können und warum [apt](../linux/software/apt.md) nicht für alle Situationen optimal ist. Raspberry Pi empfiehlt keine speziellen Tools von Drittanbietern. Potenzielle Benutzer sollten das für ihre speziellen Anforderungen am besten geeignete Werkzeug ermitteln.

[Apt](../linux/software/apt.md) ist eine bequeme Möglichkeit, die Software Ihres Geräts mit Raspberry Pi OS zu aktualisieren, aber die Einschränkung dieser Methode wird deutlich, wenn Sie einen größeren Pool von zu aktualisierenden Geräten haben. und insbesondere, wenn Sie keinen physischen Zugriff auf Ihre Geräte haben und diese geografisch verteilt sind.

Wenn Sie keinen physischen Zugriff auf Ihre Geräte haben und unbeaufsichtigte Updates Over-The-Air (OTA) bereitstellen möchten, finden Sie hier einige allgemeine Anforderungen:

- Das Update darf auf keinen Fall die Geräte brechen („Brick“), z.B. wenn das Update unterbrochen wird (Stromausfall, Netzwerkausfall, etc.), sollte das System in einen funktionierenden Zustand zurückfallen
- Die Aktualisierung muss [atomar] sein (https://en.wikipedia.org/wiki/Atomicity_%28database_systems%29): Aktualisierung erfolgreich oder Aktualisierung fehlgeschlagen; nichts dazwischen, was dazu führen könnte, dass ein Gerät noch „funktioniert“, aber mit undefiniertem Verhalten
- Bei der Aktualisierung müssen kryptographisch signierte Images/Pakete installiert werden können, um zu verhindern, dass Dritte Software auf Ihrem Gerät installieren
- Das Update muss in der Lage sein, Updates über einen sicheren Kommunikationskanal zu installieren

Leider fehlen [apt](../linux/software/apt.md) die Robustheitsmerkmale, d.h. Atomicity und Fallback. Aus diesem Grund sind Lösungen von Drittanbietern aufgetaucht, die versuchen, die Probleme zu lösen, die für die Bereitstellung unbeaufsichtigter OTA-Updates behoben werden müssen.