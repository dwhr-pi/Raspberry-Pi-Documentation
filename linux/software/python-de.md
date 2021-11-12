# Python-Pakete installieren

## passend

Einige Python-Pakete sind in den Raspberry Pi OS-Archiven zu finden und können mit apt installiert werden. Zum Beispiel:

```bash
sudo apt update
sudo apt install python3-picamera
```

Dies ist die bevorzugte Methode zur Installation von Software, da die von Ihnen installierten Module mit den üblichen Befehlen `sudo apt update` und `sudo apt full-upgrade` problemlos auf dem neuesten Stand gehalten werden können.

Python-Pakete in Raspberry Pi OS, die mit Python 2.x kompatibel sind, haben immer ein `python-`-Präfix. Das `picamera`-Paket für Python 2.x heißt also `python-picamera` (wie im obigen Beispiel gezeigt). Python 3-Pakete haben immer das Präfix `python3-`. Um `picamera` für Python 3 zu installieren, würden Sie also Folgendes verwenden:

```bash
sudo apt install python3-picamera
```

Die Deinstallation von über APT installierten Paketen kann wie folgt durchgeführt werden:

```bash
sudo apt remove python3-picamera
```

Sie können mit `purge` vollständig entfernt werden:

```bash
sudo apt purge python3-picamera
```

## pip

Nicht alle Python-Pakete sind in den Raspberry Pi OS-Archiven verfügbar, und die, die es sind, können manchmal veraltet sein. Wenn Sie in den Raspberry Pi OS-Archiven keine passende Version finden, können Sie Pakete aus dem [Python Package Index](http://pypi.python.org/) (PyPI) installieren. Verwenden Sie dazu das Werkzeug `pip`.

`pip` wird standardmäßig in Raspberry Pi OS Desktop-Images installiert (aber nicht in Raspberry Pi OS Lite). Sie können es mit `apt` installieren:

```bash
sudo apt install python3-pip
```

Um die Python 2-Version zu erhalten:

```bash
sudo apt install python-pip
```

`pip3` installiert Module für Python 3 und `pip` installiert Module für Python 2.

Der folgende Befehl installiert beispielsweise die Unicorn HAT-Bibliothek für Python 3:

```bash
sudo pip3 install unicornhat
```

Der folgende Befehl installiert die Unicorn HAT-Bibliothek für Python 2:

```bash
sudo pip install unicornhat
```

Deinstallieren Sie Python-Module mit `sudo pip3 uninstall` oder `sudo pip uninstall`.

Laden Sie Ihre eigenen Python-Module mit dem [guide at PyPI](https://wiki.python.org/moin/CheeseShopTutorial#Submitting_Packages_to_the_Package_Index) in `pip` hoch.

## piwheels

Der offizielle Python Package Index (PyPI) hostet Dateien, die von Paketbetreuern hochgeladen wurden. Einige Pakete erfordern eine Kompilierung (Kompilierung von C/C++ oder ähnlichem Code), um sie zu installieren, was eine zeitaufwändige Aufgabe sein kann, insbesondere auf dem Single-Core Raspberry Pi 1 oder Pi Zero.

piwheels ist ein Dienst, der vorkompilierte Pakete (so genannte Python-Wheels) bereitstellt, die auf dem Raspberry Pi verwendet werden können. Raspberry Pi OS ist vorkonfiguriert, um Piwheels für Pip zu verwenden. Lesen Sie mehr über das Piwheels-Projekt unter [www.piwheels.org](https://www.piwheels.org/).