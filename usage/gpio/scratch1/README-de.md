# GPIO in Scratch 1.4

## Verwendung und grundlegende Funktionen

### Starten des Servers

Bevor Sie die GPIO-Pins verwenden können, müssen Sie den GPIO-Server starten. Dazu gibt es mehrere Möglichkeiten:

- Wählen Sie **GPIO-Server starten** aus dem Menü **Bearbeiten**, um ihn einzuschalten. Wenn der Server bereits läuft, wird er durch **GPIO-Server stoppen** ausgeschaltet.
- Eine Scratch-Übertragung von `gpioserveron` oder `gpioserveroff` hat die gleichen Auswirkungen.
- Projekte, die gespeichert werden, während der GPIO-Server läuft, haben diesen Status aufgezeichnet und versuchen beim Laden, den Server zu starten, wenn er aktiviert ist.
- Sie können auch eine Option in der Scratch-INI-Datei festlegen. Siehe Anhang unten.

### Grundlegende GPIO-Nutzung

Ohne weitere Einrichtung haben Sie nun Zugriff auf die Grundlagen des GPIO-Systems. Dies verwendet derzeit die Broadcast-Blöcke, um Befehle an den GPIO-Server zu senden, genau wie die ursprünglichen Mesh-Netzwerk-basierten Broadcast-Nachrichten.

Um beispielsweise GPIO-Pin 4 als Ausgang zu konfigurieren und einzuschalten, erstellen Sie die beiden folgenden Broadcasts:

![broadcast config4 out gpio4on](images/config-on.png)

Wie immer können Sie diesen Text mit normalen Join-, Pick- oder List-Handling-Blöcken zusammenstellen. Zum Beispiel, wenn "foo" = 17, dann

![broadcast join gpio join foo 17](images/broadcastgpio17on.png) 

würde „gpio17on“ senden und damit die GPIO-Pin-Nummer 17 (unter der BCM-Nummerierung – nicht die physischen oder wiringPi-Nummern!) auf „on“ setzen.

Die Pins müssen konfiguriert werden, bevor Sie sie verwenden können, um irgendetwas zu tun. Wir können die Richtung des Pins (in oder out) und für Eingangspins den Pull-up-Modus (up, down, none) festlegen.

- Für Eingangspins können wir 'in' oder 'input' verwenden. Beide werden gleich behandelt wie „inpullup“ oder „inputpullup“ und setzen standardmäßig den Pull-up-Widerstand, um das Signal hochzuziehen.
    + Um den Pull-up-Widerstand so einzustellen, dass er das Signal nach unten zieht, verwenden wir 'inpulldown' oder 'inputpulldown'
    + Wir können den Pull-Up-Widerstand mit 'inpullnone' oder 'inputpullnone' auf Float setzen
- Ausgangspins werden einfach durch 'out' oder 'output' konfiguriert
- Ausgabe mit PWM, was nützlich ist, um LEDs teilweise hell leuchten zu lassen oder Motoren mit variabler Geschwindigkeit laufen zu lassen usw., wird mit 'outputpwm' konfiguriert

Beispielsweise:
![broadcast config 11 inpulldown](images/broadcastconfig11inpulldown.png)

Als Eingänge festgelegte Pins werden mit dem Variablensystem des Scratch-Sensors verbunden und erscheinen daher in der Liste der möglichen Werte in den Sensorblöcken:

![Sensorblock gpio11](images/sensorgpio11.png)

und kann auf die gleiche Weise verwendet werden:

![if gpio11 Sensorwert](images/ifgpio11sensorvalue.png)

Sie finden Ihren Eingabe-Pin erst in der Liste, nachdem Sie Ihre Konfigurationsübertragung ausgeführt haben. Bis dahin kann der GPIO-Server nicht wissen, dass es sich um eine Eingabe handeln soll. Wenn Sie Ihr Projekt speichern, bleibt der Eingang weiterhin verbunden.

Mit diesen sehr einfachen Befehlen können Sie ziemlich komplexe GPIO-Handhabungsskripte erstellen, um Tasten zu lesen und LEDs, Motoren usw. zu bedienen. Wir haben auch Befehle, um die Zeit zurückzugeben, die IP-Adresse der Maschine zurückzugeben, verschiedene Temperaturen zu lesen, einen Ultraschall-Abstandssensor zu lesen, einen Wetterbericht abzurufen und sogar ein Foto mit einem angeschlossenen Raspberry Pi-Kameramodul aufzunehmen und es als aktuelles Kostüm festzulegen.

