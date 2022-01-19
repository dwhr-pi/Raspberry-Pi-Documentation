# Terminal

Das Terminal (oder die „Befehlszeile“) auf einem Computer ermöglicht einem Benutzer ein hohes Maß an Kontrolle über sein System (oder in diesem Fall Pi!). Benutzer von Windows sind möglicherweise bereits auf „Eingabeaufforderung“ oder „Powershell“ gestoßen, und Benutzer von Mac OS sind möglicherweise mit „Terminal“ vertraut. Alle diese Tools ermöglichen es einem Benutzer, sein System durch die Verwendung von Befehlen direkt zu manipulieren. Diese Befehle können verkettet und/oder zu komplexen Skripten kombiniert werden (siehe die [Linux-Nutzungsseite zum Skripten](.././linux/usage/scripting.md)), die möglicherweise Aufgaben effizienter als viel größere erledigen können herkömmliche Softwarepakete.

## Öffnen eines Terminalfensters

Auf dem Raspberry Pi (mit Raspberry Pi OS) ist die Standard-Terminalanwendung „LXTerminal“. Dies ist als „Terminal-Emulator“ bekannt, das heißt, es emuliert die alten Videoterminals (aus der Zeit, bevor grafische Benutzeroberflächen entwickelt wurden) in einer grafischen Umgebung. Die Anwendung ist auf dem Desktop des Raspberry Pi zu finden und sieht nach dem Start etwa so aus:

![Terminal-Screenshot](images/terminal.png)

Sie sollten die folgende Eingabeaufforderung sehen können:

```bash
pi@raspberrypi ~ $
```

Dies zeigt Ihren Benutzernamen und den Hostnamen des Pi. Hier ist der Benutzername „pi“ und der Hostname „raspberrypi“.

Lassen Sie uns nun versuchen, einen Befehl auszuführen. Geben Sie „pwd“ (aktuelles Arbeitsverzeichnis) ein, gefolgt von der „Enter“-Taste. Dies sollte so etwas wie `/home/pi` anzeigen.

## Navigieren und Durchsuchen Ihres Pi

Einer der wichtigsten Aspekte bei der Verwendung eines Terminals ist die Möglichkeit, in Ihrem Dateisystem zu navigieren. Führen Sie zunächst den folgenden Befehl aus: `ls -la`. Sie sollten etwas Ähnliches sehen wie:

![ls Ergebnis](images/lsresult.png)

Der Befehl `ls` listet den Inhalt des Verzeichnisses auf, in dem Sie sich gerade befinden (Ihr aktuelles Arbeitsverzeichnis). Die `-la`-Komponente des Befehls ist ein sogenanntes 'Flag'. Flags ändern den ausgeführten Befehl. In diesem Fall zeigt das `l` den Inhalt des Verzeichnisses in einer Liste mit Daten wie deren Größe und wann sie zuletzt bearbeitet wurden, und das `a` zeigt alle Dateien an, einschließlich derer, die mit einem `.` beginnen, bekannt als 'Punktdateien'. Dotfiles fungieren normalerweise als Konfigurationsdateien für Software und da sie als Text geschrieben sind, können sie durch einfaches Bearbeiten geändert werden.

Um zu anderen Verzeichnissen zu navigieren, kann der Verzeichniswechselbefehl „cd“ verwendet werden. Sie können das Verzeichnis, zu dem Sie wechseln möchten, entweder über den „absoluten“ oder den „relativen“ Pfad angeben. Wenn Sie also zum Verzeichnis „python_games“ navigieren möchten, können Sie entweder „cd /home/pi/python_games“ oder einfach „cd python_games“ ausführen (wenn Sie sich gerade in „/home/pi“ befinden). Es gibt einige Sonderfälle, die nützlich sein können: `~` fungiert als Alias ​​für Ihr Home-Verzeichnis, also ist `~/python_games` dasselbe wie `/home/pi/python_games`; `.` und `..` sind Aliase für das aktuelle Verzeichnis bzw. das Elternverzeichnis, z.B. wenn Sie in `/home/pi/python_games` wären, würde `cd ..` Sie zu `/home/pi` bringen.

## Verlauf und automatische Vervollständigung

