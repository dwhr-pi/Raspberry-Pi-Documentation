# Root-Benutzer/Sudo

Das Linux-Betriebssystem ist ein Mehrbenutzer-Betriebssystem, das es mehreren Benutzern ermöglicht, sich anzumelden und den Computer zu verwenden. Um den Computer (und die Privatsphäre anderer Benutzer) zu schützen, sind die Fähigkeiten der Benutzer eingeschränkt.

Die meisten Benutzer dürfen die meisten Programme ausführen und Dateien, die in ihrem eigenen Home-Ordner gespeichert sind, speichern und bearbeiten. Normale Benutzer dürfen normalerweise keine Dateien in den Ordnern anderer Benutzer oder einer der Systemdateien bearbeiten. Es gibt einen speziellen Benutzer unter Linux, der als **Superuser** bekannt ist und normalerweise den Benutzernamen `root` erhält. Der Superuser hat uneingeschränkten Zugriff auf den Computer und kann fast alles tun.

## sudo

Normalerweise werden Sie sich nicht als `root` am Computer anmelden, aber Sie können den `sudo`-Befehl verwenden, um als Superuser Zugriff zu gewähren. Wenn Sie sich als `pi`-Benutzer an Ihrem Raspberry Pi anmelden, dann melden Sie sich als normaler Benutzer an. Sie können Befehle als `root`-Benutzer ausführen, indem Sie den `sudo`-Befehl vor dem Programm verwenden, das Sie ausführen möchten.

Wenn Sie beispielsweise zusätzliche Software auf dem Raspberry Pi OS installieren möchten, verwenden Sie normalerweise das Tool "apt". Um die Liste der verfügbaren Software zu aktualisieren, müssen Sie dem Befehl 'apt' das Präfix sudo voranstellen:

`sudo apt update`

Erfahren Sie mehr über die [apt-Befehle](../software/apt.md).

Sie können auch eine Superuser-Shell ausführen, indem Sie `sudo su` verwenden. Beim Ausführen von Befehlen als Superuser gibt es keinen Schutz vor Fehlern, die das System beschädigen könnten. Es wird empfohlen, Befehle nur bei Bedarf als Superuser auszuführen und eine Superuser-Shell zu beenden, wenn sie nicht mehr benötigt wird.

## Wer kann sudo verwenden?

Es würde den Sinn der Sicherheit zunichte machen, wenn jemand einfach `sudo` vor seine Befehle setzen könnte, sodass nur autorisierte Benutzer `sudo` verwenden können, um Administratorrechte zu erlangen. Der `pi`-Benutzer ist in der `sudoers`-Datei der zugelassenen Benutzer enthalten. Um anderen Benutzern zu erlauben, als Superuser zu agieren, fügen Sie den Benutzer mit `usermod` zur Gruppe `sudo` hinzu.

[Erfahren Sie mehr über Benutzer](users.md).