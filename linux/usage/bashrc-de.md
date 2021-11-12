# .bashrc und .bash_aliases

In Ihrem Home-Ordner finden Sie eine versteckte Datei namens `.bashrc`, die einige Benutzerkonfigurationsoptionen enthält. Sie können diese Datei nach Ihren Wünschen bearbeiten. In dieser Datei vorgenommene Änderungen werden beim nächsten Öffnen eines Terminals übernommen, da dann die Datei `.bashrc` gelesen wird.

Wenn Sie möchten, dass Ihre Änderungen in Ihrem aktuellen Terminal vorgenommen werden, können Sie entweder `source ~/.bashrc` oder `exec bash` verwenden. Diese machen tatsächlich etwas unterschiedliche Dinge: Erstere führt einfach die Datei `.bashrc‘ erneut aus, was zu unerwünschten Änderungen an Dingen wie dem Pfad führen kann, letztere ersetzt die aktuelle Shell durch eine neue Bash-Shell, die die Shell zurücksetzt auf den Status bei der Anmeldung und verwirft alle Shell-Variablen, die Sie möglicherweise gesetzt haben. Wählen Sie die am besten geeignete aus.

Einige nützliche Anpassungen werden für Sie bereitgestellt; einige davon sind standardmäßig mit einem `#` auskommentiert. Um sie zu aktivieren, entfernen Sie das `#` und sie werden das nächste Mal aktiv, wenn Sie Ihren Pi booten oder ein neues Terminal starten.

Zum Beispiel einige `ls`-Aliasnamen:

```
alias ls='ls --color=auto'
#alias dir='dir --color=auto'
#alias vdir='vdir --color=auto'

alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'
```
Aliase wie diese werden bereitgestellt, um Benutzern anderer Systeme wie Microsoft Windows zu helfen (`dir` ist das `ls` von DOS/Windows). Andere fügen der Ausgabe von Befehlen wie `ls` und `grep` standardmäßig Farbe hinzu.

Weitere Variationen von `ls` werden ebenfalls bereitgestellt:

```
# einige weitere ls-Aliasnamen
#alias ll='ls -l'
#alias la='ls -A'
#alias l='ls -CF'
```

Ubuntu-Benutzer sind möglicherweise damit vertraut, da sie standardmäßig in dieser Distribution bereitgestellt werden. Entkommentieren Sie diese Zeilen, um in Zukunft auf diese Aliase zugreifen zu können.

`.bashrc` enthält auch einen Verweis auf eine `.bash_aliases`-Datei, die standardmäßig nicht existiert. Sie können es hinzufügen, um alle Ihre Aliasnamen in einer separaten Datei zu speichern.

```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

Die `if`-Anweisung hier prüft, ob die Datei existiert, bevor sie eingeschlossen wird.

Dann erstellen Sie einfach die Datei `.bash_aliases` und fügen weitere Aliase wie folgt hinzu:

```
alias gs='git status'
```

Sie können andere Dinge direkt zu dieser Datei hinzufügen oder zu einer anderen hinzufügen und diese Datei wie im obigen Beispiel mit `.bash_aliases` einschließen.