Anstatt jeden Befehl einzugeben, ermöglicht Ihnen das Terminal, durch die vorherigen Befehle zu blättern, die Sie ausgeführt haben, indem Sie die Tasten "Auf" oder "Ab" auf Ihrer Tastatur drücken. Wenn Sie den Namen einer Datei oder eines Verzeichnisses als Teil eines Befehls schreiben, wird durch Drücken der Tabulatortaste versucht, den von Ihnen eingegebenen Namen automatisch zu vervollständigen. Wenn Sie zum Beispiel eine Datei in einem Verzeichnis mit dem Namen „aLongFileName“ haben, können Sie durch Drücken der Tabulatortaste nach der Eingabe von „a“ aus allen Datei- und Verzeichnisnamen auswählen, die mit „a“ im aktuellen Verzeichnis beginnen, sodass Sie „aLongFileName“ auswählen können `.

## Sudo

Einige Befehle, die dauerhafte Änderungen am Zustand Ihres Systems vornehmen, erfordern zum Ausführen Root-Rechte. Der Befehl `sudo` gibt Ihrem Konto (wenn Sie nicht bereits als root angemeldet sind) vorübergehend die Möglichkeit, diese Befehle auszuführen, vorausgesetzt, Ihr Benutzername befindet sich in einer Liste von Benutzern ("sudoers"). Wenn Sie „sudo“ an den Anfang eines Befehls anhängen und „Enter“ drücken, wird der Befehl nach „sudo“ mit Root-Rechten ausgeführt. Seien Sie sehr vorsichtig: Befehle, die Root-Rechte erfordern, können Ihr System irreparabel beschädigen! Beachten Sie, dass Sie auf einigen Systemen aufgefordert werden, Ihr Passwort einzugeben, wenn Sie einen Befehl mit „sudo“ ausführen.

Weitere Informationen zu `sudo` und dem Root-Benutzer finden Sie auf der [Linux-Root-Seite](../../linux/usage/root.md).

## Software mit apt installieren

Sie können den Befehl „apt“ verwenden, um Software in Raspberry Pi OS zu installieren. Dies ist der „Paketmanager“, der in allen Debian-basierten Linux-Distributionen (einschließlich Raspberry Pi OS) enthalten ist. Damit können Sie neue Softwarepakete auf Ihrem Pi installieren und verwalten. Um ein neues Paket zu installieren, würden Sie `sudo apt install <package-name>` eingeben (wobei `<package-name>` das Paket ist, das Sie installieren möchten). Durch Ausführen von `sudo apt update` wird eine Liste von Softwarepaketen aktualisiert, die auf Ihrem System verfügbar sind. Wenn eine neue Version eines Pakets verfügbar ist, aktualisiert `sudo apt full-upgrade` alle alten Pakete auf die neue Version. Schließlich entfernt oder deinstalliert `sudo apt remove <package-name>` ein Paket von Ihrem System.

Weitere Informationen dazu finden Sie im Abschnitt [Linux-Nutzung auf apt](../../linux/software/apt.md).

## Andere nützliche Befehle

Es gibt einige andere Befehle, die Sie möglicherweise nützlich finden. Diese sind unten aufgeführt:

- `cp` erstellt eine Kopie einer Datei und platziert sie an der angegebenen Stelle (im Wesentlichen macht es ein 'Kopieren-Einfügen'), zum Beispiel - `cp file_a /home/other_user/` würde die Datei `file_a` von Ihrem Zuhause kopieren Verzeichnis in das des Benutzers `other_user` (vorausgesetzt, Sie haben die Berechtigung, es dorthin zu kopieren). Beachten Sie, dass, wenn das Ziel ein Ordner ist, der Dateiname gleich bleibt, aber wenn das Ziel ein Dateiname ist, erhält die Datei den neuen Namen.
- `mv` verschiebt eine Datei und platziert sie an der angegebenen Stelle (also wo `cp` ein 'Kopieren-Einfügen' durchführt, führt `mv` ein 'Ausschneiden-Einfügen' durch). Die Verwendung ist ähnlich wie bei `cp`, also würde `mv file_a /home/other_user/` die Datei `file_a` von Ihrem Home-Verzeichnis in das des angegebenen Benutzers verschieben. `mv` wird auch verwendet, um eine Datei umzubenennen, d. h. sie an einen neuen Ort zu verschieben, z. `mv hallo.txt story.txt`.
- `rm` entfernt die angegebene Datei (oder das Verzeichnis, wenn es mit `-r` verwendet wird). **Warnung:** Auf diese Weise gelöschte Dateien können im Allgemeinen nicht wiederhergestellt werden.
- `mkdir`: Dies macht ein neues Verzeichnis, z.B. `mkdir new_dir` würde das Verzeichnis `new_dir` im aktuellen Arbeitsverzeichnis erstellen.
- `cat` listet den Inhalt von Dateien auf, z.B. `cat some_file` zeigt den Inhalt von `some_file` an.

Weitere nützliche Befehle finden Sie auf der [Befehlsseite](../../linux/usage/commands.md).

## Einen Befehl herausfinden

Um weitere Informationen zu einem bestimmten Befehl zu erhalten, können Sie „man“ gefolgt von dem Befehl ausführen, über den Sie mehr erfahren möchten (z. B. „man ls“). Die Handbuchseite (oder Handbuchseite) für diesen Befehl wird angezeigt, einschließlich Informationen über die Flags für dieses Programm und welche Auswirkungen sie haben. Einige Manpages geben Beispiele für die Verwendung.