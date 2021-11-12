# Den Kernel patchen

Beim [Building](building.md) Ihres benutzerdefinierten Kernels möchten Sie vielleicht Patches oder Sammlungen von Patches ('Patchsets') auf den Linux-Kernel anwenden.

Patchsets werden oft vorübergehend mit neuerer Hardware bereitgestellt, bevor die Patches auf den vorgelagerten Linux-Kernel ('mainline') angewendet und dann bis zu den Raspberry-Pi-Kernel-Quellen propagiert werden. Es gibt jedoch Patchsets für andere Zwecke, beispielsweise um einen vollständig präemptiven Kernel für die Echtzeitnutzung zu ermöglichen.

## Versionsidentifikation

Es ist wichtig zu überprüfen, welche Version des Kernels Sie haben, wenn Sie Patches herunterladen und anwenden. In einem Kernel-Quellverzeichnis zeigt Ihnen der folgende Befehl die Version, auf die sich die Quellen beziehen:

```
$ head Makefile -n 3
VERSION = 3
PATCHLEVEL = 10
SUBLEVEL = 25
```

In diesem Fall sind die Quellen für einen 3.10.25-Kernel. Sie können mit dem Befehl `uname -r` sehen, welche Version Sie auf Ihrem System ausführen.

## Patches anwenden

Wie Sie Patches anwenden, hängt vom Format ab, in dem die Patches bereitgestellt werden. Die meisten Patches sind eine einzelne Datei und werden mit dem Dienstprogramm `patch` angewendet. Laden wir zum Beispiel unsere Beispiel-Kernel-Version herunter und patchen sie mit den Echtzeit-Kernel-Patches:

```
$ wget https://www.kernel.org/pub/linux/kernel/projects/rt/3.10/older/patch-3.10.25-rt23.patch.gz
$ gunzip patch-3.10.25-rt23.patch.gz
$ cat patch-3.10.25-rt23.patch | patch -p1
```

In unserem Beispiel laden wir einfach die Datei herunter, dekomprimieren sie und übergeben sie dann mit dem `cat`-Tool und einer Unix-Pipe an das Dienstprogramm `patch`.

Einige Patchsets werden als Patchsets im Mailbox-Format geliefert, die als Ordner mit Patch-Dateien angeordnet sind. Wir können Git verwenden, um diese Patches auf unseren Kernel anzuwenden, aber zuerst müssen wir Git so konfigurieren, dass es weiß, wer wir sind, wenn wir diese Änderungen vornehmen:

```
$ git config --global user.name "Ihr Name"
$ git config --global user.email "Ihre E-Mail hier drin"
```

Sobald wir dies getan haben, können wir die Patches anwenden:

```
git am -3 /path/to/patches/*
```

Wenden Sie sich im Zweifelsfall an den Vertreiber der Pflaster, der Ihnen mitteilen sollte, wie Sie sie anbringen. Einige Patchsets erfordern einen bestimmten Commit, gegen den gepatcht werden kann; Folgen Sie den Angaben des Patch-Distributors.