% Mixed Reality mit Hololens
% Pascal Schulthess
  Lukas Tanner
  Dozent: Tim Weingärtner
% 30. Nov 2016

\pagebreak

# Selbständigkeitserklärung

Hiermit erkläre ich, dass ich die vorliegende Arbeit selbständig angefertigt und keine anderen als
die angegebenen Hilfsmittel verwendet habe. Sämtliche verwendeten Textausschnitte, Zitate oder
Inhalte anderer Verfasser wurden ausdrücklich als solche gekennzeichnet.


Pascal Schulthess

---

Lukas Tanner

---

\pagebreak


# Abstract (deutsch)
Hololens ist eine mixed reality Brille von Microsoft, die es erlaubt, den Raum mit
holografischen Elementen zu erweitern. Ebenso kann man damit programmatisch mit der Umgebung
interagieren.

Dieses Dokument beschreibt erste Erfahrungen mit dieser neuen Technologie und die Entwicklung
eines Frameworks, das es erlaubt, Informationen zu einem realen Objekt im Raum darstellen zu lassen.


# Abstract (english)

Hololens are mixed reality glasses by Microsoft. They can extend the environment with holographic
elements and interact programmatically with the surroundings.

This document describes first steps with this new technology and the developement of a framework
with which you can display additional information to a real life object in the room.


# Inhalt