Dieses Skript (im Ordner „Sensors and Motors“ als **Sensors and Motors/gpio-demo** bereitgestellt) veranschaulicht die meisten der oben genannten Punkte:

![gpio-demo script](images/gpio-demo.gif)

Zusammen mit einem entsprechend konfigurierten Steckbrett bietet es die Möglichkeit, LEDs per Knopfdruck ein- und auszuschalten, ein Foto mit einem Countdown aufzunehmen, der von einer progressiv heller werdenden LED bereitgestellt wird, Möglichkeiten, die Zeit zu überprüfen, und so weiter.

![gpio-demo-breadboard](images/gpio-demo-breadboard.png)

Beachten Sie, dass wir eine einzelne Sendung haben können, die mehrere Nachrichten enthält, wie z. B. „gpio24on gpio18pwm400“ im obigen Skript.

## Grundlegende GPIO-Befehle

In den folgenden Befehlslisten verwenden wir
`[comm] + pin number + [ on | off]` 
um einen Befehl der Form "comm17off" oder "comm7on" anzuzeigen.
Für eine Variable
`led + light number (1..5) =  ( 0 .. 100)`  
gibt an, dass eine Variable namens „led5“ einen Wert von 0 bis 100 haben kann.
`foo = ( 1 | 4 | 7 )`  
zeigt an, dass die Variable „foo“ auf 1, 4 oder 7 gesetzt werden kann.

### Einfache GPIO-Steuerung

Die grundlegende GPIO-Befehlsliste der Dinge, die Sie tun können, ohne dass HATs an Ihren Pi angeschlossen sind, lautet wie folgt:

