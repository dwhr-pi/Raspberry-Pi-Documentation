## vcdbg

`vcdbg` ist eine Anwendung, die beim Debuggen der VideoCore-GPU von Linux auf dem ARM hilft. Es muss als Root ausgeführt werden. Diese Anwendung ist hauptsächlich für Raspberry Pi-Ingenieure von Nutzen, obwohl es einige Befehle gibt, die allgemeine Benutzer möglicherweise nützlich finden.

`sudo vcdbg help` gibt eine Liste der verfügbaren Befehle aus.

Hier wurden nur Nutzungsmöglichkeiten für Endverbraucher beschrieben.

### Befehle

#### Ausführung

Zeigt verschiedene Versionsinformationen des VideoCore an.

#### Protokoll

Speichert Protokolle aus dem angegebenen Subsystem. Mögliche Optionen sind:

| log | Beschreibung |
|-----|--------------|
| Nachricht | Druckt das Nachrichtenprotokoll aus |
| behaupten | Druckt das Assertionsprotokoll aus |
| ex | Druckt das Ausnahmeprotokoll aus |
| info | Druckt Informationen aus den Logging-Headern aus |
| Ebene | Legt die VCOS-Protokollierungsebene für die angegebene Kategorie fest, n\|e\|w\|i\|t |
| Liste | Auflisten der VCOS-Protokollierungsstufen |

z.B. So drucken Sie den aktuellen Inhalt des Meldungsprotokolls aus:

`vcdbg log msg`

#### malloc

Listen Sie alle aktuellen Speicherzuweisungen im VideoCore-Heap auf.

#### pools

Den aktuellen Status des Poolzuordners auflisten

#### reloc

Listet ohne weitere Parameter den aktuellen Status des verschiebbaren Allokators auf. Verwenden Sie `sudo vcdbg reloc small`, um auch kleine Zuordnungen aufzulisten.

Verwenden Sie den Unterbefehl `sudo vcdbg reloc stats`, um Statistiken für den verschiebbaren Allocator aufzulisten.

#### hist

Befehle im Zusammenhang mit dem Aufgabenverlauf.

Verwenden Sie `sudo vcdbg hist gnuplot`, um den Aufgabenverlauf im gnuplot-Format in task.gpt und task.dat zu speichern

  
  
