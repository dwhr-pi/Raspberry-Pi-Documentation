# Python

Python ist eine wunderbare und leistungsstarke Programmiersprache, die einfach zu verwenden ist (leicht zu lesen ** und ** zu schreiben) und mit Raspberry Pi Ihr Projekt mit der realen Welt verbinden kann.

![Python logo](images/python-logo.png)

Die Python-Syntax ist sehr sauber, mit Schwerpunkt auf Lesbarkeit und verwendet englische Standardschlüsselwörter.

## Thonny

Der einfachste Einstieg in Python erfolgt über Thonny, eine Python3-Entwicklungsumgebung. Öffnen Sie Thonny über das Desktop- oder Anwendungsmenü:

Thonny gibt Ihnen eine REPL (Read-Evaluate-Print-Loop), eine Eingabeaufforderung, in die Sie Python-Befehle eingeben können. Da es sich um eine REPL handelt, erhalten Sie sogar die Ausgabe von Befehlen auf dem Bildschirm, ohne `print` zu verwenden. In der Thonny-Anwendung wird dies Shell-Fenster genannt.

Sie können bei Bedarf Variablen verwenden, aber Sie können es sogar wie einen Taschenrechner verwenden. Beispielsweise:

```python
>>> 1 + 2
3
>>> name = "Sarah"
>>> "Hello " + name
'Hello Sarah'
```

Thonny hat auch eine eingebaute Syntaxhervorhebung und eine gewisse Unterstützung für die automatische Vervollständigung. Mit `Alt + P` (Zurück) und `Alt + N` (Weiter) können Sie auf die Historie der Befehle zurückblicken, die Sie in die REPL eingegeben haben.

## Grundlegende Verwendung von Python

Hallo Welt in Python:

```python
print("Hello world")
```

So einfach ist das!

### Einzug

Einige Sprachen verwenden geschweifte Klammern `{` und `}`, um zusammengehörende Codezeilen einzuschließen, und überlassen es dem Autor, diese Zeilen einzurücken, damit sie optisch verschachtelt erscheinen. Python verwendet jedoch keine geschweiften Klammern, sondern erfordert stattdessen Einrückungen zum Verschachteln. Zum Beispiel eine `for`-Schleife in Python:

```python
for i in range(10):
    print("Hello")
```

Hier ist die Einrückung notwendig. Eine zweite eingerückte Zeile wäre ein Teil der Schleife, und eine zweite nicht eingerückte Zeile wäre außerhalb der Schleife. Beispielsweise:

```python
for i in range(2):
    print("A")
    print("B")
```

würde drucken:

```
A
B
A
B
```

während Folgendes:

```python
for i in range(2):
    print("A")
print("B")
```

würde drucken:

```
A
A
B
```

### Variablen

Um einen Wert in einer Variablen zu speichern, weisen Sie ihn wie folgt zu:

```python
name = "Bob"
age = 15
```

Beachten Sie, dass mit diesen Variablen keine Datentypen angegeben wurden, da Typen abgeleitet werden und später geändert werden können.

```python
age = 15
age += 1  # increment age by 1
print(age)
```

Dieses Mal habe ich Kommentare neben dem Increment-Befehl verwendet.

### Bemerkungen

Kommentare werden im Programm ignoriert, sind aber für Sie da, um Notizen zu hinterlassen, und sind mit dem Hash-Symbol "#" gekennzeichnet. Mehrzeilige Kommentare verwenden dreifache Anführungszeichen wie folgt:

„Python
"""
Dies ist ein sehr einfaches Python-Programm, das "Hallo" ausgibt.
Das ist alles, was es tut.
"""

print("Hallo")
```

### Listen

Python hat auch Listen (in einigen Sprachen Arrays genannt), die Sammlungen von Daten jeglicher Art sind:

```python
numbers = [1, 2, 3]
```

Listen werden durch die Verwendung von eckigen Klammern `[]` gekennzeichnet und jedes Element wird durch ein Komma getrennt.

### Wiederholung

Einige Datentypen sind iterierbar, was bedeutet, dass Sie die darin enthaltenen Werte durchlaufen können. Zum Beispiel eine Liste:

```python
numbers = [1, 2, 3]

