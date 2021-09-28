# Gerätebäume, Overlays und Parameter

Raspberry Pi-Kernel und -Firmware verwenden einen Device Tree (DT), um die im Pi vorhandene Hardware zu beschreiben. Diese Gerätebäume können DT-Parameter enthalten, die ein gewisses Maß an Kontrolle über einige integrierte Funktionen bieten. DT-Overlays ermöglichen die Beschreibung und Konfiguration optionaler externer Hardware und unterstützen auch Parameter für mehr Kontrolle.

Der Firmware-Loader (`start.elf` und seine Varianten) ist für das Laden des DTB (Device Tree Blob - eine maschinenlesbare DT-Datei) verantwortlich. Es wählt basierend auf der Kartenrevisionsnummer aus, welches geladen wird, und nimmt bestimmte Änderungen vor, um es weiter anzupassen (Speichergröße, Ethernet-Adressen usw.). Diese Laufzeitanpassung vermeidet die Notwendigkeit vieler DTBs mit nur geringfügigen Unterschieden.

`config.txt` wird nach vom Benutzer bereitgestellten Parametern zusammen mit allen Overlays und deren Parametern durchsucht, die dann angewendet werden. Der Loader untersucht das Ergebnis, um (zum Beispiel) zu erfahren, welcher UART, falls vorhanden, für die Konsole verwendet werden soll. Schließlich startet es den Kernel und übergibt einen Zeiger an den zusammengeführten DTB.

<a name="part1"></a>
## Gerätebäume

Ein Gerätebaum (DT) ist eine Beschreibung der Hardware in einem System. Es sollte den Namen der Basis-CPU, ihre Speicherkonfiguration und alle Peripheriegeräte (intern und extern) enthalten. Ein DT sollte nicht zur Beschreibung der Software verwendet werden, führt aber durch die Auflistung der Hardwaremodule in der Regel zum Laden von Treibermodulen. Es hilft, sich daran zu erinnern, dass DTs OS-neutral sein sollen, also sollte alles, was Linux-spezifisch ist, wahrscheinlich nicht vorhanden sein.

Ein Gerätebaum repräsentiert die Hardwarekonfiguration als eine Hierarchie von Knoten. Jeder Knoten kann Eigenschaften und Unterknoten enthalten. Eigenschaften sind benannte Byte-Arrays, die Strings, Zahlen (Big-Endian), beliebige Byte-Sequenzen und beliebige Kombinationen davon enthalten können. In Analogie zu einem Dateisystem sind Knoten Verzeichnisse und Eigenschaften Dateien. Die Positionen von Knoten und Eigenschaften innerhalb des Baums können mit einem Pfad beschrieben werden, mit Schrägstrichen als Trennzeichen und einem einzelnen Schrägstrich (`/`) zur Angabe der Wurzel.

<a name="part1.1"></a>
### 1.1: Grundlegende DTS-Syntax

Gerätebäume werden normalerweise in einer Textform geschrieben, die als Gerätebaumquelle (DTS) bekannt ist, und in Dateien mit der Endung „.dts“ gespeichert. Die DTS-Syntax ist C-ähnlich, mit geschweiften Klammern für die Gruppierung und Semikolons am Ende jeder Zeile. Beachten Sie, dass DTS nach dem Schließen von geschweiften Klammern Semikolons erfordert: Denken Sie an C-Strukturen statt an Funktionen. Das kompilierte Binärformat wird als Flattened Device Tree (FDT) oder Device Tree Blob (DTB) bezeichnet und in `.dtb`-Dateien gespeichert.

Das Folgende ist ein einfacher Baum im `.dts`-Format:

```
/dts-v1/;
/include/ "common.dtsi";

/ {
    node1 {
        a-string-property = "Ein String";
        a-string-list-property = "erster String", "zweiter String";
        a-byte-data-property = [0x01 0x23 0x34 0x56];
        cousin: child-node1 {
            first-child-property;
            second-child-property = <1>;
            a-string-property = "Hallo Welt";
        };
        child-node2 {
        };
    };
    node2 {
        an-empty-property;
        a-cell-property = <1 2 3 4>; /* jede Zahl (Zelle) ist ein uint32 */
        child-node1 {
            my-cousin = <&cousin>;
        };
    };
};

/node2 {
    another-property-for-node2;
};
```

Dieser Baum enthält:

 - ein erforderlicher Header: `/dts-v1/`.
 - Das Einbinden einer anderen DTS-Datei, herkömmlich `*.dtsi` genannt und analog zu einer `.h`-Header-Datei in C - siehe _Eine Nebenbemerkung zu /include/_ unten.
 - ein einzelner Wurzelknoten: `/`
 - ein paar untergeordnete Knoten: `node1` und `node2`
 - einige Kinder für Knoten1: `Kind-Knoten1` und `Kind-Knoten2`
 - ein Label (`cousin`) und eine Referenz auf dieses Label (`&cousin`): siehe _Labels und Referenzen_ unten.
 - mehrere Eigenschaften im Baum verstreut
 - ein wiederholter Knoten (`/node2`) - siehe _Aside about /include/_ unten.

Eigenschaften sind einfache Schlüssel-Wert-Paare, bei denen der Wert entweder leer sein oder einen beliebigen Byte-Stream enthalten kann. Während Datentypen nicht in der Datenstruktur kodiert sind, gibt es einige grundlegende Datendarstellungen, die in einer Gerätebaum-Quelldatei ausgedrückt werden können.

Textstrings (NUL-terminiert) werden mit doppelten Anführungszeichen angegeben:

```
string-property = "ein String";
```

Zellen sind 32-Bit-Ganzzahlen ohne Vorzeichen, die durch spitze Klammern getrennt sind:

```
cell-property = <0xbeef 123 0xabcd1234>;
```

Beliebige Bytedaten werden durch eckige Klammern begrenzt und in Hex eingegeben:

```
binary-property = [01 23 45 67 89 ab cd ef];
```

Daten unterschiedlicher Darstellungen können mit einem Komma verkettet werden:

```
mixed-property = "a string", [01 23 45 67], <0x12345678>;
```

Kommas werden auch verwendet, um Listen von Zeichenfolgen zu erstellen:

```
string-list = "roter Fisch", "blauer Fisch";
```

<a name="part1.2"></a>
### 1.2: Eine Nebenbemerkung zu /include/

Die `/include/`-Direktive führt zu einer einfachen textuellen Einbindung, ähnlich wie die `#include`-Direktive von C, aber eine Funktion des Device Tree-Compilers führt zu unterschiedlichen Verwendungsmustern. Da Knoten benannt sind, möglicherweise mit absoluten Pfaden, ist es möglich, dass derselbe Knoten zweimal in einer DTS-Datei (und ihren Einschlüssen) vorkommt. In diesem Fall werden die Knoten und Eigenschaften kombiniert, wobei die Eigenschaften nach Bedarf verschachtelt und überschrieben werden (spätere Werte überschreiben frühere).

Im obigen Beispiel führt das zweite Auftreten von `/node2` dazu, dass dem Original eine neue Eigenschaft hinzugefügt wird:

```
/node2 {
    an-empty-property;
    a-cell-property = <1 2 3 4>; /* jede Zahl (Zelle) ist ein uint32 */
    another-property-for-node2;
    child-node1 {
        my-cousin = <&cousin>;
    };
};
```

Somit ist es möglich, dass eine `.dtsi` mehrere Stellen in einem Baum überschreibt oder Voreinstellungen vorgibt.

<a name="part1.3"></a>
### 1.3: Labels und Referenzen

Es ist oft notwendig, dass ein Teil des Baumes auf einen anderen verweist, und es gibt vier Möglichkeiten, dies zu tun:

1. Pfadzeichenfolgen

    Pfade sollten selbsterklärend sein, analog zu einem Dateisystem - `/soc/i2s@7e203000` ist der vollständige Pfad zum I2S-Gerät in BCM2835 und BCM2836. Beachten Sie, dass die Standard-APIs dies nicht tun, obwohl es einfach ist, einen Pfad zu einer Eigenschaft zu erstellen (z. B. `/soc/i2s@7e203000/status`); Sie suchen zuerst einen Knoten und wählen dann die Eigenschaften dieses Knotens aus.

1. Phandles

   Ein Phandle ist eine eindeutige 32-Bit-Ganzzahl, die einem Knoten in seiner `Phandle`-Eigenschaft zugewiesen wird. Aus historischen Gründen können Sie auch ein redundantes, passendes `linux,phandle` sehen. Phandles sind fortlaufend nummeriert, beginnend mit 1; 0 ist kein gültiger Phandle. Sie werden normalerweise vom DT-Compiler zugewiesen, wenn er auf einen Verweis auf einen Knoten in einem Integer-Kontext trifft, normalerweise in Form eines Labels (siehe unten). Verweise auf Knoten, die Phandles verwenden, werden einfach als die entsprechenden ganzzahligen (Zellen-)Werte codiert; Es gibt kein Markup, das darauf hinweist, dass sie als Phandles interpretiert werden sollten, da dies anwendungsdefiniert ist.

