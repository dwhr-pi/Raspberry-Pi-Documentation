# Netzwerkstart über einen Windows-Server

Dies ist eine Zusammenfassung der Schritte, die zum Einrichten eines Windows-Servers als DHCP/TFTP-Server erforderlich sind
damit ein Raspberry Pi 3 über das Netzwerk booten kann.

Wir verwenden die Windows-DHCP-Dienste, die Windows-Bereitstellungsdienste als TFTP-Server und die Windows
NFS-Dienste zur Bereitstellung des Dateisystems für das gebootete Image. Mein spezieller Server ist ein HP Proliant
Mikroserver mit Windows 2008 R2.

## _Haftungsausschluss_

Bei diesen Schritten werden möglicherweise Details ausgelassen, da ich dies im Laufe der Zeit ausgearbeitet habe und keinen anderen Windows 2008-Server habe
womit man wieder anfangen kann. Ich bin auch kein Netzwerk- oder WDS-Experte, also korrigiere das gerne.

Ein Großteil dieses Dokuments wurde im Nachhinein geschrieben, daher kann es sein, dass ich den einen oder anderen Schritt verpasst habe. Hoffentlich reicht es aus, einen erfahrenen Windows-Administrator auf den richtigen Weg zu bringen.


## DHCP-Einstellungen

Dies ist ein Bereich, der verbessert werden könnte. Ich habe meinen Windows 2008 Server als DHCP-Server für mein Netzwerk eingerichtet,
Damit der Raspberry Pi jedoch vom Netzwerk booten kann, muss er eine bestimmte BOOTP-Option vom DHCP-Server zurückerhalten.
Dies ist '043 Lieferantenspezifische Informationen'. Ich habe das DHCP-MMC-Plugin verwendet, um die Option this zu meinem DHCP-Bereich hinzuzufügen. Die Daten für diese Option habe ich gesetzt auf:

2B 20 06 01 03 0A 04 00 50 58 45 14 00 00 11 52 61 73 70 62 65 72 72 79 20 50 69 20 42 6F 6F 74 FF

Ich habe diese Zeichenfolge ermittelt, indem ich eine Ablaufverfolgung eines erfolgreichen Bootvorgangs ausgeführt habe, wenn ich einen Pi als DHCP-Server verwende. Mir ist aufgefallen, dass sich WireShark beschwert
die Struktur dieser Option, so dass ich sie möglicherweise nicht perfekt eingestellt habe, aber es reichte für den Pi aus, den DHCP-Server als einen zu erkennen
von dem aus es versuchen könnte, über das Netzwerk zu booten.

Eine Sache, die mich beunruhigt hat, ist, dass diese Vendor-Option an alle DHCP-Clients und nicht nur an Pis gesendet wird. ich denke, dass
Dies kann gelöst werden, indem auf dem Server Vendor-Klassen definiert und der Client als Pi erkannt wird, aber ich hatte keine Zeit, dies vollständig zu untersuchen.

## Windows-Bereitstellungsdienst

Windows-Bereitstellungsdienste ist die Serverrolle, die TFTP unterstützt. Für Windows-Clients unterstützt es eine Vielzahl von Funktionen
aber wir brauchen nur seine TFTP-Dienste. Möglicherweise müssen Sie Ihrem Server die WDS-Rolle hinzufügen.

WDS erstellt eine REMINST-Freigabe, in der sich die TFTP-Dateien befinden. Ich habe Windows Explorer verwendet, um zu \\Servername\REMINST . zu navigieren
und dann in das Boot-Verzeichnis (einschließlich Unterverzeichnisse) für den Pi an diesen Ort kopiert (einschließlich der aktualisierten bootcode.bin und start.elf).

Wie bei DHCP bin ich mir nicht sicher, wie sich das Hinzufügen des Pi-Inhalts zu den WDS-Ordnern auf den normalen Windows-Betrieb von WDS auswirken kann, da
Das nutze ich in meinem Netzwerk nicht.

## Windows NFS-Dienste

Die obigen Schritte sollten es dem Pi ermöglichen, zumindest zu booten, aber nicht den Rest des Dateisystems haben.
Wir müssen jetzt die Dienste für NFS-Rolle zu unserem Server hinzufügen. Sobald sie installiert sind, können wir NFS-Freigaben erstellen, die der Pi verwenden kann.

Ich habe einen Ordner erstellt, der das Dateisystem des Pi enthält, und klicke dann mit der rechten Maustaste auf den Ordner, wähle Eigenschaften und gehe zum
'NFS-Freigabe' und klicken Sie auf die Schaltfläche 'NFS-Freigabe verwalten'. Ich habe die Freigabe 'PiRoot' aufgerufen und die Optionen 'Keine Serverauthentifizierung [Auth_Sys]', 'Nicht zugeordneten Benutzerzugriff aktivieren', 'Nicht zugeordneten Unix-Benutzerzugriff zulassen' gewählt.
Ich glaube, dass diese Benutzerberechtigungen und Eigentumsrechte am besten funktionieren.

Ich habe dann einen normalen Pi hochgefahren und das gesamte Dateisystem auf die neu erstellte PiRoot-Freigabe auf dem Windows-Server kopiert.

##cmdline.txt

Zuletzt habe ich die cmdline.txt so geändert, dass sie auf den PiRoot NFS-Ordner verweist

    root=/dev/nfs rootfstype=nfs nfsroot=192.168.10.200:/PiRoot/ rw ip=dhcp rootwait