for number in numbers:
    print(number)
```

Dies nimmt jedes Element in der Liste "Nummern" und druckt das Element aus:

```
1
2
3
```

Beachten Sie, dass ich das Wort „Nummer“ verwendet habe, um jedes Element zu bezeichnen. Dies ist lediglich das Wort, das ich dafür gewählt habe - es wird empfohlen, beschreibende Wörter für Variablen zu wählen - die Verwendung von Plural für Listen und Singular für jedes Element ist sinnvoll. Es erleichtert das Verständnis beim Lesen.

Andere Datentypen sind iterierbar, zum Beispiel der String:

```python
dog_name = "BINGO"

for char in dog_name:
    print(char)
```

Dadurch wird jedes Zeichen durchlaufen und ausgedruckt:

```
B
I
N
G
O
```

### Bereich

Der Integer-Datentyp ist nicht iterierbar und der Versuch, ihn zu iterieren, führt zu einem Fehler. Beispielsweise:

```python
for i in 3:
    print(i)
```

wird herstellen:

```python
TypeError: 'int' object is not iterable
```

![Python-Fehler](images/python-error.png)

Sie können jedoch mit der Funktion "range" ein iterierbares Objekt erstellen:

```python
for i in range(3):
    print(i)
```

„range(5)“ enthält die Zahlen „0“, „1“, „2“, „3“ und „4“ (insgesamt fünf Zahlen). Um die Zahlen „1“ bis „5“ (einschließlich) zu erhalten, verwenden Sie „range(1, 6)“.

### Länge

Sie können Funktionen wie `len` verwenden, um die Länge eines Strings oder einer Liste zu ermitteln:

```python
name = "Jamie"
print(len(name))  # 5

names = ["Bob", "Jane", "James", "Alice"]
print(len(names))  # 4
```

### If-Anweisungen

Sie können `if`-Anweisungen für die Ablaufsteuerung verwenden:

```python
name = "Joe"

if len(name) > 3:
    print("Nice name,")
    print(name)
else:
    print("That's a short name,")
    print(name)
```

## Python-Dateien in Thonny

Um eine Python-Datei in Thonny zu erstellen, klicken Sie auf „Datei > Neu“ und Sie erhalten ein <untitled>-Fenster. Dies ist eine leere Datei, keine Python-Eingabeaufforderung. Sie schreiben eine Python-Datei in dieses Fenster, speichern sie, führen sie dann aus und sehen die Ausgabe im anderen Fenster.

Geben Sie beispielsweise im neuen Fenster Folgendes ein:

```python
n = 0

for i in range(1, 101):
    n += i

print("The sum of the numbers 1 to 100 is:")
print(n)
```

Speichern Sie dann diese Datei (`File > Save` oder `Strg + S`) und führen Sie sie aus (`Run > Run Module` oder drücken Sie `F5`) und Sie sehen die Ausgabe in Ihrem ursprünglichen Python-Fenster.

## Ausführen von Python-Dateien über die Befehlszeile

Sie können eine Python-Datei in einem Standard-[Editor] (../../linux/usage/text-editors.md) schreiben und sie als Python-Skript über die Befehlszeile ausführen. Navigieren Sie einfach zu dem Verzeichnis, in dem die Datei gespeichert ist (verwenden Sie `cd` und `ls` als Anleitung) und führen Sie sie mit `python3` aus, z. `python3 hallo.py`.

![Python-Befehlszeile](images/run-python.png)

## Andere Verwendungsmöglichkeiten für Python

### Befehlszeile

Auf die standardmäßig eingebaute Python-Shell wird durch Eingabe von „python3“ im Terminal zugegriffen.

Diese Shell ist eine Eingabeaufforderung, die für die Eingabe von Python-Befehlen bereit ist. Sie können dies auf die gleiche Weise wie Thonny verwenden, es verfügt jedoch nicht über Syntaxhervorhebung oder Autovervollständigung. Mit den Tasten <kbd>Nach oben/Nach unten</kbd> können Sie auf die Historie der Befehle zurückblicken, die Sie in die REPL eingegeben haben. Verwenden Sie zum Beenden "Strg + D".

### IPython

IPython ist eine interaktive Python-Shell mit Syntaxhervorhebung, Autovervollständigung, hübschem Drucken, integrierter Dokumentation und mehr. IPython ist standardmäßig nicht installiert. Installieren mit:

```bash
sudo pip3 install ipython
```

Führen Sie dann mit "ipython" von der Befehlszeile aus. Es funktioniert wie das Standard-`python3`, hat aber mehr Funktionen. Versuchen Sie, „len?“ einzugeben und „Enter“ zu drücken. Ihnen werden Informationen einschließlich des Dokumentstrings für die Funktion "len" angezeigt:

```python
Type:       builtin_function_or_method
String Form:<built-in function len>
Namespace:  Python builtin
Docstring:
len(object) -> integer