1. Etiketten

   So wie ein Label in C einer Stelle im Code einen Namen gibt, weist ein DT-Label einem Knoten in der Hierarchie einen Namen zu. Der Compiler nimmt Referenzen auf Labels und wandelt sie in Pfade um, wenn er im String-Kontext (`&node`) und Phandles im Integer-Kontext (`<&node>`) verwendet wird; die ursprünglichen Beschriftungen erscheinen nicht in der kompilierten Ausgabe. Beachten Sie, dass Labels keine Struktur enthalten; sie sind nur Token in einem flachen, globalen Namensraum.

1. Aliase

   Aliase ähneln Labels, erscheinen jedoch in der FDT-Ausgabe als eine Form von Index. Sie werden als Eigenschaften des `/aliases`-Knotens gespeichert, wobei jede Eigenschaft einen Aliasnamen einer Pfadzeichenfolge zuordnet. Obwohl der aliases-Knoten in der Quelle vorkommt, erscheinen die Pfad-Strings normalerweise als Verweise auf Labels (`&node`) und werden nicht vollständig ausgeschrieben. DT-APIs, die eine Pfadzeichenfolge zu einem Knoten auflösen, betrachten normalerweise das erste Zeichen des Pfads und behandeln Pfade, die nicht mit einem Schrägstrich beginnen, als Aliase, die zuerst mithilfe der Tabelle `/aliases` in einen Pfad konvertiert werden müssen.

<a name="part1.4"></a>
### 1.4: Semantik des Gerätebaums

Wie man einen Gerätebaum erstellt und wie man ihn am besten verwendet, um die Konfiguration einiger Hardware zu erfassen, ist ein umfangreiches und komplexes Thema. Es stehen viele Ressourcen zur Verfügung, von denen einige unten aufgeführt sind, aber einige Punkte verdienen in diesem Dokument Erwähnung:

`kompatible` Eigenschaften sind das Bindeglied zwischen der Hardwarebeschreibung und der Treibersoftware. Wenn ein Betriebssystem auf einen Knoten mit einer "kompatiblen" Eigenschaft stößt, sucht es in seiner Datenbank der Gerätetreiber nach der besten Übereinstimmung. Unter Linux führt dies in der Regel dazu, dass das Treibermodul automatisch geladen wird, sofern es entsprechend gekennzeichnet und nicht auf die schwarze Liste gesetzt wurde.

Die Eigenschaft `status` gibt an, ob ein Gerät aktiviert oder deaktiviert ist. Wenn der `Status` `ok`, `okay` oder nicht vorhanden ist, dann ist das Gerät aktiviert. Andernfalls sollte `status` `disabled` sein, damit das Gerät deaktiviert ist. Es kann nützlich sein, Geräte in einer `.dtsi`-Datei mit dem Status `disabled` zu platzieren. Eine abgeleitete Konfiguration kann dann diese `.dtsi` enthalten und den Status für die benötigten Geräte auf `okay` setzen.

<a name="part2"></a>
##Teil 2: Gerätebaum-Overlays

Ein modernes SoC (System on a Chip) ist ein sehr kompliziertes Gerät; ein vollständiger Gerätebaum kann Hunderte von Zeilen lang sein. Noch einen Schritt weiter zu gehen und den SoC mit anderen Komponenten auf einem Board zu platzieren, macht die Sache nur noch schlimmer. Um dies überschaubar zu halten, insbesondere wenn es verwandte Geräte gibt, die Komponenten teilen, ist es sinnvoll, die gemeinsamen Elemente in `.dtsi`-Dateien abzulegen, um sie möglicherweise aus mehreren `.dts`-Dateien einzubinden.

Wenn ein System wie Raspberry Pi auch optionales Plug-in-Zubehör wie HATs unterstützt, wächst das Problem. Letztendlich erfordert jede mögliche Konfiguration einen Gerätebaum, um sie zu beschreiben, aber wenn man all die verschiedenen Basismodelle und die große Anzahl an verfügbarem Zubehör berücksichtigt, beginnt sich die Anzahl der Kombinationen rasant zu vervielfachen.

Was benötigt wird, ist eine Möglichkeit, diese optionalen Komponenten unter Verwendung eines partiellen Gerätebaums zu beschreiben und dann in der Lage zu sein, einen vollständigen Baum aufzubauen, indem ein Basis-DT genommen und eine Anzahl optionaler Elemente hinzugefügt wird. Sie können dies tun, und diese optionalen Elemente werden "Overlays" genannt.

