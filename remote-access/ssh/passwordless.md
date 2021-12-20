# Passwortloser SSH-Zugang

Sie können Ihren Raspberry Pi so konfigurieren, dass er den Zugriff von einem anderen Computer aus ermöglicht, ohne dass Sie bei jeder Verbindung ein Passwort eingeben müssen. Dazu müssen Sie anstelle eines Passworts einen SSH-Schlüssel verwenden. So generieren Sie einen SSH-Schlüssel:

## Nach vorhandenen SSH-Schlüsseln suchen

Überprüfen Sie zunächst, ob auf dem Computer, mit dem Sie sich mit dem Raspberry Pi verbinden, bereits Schlüssel vorhanden sind:

```bash
ls ~/.ssh
```

Wenn Sie Dateien namens `id_rsa.pub` oder `id_dsa.pub` sehen, haben Sie bereits Schlüssel eingerichtet, sodass Sie den Schritt 'Neue SSH-Schlüssel generieren' unten überspringen können.

## Neue SSH-Schlüssel generieren

Geben Sie den folgenden Befehl ein, um neue SSH-Schlüssel zu generieren:

```bash
ssh-keygen
```

Nach Eingabe dieses Befehls werden Sie gefragt, wo der Schlüssel gespeichert werden soll. Wir empfehlen, es am Standardspeicherort (`~/.ssh/id_rsa`) zu speichern, indem Sie `Enter` drücken.

Sie werden auch aufgefordert, eine Passphrase einzugeben, die optional ist. Die Passphrase wird verwendet, um den privaten SSH-Schlüssel zu verschlüsseln, sodass, wenn jemand den Schlüssel kopiert hat, er sich nicht als Sie ausgeben kann, um Zugriff zu erhalten. Wenn Sie eine Passphrase verwenden möchten, geben Sie sie hier ein und drücken Sie die Eingabetaste. Geben Sie sie dann erneut ein, wenn Sie dazu aufgefordert werden. Lassen Sie das Feld leer, um keine Passphrase zu verwenden.

Schauen Sie nun in Ihr `.ssh`-Verzeichnis:

```bash
ls ~/.ssh
```

und Sie sollten die Dateien `id_rsa` und `id_rsa.pub` sehen:

```
authorized_keys  id_rsa  id_rsa.pub  known_hosts
```

Die Datei `id_rsa` ist Ihr privater Schlüssel. Bewahren Sie diese auf Ihrem Computer auf.

Die Datei `id_rsa.pub` ist Ihr öffentlicher Schlüssel. Das teilen Sie mit Maschinen, mit denen Sie sich verbinden: in diesem Fall Ihrem Raspberry Pi. Wenn der Computer, mit dem Sie eine Verbindung herstellen möchten, Ihren öffentlichen und privaten Schlüssel abgleicht, können Sie eine Verbindung herstellen.

Sehen Sie sich Ihren öffentlichen Schlüssel an, um zu sehen, wie er aussieht:

```bash
cat ~/.ssh/id_rsa.pub
```

Es sollte in der Form sein:

```bash
ssh-rsa <REALLY LONG STRING OF RANDOM CHARACTERS> user@host
```

<a name="copy-your-public-key-to-your-raspberry-pi"></a>
## Kopieren Sie Ihren öffentlichen Schlüssel auf Ihren Raspberry Pi


Hängen Sie auf dem Computer, von dem aus Sie eine Verbindung herstellen möchten, den öffentlichen Schlüssel an Ihre Datei "authorized_keys" auf dem Raspberry Pi an, indem Sie ihn über SSH senden:

```bash
ssh-copy-id <BENUTZERNAME>@<IP-ADRESSE>
```

**Beachten Sie, dass Sie sich für diesen Schritt mit Ihrem Passwort authentifizieren müssen.**

Wenn `ssh-copy-id` auf Ihrem System nicht verfügbar ist, können Sie die Datei alternativ auch manuell über SSH kopieren:

```bash
cat ~/.ssh/id_rsa.pub | ssh <USERNAME>@<IP-ADDRESS> 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
```

Wenn Sie die Meldung `ssh: connect to host <IP-ADDRESS> port 22: Connection failed` sehen und wissen, dass die `IP-ADDRESS` korrekt ist, dann haben Sie möglicherweise SSH auf Ihrem Raspberry Pi nicht aktiviert. Führen Sie `sudo raspi-config` im Terminalfenster des Pi aus, aktivieren Sie SSH und versuchen Sie dann erneut, die Dateien zu kopieren.

Versuchen Sie nun `ssh <USER>@<IP-ADDRESS>` und Sie sollten sich ohne Passwortabfrage verbinden.

Wenn die Meldung "Agent hat Fehler beim Signieren mit dem Schlüssel zugestanden" angezeigt wird, fügen Sie Ihre RSA- oder DSA-Identitäten zum Authentifizierungsagenten `ssh-agent` hinzu und führen Sie den folgenden Befehl aus:

```bash
ssh-add
```

Wenn dies nicht funktioniert, können Sie in den [Raspberry Pi-Foren](https://www.raspberrypi.org/forums/) Hilfe erhalten.

**Hinweis:** Sie können Dateien auch über SSH mit dem Befehl `scp` senden (sichere Kopie). Weitere Informationen finden Sie im [SCP-Leitfaden](scp.md).

## Speichern Sie die Passphrase im macOS-Schlüsselbund

Wenn Sie macOS verwenden und überprüft haben, ob Ihr neuer Schlüssel die Verbindung ermöglicht, haben Sie die Möglichkeit, die Passphrase für Ihren Schlüssel im macOS-Schlüsselbund zu speichern. Auf diese Weise können Sie sich mit Ihrem Raspberry Pi verbinden, ohne die Passphrase einzugeben.

Führen Sie den folgenden Befehl aus, um ihn in Ihrem Schlüsselbund zu speichern:

```bash
ssh-add -K ~/.ssh/id_rsa
```