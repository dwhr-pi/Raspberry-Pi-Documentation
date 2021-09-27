# Beitrag zur Raspberry Pi-Dokumentation

Die Quellen für die gesamte Raspberry Pi-Dokumentation sind online auf einer Website namens GitHub gespeichert, die sich hier befindet:

https://github.com/raspberrypi/documentation

Obwohl Sie die Dokumentation im offiziellen Repository nicht direkt ändern können, können Sie sich alle Quelldateien ansehen und sehen, wie alles angeordnet ist. Die Quelldateien und Ordner folgen der hierarchischen Dokumentation, wie sie auf der Raspberry Pi-Website zu finden ist.

Bevor Sie Änderungen vornehmen, sollten Sie [hier] (https://www.raspberrypi.org/documentation/CONTRIBUTING.md) überprüfen, ob Ihr Beitrag wahrscheinlich angenommen wird.

Um eine neue oder korrigierte Dokumentation einzureichen, müssen Sie ein GitHub-Konto erstellen (falls Sie noch keins haben) und das ursprüngliche Repository auf Ihr Konto **verzweigen**. Sie nehmen nach Belieben Änderungen vor, speichern sie in Ihrem Repository und senden dann einen sogenannten **Pull-Request** an das ursprüngliche Raspberry Pi-Repository. Dieser Pull-Request (**PR**) erscheint dann im Raspberry-Pi-Repository, wo es von den Betreuern bewertet, kopiert und ggf. mit dem offiziellen Repository zusammengeführt werden kann.

Die Dokumentation, die auf der Raspberry Pi-Website erscheint, wird aus dem GitHub-Repository generiert und ungefähr stündlich aktualisiert.

Sie benötigen ein GitHub-Konto, um einen der folgenden Vorgänge auszuführen.

## Forking eines Repositorys

Das ist einfach. Gehen Sie zum Raspberry Pi-Repository https://github.com/raspberrypi/documentation und sehen Sie sich oben rechts auf der Seite an. Es sollte eine Schaltfläche mit der Bezeichnung **Fork** geben, die eine Kopie des Repositorys in Ihr eigenes GitHub-Konto einfügt.

##Änderungen vornehmen

In Ihrer eigenen Kopie des Repositorys können Sie nun Dateien ändern oder hinzufügen. Das Dateiformat ist GitHub Markdown, daher sollten neue Dateien die Endung `.md` haben. Eine Beschreibung von GitHub Markdown gibt es [hier](https://guides.github.com/features/mastering-markdown/).

Um eine Datei zu bearbeiten, suchen Sie zuerst die Datei im Dateinamenbaum und klicken Sie darauf. Dadurch wird die Seite in vollständig gerendertem Markup angezeigt, und in der Symbolleiste oben im Dokument (nicht oben auf der Seite) befindet sich ein kleines Bleistiftsymbol. Dies ist die Schaltfläche Bearbeiten. Klicken Sie darauf und die Datei wird im Github-Editor angezeigt. Sie können jetzt nach Herzenslust bearbeiten. Sie können auf **Vorschau der Änderungen** klicken, um die vollständig gerenderte Datei mit Ihren Änderungen anzuzeigen.

Am Ende der Seite befindet sich eine Box namens **Commit Changes**. Sie können Ihre Änderungen entweder direkt in Ihren eigenen Master-Branch übertragen oder einen neuen Branch zur Verwendung als Pull-Request erstellen. Verwenden Sie die Master-Option, da Sie damit Änderungen an Ihrer Master-Kopie vornehmen. Wenn Sie die Branch-Option verwenden, wird ein neuer Branch in Ihrem eigenen Repository erstellt, aber das ist etwas komplizierter, daher wird es hier nicht beschrieben. Wenn Sie im Laufe der Zeit viele unabhängige Änderungen vornehmen, bevor Sie die Änderungen auf Raspberry Pi übertragen, möchten Sie möglicherweise die Verzweigungsoption untersuchen. Aktualisieren Sie den Commit-Titel und geben Sie an dieser Stelle eine Beschreibung der Änderung ein.

Wenn Sie **Änderungen übernehmen** auswählen, wird die Änderung an Ihrem Master-Branch vorgenommen. Sie müssen nun diese Änderung übernehmen und daraus eine Pull-Anfrage stellen.

## Pull-Request öffnen

Dies ist ziemlich einfach. Klicken Sie in der Symbolleiste auf die Registerkarte **Pull Requests**. Danach sollte sich direkt unter der Symbolleiste ein grüner Button befinden, der mit **Neuer Pull-Request** beschriftet ist. Klicken Sie darauf, und eine Seite sollte erscheinen, die Sie auffordert, die Änderungen zu vergleichen. Diese PR-Seite befindet sich tatsächlich auf der Raspberry Pi GitHub-Seite, nicht auf der des Mitwirkenden, da ein PR die Raspberry Pi-Repository-Betreuer auffordert, aus dem Repository des Mitwirkenden zu „ziehen“. Die linke Seite sollte das Repository `raspberrypi/documentation` sein und der Zweig sollte der Master sein. Auf der rechten Seite kommt die PR: Ihr GitHub-Konto und Ihr Master-Zweig. Weiter unten auf der Seite sollten Sie eine Liste der Commits sehen, die Sie in der PR haben möchten, und darunter die tatsächlichen Änderungen.

Wenn Sie damit einverstanden sind, dass die PR erstellt wird, klicken Sie auf **Pull-Request erstellen**.

Und das ist es! Die PR-Liste der Raspberry Pi-Dokumentation enthält jetzt Ihren Eintrag. Es wird gelesen, auf technische Korrektheit geprüft, zur abschließenden Prüfung an Lektoren übergeben und schließlich in den Hauptdokumentationsbaum eingebunden.


Dies ist eine sehr kurze Anleitung zum Beitragen über GitHub, aber es wird Ihnen den Einstieg erleichtern und Ihnen ermöglichen, etwas zu bewegen!
