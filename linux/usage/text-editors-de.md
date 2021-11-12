# Texteditoren

Unter Linux haben Sie die Wahl zwischen Texteditoren. Einige sind einfach zu verwenden, haben jedoch eine eingeschränkte Funktionalität; andere erfordern eine Schulung zur Verwendung und es dauert lange, sie zu beherrschen, bieten jedoch eine unglaubliche Funktionalität.

## Grafische Desktop-Editoren

### Texteditor

Wenn Sie Raspberry Pi OS Desktop verwenden, gibt es im Zubehörmenü die Option, einen Texteditor auszuführen. Dies ist ein einfacher Editor, der sich wie eine normale Anwendung in einem Fenster öffnet. Es ermöglicht die Verwendung von Maus und Tastatur und verfügt über Registerkarten und Syntaxhervorhebung.

Sie können Tastaturkürzel wie `Strg + S` zum Speichern einer Datei und `Strg + X` zum Beenden verwenden.

### Thonny

Thonny ist eine Python REPL und IDE, sodass Sie Python-Code in einem Fenster schreiben und bearbeiten und von dort aus ausführen können.

Thonny hat unabhängige Fenster und Syntaxhervorhebung und verwendet Python 3

### GVim

Siehe Vim unten.

### Geany

Eine schnelle und leichte IDE, die viele verschiedene Dateitypen unterstützt, einschließlich C/C++ und Python. Standardmäßig auf Raspberry Pi OS installiert.

##Befehlszeilen-Editoren

### Nano

GNU Nano steht am einfachsten Ende der Kommandozeilen-Editoren. Es ist standardmäßig installiert, verwenden Sie also `nano somefile.txt`, um eine Datei zu bearbeiten, und Tastenkombinationen wie `Strg + O` zum Speichern und `Strg + X` zum Beenden.

### Vi

Vi ist ein sehr alter (ca. 1976) Befehlszeileneditor, der auf den meisten UNIX-Systemen verfügbar und auf Raspberry Pi OS vorinstalliert ist. Es wird von Vim (Vi Improved) abgelöst, das eine Installation erfordert.

Im Gegensatz zu den meisten Editoren haben Vi und Vim eine Reihe verschiedener Modi. Wenn Sie Vi mit `vi somefile.txt` öffnen, starten Sie im Befehlsmodus, der keine direkte Texteingabe zulässt. Drücken Sie `i`, um in den Einfügemodus zu wechseln, um die Datei zu bearbeiten, und tippen Sie weg. Um die Datei zu speichern, müssen Sie in den Befehlsmodus zurückkehren, also drücken Sie die `Escape`-Taste und geben `:w` (gefolgt von `Enter`) ein, das ist der Befehl zum Schreiben der Datei auf die Festplatte.

Um in einer Datei nach dem Wort 'raspberry' zu suchen, vergewissern Sie sich, dass Sie sich im Befehlsmodus befinden (drücken Sie 'Escape'), geben Sie dann '/raspberry' gefolgt von 'n' und 'N' ein, um durch die Ergebnisse vorwärts/rückwärts zu blättern .

Geben Sie zum Speichern und Beenden den Befehl `:wq` ein. Um den Vorgang ohne Speichern zu beenden, geben Sie den Befehl `:q!` ein.

Abhängig von Ihrer Tastaturkonfiguration können Sie feststellen, dass Ihre Cursortasten nicht funktionieren. In diesem Fall können Sie die H-J-K-L-Tasten (die sich jeweils nach links, unten, oben und rechts bewegen) verwenden, um im Befehlsmodus durch die Datei zu navigieren.

### Vim

Vim ist eine Erweiterung von Vi und funktioniert ähnlich, mit einer Reihe von Verbesserungen. Nur Vi ist standardmäßig installiert. Um die vollen Funktionen von Vim zu erhalten, installieren Sie es mit APT:

```
sudo apt install vim
```

Sie können eine Datei in Vim mit `vim somefile.txt` bearbeiten. Vim hat auch eine grafische Version, die sich in einem Fenster öffnet und die Interaktion mit der Maus ermöglicht. Diese Version ist separat installierbar:

```
sudo apt install vim-gnome
```

Um die grafische Version von Vim zu verwenden, verwenden Sie `gvim somefile.txt`. Sie können die Konfiguration in einer `.vimrc`-Datei im Home-Verzeichnis Ihres Benutzers speichern. Um mehr über die Bearbeitung in Vi und Vim zu erfahren, können Sie `vimtutor` ausführen und dem Tutorial folgen.

### Emacs

Emacs ist ein GNU-Befehlszeilen-Texteditor; Es ist leistungsstark, erweiterbar und anpassbar. Sie können es mit APT installieren:

```
sudo apt install emacs
```

Sie können Tastaturkombinationsbefehle verwenden, wie zum Beispiel „Strg + X Strg + S“ zum Speichern und „Strg + X Strg + C“ zum Schließen.