- [Selbständigkeitserklärung](#section-id-9)
- [Inhalt](#section-id-26)
- [Abstract (deutsch)](#section-id-31)
- [Abstract (english)](#section-id-40)
- [Abkürzungen und Definitionen](#section-id-49)
- [Ziele und erwartete Resultate](#section-id-77)
    - [Einführung](#section-id-79)
    - [Mögliche Anwendungsfälle](#section-id-96)
    - [Mögliche Frameworks](#section-id-116)
        - [Virtueller Desktop](#section-id-121)
        - [Geräteinformationen darstellen](#section-id-131)
    - [Anforderungen](#section-id-145)
      - [Framework (muss)](#section-id-151)
      - [Framework (kann)](#section-id-170)
- [Lösungsentwicklung](#section-id-180)
    - [Erste Schritte](#section-id-184)
        - [Demoapplikationen](#section-id-217)
        - [Tutorials](#section-id-235)
    - [Entwicklung Gerätestatus](#section-id-258)
        - [Workflow](#section-id-273)
        - [Datenquelle](#section-id-284)
- [Schlussfolgerungen und Ausblick](#section-id-292)
- [Lessons learned](#section-id-303)


# Abkürzungen und Definitionen

----------------------------------------------------------------------------------------------------
Begriff             Bedeutung
-----------------   ---------------------------------------------------------------
Virtual Reality     Künstliche Welt, in der sich eine Person wie in der realen Welt bewegen kann.
                    Die reale Welt wird komplett mit einer künstlichen ersetzt.

Augmented Reality   Die Realität wird durch zusätzliche virtuelle Elemente angereichert und erhält
                    so einen Mehrwert. Im Gegensatz zur virtual reality nimmt man hier die reale
                    Welt immer noch wahr.

Mixed Reality       Wird äquivalent zu "Augemented Reality" verwendet.

Tap                 Gestik bei welcher der Zeigefinger auf den Daumen tippt. Damit wird eine Aktion
                    ausgelöst (entspricht einem Mausklick).

Press               Gestik bei welcher ein Tap durchgeführt wird, die Finger jedoch geschlossen
                    bleiben. Damit kann etwas "festgehalten" werden und mit einer Handbewegung ein
                    Objekt manipuliert werden (Verschieben, Navigieren, Scrollen etc.).

Bloom               Gestik bei welcher die fünf Finger einer Hand sich berühren und nach oben
                    zeigen um danach die Finger zu spreizen (wie eine Blüte öffnen). Wird als
                    Menü/Windwos Taste genutzt.

Gaze                der Blick; wo der Benutzer hinschaut und was die Hololens dort identifiziert.
                    Das kann sowohl ein reales Objekt sein wie auch ein Hologramm.

Unity               Unity ist eine Game Engine für 21 verschiedene Plattformen.

Game Engine         Eine Game Engine ist ein Framework für die Erstellung von Videospielen. Es
                    beinhaltet typischerweise Funktionalität für das Rendern von Grafiken,
                    Simulationen für physikalisches Verhalten, Geräusche und Musik, Animationen,
                    künstliche Intelligenz u.v.m.

----------------------------------------------------------------------------------------------------
Table: Definitionen

----------------------------------------------------------------------------------------------------
**Abkürzung**       **Bedeutung**
-----------------   ---------------------------------------------------------------
API                 Application Program Interface, Schnittstelle von Computerprogrammen für
                    Computerprogramme

HL                  Hololens

ReST                Representational State Transfer, Konzept für APIs über http(s)

----------------------------------------------------------------------------------------------------
Table: Abkürzungen

# Ziele und erwartete Resultate

## Einführung

In den letzten Monaten wurden von vielen Herstellern Virtual Reality Brillen auf den Markt
gebracht, meist im Gaming Bereich. Microsoft hat mit ihrer Mixed Reality Brille ein anderes Zielpublikum
und andere Anwendungsbereiche im Fokus. Mit der Hololens wird die Umwelt, in welcher sich der Träger
befindet, mit zusätzlichen virtuellen Elementen ergänzt.

Dank einer 3D-Kamera kann die Brille die Umgebung wahrnehmen und Objekte darin erkennen. Somit
können die virtuellen Elemente auf echte Objekte gesetzt werden und damit interagieren.

Ziele dieses Projektes sind

* Die Funktionsweise und Möglichkeiten der Hololens erkunden
* Mögliche Anwendungsfälle identifizieren und evaluieren
* Framework entwickeln, auf denen zukünftige Applikationen aufbauen können
* Prototyp des Frameworks in Code umsetzen

## Mögliche Anwendungsfälle

Die Hololens bietet sicherlich Einsatzmöglichkeiten im Spielebereich, wie Microsoft mit Games wie
[RoboRaid](https://www.microsoft.com/microsoft-hololens/en-us/apps/roboraid) oder
[Fragments](https://www.microsoft.com/microsoft-hololens/en-us/apps/fragments) bereits gezeigt haben.

Aber sie wurde für Geschäftslösungen entwickelt und dort will sie Microsoft auch positionieren.
Dazu gibt es bereits eine
[Sketchup Adaption](https://www.microsoft.com/en-us/store/p/sketchup-viewer/9nblggh4338q#),
mit welcher mehrere Personen am selben 3D-Modell arbeiten können

Wir haben folgende Ideen entwickelt, bei welchen wir auch die Möglichkeit sahen, etwas davon
umzusetzen.

* Brettspiel Simulation
* Mit einem Smartphone koppeln
    * Auf Geodaten (und andere Sensoren) des Smartphones zugreifen
    * (Video-)anruf vom Smartphone übernehmen
* Zusatzinformationen zu realen Objekten darstellen

## Mögliche Frameworks

Ein weiteres Ziel des Projektes ist die Entwicklung eines Frameworks, auf welchem weitere
Applikationen aufbauen können. Dazu haben wir uns folgende Gedanken gemacht.

### Virtueller Desktop

Die Hololens speichert offene Fenster im Raum für eine bestimmte Zeit. Wenn man aber den Raum
verlässt oder die Hololens zu lange abgestellt war, verliert sie diese Informationen. Die Idee
dieses Frameworks ist, Informationen zu speichern, welche Applikation wo geöffnet ist. So wäre es
möglich, zu definieren dass ein pdf an einer Wand, Outlook an einer anderen, OneNote und ein
Word-Dokument auf dem Pult geöffnet sind. Nun kann man den Raum wechseln und muss nun definieren,
welches Wand 1, Wand 2 und Pult ist und die entsprechenden Applikationen werden dort wieder
hergestellt.

### Geräteinformationen darstellen

Die Hololens erkennt ein Gerät und holt sich Informationen zu diesem Gerät ab, das sie dann in der
Nähe des Geräts darstellt. Die Informationen können dynamisch sein, wie Füllstände,
Geschwindigkeiten, Temperaturen, Druck, Leistung etc. oder auch statische wie Spezifikationen
oder Anleitungen.

Damit wäre es denkbar, dass ganze Vorgänge Schritt für Schritt beschrieben werden. Dabei könnte die
Hololens auf dem Gerät die Teile hervorheben, die gerade bearbeitet werden müssen (z.B. "Um die
Abdeckung zu entfernen, lösen sie die beleuchteten vier Schrauben mit einem Kreuz-Schraubenzieher")

Eine weitere Anwendung dieses Frameworks wäre, dass die Informationen, die bei einem Smartphone im
Notification Menu vorhanden sind, mit der Hololens über dem Smartphone schweben zu lassen.

## Anforderungen

Nachdem erste Erfahrungen gesammelt werden konnten, wurden folgende Anforderungen an das Framework
und die darauf zu entwickelnde Applikation gestellt.


### Framework (muss)

* Erkennen und Lesen QR-Code
    * Gerätetyp (mandatory)
    * Geräteid (mandatory)
    * Verbindungsinformationen (IP) (mandatory)
    * statische Zusatzinformationen zum Gerät (optional beliebige Anzahl)
* Falls definiert, darstellen eines 3-D-Modells des Gerätetyps.
    * Modell lokal gespeichert
* minimalste Webschnittstelle für Geräteinformationen
    * Gibt Demoinformationen zurück
    * Wird von uns über eine DotNet WebApi Applikation in einem IIS >7 realisiert
* Ansprechen der Webschnittstelle über die Hololens
* Darstellen der erhaltenen Informationen (Text) im Raum, verbunden mit dem entsprechenden Objekt
    * Die Informationen sollen regelmässig abgefragt und entsprechend aktualisiert werden
* Darstellen eines Bildes (analog Text Information)
    * Bild lokal gespeichert


### Framework (kann)

* Darstellen eines Benutzerhandbuchs (pdf, analog Text Information)
    * pdf lokal gespeichert
* Ein-/Ausblenden von Informationen mittels holografischem Menus
* Es kann entschieden werden, ob die Informationen am realen Gerät oder am Hologramm dargestellt
  wird



# Lösungsentwicklung

Hier werden die Erfahrungen und die Schritte beschrieben, die während des Projektes gemacht wurden

## Erste Schritte

Die Hololens ist als Stand-alone Gerät konzipiert. Sie benötigt keine externen Geräte um zu
funktionieren. Das heisst, die Applikationen werden direkt in der Brille ausgeführt und die
Hologramme werden auch von der Brille selber gerendert.
Die Vorteile davon sind:

* Keine zusätzlichen Kosten für weitere Geräte
* Keine Abhängigkeiten und Inkompatibilitäten (PCs, Treiber, Grafikkarten etc.)
* Keine Kabel die von der Brille hängen

Es gibt auch Nachteile:

* Die Brille wiegt schwer nach ein paar Stunden, weil sowohl Rechner, Display als auch Batterie drin
untergebracht sind.
* Die Laufzeit ist limitiert durch die Batterie.
* Die Displays, auf welchen die Hologramme angezeigt werden können, ist klein.

Die ersten Schritte als Anwender mit der Hololens waren erstaunlich einfach und intuitiv. Beim
ersten Verwenden gibt es ein Tutorial, das dem Träger erklärt, wie mit dem neuen Interface
interagiert wird. Als Pointer gibt es im Zentrum des Sichtfelds einen Punkt, Gaze genannt. Mit
diesem Pointer zielt man auf die Objekte, mit welchen man interagieren will.

Als Eingabegerät dient die Hand. Sie erkennt drei Gesten. Einerseits kann man das Start Menü
aufrufen, indem man die fünf Finger nach oben zusammenhält und danach öffnet wie eine Blume.
Andererseits gibt es den bekannten Klickevent. Für diesen hält man den Zeigefinger wie ermahnend vor
das Gesicht. Der Cursor im Blickfeld ändert daraufhin sein Aussehen um dem Benutzer zu
signalisieren, dass der Finger erkannt wurde. Nun kann der Träger mit dem Zeigefinger auf den Daumen
tippen um einen Klick auszulösen.

Lässt man den Zeigefinger und den Daumen nach dem Tippen zusammen, kann man Hologramme
festhalten. Damit kann man je nach Kontext das Hologramm im Raum verschieben oder durch einen langen
Text scrollen.

Dies funktioniert sehr gut und recht intuitiv, solange man sich auf den Cursor konzentriert. Der
verändert sich im Aussehen, je nach dem, mit was die Blickrichtung  kollidiert und ob der
Zeigefinger für den Klick erkannt wurde.

Der Cursor bewegt sich anhand der Hololens, also so wie sich auch der Kopf bewegt. Dies ist dann
problematisch, wenn man den Cursor etwas weiter entfernt oder sehr genau platzieren will. Jede
kleinste Bewegung des Kopfs resultiert in einer Verschiebung des Cursors. Vor allem beim Benutzen
der holografischen Tastatur kann dies ein Problem sein. Eine deutliche Verbesserung für eine
nächste Version der HL wäre unserer Meinung nach, wenn sich der Cursor nach dem Blick richten würde,
also nach den Augen und nicht nur den Kopf.

Die Tap-Gestik kann bei längerer Arbeit ermüdend auf Hand und Arm wirken. Abhilfe schaffen können
hier folgende Erweiterungen:

- Für Texteingabe kann eine Bluetooth-Tastatur angehängt werden.
- Die meisten Schaltflächen können auch über Sprachkommandos angesprochen werden.
- Im Lieferumfang der HL ist ein Clicker enthalten, der den selben Event auslöst wie der Tap.

### Demoapplikationen

Um eine Applikation starten zu können, geht man, wie unter Windows üblich, ins Startmenu. In der
Hololens öffnet  man es mittels der Bloom-Geste. Es gibt z.B. den unter Windows 8 eingeführten
App-Store den Webbrowser Edge, die Systemsteuerungen, eine Bibliothek für Hologramme.
Dies sind alles Applikationen, die in einem Fenster laufen und nicht die ganze Umgebung einnehmen.

Die Fenster selber sind nicht flach wie auf einem Desktop, sondern haben eine Tiefe von ca. 2 cm.
Der Benutzer kann nun diese Fenster irgendwo im Raum platzieren. Dazu greift er sich das Fenster
mittels der Press-Geste und kann es nun im Raum verschieben und zwar in allen 3 Richtungen des
Raums. Er muss nur darauf achten, dass die Hand nicht aus dem Wahrnehmungsbereich der HL kommt.

Die Hololens versucht dabei das Fenster an den realen Begebenheiten auszurichten. Mit anderen Worten,
wenn das Fenster in die Nähe einer Wand kommt, richtet es sich automatisch so aus, dass
es wie ein Bild an der Wand hängt.

#### Hologramme

Auf der Hololens gibt es eine Bibliothek mit vorgefertigten Hologrammen, die der Benutzer im Raum
platzieren kann. Dies ist ein guter Einstiegspunkt, um das Konzept der Hologrammen und deren
Platzierung im Raum kennen zu lernen.


#### RoboRaid

RoboRaid ist ein Spiel, bei welchem der Spieler Angriffe von Robotern abwehren muss. Es spielt im
Zimmer, in welchem sich der Spieler befindet.

Bevor das Spiel zum ersten Mal startet wird der Spieler aufgefordert, seine Umgebung, sprich den
Raum, in welchem er sich befindet, zu scannen. Dazu muss er mit der Hololens die Wände anschauen,
bis der Applikation klar ist, wo sie sich befinden. Danach kann das Spiel auch schon beginnen.

Als erstes wird der Spieler von einem schwebenden Roboter informiert, dass man Angriffe von
feindlichen Robotern abwehren muss. Dazu muss man sie mit dem Cursor anvisieren und mittels
Tap-Geste abschiessen.

Die feindlichen Roboter kommen aus einer Landungsbrücke eines Raumschiffes, welche durch die Wand
des Raumes bricht und sich mittels dreier Arme fest krallt. Wenn man daneben schiesst, reisst man
ebenfalls Löcher in die Wand. Dies sind tolle Beispiele, wie man die reale Welt mit der
holografischen verschmelzt. Auch wenn man den Hologrammen anmerkt, dass sie künstlich sind und dass
die Wand nicht wirklich beschädigt ist, so sind die Effekte dennoch beeindruckend und der Spieler
findet sich in einer Zwischenwelt wieder.

Die Angriffe laufen in verschiedenen Wellen ab. Nachdem man alle Roboter aus einer Landungsbrücke
zerstört hat, bricht eine nächste Landungsbrücke an einem anderen Ort im Raum durch die Wand und
die Angriffe gehen weiter.

Es gibt zwei Sorten von Robotern, die den Spieler angreifen: Einerseits gibt es diejenigen, die
im Raum schweben und diejenigen, die über die Wände krabbeln und sich auch hinter den Wänden
verstecken können. Um sie auch hinter der Wand erreichen zu können, kann der Spieler mittels
Sprachkommando den Röntgenblick aktivieren. Damit kann man eine begrenzte Zeit hinter die Wände
schauen. Danach dauert es eine Weile, bis die Fähigkeit wieder vorhanden ist.

Die Roboter schiessen natürlich zurück. Der Spieler wehrt diese Angriffe ab, indem er den Schüssen
ausweicht. Dies geschieht, indem er sich im Raum bewegt. Dies führt dazu, dass der Spieler
rasche Ausfallschritte nach rechts oder links macht oder sich urplötzlich duckt. Im Verlauf des
Spiels gibt es immer mehr Roboter, die gleichzeitig auf den Spieler schiessen. Dann kann der
Spieler durch die Ausweichmanöver auch mal ausser Atem geraten.

Hier zeigen sich jedoch die Probleme, die sich ergeben, dass der Fokus an die Blickrichtung der
Hololens und nicht die der Augen gebunden ist. In der Hitze des Gefechts und mit vielen Robotern und
Schüssen im Blickfeld verliert man oft den Cursor aus den Augen. Und der zielt dann am Roboter
vorbei.

Es ist auch vorgekommen, dass wir mit dem Finger für den Tap gezielt hatten, statt mit dem Kopf.

<!--TODO: Fazit-->

#### Jenga

Bei Jenga werden 54 Holzblöcke, die dreimal so lang wie breit sind, aufeinander geschichtet, so dass
ein Turm mit quadratischer Grundfläche entsteht. Die einzelnen Schichten bestehen dabei aus je drei
Blöcken, die kreuzweise geschichtet werden. Ziel ist es, einzelne Blöcke aus dem Turm zu nehmen und
sie oben auf den Turm zu legen, ohne dass dieser einstürzt.

Jenga wurde für die Hololens adaptiert und kann über den Windows Store installiert werden.

Dieses Spiel ist ein gutes Beispiel, wo die Limitationen der Hololens und ihrer Hologrammen liegen.
Man kann die Blöcke relativ einfach fassen und bewegen, aber es gibt kein haptisches Feedback. Bei
der realen Version gibt es bei den Blöcken  minimale Abweichungen der Dimensionen, so dass der
Spieler testen kann, ob sich ein Block einfach bewegen lässt oder nicht. Er merkt auch, wenn er den
Block schräg bewegt, da sich der Block verkantet und deshalb schwerer zu bewegen ist. Zudem kann der
Spieler auch zwei Hände benutzen, was vor allem beim Zurücklegen auf den Turm hilft.

All dies gibt es bei der Hololens nicht. Man kann den Block nicht antippen, um zu sehen, ob er sich
bewegt, wenn man versucht,  ihn schräg aus dem Turm zu ziehen, merkt man es meist erst, wenn der
Turm schon gefährlich wackelt. Und es ist oft nicht ersichtlich, in welche Richtung man korrigieren
muss. Es ist grundsätzlich gewöhnungsbedürftig, Hologramme festzuhalten und zu verschieben wegen der
fehlenden Haptik.

#### Fragments

Fragments ist ein Mystery Game für die Hololens. Man schlüpft in die Rolle eines Detektivs, der
Hinweise sammeln muss um einem Kindsentführer auf die Schliche zu kommen. Die Entwickler haben hier
sämtliche Möglichkeiten der Hololens ausgelotet, die möglich sind:

- Interaktion mit holografischen Personen
    - adaptieren ihr Verhalten anhand der Ausrichtung des Spielers im Raum
- Anpassung des Spielfelds an die reale Umgebung
    - holografische Personen passen sich der Umgebung an
- Raumklang
- Gestensteuerung
- Sprachsteuerung

Auch in diesem Spiel wird man zuerst aufgefordert, die Umgebung zu scannen, damit die Applikation
weiss, wo sie Hologramme platzieren kann, damit der Spieler damit interagieren kann. Das gewählte
Zimmer war zuerst zu klein, also mussten wir noch Teile des Flurs dazu nehmen. Das führte z.B. dazu, dass
die Karte im Zimmer am Schrank hing, man aber für die Interaktion mit dem Terminal in den Flur gehen
musste. Dieses Bewusstsein der Applikation über die räumlichen Begebenheiten sind beeindruckend.

Der Spieler wird Teil eines Ermittlerteams, das beauftragt wird ein entführtes Kind zu finden und
einen Mord aufzuklären. Dabei wird er an verschiedene Schauplätze geführt um Hinweisen nachzugehen.
Hier ziehen die Entwickler alle Register. Es geht darum, sich einen Überblick über die Situation zu
verschaffen, also einen Schritt zurück zu machen und von weitem zu schauen. Dann muss man aber auch
Teile der Szenerie genauer anschauen und sogar Objekte aufheben, um sie zu untersuchen.

Es gibt auch Situationen, bei denen muss man sich auf sein Gehör verlassen und einem Geräusch
folgen muss. Wenn man die Quelle dann gefunden hat, muss man es identifizieren. Das sind grandiose
Beispiele, wie realistisch die beiden kleinen Lautsprecher über den Ohren die Illusion erzeugen
können, der Ton käme von einem entfernten Ort im Raum.

Die gewonnenen Erkenntnisse können dann in einem Terminal eingegeben und Schlussfolgerungen
gezogen werden, die das Gebiet, wo man suchen muss, eingrenzen. Dabei helfen die anderen Mitglieder
des Ermittlerteams. Das sind lebensgrosse Figuren, die mit dem Spieler sprechen und auf sein
Verhalten reagieren. So weisen sie z.B. den Spieler darauf hin, dass er zur Seite gehen soll,
wenn er  einer Figur im Weg steht oder setzen sich sogar mal auf das Sofa des Spielers, um mit ihm zu
plaudern.

Es ist ein ganz neues Spielerlebnis, wenn man sich mit gleichgrossen Non-Player Characters in einer
Umgebung, die man aus dem täglichen Leben kennt, auseinander setzt. Da schaut man auch gerne über
die noch hölzernen Bewegungen und die kaum vorhandene Mimik hinweg.

Ein weiteres Problem kann das reale Licht im Raum darstellen. Wenn die realen Lichtverhältnisse nicht
den in der Virtualität vorgesehenen entsprechen, kann es dazuführen, dass man wichtige Details nur
sehr schwer sieht. Es kann vorkommen, dass die Lampe im Raum blendet oder das Hologramm für dunkle
Verhältnisse zu hell und deshalb zu transparent dargestellt wird.

Wenn der Raum zu verwinkelt ist oder die Wände keine regelmässigen Oberflächen haben, hat auch die
Hololens Mühe mit der Raumaufteilung. Ein Teil des Spielfelds war mal teilweise in einem Schrank und
in der Wand, so dass die Interaktion mit den Hologrammen sehr schwierig war.
komplexe Raumaufteilung schwierig

### Tutorials

Microsoft stellt viele [Tutorials](https://developer.microsoft.com/en-us/windows/holographic/academy)
zur Verfügung, die es dem Entwickler erleichtern soll, den Einstieg in die neue Denke der Hololens
zu erleichtern. Obwohl es Microsoft offen gelassen hat, wie die Hologramme erstellt werden, so
haben sie sich dennoch auf [Unity](https://unity3d.com/) konzentriert.

Man kann die Applikationen entweder auf eine Hololens installieren oder man lässt sie in einem
Emulator laufen. Dazu muss aber die Virtualisierung vom Prozessor unterstützt werden.

Als Einsteiger gilt es, sich mit folgenden Themen auseinander zusetzen

* **Blickrichtung:** Damit wird der Cursor in der Hololens bewegt
* **Gesten:** Das Erkennen von Bewegungen mit der Hand und darauf reagieren
* **Stimme:** Man kann der Hololens die Befehle auch mittels Stimme übergeben
* **Raumklang:** Die Hololens hat zwei kleine Lautsprecher, die sich über den Ohren befinden. Dank
geschickter Modulation der Töne, kann der Eindruck erzeugt werden, dass die Geräusche von irgendwo
im Raum her kommen.
* **Raumerkennung:** Die Hololens kann den Raum, in welcher sie sich befindet wahrnehmen und auch
die Tiefe erkennen. Dieses Scannen geschieht automatisch, jedoch kann der Entwickler mit diesen
Informationen arbeiten und darauf reagieren.


## Entwicklung "Gerätestatus" Framework

Für die Demonstration des Frameworks haben wir uns für einen Laptop als Gerät entschieden. Der ist
mobil und kann ebenfalls als Webserver dienen. Somit ist kein dediziertes Demogerät und zusätzliche
Infrastruktur notwendig.

Für die Darstellung der Informationen werden folgende Daten benötigt, die wir über die
Webschnittstelle und QR-Code erhalten werden.

- Textinformation die dargestellt werden soll
- Bezeichner der Quelle
- Vektor für die relative Position der Information zum Gerät
- Vektor für die relative Position der Quelle zum Gerät

### Workflow

Ein Gerät, das mit zusätzlichen Informationen muss sich einmalig bei der Hololens anmelden. Dies
geschieht über einen QR-Code, der mit der Applikation auf der Hololens gescannt werden muss. Mit den
Informationen konfiguriert die Hololens das Gerät, indem es sich merkt, wo es steht und wo es die
zusätzlichen Daten abholen kann.

Wenn sich die Daten dynamisch ändern können (wie Füllstand oder Temperatur oder ähnliches),
holt sich die Hololens periodisch die aktualisierten Daten und stellt sie erneut dar. Wenn es sich
um statische Daten handelt, fällt dies natürlich weg.

### Aufbau des Frameworks
Das Framework besteht aus einem UnityPackage und einer Anleitung wie die einzelnen Komponenten
genutzt werden sollen. Zusätzlich wird das Microsoft HoloToolkit UnityPackage benötigt. Dieses ist
[auf GitHub verfügbar](https://github.com/Microsoft/HoloToolkit-Unity) und enthält ein Readme
mit der Installationsanleitung.


#### Die Assets
Als Assets werden alle Dateien bezeichnet, welche in einer Unity App benutzt werden.

- **3D-Modelle:** Unity-Objekte mit Meshes, Positionierung, Collider und Materialien.
- **Scripts:** C# Klassen welche Unity-Objekten angehängt werden können. (Javascript ist auch möglich)
- **Materialien:** Oberfläche welche mittels Texturen und Shader auf 3D-Modelle angewendet wird.
- **Prefabs:** Eine Gruppierung von Assets welche zur Laufzeit instanziert werden kann.
- Weiter gibt es Bilder, Sprites, Audio, Lichtquellen, Physikmaterialien und Animationen

Im erstellten Framework befinden sich hauptsächlich Scripts und Prefabs.

**DeviceManager.cs**
Der `DeviceManager` behandelt die Erstellung und das Entfernen von Geräten. ????????????

**DeviceBehavior.cs**
Jedes Gerät wird durch das `DeviceBehavior` Script gesteuert. Es enthält alle offline
Informationen zum Gerät und die URL zur Webschnittstelle. In regelmässigen Abständen,
konfigurierbar durch `PollRateInSec` werden die Informationen abgefragt und dargestellt.

**Device Prefab**
Für jedes neue Gerät muss ein Device Prefab erstellt werden. Es enthält das 3D-Modell, den
Collider und das `DeviceBehavior` Skript mit der gewünschten Konfiguration aus
`DeviceName`, `DeviceInformationUrl` und `PollRateInSec`. Weiter müssen die `TextInformationPrefab` und
`ImageInformationPrefab` sowie die Materialien (Normal und Selektiert) für die 3D-Linien gesetzt werden.
Das Hauptobjekt im Prefab muss alle Skallierungs- und Rotationswerte auf 0 gesetzt haben. Die falls das 
3D-Modell nicht in der gewünschten Grösse oder Ausrichtung vorhanden ist muss es in einem Kindobjekt des 
Prefab angepasst werden.

**InformationBaseScript.cs**
Um Informationen wie Text oder Bild sinnvoll darzustellen, wird dynamisch eine "3D-Linie" von
einem Ort des Gerätes (Anker) zum Ort an welchem die Information dargestellt wird (Ziel) erzeugt.
Diese "3D-Linie" besteht aus einer Kugel beim Anker, einem langen Zylinder als Verbindung und
einem breiten Zylinder als Podest für die Information. Es ermöglicht die Information mit der
Press-Gestik zu verschieben und wechselt das Material der "3D-Linie" falls die Information fokussiert wird.

**TextInformation.cs**
Diese Ableitung des `InformationBaseScript` ermöglicht es Text mittels `SetText` darzustellen.

**TextInformation Prefab**
Damit der Benutzer den Text sehen kann sind in diesem Prefab nebst dem `TextInformation` Scripts
mehrere Assets nötig. Ein Billboard Script, aus dem HoloToolkit, richtet das Objekt relativ zum Blickwinkel der Kamera aus.

Ein `BoxCollider` wird verwendet damit registriert werden kann, ob der Benutzer das Objekt
fokussiert. Der Collider passt sich mittels dem `InformationBaseScript` dem Inhalt des Prefabs an.
Auch die Unity Scripts `HorizontalLayerGroup` und `ContentSizeFitter` werden benötigt damit sich die
Grösse dynamisch dem Inhalt anpasst. Hierarchisch enthält das Prefab die Unity UI Objekte Canvas,
Panel und Text. Das Panel besitzt ein Image Script ohne Bild aber mit der Farbe blau um einen
Hintergrund für den Text zu haben. Ein Padding im  `HorizontalLayerGroup` des Panels erleichtert die
Lesbarkeit des Textes. Auf der untersten Ebene befindet sich das Textobjekt,  welches vom
`TextInformation` Script aktualisiert wird.

**ImageInformation.cs**
Vergleichbar mit dem `TextInformation` Script wird stattdessen ein Bild durch `SetImage` gesetzt.
Zusätzlich wird die Grösse des Prefabs der Grösse des Bildes angepasst. Dies war nötig da der
`ContentSizeFitter` nicht wie bei dem Textobjekt funktioniert hat.

**ImageInformation Prefab**
Dieses Prefab gleicht dem `TextInformation` Prefab bis auf zwei Änderungen. Der `ContentSizeFitter`
wird nicht verwendet und es wird das Text Objekt mit `RawImage` ersetzt.

**InformationSelectionScript.cs**
Die verschiedenen Informationen werden zu Beginn nicht dargestellt. Es in einem Menu erscheint jede Information als Button. Wird der Button geklickt, verschwindet er und die Information wird dargestellt. Falls alle Informationen dargestellt werden verschwindet das Menu. Informationen auf welche gecklickt werden verschwinden und der Button im Menu erscheint erneut.
Das Script benötigt ein `PrefabButton` und das `PanelTransform` um die Buttons dynamisch zu erstellen und positonieren. Die Methode `DeviceToEnable` registriert eine Information welche momentan Disabled ist. Sobald ein Button gedrückt wird wird er entfernt und das Event `EnableDevice` ausgelöst. `DeviceBehavior` ruft `DeviceToEnable` auf und behandelt `EnableDevice`.

**InformationSelection Prefab**
Dem `TextInformation` Prefab sehr ähnlich, unterscheidet sich dieses nur durch die `VerticalLayerGroup` und den vordefinierten Text. Auf der Layer Group sind die Abstände zwischen den generierten Buttons und zu dem Rand definiert.

#### Konfiguration
Die oben genannten Assets müssen in einer Unity Scene konfiguriert werden. Die minimale Hierarchie
besteht aus den folgenden Assets:

- MainCamera
    - ManualCameraControl (Script)
- Managers
    - GazeManager (Script)
    - GestureManager (Script)
    - DeviceManager (Script)
    - KeywordManager (Script)
- EventSystem
- Cursor
    - CursorManager (Script)
    - CursorOnHolograms
    - CursorOffHolograms
- DiractionalLight
- SpatialMapping
    - SpatialMappingObserver (Script)
    - SpatialMappingManager (Script)
    - ObjectSurfaceObserver (Script)

**MainCamera**
Unter `HoloToolkit/Utilities/Prefabs/Main Camera.prefab` befindet sich die hololens-kompatible
Kamera. Um im Unity Editor die Kamera zu steuern, gibt es das Script
`HoloToolkit/Utilities/Scripts/ManualCameraControl.cs`.

**Managers**
Das mittels `CreateEmpty` erstellte Gameobjekt wird benutzt um generelle Manager Skripte anzuhängen.
Der `HoloToolkit/Input/Scripts/GazeManager.cs` steuert die Gaze Gestik, welche zum Fokussieren von
Objekten benutzt wird. Mit `StabilizationPlane` und `GazeStabilization` wird der Gaze, equivalent zur
Maus auf einem PC, stabilisiert. Dies ist nützlich, da eine Maus auf dem Tisch stabiler ist als
die Kopfbewegungen eines Menschen.
Mittels des `HoloToolkit/Input/Scripts/GestureManagers.cs` werden die Meldungen `OnSelect`, `OnPressed`
und `OnReleased` an fokussierte Objekte gesendet. Falls das fokussierte Objekt eine dieser Methoden
implementiert hat, wird sie ausgeführt. Zusätzlich bietet der Manager verschiedene Events bezüglich
Manipulation, z.B. `ManipulationStartet`, und Properties wie das `FocusedObject` an.

Um die Spracherkennung zu nutzen, wurde ein
`HoloToolkit/Input/Scripts/KeywordManager.cs` kreiert.
Das Property `KeywordsAndResponses` beinhaltet gesprochene Worte (Keyword) und den dazugehörigen
Methodenaufruf auf ein Gameobjekt. Als Beispiel wird bei "remove all" die Methode `RemoveDevices()`
auf dem `DeviceManager` Script ausgeführt. Das Keyword sollte aus mehr als einem Wort bestehen und
anderen Keywords nicht zu sehr gleichen. Dies erhöht die Chance, dass die Hololens den richtigen
Befehl erkennt.

**EventSystem**
Ein `EventSystem` wird benötigt falls man mit Unity-UI Elementen arbeitet. Nebst dem normalen
`StandaloneInputModule` enthällt das Holo Toolkit ein `HoloLensInputModule`.

**Cursor**
Das `HoloToolkit/Input/Prefab/Cursor.prefab` ist die Darstellung des Cursors. Es können zwei
verschiedene Modelle angegeben werden. Ein normales Modell und eines für den Fall dass etwas
fokussiert ist. Zudem kann eine Distanz zwischen Fokussierungspunkt und Cursor konfiguriert
werden. Falls es einen `GazeManager` gibt, in unserem Fall im Managers Objekt, wird der Cursor
automatisch positioniert.

**DirectionalLight**
Das `DirectionalLight` von Unity ist nicht nötig, lässt die Hologramme aber natürlicher aussehen. Es
können auch andere Lichtquellen benutzt werden, da jedoch die Umgebung in welcher die Hololens
eingesetzt wird nicht bekannt ist, empfehlen wir eine weit entfernte Lichtquelle.

**SpacialMapping**
Damit die Umgebung wahrgenommen werden kann, wird das
`HoloToolkit/SpatialMapping/Prefabs/SpatialMapping.prefab` genutzt. Mit dem `SpatialMappingObserver`
kann die Auflösung und Aktualisierungszeit konfiguriert werden. Der `SpatialMappingManager` kann die
Umrisse der Strukturen darstellen. Dies wird in der Demo Applikation nicht genutzt, da es vom
Gerät ablenken würde. ObjectSurfaceObserver???????????


### QR-Code

Der QR-Code ist sozusagen der Einstiegspunkt für die Applikation. Darauf ist die Id des Geräts und
die Datenquelle vermerkt, wo sich die Applikation die Informationen für das entsprechende Gerät
abholen kann.

Diese Informationen würden dann verwendet um das entsprechende Hologramm im Raum zu platzieren und
die Informationen an das Objekt zu heften und regelmässig zu aktualisieren.

Um Bilder mit der Kamera der  Hololens aufzunehmen gibt es das `PhotoCapture` Objekt im Namespace
`UnityEngine.VR.WSA.WebCam`.

Für die Erkennung und das Decodieren des QR-Codes haben wir uns für die
ZXing [^zxing-library] Bibliothek entschieden. Sie unterstützt sehr viele Formate und es gibt sie
für viele Plattformen

Je nach Version ist die Kapazität auf einem QR-Code sehr limitiert[^qr-capacity].

[^qr-capacity]:[Kapazitäten von QR-Codes](http://www.qrcode.com/en/about/version.html)
[^zxing-library]:[Zebra Crossing Barcode Reader](https://zxingnet.codeplex.com/)

Leider sind wir an der Umsetzung dieses Features gescheitert.


#### Gründe für das Scheitern

Der erste Versuch scheiterte an falschen Annahmen, wie das Teilprojekt aufgebaut sein soll. Der
Teil, der QR-Codes liest und eine externe Datenquelle anspricht benötigt keine virtuellen Elemente.
Er sollte nur die Kamera ansprechen und auf den erhaltenen  Bilder mittels einer Bibliothek nach einem
QR-Code zu suchen. Wie die  gefundenen Informationen an den Teil der Applikation, der für das
Rendering des Hologramms und der Informationen zuständig ist, gesendet wird, wollten wir in einem
späteren Schritt lösen.

So versuchten wir eine Applikation zu entwickeln, die komplett ohne Unity auskommt und nur Bilder
von der Kamera vom  QR-Framework verarbeiten lässt. Das dies möglich sein sollte, entnahmen wir der
Tatsache, dass es im Visual Studio 2015 ein Projekttemplate für holografische Applikationen gibt.
Diese Applikation wurde aber nie lauffähig auf der Hololens.

<!--TODO: Pasci weiss vielleicht wieso-->

Als nächstes versuchten wir, die Funktionalität in einem Unity Projekt



### Datenquelle

Als dynamische Datenquelle haben wir uns für eine ReST-Schnittstelle entschieden[^source]. Sie liefert für
vier verschiedene Geräte unterschiedliche Daten, die in etwa so aussehen.

```json
{
    "displayData":"Dynamic data about the device",
    "deviceDescription":"Static description of the device",
    "positionToSource":{
        "xValue":3.5,
        "yValue":4,
        "zValue":8.35
    },
    "positionToDevice":{
        "xValue":3.5,
        "yValue":4,
        "zValue":8.35
    },
}
```

`displayData` ist die Information, an der man interessiert ist. Hier kann der Füllstand,
    Fehlermeldungen etc. stehen.

`deviceDescription` ist die Bezeichnung des Teils des überwachten Geräts, z.B. "Einschaltknopf",
    "Wassertank"

`positionToSource` ist die relative Position zum Teil, wo die Information dargestellt werden soll.

`positionToDevice` ist die relative Position des Teil zum Gerät selber. Zwischen den beiden
Positionen gibt es eine logische Verbindung, die visuell dargestellt wird.


[^source]:[Source on GitHub](https://github.com/ledux/pawi-hololens-dummyapi.git)

### Tipps und Stolpersteine bei der Entwicklung

Während der Entwicklung sind wir auf einige Probleme gestossen, welche wir in diesem Abschnitt
zusammen mit den allfälligen Lösungen auflisten.

**Setup der Entwicklungsumgebung**

Um für die Hololens zu entwickeln wird ein Windows 10 Pro, Enterprise oder Education Computer mit
aktiviertem Hyper-V benötigt. Das SDK unterstützt offiziell auch die Betriebssysteme ab Windows 7,
jedoch nicht alle benötigten Tools.

[Installationsanleitung](https://developer.microsoft.com/en-us/windows/holographic/install_the_tools)

**Einschränkungen des Hololens Betriebssystems**

Obwohl die Hololens selbst ein Computer ist, bietet das WindowsHolographic nicht die gewohnten
Funktionalitäten. Es gibt keine CommandLine, keinen TaskManager und auch keinen Windows Explorer.
Apps,  welche man auf dem Desktop (genannt Shell) platziert hat, sind gestartet. Um den Prozess zu beenden
muss das Icon, welches 2D oder 3D sein kann, entfernt werden.

**Hololens auf sich Anpassen**

Mit einem Gewicht von immerhin 579 Gramm ist die Hololens nicht gerade leicht. Wenn man sich aber
etwas Zeit nimmt, um die Hololens an den eigenen Kopf anzupassen, ist dennoch ein grosser
Tragekomfort gewährleistet. Dazu gehört die Wahl des geeigneteren der beiden Stegstützen, auf
welchen die Hololens auf der Nase getragen wird und der Verwendung des Overhead Straps.


**Einrichten der Hololens für die Entwicklung**

Um mit den Entwicklungstools auf die Hololens zugreifen zu können, muss der Debug Mode aktiviert
werden. Microsoft hat die Anleitung dazu auf der Seite
[Using Visual Studio](https://developer.microsoft.com/en-us/windows/holographic/Using_Visual_Studio.html#enabling_developer_mode)
versteckt. Die IP-Adresse der Hololens kann man über `Settings->Network&Internet->Advanced options` auslesen.

**Konfiguration eines neuen Projektes**

Ein Projekt mit Unity muss spezifisch für die Hololens angepasst werden. Das Tutorial
[Microsoft Holograms 100](https://developer.microsoft.com/en-us/windows/holographic/holograms_100)
enthält die nötigen Konfigurationen. Der Unity Editor ermöglicht es vieles ausserhalb der Hololens
und dessen Emulators zu testen. Viele der Skripte wie `ManualCameraControl`, `GestureManager` und
`KeywordManager` bieten die Möglichkeit einen alternativen Input per Tastatur zu definieren.

**Holographic Academy**

In 9 Tutorials werden die wichtigsten Elemente für die Entwicklung beigebracht.
[Microsoft Holographic Academy](https://developer.microsoft.com/en-us/windows/holographic/academy)

**Weiss und Schwarz**

Die Farben Weiss und Schwarz haben jeweils ihre eigenen Probleme. Da die Hololens nur additiv RGB
Farben darstellen kann ist Schwarz die gänzlich Transparent. Dadurch kann man nicht beeinflussen
was der Benutzer sieht. Im gegensatz dazu hat Weiss das Problem dass die Hololens RGB in 3
separaten Schichten darstellt und diese nicht immer perfekt übereinander Liegen. Dadurch sieht man
z.B. bei einem weit entfernten Weissen Punkt stattdessen drei leicht versetzte Punkte in rot, grün und blau.

**Update nicht blockieren**

Die Update Methode wird jedes Frame aufgerufen und ist somit nur für nicht blockierende Aufgaben
geeignet. Die Methode InvokeRepeating von MonoBehaviour bietet die Möglichkeit für zyklische
Methodenaufrufe.

```cs
// Aufruf alle 3 Sekunden nach initialem warten von 2 Sekunden
void Start ()
{
    InvokeRepeating("DoSomething", 2, 3);
}

void DoSomething()
{
    //xyz
}
```

Eine weitere Möglichkeit ist eine Coroutine aufzurufen. Dies ist eine Methode welche pro Frame
einen bestimmten Bereich durchführt und dann die Ausführung pausiert.

```cs
void Update()
{
    if (Input.GetKeyDown("f")) {
        StartCoroutine("Fade");
    }
}

IEnumerator Fade()
{
    for (float f = 1f; f >= 0; f -= 0.1f)
    {
        Color c = renderer.material.color;
        c.a = f;
        renderer.material.color = c;
        yield return null;
    }
}
```

Wenn Update aufgerufen wird, startet die Coroutine Fade und läuft durch bis zum ersten yield
return. Im folgenden Frame fährt die Ausführung in der Methode nach dem yield weiter. Weitere
Informationen zum Beispiel findet man in der
[Unity Dokumentation](https://docs.unity3d.com/Manual/Coroutines.html).

Eine Coroutine wurde auch im Script `DeviceBehavior` benutzt für den asynchronen HTTP Request.

```cs
void UpdateText()
{
    StartCoroutine(GetInformationFromUrl());
}

IEnumerator GetInformationFromUrl()
{
    // Erstellung des HTTP Requests mittels der URL
    UnityWebRequest www = UnityWebRequest.Get(DeviceInformationUrl);
    // Asynchroner aufruf des Requestes
    yield return www.Send();
    // Behandlung der Antwort
    Debug.Log(www.downloadHandler.text);
}
```

**JSON Serialisieren**
Unity bietet mit der UnityEngine.JsonUtility eine einfache Möglichkeit mit JSON Objekten
umzugehen. Den Aufbau der Daten kann als Klasse oder Struct definiert werden. Falls man Klassen
verwendet können jedoch Probleme mit Hierarchischer Serialisierung auftauchen.

```cs
[System.Serializable]
public struct JsonInput
{
    [System.Serializable]
    public struct JsonInfo
    {
        [System.Serializable]
        public struct JsonVector
        {
            public float x;
            public float y;
            public float z;
        }

        public string descriptor;
        public string text;

        public JsonVector anchor;
        public JsonVector target;
    }

    public JsonInfo[] information;
}
```

Die Serialisierung ist mit einer Zeile möglich.

```cs
var input = JsonUtility.FromJson<JsonInput>(jsonString);
```

**Dynamisch Formen erstellen**

Es gibt vordefinierte [primitive Formen](https://docs.unity3d.com/Manual/PrimitiveObjects.html)
wie Würfel und Kugel welche einfach erstellt werden können. Im erstellten Framework bestehen die
Verbindungen aus langen  schmalen Zylindern.

```cs
// Erstellung des GameObjects
var Cylinder = GameObject.CreatePrimitive(PrimitiveType.Cylinder);
// Positionierung relativ zum parent ermöglichen
Cylinder.transform.parent = this.gameObject.gameObject.transform;
// Position setzen
Cylinder.transform.position = new Vector3(2f, 1f, 4f);
// Grösse verändern
Cylinder.transform.localScale = new Vector3(50f, 5f, 50f);
// Das material setzen
Cylinder.GetComponent<Renderer>().material = SomeMaterial;
//

//Das Objekt muss selbst wieder zerstört werden
void OnDestroy()
{
    Destroy(Cylinder);
}
```

**Hierarchie beachten**

Jedes GameObject befindet sich in einer Hierarchie. Die Methode `.transform.SetParent()` kann das Elternobjekt verändert werden. Wird das Elternobjekt positioniert oder verschoben beeinflusst dies auch die Position der Kinder. Die `transform` Variable jedes GameObjects besitzt die absolute Position `position` sowie auch die lokale Position `localPosition`. Meistens ist es sinnvoll die Hierarchien zu bilden und mit den lokalen Werten zu arbeiten. Ansonsten werden relative Positionierungen und Bewegungen schnell unübersichtlich und kompliziert.

**Plazierung relativ zur Ecke eines Objektes**
Für das Geräteinformation Framework muss es möglich sein Koordinaten relativ zur Ecke eines Gerätes zu definieren. Da jedes Gerät einen `BoxCollider` besitzen muss, wird dessen Ecke als Ursprung benutzt. Die Variable `size` besitzt die Durchmesser des Collider pro Dimension. Da diese Werte unabhängig von der lokalen Skallierung sind, müssen sie skalliert werden. Als letzer Schritt werden alle Werte im Vektor halbiert.
```cs
boxColliderSize.Scale(Device.transform.localScale);
boxColliderSize = boxColliderSize / 2;
```
Es gibt eine weitere Möglichkeit die Grösse eines Collider, Renderer oder Mesh auszulesen. Mit der 'bounds' Variable erhällt man Zugriff auf das Zenturm, die Grösse und weiteren Werten. Diese Werte sind relativ zum Koordinatensystem der Welt, somit verändert sich die Breite eines Objektes durch die Rotation. Im Framework war jedoch die Distanz im lokalen Koordinatensystem gesucht.

**Rotation nach anderem Objekt ausrichten**

Um die oben erwähnten Zylinder-Verbindungen zu realisieren, wird der Zylinder zuerst zwischen
Start und Ziel positioniert. Die Rotation des Zylinders kann mit der Methode `LookAt` einfach gesetzt
werden.

```cs
Cylinder.transform.LookAt(connectionSource.transform.position, Vector3.up);
```

**3D-Modell Dateiformate**

Unity kann folgende Formate nativ importieren: `.fbx, .dae, .3ds, .dxf` und `.obj` [^3d-formats]
Maschinenbauer in der Industrie nutzen jedoch andere Dateiformate wie `.step` und `.stl`, da die
exakte Form in ihrem Bereich eine grössere Bedeutung hat als die Texturen und Animationen. Mittels
Konverter, wie dem [Spin 3D Converter Software](http://www.nchsoftware.com/3dconverter/),
können `.stl` in `.obj` konvertiert werden. Das Problem an `.stl` ist jedoch, dass es keine
Materialinformationen beinhaltet.

[^3d-formats]:[Unity 3D Formate](https://docs.unity3d.com/Manual/3D-formats.html)

**Shader auf der Hololens**

Damit ein Shader auch auf der Hololens funktioniert muss er in Unity unter
`Edit/Project Settings/Graphics/Always Included Shaders` aufgeführt werden. Nicht funktionierende
Shader werden Pink dargestellt.

**Probleme mit 2D Linien**

Mit dem `LineRenderer` können 2D Linien in der 3D Umgebung dargestellt werden. Vielen Materialien
haben Probleme die Linie zu färben. Der `Particles/Additive Shader` funktioniert, ist jedoch nicht
deckend. Die Linie hat eine definierte Breite und richtet sich nach der Kamera aus. Falls sie mehr als
2 Punkte verbindet, ist diese Breite im  dreidimensionalen Raum und aus der Sicht der Kamera nicht
mehr konstant. Statt 2D Linien wird im erstellten Framework mit 3D Zylinder gearbeitet. Dies
vermeidet einen Stilbruch zwischen 2D und 3D.

```cs
LineRenderer lineRenderer = gameObject.AddComponent<LineRenderer>();
lineRenderer.SetColors(Color.red, Color.red);
lineRenderer.material = new Material(Shader.Find("Particles/Additive"));
lineRenderer.SetWidth(0.01f, 0.01f);

// Eckpunkte setzen
// GetLineCorners erzeugt die Koordinaten
Vector3[] linePoints = this.GetLineCorners();
lineRenderer.SetVertexCount(linePoints.Length);
lineRenderer.SetPositions(linePoints);
```

**Spline Bewegung**

Falls sich ein Objekt einer stetigen Linie folgen soll, gibt es ein Spline Controller Skript von
der [Unity Community](http://wiki.unity3d.com/index.php/Main_Page). Dieses Skript wurde
ausprobiert, wird jedoch im Framework und in der Demo App nicht verwendet. Kind Objekte eines
GameObjects geben die Fixpunkte an, welche besucht werden.

**Raycast**

Ein Raycast erkennt die erste Kollision von einem Punkt und einer Richtung aus. Ein Beispiel dazu
ist die Gaze Gestik. Von der Kamera aus wird ein Raycast gesendet und liefert das Objekt und die
Position. Es ist möglich ein bestimmtes Objekt als Ziel zu wählen. Im nachfolgenden Beispiel
möchte man herausfinden ob sich das Objekt über einem bestimmten anderen Zielobjekt befindet.

```cs
// Raycast nach unten
var rayOrigin = new Ray(transform.position, Vector3.down);
RaycastHit hit;
// True wenn der Raycast den targetCollider innerhalb von
// der Distanz 2 trifft.
if (targetCollider.Raycast(rayOrigin, out hit, 2f))
{
    var position = hit.point;
}
```

**Manipulations Gestik**

Das Verschieben von Objekten kann wie folgt gelöst werden. Beim `OnPressed` Methodenaufruf wird die
aktuelle Position gespeichert und ein Flag gesetzt. Die `OnReleased` Methode wird nicht benötigt, da
der Benutzer sehr wahrscheinlich das Objekt nicht mehr fokussiert hat, wenn er die Gestik beendet.
Stattdessen wird das Flag bei den Events `ManipulationCanceled` und `ManipulationCompleted`
zurückgesetzt. Dies ist unabhängig davon wie die Gestik beendet wird.

```cs
private bool isPressed = false;
private Vector3 previousPosition;

void Start()
{
    GestureManager.Instance.ManipulationCanceled += this.ResetIsPressed;
    GestureManager.Instance.ManipulationCompleted += this.ResetIsPressed;
}

private void ResetIsPressed()
{
    this.isPressed = false;
}

void OnPressed()
{
    this.previousPosition = this.gameObject.transform.position;
    this.isPressed = true;
}

void Update()
{
    if (this.isPressed && GestureManager.Instance.ManipulationInProgress)
    {
        this.gameObject.transform.position = this.previousPosition + GestureManager.Instance.ManipulationOffset;
    }
}

void OnDestroy()
{
    GestureManager.Instance.ManipulationCanceled -= this.ResetIsPressed;
    GestureManager.Instance.ManipulationCompleted -= this.ResetIsPressed;
}
```

# Schlussfolgerungen und Ausblick

Da diese Arbeit nur ein Prototyp ist und höchstens als proof of concept gelten kann, gibt es
einiges, das wir ausgespart haben.

## Sicherheit

Die Informationen, die die Hololens über die Geräte darstellt, sind potentiell kritisch und dürfen
nicht in jedem Fall frei verfügbar sein. In unserem Szenario haben wir diesen Aspekt vollständig
ausgeklammert. Aber grundsätzlich muss es möglich sein, dass sich die Hololens an der Datenquelle
identifizieren muss, bevor sie Daten erhält. Hier sind folgende Szenarien möglich:

- Es können die Credentials im QR-Code selber untergebracht werden. Damit hat aber jeder, der Zugang
zum QR-Code hat automatisch Zugang zu den Daten.
- Die Hololens wird bei der Datenquelle registriert und die die Credentials auf der Hololens selber
installiert (z.B. Username/Password oder Zertifikat)
- fixes Pairing bei Bluetooth

## Art der Datenquelle

Es ist denkbar, dass auch andere Datenquellen als eine Webschnittstelle zum Einsatz kommen können.
Gerade wenn es sich um technische Geräte handelt (und nicht um z.B Bilder, Skulpturen,
Gebäudeteile) und man sich sowieso in der unmittelbaren Umgebung aufhält, liegt der Einsatz von
Bluetooth auf der Hand.

Diese Art und Weise, wie die Applikation zu seinen Daten kommt, müsste auch noch im QR-Code
untergebracht werden.

_--> Notizen TODO: ausformulieren_

- Update interval im QR-Code
- Ob statisch oder dynamische Daten im QR-Code

# Lessons learned