Gibt die Anzahl der Elemente einer Sequenz oder Zuordnung zurück.
```

Versuchen Sie das folgende Wörterbuchverständnis:

```python
{i: i ** 3 for i in range(12)}
```

Dies wird hübsch Folgendes drucken:

```python
{0: 0,
 1: 1,
 2: 8,
 3: 27,
 4: 64,
 5: 125,
 6: 216,
 7: 343,
 8: 512,
 9: 729,
 10: 1000,
 11: 1331}
```

In der Standard-Python-Shell wäre dies in einer Zeile gedruckt worden:

```python
{0: 0, 1: 1, 2: 8, 3: 27, 4: 64, 5: 125, 6: 216, 7: 343, 8: 512, 9: 729, 10: 1000, 11: 1331}
```

![Python vs. ipython](images/python-vs-ipython.png)

Sie können auf den Verlauf der Befehle zurückblicken, die Sie in die REPL eingegeben haben, indem Sie die Tasten <kbd>Up/Down</kbd> wie in `python` verwenden. Der Verlauf bleibt auch bis zur nächsten Sitzung erhalten, sodass Sie „ipython“ beenden und zurückkehren (oder zwischen v2/3 wechseln) können und der Verlauf erhalten bleibt. Verwenden Sie zum Beenden "Strg + D".

## Installieren von Python-Bibliotheken

### passend

Einige Python-Pakete sind in den Archiven des Raspberry Pi OS zu finden und können beispielsweise mit apt installiert werden:

```bash
sudo apt update
sudo apt install python-picamera
```

Dies ist eine bevorzugte Installationsmethode, da die von Ihnen installierten Module einfach mit den üblichen Befehlen „sudo apt update“ und „sudo apt full-upgrade“ auf dem neuesten Stand gehalten werden können.

### Pip

Nicht alle Python-Pakete sind in den Raspberry Pi OS-Archiven verfügbar, und die, die vorhanden sind, können manchmal veraltet sein. Wenn Sie in den Archiven des Raspberry Pi OS keine geeignete Version finden, können Sie Pakete aus dem [Python Package Index](http://pypi.python.org/) (bekannt als PyPI) installieren.

Installieren Sie dazu pip:

```bash
sudo apt install python3-pip
```

Installieren Sie dann Python-Pakete (z. B. `simplejson`) mit `pip3`:

```bash
sudo pip3 install simplejson
```

Lesen Sie [hier](../../linux/software/python.md) mehr über die Installation von Software in Python.

#### Piwheels

Der offizielle Python Package Index (PyPI) hostet Dateien, die von Paketbetreuern hochgeladen wurden. Einige Pakete erfordern eine Kompilierung (Kompilieren von C/C++ oder ähnlichem Code), um sie zu installieren, was eine zeitaufwändige Aufgabe sein kann, insbesondere auf dem Single-Core-Raspberry Pi 1 oder Pi Zero.

piwheels ist ein Dienst, der vorkompilierte Pakete (genannt *Python-Räder*) bereitstellt, die auf dem Raspberry Pi verwendet werden können. Raspberry Pi OS ist vorkonfiguriert, um Piwheels für Pip zu verwenden. Lesen Sie mehr über das Piwheels-Projekt unter [www.piwheels.org](https://www.piwheels.org/).

## Mehr

- [GPIO mit Python](../gpio/python/README.md)
- [Offizielle Python-Website] (https://www.python.org/)
- [Offizielle Python-Dokumentation] (https://www.python.org/doc/)