- `config + pin number +`
    + in`, `input`, `inpullup` oder `inputpullup` um als Eingang mit Pullup zu setzen
    +  `inpulldown` oder `inputpulldown`
    + `inpullnone` oder `inputpullnone`
    + `out` oder `output` um als digitalen Ausgang zu setzen
    + `outputpwm` zum Einstellen als PWM-Pin

  Zum Beispiel „config12in“, um Pin 12 als Eingang mit dem Standard-Pullup festzulegen und eine Sensorvariable „gpio12“ hinzuzufügen.

- `gpio + pin number + [ on | high | off | low ]`, um einen Ausgang ein- oder auszuschalten
Beispiel: „gpio17on“, um Pin 17 einzuschalten.

- `gpio + pin number + pwm + [ 0..1024 ]`, um den PWM-Ausgang zu verwenden

  Zum Beispiel „gpio22pwm522“, um das PWM-Tastverhältnis auf 522 von 1024 oder etwa die Hälfte der Leistung einzustellen. Beachten Sie, dass viele LEDs ihre Helligkeit nicht einfach linear zu ändern scheinen, sodass 522 möglicherweise kaum leuchtet oder fast die volle Helligkeit hat.

### Servoantrieb

- `servo + pin number + [percent | %] + [-100...100]` um ein angeschlossenes Servo in Position zu bringen.

  Zum Beispiel "servo15%0", um ein Servo in der Mitte seines Bereichs zu positionieren.
- `servo stop`, um den Servotreiber auszuschalten.

Im Skript **Servos und Motoren/gpio-servoDemo** können Sie sehen, wie ein Servo bewegt oder mit einer Variablen wie der Position eines Sprites verbunden wird. Sie müssen Ihr Servo wie folgt verdrahten:

![gpio servo wiring layout](images/gpio-servoDemo.png)

### Ultraschallsensor

- `ultrasonic + trigger + trigger pin + echo  + echo pin` zum Anschluss eines typischen SR04-Ultraschallsensors
- `ultrasonic stop`, um die Sensorunterstützung am Ende Ihres Skripts auszuschalten

Hier ist ein Beispiel für die Verdrahtung mit Pin 16 als Trigger und 26 als Echo:

![gpio ultrasonic wiring layout](images/gpio-ultrasonic.png)

Wenn Sie diese Verkabelung mit dem Skript in **Sensors and Motors/gpio-ultrasonicDemo** verwenden, werden Sie sehen, wie Sie die Entfernung ablesen und ein Sprite entsprechend bewegen. Die andere Ultraschall-Demo in **Sensor and Motors/gpio-ultrasonicIntruderAlarm** erfordert ein Kameramodul und macht einen Schnappschuss, wenn jemand zu nahe kommt.

### Wetterberichte

- `getweather + city name + , + country two-letter code + , + your user key from [OpenWeatherMaps](http://www.openweathermaps.org)` erstellt Sensorvariablen für die Temperatur, Windgeschwindigkeit und -richtung der benannten Stadt , Niederschlag und Bewölkung. Sie müssen sich anmelden, um einen Schlüssel von ihnen zu erhalten (kostenlose Konten sind verfügbar). Einzelheiten finden Sie unter [OpenWeatherMaps](http://www.openweathermaps.org).

Beispielsweise

`getweather Rio de Janeiro, BR, 1234EF65B42DEAD`  

würde die Sensorvariablen machen

`Regen in Rio de Janeiro`  
`Zeit in Rio de Janeiro` 

...und so weiter. Die Kommas zwischen dem Namen der Stadt und dem Ländercode und Ihrem Schlüssel sind wichtig, damit der GPIO-Server weiß, wo er Dinge aufteilen soll. Einige Städte haben einfache Namen wie „Ee“ oder „Manchester“, während andere etwas komplizierter sind, wie „Sault Ste Marie“ oder „Llanfairpwllgwyngyllgogerychwyrndrobwllllantysiliogogogoch“. Beachten Sie, dass der OpenWeatherMaps-Server nicht jede Stadt in jedem Land kennt und auch nicht jede Art von Wetterdaten für alle kennt, sodass Sie manchmal keine nützlichen Informationen erhalten.

Das Skript **Sensors and Motors/gpio-citytemperaturegraph** zeigt, wie man die Wetterdaten für London erhält und die Temperatur grafisch darstellt. Da sich Wetterdaten normalerweise nicht schnell ändern, rufen wir die Daten nur alle 15 Minuten ab, um die Website nicht zu überlasten.

### Temperaturen ablesen

- `gettemp` verbindet sich mit ein paar möglichen Temperatursensoren.
    + `gettemp + cpu` liest die CPU-Temperatur und erstellt eine Sensorvariable `cputtemp`. Zum Beispiel: „gettempcpu“.
    Ein Beispielprojekt, das ein Diagramm der CPU-Temperatur zeichnet, finden Sie im Projekt **Sensors and Motors/gpio-cputtemperaturegraph**.
    + „gettemp“ selbst versucht, einen angeschlossenen 1-Draht-Wärmesensor DS18B20 zu finden, und erstellt eine Sensorvariable mit dem Namen „temp + [die zwölfstellige Sensor-ID]“.
    + `gettemp + [a previously discovered twelve-digit 1-wire id]` stellt nach Möglichkeit eine direkte Verbindung zu diesem identifizierten DS18B20-Sensor her.

      Beachten Sie, dass das Lesen von 1-Wire-Sensoren etwa eine halbe Sekunde dauert. Daher kann das häufige Lesen des Sensors dazu führen, dass Scratch sehr langsam erscheint.

### Fotos

- `photo` verwendet die Kamera, um ein Foto aufzunehmen und es als aktuelles Kostüm des Sprites (oder der Bühne, falls ausgewählt) einzufügen.
- `photo + [big/large]`: Ein „großes“ Foto hat die gleiche Größe wie die Bühne. Zum Beispiel: „photobig“ oder „photo large“.
- `photo + [width @ height]` nimmt eine Fotogröße Breite mal Höhe Pixel auf, bis zu den Grenzen der Kamera. Sie können fast jede vernünftige Zahl für die Breite und Höhe ausprobieren, aber denken Sie daran, dass sehr kleine Zahlen (unter 32 oder so) nicht unbedingt ein richtiges Bild erzeugen und sehr große Zahlen ein Bild so groß machen können, dass es Scratch zum Absturz bringt. Beispielsweise ist „photo800@600“ normalerweise akzeptabel, aber „photo2000@1600“ kann Probleme verursachen.

### Sonstig

- `gettime` fügt einige Zeitwerte zu den Sensorvariablen hinzu, insbesondere den „hours“-Wert, den „minutes“-Wert und das vollständige Datum und die vollständige Uhrzeit als „YYMMDDhhmmss“.
- `getip` fügt eine Sensorvariable für die IP-Nummer der lokalen Hostadresse der Maschine hinzu.

## Zusatzhardware

Wir können auch Pi-Zusatzkarten wie PiGlow, Pibrella, Explorer HAT usw. steuern. Um eine Karte einzurichten, müssen wir zuerst dem GPIO-Server mitteilen, um welche Karte es sich handelt; Dies geschieht durch Erstellen und Setzen einer Variablen `AddOn`, wie folgt:

![Addon auf Piglow setzen](images/setaddonpiglow.png)

Jedes Board hat seinen eigenen Befehlssatz, der auf den oben beschriebenen grundlegenden GPIO-Funktionen aufgesetzt ist.
Viele Boards können auch Scratch-Variablen-Broadcasts verwenden, wobei eine passend benannte Variable erstellt und ihr Wert gesendet wird, wenn sie sich ändert.
Für ein PiGlow-Board ist es beispielsweise sinnvoll, Variablen für jede LED oder jeden LED-Ring zu benennen und jeden Wert als Möglichkeit zur Steuerung der Helligkeit festzulegen. Es ist möglich, Verwirrung zu stiften, indem Sie beide Formen der Steuerung gleichzeitig verwenden. Das Senden von `myCommand400` im selben Skript wie das Setzen von `myValue` auf 200 kann im Extremfall zu Flackern, scheinbarer Nichtfunktion oder sogar Hardwarefehlern führen.
Sie müssen lediglich eine Variable mit dem entsprechenden Namen erstellen und ihren Wert mit den normalen Skriptblöcken festlegen.

Einige Boards bieten Eingänge, auf die über die Sensorvariablen zugegriffen werden kann, wie oben in der beispielhaften Verwendung von Pin 11 gezeigt.

### PiGlow

Das PiGlow-Board verfügt über mehrere Ringe aus bunten LEDs, die als Ringe, Beine, einzeln oder alle zusammen gesteuert werden können. Seien Sie vorsichtig: Es kann etwas grell aussehen, daher ist ein Diffusor aus Pauspapier oder getöntem Plexiglas eine gute Idee. Um das Board zu verwenden, stellen Sie `AddOn` auf `PiGlow` ein.

PiGlow hat eine ganze Reihe von Befehlen, und viele davon werden im **Sensors and Motors/gpio-PiGlow**-Projekt demonstriert.

### Unterstützte Befehle

- `leg + leg number [ 1 | 2 | 3 ] + [ on | high | off | low ]` z.B. `leg2off`
- `arm` - als Bein
- `all +  [ on | high | off | low ]`
- `[ led | light ] + led number +  [ on | high | off | low ]` z.B. `light12high`
- `bright + [ 0 .. 255 ]` (setzt den Helligkeitsmultiplikator für jeden nachfolgenden LED-An-Befehl)
- `[ red | orange | yellow | green | blue | white ] +  [ on | high | off | low ]` z.B. `redlow`

### Variablen

- `bright = ( 0 .. 255)`
- `[ leg | arm ] + [ 1 | 2 | 3 ] = (0 .. 255)`
- `[ led | light ] + led number (1..18) = (0 .. 255)`
- `[ red | orange | yellow | green | blue | white ] = ( 0 .. 255)`
- `ledpattern` = (eine 18-stellige Zeichenfolge, die als Binärzahl behandelt wird, z. B. „011111101010101010“, wobei alles, was nicht 0 ist, als 1 betrachtet wird)

### PiFace

Das PiFace Digital Board bietet acht digitale Eingänge und acht digitale Ausgänge, wobei die ersten vier Eingänge über Parallelschalter und die ersten beiden Ausgänge über 20V/5A-Relais verfügen. Setzen Sie `AddOn` auf `PiFace`, um dieses Board zu aktivieren.

### Unterstützte Befehle

- `all + [ on | off]`
- `output + output number + [ on | high | off | low ]` z.B. `output6on`

### Variablen

- `output + [ 0 .. 7 ] = (0 |1 )` - der Wert wird gerundet und einer Max/Min-Begrenzung unterzogen, also rundet -1 auf 0 und 400000000 auf 1 ab.

Es gibt auch acht Eingangssensorvariablen mit den Namen „Input1“ bis „Input8“, die mögliche Werte (0/1) haben. Das **Sensors and Motors/gpio-PiFace**-Projekt zeigt, wie es funktioniert.

### Pibrella

Dies bietet einen schönen großen roten Knopf, drei große LEDs, vier digitale Eingänge, vier digitale Ausgänge und einen lauten Summer. Um dieses Board zu verwenden, setzen Sie `AddOn` auf `Pibrella`.

### Unterstützte Befehle

- `[ red | yellow | green ] + [ on | high | off | low ]` e.g. `yellowhigh`
- `Buzzer + (0 .. 4000)` e.g. `buzzer2100`
- `Output + [ E | F | G | H ] + [ on | high | off | low ]`

### Variablen

- `Buzzer = (0..10000)`
- `[ red | green | yellow ]  = (0 |1 )`
- `Output + [ E | F | G | H ]  = (0 |1 )`

Als Sensorvariablen stehen die Eingänge A, B, C, D und der große rote Knopf mit möglichen Werten (0/1) zur Verfügung. Es gibt eine Demo in **Motors and Sensors/gpio-pibrella**.

### Explorer HAT Pro

Dieses Board ist etwas schwieriger zu fahren, da es Teile hat, die GPIO-verbunden sind, und Teile, die I2C-verbunden sind:

- Vier LEDs
- Vier 5-V-Ausgangsanschlüsse
- Vier gepufferte Eingangsanschlüsse
- Zwei H-Brücken-Motortreiber
- Vier analoge Eingänge
- Vier kapazitive Eingangspads

Um dieses Board zu verwenden, setzen Sie `AddOn` auf `ExplorerHAT`.

### Unterstützte Befehle

- `led + led number ( 1 .. 3) +  [ on | high | off | low ]`
- `output + input number ( 1 .. 3) +  [ on | high | off | low ]`
- `motor + motor number (1|2) + speed + (0..100)` - Motordrehzahl wird in Prozent eingestellt z.B. `Motor1 Geschwindigkeit 42`

Sie haben übereinstimmende Variablenformen:

- `led + led number  = (0 |1 )`
- `output + led number  = (0 |1 )`
- `motor + motor number (0|1) = (0..100)`

### Variablen

Dazu kommen die Sensorvariablen „Input1“ bis „Input4“ mit Werten (0|1) und die vier ADC-Pins (1 .. 4) mit Werten +-6,1V. Wenn das Signal von einem Potentiometer abgeleitet wird, das an 5V/GND des Explorer HAT angeschlossen ist, dann ist der Bereich (0 .. ~5).

Das Demoskript in **Sensoren und Motoren/gpio-ExplorerHAT** erfordert, dass Sie einen Motor, eine LED, ein Drehpotentiometer usw. verdrahten, wie in [diesem Diagramm](images/gpio-ExplorerHAT.png) gezeigt.

Beachten Sie, dass die kapazitiven Eingangspads noch nicht betriebsbereit sind und Unterstützung auf Bibliotheksebene erfordern.

### Sense HAT (wie im Astro Pi verwendet)

Dieses von der Foundation gebaute Board bietet eine Reihe ungewöhnlicher Sensoren und eine große 8 x 8-Anordnung von RGB-LEDs.

Die Sensoren messen:

- Temperatur
- Feuchtigkeit
- Druck
- Beschleunigungsmesser/Kreisel
- Magnetometer/Kompass
- Mini-Joystick-Aktionen links/rechts/oben/unten/zurück

Um dieses Board zu verwenden, setzen Sie `AddOn` auf `SenseHAT`.

### Unterstützte Befehle

- `clearleds`: alle LEDs auf Hintergrundfarbe setzen
- `ledbackground + color` oder `ledforeground + color`: Legen Sie die Hintergrund- und Vordergrundfarben für die zu verwendenden String- und Graph-Befehle fest. Die Farbe wird mit einem der folgenden angegeben:
    + ein Name aus der Liste rot cyan blau grau schwarz weiß grün braun orange gelb magenta hellrot paletan hellrot hellblau palebuff dunkelgrau hellblau... z.B. `ledforegroundcyan`
    + eine sechsstellige Hexadezimalzahl im HTML-Stil, die mit einem Rautezeichen beginnt, z. B. `#34F2A0`
    + oder ein RGB-Triplett aus Zahlen zwischen 0 und 255, z. B. `42, 234, 17`.
- `ledscrollspeed + [Anzahl Millisekunden Verzögerung pro Scrollschritt]`: eine Zeichenkette
- `ledscrollstring + [string]`: Scrollen Sie den folgenden String mit den zuvor eingestellten Vorder- und Hintergrundfarben, z. `ledscrollstring HalloWelt`
- `ledshowchar + [character]`: zeigt nur ein einzelnes Zeichen mit den zuvor eingestellten Vorder- und Hintergrundfarben
- `ledbargraph + [8 digits 0..8]`: Erstellen Sie ein einfaches Balkendiagramm mit bis zu acht Ziffern mit den zuvor eingestellten Vorder- und Hintergrundfarben, z. `ledbargraph20614590`
- `ledshowsprite + [Name des Sprites]`: Zeigt das benannte Sprite auf den LEDs an, z. `ledshowsprite Sprite1`. Das Sprite ist über dem 8 x 8-Array zentriert, sodass Sie möglicherweise nur sehr wenig von einem großen Sprite sehen.
- `ledpixel + [ x | at] + [0..7] + [y | @] + [0..7] + [Farbe | Farbe] + [Name der Farbe oder Code als LED-Hintergrund]`. Zum Beispiel: „ledpixelx4y3colourwhite“ oder „ledpixelat2@7color42,231,97“ oder „ledpixelx3@1colour#4A76A0“.

### Variablen

- `gyroX`
- `gyroY`
- `gyroZ`
- `accelX`
- `accelY`
- `accelZ`
- `compassX`
- `compassY`
- `compassZ`
- `temp`
- `pressure`
- `humidity`

### Pi-LITE

Das Pi-LITE-Board bietet eine einfache Anordnung weißer LEDs, die einzeln adressiert oder als Lauftextanzeige, Balkendiagramm oder VU-Meter behandelt werden können. Es funktioniert über den seriellen GPIO-Port und stellt einige interessante Herausforderungen dar, insbesondere das Einrichten der seriellen Verbindung, wie in [RaspberryPi-Spy's Pi-LITE-Anleitung] (http://www.raspberrypi-spy.co.uk/2013/09/) beschrieben. wie-man-das-pi-lite-led-matrix-board einrichtet/).

Um dieses Board zu verwenden, setzen Sie `AddOn` auf `PiLite`.

### Unterstützte Befehle

- `allon`
- `alloff`
- `scrollstringABCDEF` um ABCDEF anzuzeigen.
- `bargraph[1..14],[1-100]` stellt den Balken ein, der auf einer der 14 LED-Spalten angezeigt wird, um den Prozentsatz darzustellen.
- `vumeter[1|2],[1…100]` zeigt ein zweispaltiges Balkendiagramm im Stil der Boombox-Grafik-Equalizer der 1980er Jahre.

### RyanTeck, Pololu und CamJam EduKit 3 Motorsteuerung

Diese Platinen können zwei Gleichstrommotoren antreiben.

Um sie zu verwenden, setzen Sie `AddOn` auf:

- `RyanTek001` für das RyanTeck-Board
- `Pololu8835` für das Pololu-Board
- `EdukitMotorBoard` für das CamJam-Board

### Unterstützte Befehle

Obwohl sie ganz unterschiedlich funktionieren, haben sie die gleichen Befehle:

- `motor + motor number (1|2) + speed + value (-100..100)`

### Variablen

- `motor + motor number (0|1) = (-100..100)`

## Anhang: Aktivieren und Deaktivieren des GPIO-Servers

Bei normaler Verwendung sollten Sie den GPIO-Server nicht aktivieren müssen, da er standardmäßig aktiviert, aber gestoppt ist. Wir können dies ändern, indem wir der Init-Datei eine Zeile hinzufügen. Im Home-Verzeichnis können wir eine Datei namens `.scratch.ini` haben - der Anfangspunkt ist wichtig, um daraus eine versteckte Unix-Datei zu machen. Fügen Sie der Datei einfach die Zeile `gpioserver=X` hinzu, wobei X für Folgendes steht:

   - `0` - zum Deaktivieren des GPIO-Servers, wodurch Benutzer oder geladene Projekte daran gehindert werden, ihn zu verwenden
   - `1` - um den GPIO-Server zu aktivieren, aber ausgeschaltet zu lassen, was die Standardeinstellung ist, wenn es keine `.scratch.ini`-Datei gibt
   - `2` - um den Server sowohl zu aktivieren als auch zu starten, was vielleicht in einem Klassenzimmer nützlich sein könnte, wenn es in der Lektion um die Verwendung von GPIO geht

Beachten Sie, dass das ältere Mesh-/Netzwerkserver-Setup derzeit unter dem **Share menu** halb versteckt ist: Sie müssen die Umschalttaste gedrückt halten, während Sie dieses Menü öffnen. Es funktioniert genau wie zuvor und stellt weiterhin eine Verbindung zu externen Socket-basierten Servern her.