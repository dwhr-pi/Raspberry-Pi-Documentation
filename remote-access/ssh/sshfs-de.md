# SSHFS (SSH-Dateisystem)

Mit SSHFS können Sie die Dateien eines Raspberry Pi über eine SSH-Sitzung mounten.

## Installieren

### Linux

Installieren Sie SSHFS auf Ihrem Computer mit:

```bash
sudo apt install sshfs
```

(Dies setzt voraus, dass Sie ein Debian-basiertes System verwenden)

### Mac

Siehe [osxfuse](https://github.com/osxfuse/osxfuse/wiki/SSHFS)

## Verwendung

Erstellen Sie zunächst ein Verzeichnis auf Ihrem Host-Computer:

```bash
mkdir pi
```

Mounten Sie dann das Dateisystem des Raspberry Pi an diesem Ort:

```bash
sshfs pi@192.168.1.3: pi
```

Geben Sie nun dieses Verzeichnis ein, als ob es ein normaler Ordner wäre; Sie sollten den Inhalt des Raspberry Pi sehen und darauf zugreifen können:

```bash
cd pi
ls
```

Sie können das Dateisystem des Pi auch mit dem Dateimanager Ihres Computers durchsuchen (einschließlich Drag-and-Drop zum Kopieren von Dateien zwischen Geräten) und die Anwendungen Ihres Computers (Texteditoren, Bildbearbeitungstools usw.) verwenden, um Dateien direkt auf dem Pi zu bearbeiten .