Es sei denn, wenn Sie lernen möchten, wie man Overlays für Raspberry Pis schreibt, können Sie lieber zu [Teil 3: Verwenden von Gerätebäumen auf Raspberry Pi] (#Teil3) springen.

<a name="part2.1"></a>
### 2.1: Fragmente

Ein DT-Overlay umfasst eine Anzahl von Fragmenten, von denen jedes auf einen Knoten und seine Unterknoten abzielt. Obwohl das Konzept einfach genug klingt, erscheint die Syntax zunächst ziemlich seltsam:

```
// Aktivieren Sie die i2s-Schnittstelle
/dts-v1/;
/plugin/;

/ {
    compatible = "brcm,bcm2835";

    fragment@0 {
        target = <&i2s>;
        __overlay__ {
            status = "Okay";
            test_ref = <&test_label>;
            test_label: test_subnode {
                dummy;
            };
        };
    };
};
```

Die Zeichenfolge "kompatibel" identifiziert dies als für BCM2835, die die Basisarchitektur für die Raspberry Pi-SoCs ist; Wenn das Overlay Funktionen eines Pi 4 verwendet, ist `brcm,bcm2711` der richtige Wert, ansonsten kann `brcm,bcm2835` für alle Pi-Overlays verwendet werden. Dann kommt das erste (und in diesem Fall einzige) Fragment. Fragmente sollten fortlaufend von null an nummeriert werden. Wenn Sie dies nicht einhalten, können einige oder alle Ihrer Fragmente übersehen werden.

Jedes Fragment besteht aus zwei Teilen: einer "Ziel"-Eigenschaft, die den Knoten identifiziert, auf den das Overlay angewendet werden soll; und das `__overlay__` selbst, dessen Hauptteil dem Zielknoten hinzugefügt wird. Das obige Beispiel kann so interpretiert werden, als wäre es so geschrieben:

```
/dts-v1/;
/plugin/;

/ {
    compatible = "brcm,bcm2835";
};

&i2s {
    status = "Okay";
    test_ref = <&test_label>;
    test_label: test_subnode {
        dummy;
    };
};
```
(Tatsächlich können Sie es mit einer ausreichend neuen Version von `dtc` genau so schreiben und eine identische Ausgabe erhalten, aber einige hausgemachte Tools verstehen dieses Format noch nicht, sodass jedes Overlay, das Sie möglicherweise in den Standard-Raspberry integriert haben möchten Pi OS Kernel sollte vorerst im alten Format geschrieben werden).

Der Effekt des Zusammenführens dieses Overlays mit einem standardmäßigen Raspberry Pi-Basis-Gerätebaum (z. B. `bcm2708-rpi-b-plus.dtb`), vorausgesetzt, das Overlay wird anschließend geladen, besteht darin, die I2S-Schnittstelle zu aktivieren, indem der Status auf `okay . geändert wird `. Aber wenn Sie versuchen, dieses Overlay zu kompilieren, indem Sie:

```
dtc -I dts -O dtb -o 2nd.dtbo 2nd-overlay.dts
```

Sie erhalten einen Fehler:

```
Label oder Pfad i2s nicht gefunden
```

Dies sollte nicht zu unerwartet sein, da es keinen Verweis auf die Basisdatei `.dtb` oder `.dts` gibt, damit der Compiler das `i2s`-Label finden kann.

Versuchen Sie es erneut, verwenden Sie dieses Mal das ursprüngliche Beispiel und fügen Sie die Option `-@` hinzu, um unaufgelöste Verweise zuzulassen (und `-Hepapr`, um etwas Unordnung zu entfernen):

```
dtc -@ -Hepapr -I dts -O dtb -o 1st.dtbo 1st-overlay.dts
```

Wenn `dtc` einen Fehler bezüglich der dritten Zeile zurückgibt, hat es nicht die für die Überlagerungsarbeit erforderlichen Erweiterungen. Führen Sie `sudo apt install device-tree-compiler` aus und versuchen Sie es erneut - diesmal sollte die Kompilierung erfolgreich abgeschlossen werden. Beachten Sie, dass ein geeigneter Compiler auch im Kernelbaum als `scripts/dtc/dtc` verfügbar ist, der erstellt wird, wenn das make-Ziel `dtbs` verwendet wird:
```
make ARCH=arm dtbs
```

Es ist interessant, den Inhalt der DTB-Datei zu dumpen, um zu sehen, was der Compiler generiert hat:

```
$ fdtdump 1st.dtbo
/dts-v1/;
// magic:		0xd00dfeed
// totalsize:		0x207 (519)
// off_dt_struct:	0x38
// off_dt_strings:	0x1c8
// off_mem_rsvmap:	0x28
// version:		17
// last_comp_version:	16
// boot_cpuid_phys:	0x0
// size_dt_strings:	0x3f
// size_dt_struct:	0x190

/ {
    compatible = "brcm,bcm2835";
    fragment@0 {
        target = <0xffffffff>;
        __overlay__ {
            status = "Okay";
            test_ref = <0x00000001>;
            test_subnode {
                dummy;
                phandle = <0x00000001>;
            };
        };
    };
    __symbols__ {
        test_label = "/fragment@0/__overlay__/test_subnode";
    };
    __fixups__ {
        i2s = "/fragment@0:target:0";
    };
    __local_fixups__ {
        fragment@0 {
            __overlay__ {
                test_ref = <0x00000000>;
            };
        };
    };
};
```

Nach der ausführlichen Beschreibung der Dateistruktur folgt unser Fragment. Aber schauen Sie genau hin - wo wir `&i2s` geschrieben haben, steht jetzt `0xffffffff`, ein Hinweis darauf, dass etwas Seltsames passiert ist (ältere Versionen von dtc könnten stattdessen `0xdeadbeef` sagen). Der Compiler hat auch eine `Phandle`-Eigenschaft hinzugefügt, die eine (zu diesem Overlay) eindeutige kleine Ganzzahl enthält, um anzuzeigen, dass der Knoten ein Label hat, und alle Verweise auf das Label durch dieselbe kleine ganze Zahl ersetzt.

Parameter werden im DTS definiert, indem der Wurzel ein `__overrides__`-Knoten hinzugefügt wird. Es enthält Eigenschaften, deren Namen die ausgewählten Parameternamen sind und deren Werte eine Sequenz sind, die ein Phandle (Referenz auf ein Label) für den Zielknoten und eine Zeichenfolge umfasst, die die Zieleigenschaft angibt; String-, Integer- (Zelle) und boolesche Eigenschaften werden unterstützt.

<a name="part2.2.1"></a>
#### 2.2.1: String-Parameter

String-Parameter werden wie folgt deklariert:

```
name = <&label>,"property";
```

wobei `label` und `property` durch geeignete Werte ersetzt werden. Zeichenfolgenparameter können dazu führen, dass ihre Zieleigenschaften wachsen, schrumpfen oder erstellt werden.

Beachten Sie, dass Eigenschaften namens `status` speziell behandelt werden; Werte ungleich null/wahr/ja/ein werden in die Zeichenfolge `"okay"` umgewandelt, während null/falsch/nein/aus `"deaktiviert"` wird.

<a name="part2.2.2"></a>
#### 2.2.2: Integer-Parameter

Integer-Parameter werden wie folgt deklariert:

```
name = <&label>,"property.offset"; // 8-bit
name = <&label>,"property;offset"; // 16-bit
name = <&label>,"property:offset"; // 32-bit
name = <&label>,"property#offset"; // 64-bit
```

wobei `label`, `property` und `offset` durch geeignete Werte ersetzt werden; der Offset wird in Bytes relativ zum Anfang der Eigenschaft (standardmäßig in Dezimalzahl) angegeben, und das vorangestellte Trennzeichen bestimmt die Größe des Parameters. In einer Änderung gegenüber früheren Implementierungen können sich ganzzahlige Parameter auf nicht vorhandene Eigenschaften oder auf Offsets über das Ende einer vorhandenen Eigenschaft hinaus beziehen.

<a name="part2.2.3"></a>
#### 2.2.3: Boolesche Parameter

Der Gerätebaum codiert boolesche Werte als Eigenschaften der Länge Null; wenn vorhanden, dann ist die Eigenschaft wahr, andernfalls ist sie falsch. Sie sind wie folgt definiert:

```
boolean_property; // Setzt die 'boolean_property' auf true (wahr)
```

Beachten Sie, dass einer Eigenschaft der Wert `false` zugewiesen wird, indem sie nicht definiert wird. Boolesche Parameter werden wie folgt deklariert:

```
name = <&label>,"property?";
```

wobei `label` und `property` durch geeignete Werte ersetzt werden.

Invertierte boolesche Werte invertieren den Eingabewert, bevor er angewendet wird, wie ein regulärer boolescher Wert. sie werden ähnlich deklariert, aber verwenden Sie `!`, um die Inversion anzuzeigen:

```
name = <&label>,"property!";
```

Boolesche Parameter können dazu führen, dass Eigenschaften erstellt oder gelöscht werden, aber sie können keine Eigenschaft löschen, die bereits in der Basis-DTB vorhanden ist.

<a name="part2.2.4"></a>
#### 2.2.4: Byte-String-Parameter

Byte-String-Eigenschaften sind beliebige Folgen von Bytes, z.B. MAC-Adressen. Sie akzeptieren Zeichenfolgen von hexadezimalen Bytes mit oder ohne Doppelpunkte zwischen den Bytes.

``` 
mac_address = <&ethernet0>,"local_mac_address[";
```

Das `[` wurde gewählt, um der DT-Syntax zum Deklarieren einer Byte-Zeichenfolge zu entsprechen:

```
local_mac_address = [aa bb cc dd ee ff];
```

<a name="part2.2.5"></a>
#### 2.2.5: Parameter mit mehreren Zielen

Es gibt Situationen, in denen es praktisch ist, denselben Wert an mehreren Stellen im Gerätebaum einstellen zu können. Anstelle des plumpen Ansatzes, mehrere Parameter zu erstellen, ist es möglich, einem einzelnen Parameter mehrere Ziele hinzuzufügen, indem Sie sie wie folgt verketten:

```
    __overrides__ {
        gpiopin = <&w1>,"gpios:4",
                  <&w1_pins>,"brcm,pins:0";
        ...
    };
```

(Beispiel aus dem `w1-gpio`-Overlay)

Beachten Sie, dass es sogar möglich ist, mit einem einzigen Parameter auf Eigenschaften unterschiedlicher Typen abzuzielen. Sie könnten einen "enable"-Parameter vernünftigerweise mit einem `status`-String, Zellen, die null oder eins enthalten, und einer richtigen booleschen Eigenschaft verbinden.

<a name="part2.2.6"></a>
#### 2.2.6: Wörtliche Zuordnungen

Wie in [2.2.5](#part2.2.5) zu sehen ist, ermöglicht der DT-Parametermechanismus das Patchen mehrerer Ziele von demselben Parameter, aber das Dienstprogramm ist dadurch eingeschränkt, dass derselbe Wert an alle Speicherorte geschrieben werden muss ( mit Ausnahme der Formatkonvertierung und der Negation, die von invertierten booleschen Werten verfügbar ist). Das Hinzufügen eingebetteter Literalzuweisungen ermöglicht es einem Parameter, beliebige Werte zu schreiben, unabhängig vom vom Benutzer bereitgestellten Parameterwert.

Zuweisungen erscheinen am Ende einer Deklaration und werden durch ein `=` gekennzeichnet:

```
str_val  = <&target>,"strprop=value";              // 1
int_val  = <&target>,"intprop:0=42                 // 2
int_val2 = <&target>,"intprop:0=",<42>;            // 3
bytes    = <&target>,"bytestr[=b8:27:eb:01:23:45"; // 4
```

Die Zeilen 1, 2 und 4 sind ziemlich offensichtlich, aber Zeile 3 ist interessanter, da der Wert als ganzzahliger (Zellen-) Wert angezeigt wird. Der DT-Compiler wertet Integer-Ausdrücke zur Kompilierzeit aus, was praktisch sein kann (insbesondere wenn Makrowerte verwendet werden), aber die Zelle kann auch einen Verweis auf ein Label enthalten:

```
// Erzwinge, dass eine LED einen GPIO auf dem internen GPIO-Controller verwendet. 
exp_led = <&led1>,"gpios:0=",<&gpio>,
          <&led1>,"gpios:4";
```

Wenn das Overlay angewendet wird, wird das Label wie üblich mit dem Basis-DTB aufgelöst. Beachten Sie, dass es eine gute Idee ist, mehrteilige Parameter auf mehrere Zeilen wie diese aufzuteilen, um sie leichter lesbar zu machen - etwas, das mit dem Hinzufügen von Zellenwertzuweisungen wie dieser immer notwendiger wird.

Beachten Sie, dass Parameter nichts tun, wenn sie nicht angewendet werden - ein Standardwert in einer Nachschlagetabelle wird ignoriert, es sei denn, der Parametername wird ohne Zuweisung eines Werts verwendet.

<a name="part2.2.7"></a>
#### 2.2.7: Nachschlagetabellen

Lookup-Tabellen ermöglichen die Transformation von Parametereingabewerten, bevor sie verwendet werden. Sie fungieren als assoziative Arrays, ähnlich wie switch/case-Anweisungen:

```
phonetic = <&node>,"letter{a=alpha,b=bravo,c=charlie,d,e,='tango uniform'}";
bus      = <&fragment>,"target:0{0=",<&i2c0>,"1=",<&i2c1>,"}";
```

Ein Schlüssel ohne `=Wert` bedeutet, den Schlüssel als Wert zu verwenden, ein `=` ohne Schlüssel davor ist der Standardwert, falls keine Übereinstimmung vorliegt und die Liste mit einem Komma (oder einem leeren) zu beginnen oder zu beenden Schlüssel=Wert-Paar irgendwo) gibt an, dass der nicht übereinstimmende Eingabewert unverändert verwendet werden soll; Andernfalls ist es ein Fehler, keine Übereinstimmung zu finden.

Beachten Sie, dass das Komma-Trennzeichen innerhalb der Tabellenzeichenfolge nach einem ganzzahligen Zellenwert implizit ist - das Hinzufügen von einem explizit erzeugt ein leeres Paar (siehe oben).

Hinweis Da Lookup-Tabellen mit Eingabewerten arbeiten und literale Zuweisungen diese ignorieren, ist es nicht möglich, die beiden - Zeichen nach dem schließenden `}` in der Lookup-Deklaration zu kombinieren, werden als Fehler behandelt.

<a name="part2.2.8"></a>
#### 2.2.8 Overlay-/Fragmentparameter

Der beschriebene DT-Parametermechanismus weist eine Reihe von Einschränkungen auf, einschließlich der Unfähigkeit, den Namen eines Knotens zu ändern und beliebige Werte in beliebige Eigenschaften zu schreiben, wenn ein Parameter verwendet wird. Eine Möglichkeit, einige dieser Einschränkungen zu überwinden, besteht darin, bestimmte Fragmente bedingt einzuschließen oder auszuschließen.

Ein Fragment kann vom endgültigen Zusammenführungsprozess ausgeschlossen (deaktiviert) werden, indem der Knoten `__overlay__` in `__dormant__` umbenannt wird. Die Syntax der Parameterdeklaration wurde erweitert, damit das ansonsten unzulässige Nullziel-Phandle anzeigt, dass die folgende Zeichenfolge Operationen im Fragment- oder Überlagerungsbereich enthält. Bisher wurden vier Operationen durchgeführt:

```
+<n> // Fragment aktivieren <n>
-<n> // Fragment deaktivieren <n>
=<n> // Fragment <n> aktivieren, wenn der zugewiesene Parameterwert wahr (true) ist, andernfalls deaktivieren
!<n> // Fragment <n> aktivieren, wenn der zugewiesene Parameterwert falsch (false) ist, andernfalls deaktivieren
```

Beispiele:

```
just_one    = <0>,"+1-2"; // Aktivieren 1, deaktivieren 2
conditional = <0>,"=3!4"; // 3 aktivieren, 4 deaktivieren, wenn der Wert wahr ist,
                          // sonst 3 deaktivieren, 4 aktivieren.
```

Das Overlay `i2c-rtc` verwendet diese Technik.

<a name="part2.2.9"></a>
#### 2.2.9 Besondere Eigenschaften

Einige Eigenschaftsnamen werden, wenn sie von einem Parameter als Ziel verwendet werden, speziell behandelt. Einer, den Sie vielleicht schon bemerkt haben - `status` - der einen booleschen Wert entweder in `okay` für true und `disabled` für false umwandelt.

Die Zuweisung an die Eigenschaft `bootargs` wird an diese angehängt, anstatt sie zu überschreiben - so können Einstellungen zur Kernel-Befehlszeile hinzugefügt werden.

Die Eigenschaft `reg` wird verwendet, um Geräteadressen anzugeben - die Position eines speicherabgebildeten Hardwareblocks, die Adresse auf einem I2C-Bus usw. Die Namen der untergeordneten Knoten sollten mit ihren Adressen in hexadezimaler Form qualifiziert werden, indem `@` als verwendet wird ein Trennzeichen:
```
        bmp280@76 {
            reg = <0x77>;
            ...
        };
```
Bei der Zuweisung an die Eigenschaft `reg` wird der Adressteil des Elternknotennamens durch den zugewiesenen Wert ersetzt. Dies kann verwendet werden, um einen Konflikt zwischen Knotennamen zu verhindern, wenn dasselbe Overlay mehrmals verwendet wird - eine Technik, die vom `i2c-gpio`-Overlay verwendet wird.

Die `name`-Eigenschaft ist eine Pseudo-Eigenschaft - sie sollte nicht in einem DT erscheinen, aber ihre Zuweisung bewirkt, dass der Name ihres Elternknotens auf den zugewiesenen Wert geändert wird. Wie die Eigenschaft `reg` kann dies verwendet werden, um Knoten eindeutige Namen zu geben.

<a name="part2.2.10"></a>
#### 2.2.10 Die Overlay-Kartendatei

Die Einführung des Pi 4, der auf dem BCM2711-SoC basiert, brachte viele Änderungen mit sich; Einige dieser Änderungen sind zusätzliche Schnittstellen, und einige sind Modifikationen (oder Entfernungen) vorhandener Schnittstellen. Es gibt neue Overlays speziell für den Pi 4 die auf älterer Hardware keinen Sinn machen, z.B. Overlays, die die neuen SPI-, I2C- und UART-Schnittstellen aktivieren, aber andere Overlays werden nicht korrekt angewendet, obwohl sie Funktionen steuern, die auf dem neuen Gerät noch relevant sind.

Es besteht daher ein Bedarf an einem Verfahren zum Anpassen eines Overlays an mehrere Plattformen mit unterschiedlicher Hardware. Sie alle in einer einzigen .dtbo-Datei zu unterstützen, würde eine starke Nutzung versteckter ("ruhender") Fragmente und einen Wechsel zu einem On-Demand-Symbolauflösungsmechanismus erfordern, damit ein fehlendes Symbol, das nicht benötigt wird, keinen Fehler verursacht. Eine einfachere Lösung besteht darin, eine Funktion hinzuzufügen, um einen Overlay-Namen je nach aktueller Plattform einer von mehreren Implementierungsdateien zuzuordnen.

Die Overlay-Map, die mit der Umstellung auf Linux 5.4 ausgerollt wird, ist eine Datei, die beim Booten von der Firmware geladen wird. Es ist im DTS-Quellformat - `overlay_map.dts` geschrieben, in `overlay_map.dtb` kompiliert und im Overlays-Verzeichnis gespeichert.

Dies ist eine bearbeitete Version der aktuellen Kartendatei (die Vollversion ist [hier](https://github.com/raspberrypi/linux/blob/rpi-5.4.y/arch/arm/boot/dts/overlays/overlay_map.dts) verfügbar).:

```
/ {
    vc4-kms-v3d {
        bcm2835;
        bcm2711 = "vc4-kms-v3d-pi4";
    };

    vc4-kms-v3d-pi4 {
        bcm2711;
    };

    uart5 {
        bcm2711;
    };

    pi3-disable-bt {
        renamed = "disable-bt";
    };

    lirc-rpi {
        deprecated = "use gpio-ir";
    };
};
```

Jeder Knoten hat den Namen eines Overlays, das eine besondere Behandlung erfordert. Die Eigenschaften jedes Knotens sind entweder Plattformnamen oder eine von wenigen speziellen Direktiven. Die derzeit unterstützten Plattformen sind "bcm2835", die alle Pis enthalten, die um die SoCs BCM2835, BCM2836 und BCM2837 herum gebaut wurden, und "bcm2711" für Pi 4B.

Ein Plattformname ohne Wert (eine leere Eigenschaft) zeigt an, dass das aktuelle Overlay mit der Plattform kompatibel ist. beispielsweise ist `vc4-kms-v3d` mit der `bcm2835`-Plattform kompatibel. Ein nicht leerer Wert für eine Plattform ist der Name eines alternativen Overlays, das anstelle des angeforderten verwendet werden soll; Wenn Sie auf BCM2711 nach `vc4-kms-v3d` fragen, wird stattdessen `vc4-kms-v3d-pi4` geladen. Jede Plattform, die nicht im Knoten eines Overlays enthalten ist, ist mit diesem Overlay nicht kompatibel.

Der zweite Beispielknoten – `vc4-kms-v3d-pi4` – könnte aus dem Inhalt von `vc4-kms-v3d` abgeleitet werden, aber diese Intelligenz fließt in die Konstruktion der Datei ein, nicht in ihre Interpretation.

Für den Fall, dass eine Plattform nicht für ein Overlay gelistet ist, kann eine der Sonderrichtlinien gelten:

* Die Direktive `renamed` gibt den neuen Namen des Overlays an (der weitgehend mit dem Original kompatibel sein sollte), protokolliert aber auch eine Warnung vor der Umbenennung.

* Die Direktive `deprecated` enthält eine kurze erklärende Fehlermeldung, die protokolliert wird, nachdem das allgemeine Präfix `overlay '...' veraltet ist:`.

Denken Sie daran: Es müssen nur Ausnahmen aufgelistet werden - das Fehlen eines Knotens für ein Overlay bedeutet, dass die Standarddatei für alle Plattformen verwendet werden sollte.

Der Zugriff auf Diagnosemeldungen von der Firmware wird in [Debugging] (#part4.1) behandelt.

Die Dienstprogramme `dtoverlay` und `dtmerge` wurden erweitert, um die Kartendatei zu unterstützen:
* `dtmerge` extrahiert den Plattformnamen aus dem kompatiblen String im Basis-DTB.
* `dtoverlay` liest den kompatiblen String aus dem Live-Gerätebaum unter `/proc/device-tree`, aber Sie können die Option `-p` verwenden, um einen alternativen Plattformnamen anzugeben (nützlich für Probeläufe auf einer anderen Plattform).

Beide senden Fehler, Warnungen und alle Debug-Ausgaben an STDERR.

<a name="part2.2.11"></a>
#### 2.2.11 Beispiele

Hier sind einige Beispiele für verschiedene Arten von Eigenschaften mit Parametern, um sie zu ändern:

```
/ {
    fragment@0 {
        target-path = "/";
        __overlay__ {

            test: test_node {
                string = "Hello";
                status = "disabled";
                bytes = /bits/ 8 <0x67 0x89>;
                u16s = /bits/ 16 <0xabcd 0xef01>;
                u32s = /bits/ 32 <0xfedcba98 0x76543210>;
                u64s = /bits/ 64 < 0xaaaaa5a55a5a5555 0x0000111122223333>;
                bool1; // Defaults to true
                       // bool2 defaults to false
                mac = [01 23 45 67 89 ab];
                spi = <&spi0>;
            };
        };
    };

    fragment@1 {
        target-path = "/";
        __overlay__ {
            frag1;
        };
    };

    fragment@2 {
        target-path = "/";
        __dormant__ {
            frag2;
        };
    };

    __overrides__ {
        string =      <&test>,"string";
        enable =      <&test>,"status";
        byte_0 =      <&test>,"bytes.0";
        byte_1 =      <&test>,"bytes.1";
        u16_0 =       <&test>,"u16s;0";
        u16_1 =       <&test>,"u16s;2";
        u32_0 =       <&test>,"u32s:0";
        u32_1 =       <&test>,"u32s:4";
        u64_0 =       <&test>,"u64s#0";
        u64_1 =       <&test>,"u64s#8";
        bool1 =       <&test>,"bool1!";
        bool2 =       <&test>,"bool2?";
        entofr =      <&test>,"english",
                      <&test>,"french{hello=bonjour,goodbye='au revoir',weekend}";
        pi_mac =      <&test>,"mac[{1=b8273bfedcba,2=b8273b987654}";
        spibus =      <&test>,"spi:0[0=",<&spi0>,"1=",<&spi1>,"2=",<&spi2>;

        only1 =       <0>,"+1-2";
        only2 =       <0>,"-1+2";
        enable1 =     <0>,"=1";
        disable2 =    <0>,"!2";
    };
};
```

Für weitere Beispiele gibt es eine große Sammlung von Overlay-Quelldateien, die im Raspberry Pi Linux GitHub-Repository [hier](https://github.com/raspberrypi/linux/tree/rpi-5.4.y/arch/arm/boot/dts/overlays) gehostet werden.

<a name="part2.3"></a>
### 2.3: Etiketten exportieren

Die Overlay-Handhabung in der Firmware und die Laufzeit-Overlay-Anwendung, die das Dienstprogramm "dtoverlay" verwenden, behandeln Labels, die in einem Overlay definiert sind, als privat für dieses Overlay. Dies vermeidet die Notwendigkeit, global eindeutige Namen für Labels zu erfinden (was sie kurz hält) und ermöglicht die mehrfache Verwendung desselben Overlays ohne Kollisionen (vorausgesetzt, einige Tricks werden verwendet - siehe [Besondere Eigenschaften](#part2.2.9)) .

Manchmal ist es jedoch sehr nützlich, ein Etikett mit einem Overlay erstellen und von einem anderen verwenden zu können. Die seit dem 14. Februar 2020 veröffentlichte Firmware kann einige Labels als global deklarieren - den Knoten `__export__`:

```
    ...
    public: ...

    __exports__ {
        public; // Export the label 'public' to the base DT
    };
};
```

Wenn diese Überlagerung angewendet wird, entfernt der Loader alle Symbole außer den exportierten, in diesem Fall "öffentlich", und schreibt den Pfad neu, um ihn relativ zum Ziel des Fragments zu machen, das das Label enthält. Danach geladene Overlays können auf `&public` verweisen.

<a name="part2.4"></a>
###2.4: Reihenfolge der Overlay-Anwendung

Unter den meisten Umständen sollte es keine Rolle spielen, in welcher Reihenfolge die Fragmente angewendet werden, aber für Overlays, die sich selbst patchen (wo das Ziel eines Fragments ein Label im Overlay ist, bekannt als Intra-Overlay-Fragment), wird es wichtig. In älterer Firmware werden Fragmente streng nacheinander von oben nach unten angewendet. Mit der seit dem 14. Februar 2020 veröffentlichten Firmware werden Fragmente in zwei Durchgängen angewendet:

ich. Zuerst werden die Fragmente, die auf andere Fragmente abzielen, angewendet und ausgeblendet.
ii. Dann werden die regulären Fragmente angewendet.

Diese Aufteilung ist besonders wichtig für Laufzeit-Overlays, da Schritt (i) im Dienstprogramm `dtoverlay` erfolgt und Schritt (ii) vom Kernel ausgeführt wird (der keine Intra-Overlay-Fragmente verarbeiten kann).

<a name="part3"></a>
## Teil 3: Verwenden von Gerätebäumen auf Raspberry Pi

<a name="part3.1"></a>
### 3.1: DTBs, Overlays und config.txt

Auf einem Raspberry Pi ist es die Aufgabe des Loaders (eines der `start.elf`-Images) Overlays mit einem entsprechenden Basis-Gerätebaum zu kombinieren und dann einen vollständig aufgelösten Gerätebaum an den Kernel zu übergeben. Die Basis-Gerätebäume befinden sich neben `start.elf` in der FAT-Partition (/boot from Linux) mit den Namen `bcm2711-rpi-4-b.dtb`, `bcm2710-rpi-3-b-plus.dtb`, usw. Beachten Sie, dass einige Modelle (3A+, A, A+) die entsprechenden "b"-Äquivalente (3B+, B, B+) verwenden. Diese Auswahl erfolgt automatisch und ermöglicht die Verwendung desselben SD-Karten-Images in einer Vielzahl von Geräten.

Beachten Sie, dass sich DT und ATAGs gegenseitig ausschließen und die Übergabe eines DT-Blobs an einen Kernel, der ihn nicht versteht, zu einem Boot-Fehler führt. Die Firmware wird immer versuchen, den DT zu laden und an den Kernel weiterzugeben, da alle Kernel seit rpi-4.4.y ohne DTB nicht funktionieren. Sie können dies überschreiben, indem Sie `device_tree=` in config.txt hinzufügen, was die Verwendung von ATAGs erzwingt, was für einfache "Bare-Metal"-Kernel nützlich sein kann.

[ Die Firmware suchte nach einem Trailer, der vom Dienstprogramm `mkknlimg` an Kernel angehängt wurde, aber die Unterstützung dafür wurde eingestellt. ]

Der Loader unterstützt jetzt Builds, die bcm2835_defconfig verwenden, wodurch die vorgeschaltete BCM2835-Unterstützung ausgewählt wird. Diese Konfiguration führt dazu, dass `bcm2835-rpi-b.dtb` und `bcm2835-rpi-b-plus.dtb` erstellt werden. Wenn diese Dateien mit dem Kernel kopiert werden, versucht der Loader standardmäßig, eine dieser DTBs zu laden.

Um den Gerätebaum und die Overlays zu verwalten, unterstützt der Loader eine Reihe von `config.txt`-Anweisungen:

```
dtoverlay=acme-board
dtparam=foo=bar,level=42
```

Dadurch sucht der Loader nach `overlays/acme-board.dtbo` in der Firmware-Partition, die Raspberry Pi OS auf `/boot` mountet. Es sucht dann nach den Parametern `foo` und `level` und weist ihnen die angezeigten Werte zu.

Der Loader sucht auch nach einem angeschlossenen HAT mit einem programmierten EEPROM und lädt das unterstützende Overlay von dort - entweder direkt oder namentlich aus dem "Overlays"-Verzeichnis; Dies geschieht ohne Benutzereingriff.

Es gibt mehrere Möglichkeiten, um festzustellen, ob der Kernel den Gerätebaum verwendet:

1. Die Kernel-Meldung "Machine model:" während des Bootens hat einen Board-spezifischen Wert wie "Raspberry Pi 2 Model B" anstelle von "BCM2709".
2. `/proc/device-tree` existiert und enthält Unterverzeichnisse und Dateien, die genau die Knoten und Eigenschaften des DT widerspiegeln.

Mit einem Gerätebaum sucht und lädt der Kernel automatisch Module, die die angegebenen aktivierten Geräte unterstützen. Infolgedessen ersparen Sie Benutzern des Geräts durch das Erstellen eines geeigneten DT-Overlays das Bearbeiten von `/etc/modules`; die gesamte Konfiguration geht in `config.txt`, und im Fall eines HAT ist selbst dieser Schritt unnötig. Beachten Sie jedoch, dass geschichtete Module wie `i2c-dev` immer noch explizit geladen werden müssen.

Die Kehrseite ist, dass es nicht mehr erforderlich sein sollte, Module, die aufgrund von im Board-Support-Code definierten Plattformgeräten geladen wurden, auf die schwarze Liste zu setzen, da Plattformgeräte nur dann erstellt werden, wenn dies vom DTB angefordert wird. Tatsächlich werden aktuelle Raspberry Pi OS-Images ohne Blacklist-Dateien geliefert (außer bei einigen WLAN-Geräten, bei denen mehrere Treiber verfügbar sind).

<a name="part3.2"></a>
### 3.2: DT-Parameter

Wie oben beschrieben, sind DT-Parameter eine bequeme Möglichkeit, kleine Änderungen an der Konfiguration eines Geräts vorzunehmen. Die aktuellen Basis-DTBs unterstützen Parameter zum Aktivieren und Steuern der Onboard-Audio-, I2C-, I2S- und SPI-Schnittstellen, ohne dedizierte Overlays zu verwenden. Im Gebrauch sehen die Parameter so aus:

```
dtparam=audio=on,i2c_arm=on,i2c_arm_baudrate=400000,spi=on
```

Beachten Sie, dass mehrere Zuweisungen in derselben Zeile platziert werden können, aber stellen Sie sicher, dass Sie die Beschränkung von 80 Zeichen nicht überschreiten.

Wenn Sie ein Overlay haben, das einige Parameter definiert, können diese entweder in nachfolgenden Zeilen wie folgt angegeben werden:

```
dtoverlay=lirc-rpi
dtparam=gpio_out_pin=16
dtparam=gpio_in_pin=17
dtparam=gpio_in_pull=down
```

oder wie folgt an die Overlay-Zeile angehängt:

```
dtoverlay=lirc-rpi,gpio_out_pin=16,gpio_in_pin=17,gpio_in_pull=down
```

Overlay-Parameter sind nur gültig, bis das nächste Overlay geladen wird. Falls ein Parameter mit demselben Namen sowohl von der Überlagerung als auch von der Basis exportiert wird, hat der Parameter in der Überlagerung Vorrang; Aus Gründen der Übersichtlichkeit wird empfohlen, dies zu vermeiden. Um stattdessen den von der Basis-DTB exportierten Parameter verfügbar zu machen, beenden Sie den aktuellen Overlay-Bereich mit:

```
dtoverlay=
```

<a name="part3.3"></a>
### 3.3: Boardspezifische Labels und Parameter

Raspberry Pi-Boards haben zwei I2C-Schnittstellen. Diese sind nominell aufgeteilt: eine für den ARM und eine für VideoCore (die "GPU"). Bei fast allen Modellen gehört `i2c1` zu ARM und `i2c0` zu VC, wo es zur Steuerung der Kamera und zum Auslesen des HAT EEPROM verwendet wird. Es gibt jedoch zwei frühe Überarbeitungen des Modells B, bei denen diese Rollen vertauscht sind.

Um die Verwendung eines Satzes von Overlays und Parametern mit allen Pis zu ermöglichen, erstellt die Firmware einige boardspezifische DT-Parameter. Diese sind:

```
i2c/i2c_arm
i2c_vc
i2c_baudrate/i2c_arm_baudrate
i2c_vc_baudrate
```

Dies sind Aliase für `i2c0`, `i2c1`, `i2c0_baudrate` und `i2c1_baudrate`. Es wird empfohlen, `i2c_vc` und `i2c_vc_baudrate` nur zu verwenden, wenn es wirklich nötig ist - zum Beispiel wenn Sie ein HAT-EEPROM programmieren (was besser mit einem Software-I2C-Bus unter Verwendung des `i2c-gpio`-Overlays geschieht). Die Aktivierung von `i2c_vc` kann dazu führen, dass die Pi-Kamera oder das 7-Zoll-DSI-Display nicht richtig funktioniert.

Für Leute, die Overlays schreiben, wurde das gleiche Aliasing auf die Labels auf den I2C-DT-Knoten angewendet. Sie sollten also schreiben:

```
fragment@0 {
    target = <&i2c_arm>;
    __overlay__ {
        status = "okay";
    };
};
```

Alle Overlays, die die numerischen Varianten verwenden, werden geändert, um die neuen Aliase zu verwenden.

<a name="part3.4"></a>
### 3.4: HATs und Gerätebaum

Ein Raspberry Pi HAT ist ein Add-On-Board mit einem eingebetteten EEPROM, das für einen Raspberry Pi mit einem 40-Pin-Header entwickelt wurde. Das EEPROM enthält jedes DT-Overlay, das erforderlich ist, um das Board zu aktivieren (oder den Namen eines Overlays zum Laden aus dem Dateisystem), und dieses Overlay kann auch Parameter anzeigen.

Das HAT-Overlay wird automatisch von der Firmware nach dem Basis-DTB geladen, sodass seine Parameter zugänglich sind, bis alle anderen Overlays geladen werden oder bis der Overlay-Scope mit `dtoverlay=` beendet wird. Wenn Sie aus irgendeinem Grund das Laden des HAT-Overlays unterdrücken möchten, setzen Sie `dtoverlay=` vor jeder anderen `dtoverlay`- oder `dtparam`-Direktive.

<a name="part3.5"></a>
### 3.5: Dynamischer Gerätebaum

Ab Linux 4.4 unterstützen die RPi-Kernel das dynamische Laden von Overlays und Parametern. Kompatible Kernel verwalten einen Stapel von Overlays, die auf dem Basis-DTB angewendet werden. Änderungen werden sofort in `/proc/device-tree` widergespiegelt und können dazu führen, dass Module geladen und Plattformgeräte erstellt und zerstört werden.

Die Verwendung des Wortes "Stapel" oben ist wichtig - Überlagerungen können nur oben im Stapel hinzugefügt und entfernt werden; Wenn Sie etwas weiter unten im Stapel ändern, müssen Sie zuerst alles darüber entfernen.

Es gibt einige neue Befehle zum Verwalten von Overlays:

<a name="part3.5.1"></a>
#### 3.5.1 Der dtoverlay-Befehl

`dtoverlay` ist ein Befehlszeilen-Dienstprogramm, das Overlays lädt und entfernt, während das System läuft, sowie die verfügbaren Overlays auflistet und ihre Hilfeinformationen anzeigt:

```
pi@raspberrypi ~ $ dtoverlay -h
Usage:
  dtoverlay <overlay> [<param>=<val>...]
                           Overlay (mit Parametern) hinzufügen.
  dtoverlay -D [<idx>]     Probelauf (Overlay vorbereiten, aber nicht anwenden -
                           als dry-run.dtbo abspeichern)
  dtoverlay -r [<overlay>] Entfernen eines Overlays (nach Name, Index oder dem letzten)
  dtoverlay -R [<overlay>] Entfernen aus einem Overlay (nach Name, Index oder allen)
  dtoverlay -l             Aktive Overlays/Parameter auflisten
  dtoverlay -a             Listet alle Overlays auf (Markierung der aktiven)
  dtoverlay -h             Diese Nutzungsnachricht anzeigen
  dtoverlay -h <overlay>   Hilfe zu einem Overlay anzeigen
  dtoverlay -h <overlay> <param>..  Oder seine Parameter
				wobei <overlay> der Name eines Overlays oder 'dtparam' für dtparams ist
Optionen für die meisten Varianten:
    -d <dir>    eben Sie einen alternativen Speicherort für die Overlays an
                (standardmäßig /boot/overlays or /flash/overlays)
    -v          Ausführliche Operation
```

Im Gegensatz zum `config.txt`-Äquivalent müssen alle Parameter eines Overlays in derselben Befehlszeile enthalten sein - der Befehl [dtparam](#part3.5.2) gilt nur für Parameter der Basis-DTB.

Zwei Punkte zu beachten:
1. Befehlsvarianten, die den Kernelstatus ändern (Hinzufügen und Entfernen von Dingen) erfordern Root-Rechte, daher müssen Sie dem Befehl möglicherweise `sudo` voranstellen.

2. Nur Overlays und Parameter, die zur Laufzeit angewendet werden, können entladen werden - ein Overlay oder Parameter, der von der Firmware angewendet wird, wird "eingebrannt", so dass er nicht von `dtoverlay` aufgelistet wird und nicht entfernt werden kann.

<a name="part3.5.2"></a>
#### 3.5.2 Der dtparam-Befehl

`dtparam` erstellt und lädt ein Overlay, das weitgehend den gleichen Effekt hat wie die Verwendung einer dtparam-Direktive in `config.txt`. In der Verwendung entspricht es weitgehend `dtoverlay` mit dem Overlay-Namen `-`, es gibt jedoch einige Unterschiede:

1. `dtparam` listet die Hilfeinformationen für alle bekannten Parameter des Basis-DTB auf. Hilfe zum Befehl dtparam ist weiterhin mit `dtparam -h` verfügbar.

2. Bei der Angabe eines zu entfernenden Parameters können nur Indexnummern verwendet werden (keine Namen).

3. Nicht alle Linux-Subsysteme reagieren auf das Hinzufügen von Geräten zur Laufzeit - I2C-, SPI- und Sound-Geräte funktionieren, einige jedoch nicht.

<a name="part3.5.3"></a>
#### 3.5.3 Richtlinien zum Schreiben von laufzeitfähigen Overlays

Dieser Bereich ist schlecht dokumentiert, aber hier sind einige gesammelte Tipps:

* Das Anlegen oder Löschen eines Geräteobjekts wird ausgelöst, indem ein Knoten hinzugefügt oder entfernt wird oder der Status eines Knotens von deaktiviert zu aktiviert wechselt oder umgekehrt. Achtung - das Fehlen einer "Status"-Eigenschaft bedeutet, dass der Knoten aktiviert ist.

* Erstellen Sie keinen Knoten innerhalb eines Fragments, der einen vorhandenen Knoten im Basis-DTB überschreibt - der Kernel benennt den neuen Knoten um, um ihn eindeutig zu machen. Wenn Sie die Eigenschaften eines vorhandenen Knotens ändern möchten, erstellen Sie ein Fragment, das darauf abzielt.

* ALSA verhindert nicht, dass seine Codecs und andere Komponenten entladen werden, während sie verwendet werden. Das Entfernen eines Overlays kann eine Kernel-Ausnahme verursachen, wenn es einen Codec löscht, der noch von einer Soundkarte verwendet wird. Experimente haben ergeben, dass Geräte in umgekehrter Reihenfolge der Fragmente im Overlay gelöscht werden, sodass das Platzieren des Knotens für die Karte nach den Knoten für die Komponenten ein geordnetes Herunterfahren ermöglicht.

<a name="part3.5.4"></a>
#### 3.5.4 Vorbehalte

Das Laden von Overlays zur Laufzeit ist eine neue Erweiterung des Kernels, und bisher gibt es keine akzeptierte Möglichkeit, dies vom Userspace aus zu tun. Durch das Verbergen der Details dieses Mechanismus hinter Befehlen besteht das Ziel darin, Benutzer vor Änderungen zu schützen, falls eine andere Kernel-Schnittstelle standardisiert wird.

* Einige Overlays funktionieren zur Laufzeit besser als andere. Teile des Gerätebaums werden nur beim Booten verwendet – eine Änderung mit einem Overlay hat keine Auswirkung.

* Das Anwenden oder Entfernen einiger Overlays kann zu unerwartetem Verhalten führen, daher sollte dies mit Vorsicht erfolgen. Dies ist einer der Gründe, warum es `sudo` erfordert.

* Das Entladen des Overlays für eine ALSA-Karte kann zum Stillstand kommen, wenn etwas aktiv ALSA verwendet - das LXPanel-Lautstärkeregler-Plugin demonstriert diesen Effekt. Um das Entfernen von Overlays für Soundkarten zu ermöglichen, hat das Dienstprogramm `lxpanelctl` zwei neue Optionen erhalten - `alsastop` und `alsastart` - und diese werden zuvor von den Hilfsskripten `dtoverlay-pre` und `dtoverlay-post` aufgerufen und nachdem Overlays geladen bzw. entladen wurden.

* Das Entfernen einer Überlagerung führt nicht dazu, dass ein geladenes Modul entladen wird, aber es kann dazu führen, dass der Referenzzähler einiger Module auf Null sinkt. Wenn `rmmod -a` zweimal ausgeführt wird, werden nicht verwendete Module entladen.

* Overlays müssen in umgekehrter Reihenfolge entfernt werden. Mit den Befehlen können Sie einen früheren entfernen, aber alle dazwischenliegenden Befehle werden entfernt und erneut angewendet, was unbeabsichtigte Folgen haben kann.

* Es werden nur Gerätebaumknoten auf der obersten Ebene des Baums und Kinder eines Busknotens geprüft. Für zur Laufzeit hinzugefügte Knoten gibt es die weitere Einschränkung, dass sich der Bus für Benachrichtigungen über das Hinzufügen und Entfernen von Kindern registrieren muss. Es gibt jedoch Ausnahmen, die diese Regel brechen und Verwirrung stiften: Der Kernel durchsucht den gesamten Baum explizit nach einigen Gerätetypen - Uhren und Interrupt-Controller sind die beiden wichtigsten - um sie (bei Uhren) früh zu initialisieren und/oder (bei Interrupt-Controller) in einer bestimmten Reihenfolge. Dieser Suchmechanismus tritt nur während des Bootens auf und funktioniert daher nicht für Knoten, die zur Laufzeit durch ein Overlay hinzugefügt werden. Es wird daher für Overlays empfohlen, Knoten mit festem Takt in der Wurzel des Baums zu platzieren, es sei denn, es ist garantiert, dass das Overlay zur Laufzeit nicht verwendet wird.

<a name="part3.6"></a>
### 3.6: Unterstützte Overlays und Parameter

Da es zu zeitaufwändig ist, die einzelnen Overlays hier zu dokumentieren, lesen Sie bitte die Datei [README](https://github.com/raspberrypi/firmware/blob/master/boot/overlays/README) neben dem Overlay ` .dtbo`-Dateien in `/boot/overlays`. Es wird mit Ergänzungen und Änderungen auf dem neuesten Stand gehalten.

<a name="part4"></a>
##Teil 4: Fehlerbehebung und Profi-Tipps

<a name="part4.1"></a>
### 4.1: Debuggen

Der Loader überspringt fehlende Overlays und fehlerhafte Parameter, aber bei schwerwiegenden Fehlern wie einem fehlenden oder beschädigten Basis-DTB oder einer fehlgeschlagenen Overlay-Zusammenführung greift der Loader auf einen Nicht-DT-Boot zurück. In diesem Fall oder wenn sich Ihre Einstellungen nicht wie erwartet verhalten, sollten Sie nach Warnungen oder Fehlern des Loaders suchen:

```
sudo vcdbg log msg
```

Zusätzliches Debugging kann aktiviert werden, indem `dtdebug=1` zu `config.txt` hinzugefügt wird.

Sie können eine für Menschen lesbare Darstellung des aktuellen DT-Zustands wie folgt erstellen:

```
dtc -I fs /proc/device-tree
```

Dies kann nützlich sein, um die Auswirkungen des Zusammenführens von Überlagerungen auf den zugrunde liegenden Baum zu sehen.

Wenn Kernel-Module nicht wie erwartet geladen werden, überprüfen Sie, ob sie in `/etc/modprobe.d/raspi-blacklist.conf` nicht auf der schwarzen Liste stehen; Blacklisting sollte bei Verwendung des Gerätebaums nicht erforderlich sein. Wenn dies nichts Ungewöhnliches anzeigt, können Sie auch überprüfen, ob das Modul die richtigen Aliase exportiert, indem Sie in `/lib/modules/<version>/modules.alias` nach dem `kompatiblen` Wert suchen. Andernfalls fehlt Ihr Treiber wahrscheinlich entweder:

```
.of_match_table = xxx_of_match,
```

oder:

```
MODULE_DEVICE_TABLE(of, xxx_of_match);
```

Andernfalls ist `depmod` fehlgeschlagen oder die aktualisierten Module wurden nicht auf dem Zieldateisystem installiert.

<a name="part4.2"></a>
### 4.2: Testen von Overlays mit dtmerge, dtdiff und ovmerge

Neben den Befehlen `dtoverlay` und `dtparam` gibt es ein Dienstprogramm zum Anwenden eines Overlays auf eine DTB - `dtmerge`. Um es zu verwenden, müssen Sie zuerst Ihren Basis-DTB besorgen, den Sie auf zwei Arten erhalten können:

a) Generieren Sie es aus dem Live-DT-Zustand in `/proc/device-tree`:
```
dtc -I fs -O dtb -o base.dtb /proc/device-tree
```
Dies schließt alle Overlays und Parameter ein, die Sie bisher angewendet haben, entweder in `config.txt` oder indem Sie sie zur Laufzeit laden, was möglicherweise Ihren Wünschen entspricht oder auch nicht. Alternative...

b) kopieren Sie es von den Quell-DTBs in /boot. Dies beinhaltet keine Overlays und Parameter, aber auch keine anderen Modifikationen durch die Firmware. Um das Testen aller Overlays zu ermöglichen, erstellt das Dienstprogramm `dtmerge` einige der Board-spezifischen Aliase ("i2c_arm" usw.), aber das bedeutet, dass das Ergebnis einer Zusammenführung mehr Unterschiede zum ursprünglichen DTB enthält, als Sie möglicherweise erwarten. Die Lösung hierfür besteht darin, dtmerge zu verwenden, um die Kopie zu erstellen:

```
dtmerge /boot/bcm2710-rpi-3-b.dtb base.dtb -
```

(das `-` zeigt einen fehlenden Overlay-Namen an).

Sie können jetzt versuchen, ein Overlay oder einen Parameter anzuwenden:

```
dtmerge base.dtb merged.dtb - sd_overclock=62
dtdiff base.dtb merged.dtb
```

wird zurück kommen:

```
--- /dev/fd/63  2016-05-16 14:48:26.396024813 +0100
+++ /dev/fd/62  2016-05-16 14:48:26.396024813 +0100
@@ -594,7 +594,7 @@
                };

                sdhost@7e202000 {
-                       brcm,overclock-50 = <0x0>;
+                       brcm,overclock-50 = <0x3e>;
                        brcm,pio-limit = <0x1>;
                        bus-width = <0x4>;
                        clocks = <0x8>;
```

Sie können auch verschiedene Overlays oder Parameter vergleichen. 

```
dtmerge base.dtb merged1.dtb /boot/overlays/spi1-1cs.dtbo
dtmerge base.dtb merged2.dtb /boot/overlays/spi1-2cs.dtbo
dtdiff merged1.dtb merged2.dtb
```

to get:

```
--- /dev/fd/63  2016-05-16 14:18:56.189634286 +0100
+++ /dev/fd/62  2016-05-16 14:18:56.189634286 +0100
@@ -453,7 +453,7 @@

                        spi1_cs_pins {
                                brcm,function = <0x1>;
-                               brcm,pins = <0x12>;
+                               brcm,pins = <0x12 0x11>;
                                phandle = <0x3e>;
                        };

@@ -725,7 +725,7 @@
                        #size-cells = <0x0>;
                        clocks = <0x13 0x1>;
                        compatible = "brcm,bcm2835-aux-spi";
-                       cs-gpios = <0xc 0x12 0x1>;
+                       cs-gpios = <0xc 0x12 0x1 0xc 0x11 0x1>;
                        interrupts = <0x1 0x1d>;
                        linux,phandle = <0x30>;
                        phandle = <0x30>;
@@ -743,6 +743,16 @@
                                spi-max-frequency = <0x7a120>;
                                status = "okay";
                        };
+
+                       spidev@1 {
+                               #address-cells = <0x1>;
+                               #size-cells = <0x0>;
+                               compatible = "spidev";
+                               phandle = <0x41>;
+                               reg = <0x1>;
+                               spi-max-frequency = <0x7a120>;
+                               status = "okay";
+                       };
                };

                spi@7e2150C0 {
```

Das Repository [Utils](https://github.com/raspberrypi/utils) enthält ein weiteres DT-Dienstprogramm - `ovmerge`. Im Gegensatz zu `dtmerge` kombiniert `ovmerge` Dateien und wendet Overlays in Quellform an. Da die Überlagerung nie kompiliert wird, bleiben Beschriftungen erhalten und das Ergebnis ist normalerweise besser lesbar. Es hat auch eine Reihe anderer Tricks, wie die Möglichkeit, die Reihenfolge der Dateieinbindung aufzulisten.

<a name="part4.3"></a>
### 4.3: Erzwingen eines bestimmten Gerätebaums

Wenn Sie sehr spezielle Anforderungen haben, die von den Standard-DTBs nicht unterstützt werden, oder wenn Sie nur mit dem Schreiben Ihrer eigenen DTs experimentieren möchten, können Sie den Loader anweisen, eine alternative DTB-Datei wie folgt zu laden:

```
device_tree=my-pi.dtb
```

<a name="part4.4"></a>
### 4.4: Verwendung des Gerätebaums deaktivieren

Seit der Umstellung auf den 4.4-Kernel und der Verwendung von mehr Upstream-Treibern ist die Verwendung des Gerätebaums in Pi-Linux-Kerneln erforderlich. Für Bare-Metal- und andere Betriebssysteme besteht die Methode zum Deaktivieren der DT-Nutzung jedoch darin, Folgendes hinzuzufügen:

```
device_tree=
```

in `config.txt`.

<a name="part4.5"></a>
### 4.5: Shortcuts und Syntaxvarianten

Der Loader versteht einige Abkürzungen:

```
dtparam=i2c_arm=on
dtparam=i2s=ein
```

kann gekürzt werden zu:

```
dtparam=i2c,i2s
```

(`i2c` ​​ist ein Alias ​​von `i2c_arm`, und `=on` wird vorausgesetzt). Es akzeptiert auch weiterhin die Langversionen: `device_tree_overlay` und `device_tree_param`.

<a name="part4.6"></a>
### 4.6: Andere DT-Befehle in config.txt verfügbar

`device_tree_address`
Dies wird verwendet, um die Adresse zu überschreiben, an der die Firmware den Gerätebaum lädt (nicht dt-blob). Standardmäßig wählt die Firmware einen geeigneten Ort aus.

`device_tree_end`
Dies setzt eine (exklusive) Grenze für den geladenen Gerätebaum. Standardmäßig kann der Gerätebaum bis zum Ende des nutzbaren Speichers wachsen, was mit ziemlicher Sicherheit erforderlich ist.

`dtdebug`
Wenn nicht Null, aktivieren Sie zusätzliche Protokollierung für die Verarbeitung des Gerätebaums der Firmware.

`enable_uart`
Aktivieren Sie den primären/Konsolen-UART (ttyS0 auf einem Pi 3, sonst ttyAMA0 - sofern nicht mit einem Overlay wie miniuart-bt getauscht). Wenn der primäre UART ttyAMA0 ist, ist enable_uart standardmäßig auf 1 (aktiviert), andernfalls auf 0 (deaktiviert). Dies liegt daran, dass die Kernfrequenz daran gehindert werden muss, sich zu ändern, was ttyS0 unbrauchbar machen würde, also impliziert `enable_uart=1` core_freq=250 (außer force_turbo=1). In einigen Fällen ist dies ein Leistungseinbruch, daher ist es standardmäßig deaktiviert. Weitere Details zu UARTs finden Sie [hier](uart.md)

`overlay_prefix`
Gibt ein Unterverzeichnis/ein Präfix an, aus dem Overlays geladen werden sollen - standardmäßig "overlays/". Beachten Sie das abschließende "/". Wenn Sie möchten, können Sie nach dem letzten "/" etwas hinzufügen, um jeder Datei ein Präfix hinzuzufügen, obwohl dies wahrscheinlich nicht erforderlich ist.

Weitere Ports können vom DT angesteuert werden, für weitere Details siehe [Abschnitt 3](#part3).

<a name="part4.7"></a>
### 4.7 Weitere Hilfe

Wenn Sie dieses Dokument gelesen und keine Antwort auf ein Problem mit dem Gerätebaum gefunden haben, steht Ihnen Hilfe zur Verfügung. Der Autor ist normalerweise in Raspberry Pi-Foren zu finden, insbesondere im Forum [Device Tree](https://www.raspberrypi.org/forums/viewforum.php?f=107).
