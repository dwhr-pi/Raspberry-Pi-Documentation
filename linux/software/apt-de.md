# APT

Der einfachste Weg, das Installieren, Aktualisieren und Entfernen von Software zu verwalten, ist die Verwendung von APT (Advanced Packaging Tool) von Debian. Wenn eine Software in Debian verpackt ist und auf der ARM-Architektur des Raspberry Pi funktioniert, sollte sie auch in Raspberry Pi OS verfügbar sein.

Um Pakete zu installieren oder zu entfernen, benötigen Sie Root-Benutzerrechte, also muss Ihr Benutzer in `sudoers` sein oder Sie müssen als `root` eingeloggt sein. Lesen Sie mehr über [users](../usage/users.md) und [root](../usage/root.md).

Um neue Pakete zu installieren oder vorhandene zu aktualisieren, benötigen Sie eine Internetverbindung.

Beachten Sie, dass die Installation von Software Speicherplatz auf Ihrer SD-Karte verbraucht, daher sollten Sie die Festplattennutzung im Auge behalten und eine SD-Karte entsprechender Größe verwenden.

Beachten Sie auch, dass während der Installation der Software eine Sperre ausgeführt wird, sodass Sie nicht mehrere Pakete gleichzeitig installieren können.

## Softwarequellen

APT führt eine Liste der Softwarequellen auf Ihrem Pi in einer Datei unter `/etc/apt/sources.list`. Bevor Sie Software installieren, sollten Sie Ihre Paketliste mit `apt update` aktualisieren:

```bash
sudo apt update
```

## Installieren eines Pakets mit APT

```bash
sudo apt install tree
```

Die Eingabe dieses Befehls sollte den Benutzer darüber informieren, wie viel Speicherplatz das Paket belegen wird, und zur Bestätigung der Paketinstallation aufgefordert werden. Die Eingabe von `Y` (oder einfach das Drücken von `Enter`, da ja die Standardaktion ist) ermöglicht die Installation. Dies kann umgangen werden, indem dem Befehl das Flag `-y` hinzugefügt wird:

```bash
sudo apt install tree -y
```

Die Installation dieses Pakets macht `tree` für den Benutzer verfügbar.

## Ein installiertes Paket verwenden

`tree` ist ein Kommandozeilen-Tool, das eine Visualisierung der Struktur des aktuellen Verzeichnisses und seines gesamten Inhalts bereitstellt.

- Die Eingabe von `tree` führt den Baumbefehl aus. Zum Beispiel:

```bash
tree
..
├── hello.py
├── games
│   ├── asteroids.py
│   ├── pacman.py
│   ├── README.txt
│   └── tetris.py
```

- Die Eingabe von `man tree` gibt den manuellen Eintrag für das Paket `tree`.
- Die Eingabe von `whereis tree` zeigt, wo `tree` lebt:

```bash
tree: /usr/bin/tree
```

## Paket mit APT deinstallieren

### Entfernen

Sie können ein Paket mit `apt remove` deinstallieren:

```bash
sudo apt remove tree
```

Der Benutzer wird aufgefordert, das Entfernen zu bestätigen. Auch hier wird das Flag `-y` automatisch bestätigt.

### Purge - Säubern

Sie können das Paket und die zugehörigen Konfigurationsdateien auch mit `apt purge` vollständig entfernen:

```bash
sudo apt purge tree
```

## Aktualisieren vorhandener Software

Wenn Software-Updates verfügbar sind, können Sie die Updates mit `sudo apt update` abrufen und die Updates mit `sudo apt full-upgrade` installieren, wodurch alle Ihre Pakete aktualisiert werden. Um ein bestimmtes Paket zu aktualisieren, ohne gleichzeitig alle anderen veralteten Pakete zu aktualisieren, können Sie `sudo apt install somepackage` verwenden (was nützlich sein kann, wenn Sie wenig Speicherplatz oder eine begrenzte Download-Bandbreite haben ).

## Software suchen

Sie können die Archive nach einem Paket mit einem bestimmten Schlüsselwort mit `apt-cache search` durchsuchen:

```bash
apt-cache search locomotive
sl - Korrigiere dich, wenn du aus Versehen `sl' tippst
```

Sie können weitere Informationen zu einem Paket anzeigen, bevor Sie es mit `apt-cache show` installieren:

```bash
apt-cache show sl
Package: sl
Version: 3.03-17
Architecture: armhf
Maintainer: Hiroyuki Yamamoto <yama1066@gmail.com>
Installed-Size: 114
Depends: libc6 (>= 2.4), libncurses5 (>= 5.5-5~), libtinfo5
Homepage: http://www.tkl.iis.u-tokyo.ac.jp/~toyoda/index_e.html
Priority: optional
Section: games
Filename: pool/main/s/sl/sl_3.03-17_armhf.deb
Size: 26246
SHA256: 42dea9d7c618af8fe9f3c810b3d551102832bf217a5bcdba310f119f62117dfb
SHA1: b08039acccecd721fc3e6faf264fe59e56118e74
MD5sum: 450b21cc998dc9026313f72b4bd9807b
Description: Correct you if you type `sl' by mistake
 Sl is a program that can display animations aimed to correct you
 if you type 'sl' by mistake.
 SL stands for Steam Locomotive.
```