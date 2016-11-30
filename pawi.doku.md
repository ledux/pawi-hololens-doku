# Mixed Reality mit Hololens

\pagebreak

## Selbständigkeitserklärung

Hiermit erkläre ich, dass ich die vorliegende Arbeit selbständig angefertigt und keine anderen als
die angegebenen Hilfsmittel verwendet habe. Sämtliche verwendeten Textausschnitte, Zitate oder
Inhalte anderer Verfasser wurden ausdrücklich als solche gekennzeichnet


Pascal Schulthess

---

Lukas Tanner

---

\pagebreak

## Abstract (deutsch)
Hololens ist eine mixed reality Brille von Microsoft, die es erlaubt, die Umgebung mit
holografischen Elementen zu erweitern. Ebenso kann man damit programmatisch mit der Umgebung
interagieren.

Dieses Dokument beschreibt erste Erfahrungen mit der dieser neuen Technologie und die Entwicklung
eines Frameworks, das es erlaubt, Informationen zu einem realen Objekt im Raum darstellen zu lassen.



## Abstract (english)

## Abkürzungen und Definitionen

## Ziele und erwartete Resultate

### Einführung

In den letzten Monaten wurden von vielen Herstellern Virtual Reality Brillen auf den Markt
gebracht, meist im Gaming Bereich. Microsoft hat mit ihrer Brille ein anderes Zielpublikum
und andere Anwendungsbereiche im Fokus. Mit der Hololens wird die Umwelt, in welcher sich der Träger
befindet, mit zusätzlichen virtuellen Elementen ergänzt.

Dank einer 3D-Kamera kann die Brille die Umgebung wahrnehmen und Objekte darin erkennen. Somit
können die virtuellen Elemente auf echte Objekte gesetzt werden und damit interagieren.

Ziele dieses Projektes sind

* Die Funktionsweise und Möglichkeiten der Hololens erkunden
* Mögliche Anwendungsfälle identifizieren und evaluieren
* Framework entwickeln, auf denen zukünftige Applikationen aufbauen können
* Prototyp des Frameworks in Code umsetzen

### Mögliche Anwendungsfälle

Die Hololens bietet sicherlich Einsatzmöglichkeiten im Spielebereich, wie Microsoft mit Games wie
[RoboRaid](https://www.microsoft.com/microsoft-hololens/en-us/apps/roboraid) oder
[Fragments](https://www.microsoft.com/microsoft-hololens/en-us/apps/fragments) bereits gezeigt haben.

Aber sie wurde für Geschäftslösungen entwickelt und dort will sie Microsoft auch positionieren.
Dabei gibt es bereits eine
[Sketchup Adaption](https://www.microsoft.com/en-us/store/p/sketchup-viewer/9nblggh4338q#),
mit welcher mehrere Personen am selben 3D-Modell arbeiten können

Wir haben folgende Ideen entwickelt, bei welchen wir auch die Möglichkeit sahen, etwas davon
umzusetzen.

* Brettspiel Simulation
* Mit einem Smartphone koppeln
    * Auf Geodaten (und andere Sensoren) des Smartphones zugreifen
    * (Video-)anruf vom Smartphone übernehmen
* Zusatzinformationen zu realen Objekten darstellen

### Mögliche Frameworks

Ein weiteres Ziel des Projektes ist die Entwicklung eines Frameworks, auf welchem weitere
Applikationen aufbauen können. Dazu haben wir uns folgende Gedanken gemacht.

#### Virtueller Desktop

Die Hololens speichert offene Fenster im Raum für eine bestimmte Zeit. Wenn man aber den Raum
verlässt oder die Hololens zu lange abgestellt war, verliert sie diese Informationen. Die Idee
dieses Frameworks ist, Informationen zu speichern, welche Applikation wo geöffnet ist. So wäre es
möglich, zu definieren dass ein pdf an einer Wand, Outlook an einer anderen, OneNote und ein
Word-Dokument auf dem Pult geöffnet sind. Nun kann man den Raum wechseln und muss nun definieren,
welches Wand 1, Wand 2 und Pult ist und die entsprechenden Applikationen werden dort wieder
dargestellt.

#### Geräteinformationen darstellen

Die Hololens erkennt ein Gerät und holt sich Informationen zu diesem Gerät ab, das sie dann in der
Nähe des Geräts darstellt. Die Informationen können dynamisch sein, wie Füllstände,
Geschwindigkeiten, Temperaturen, Druck, Leistung etc oder auch statische wie Spezifikationen
oder Anleitungen.

Damit wäre es denkbar, dass ganze Vorgänge Schritt für Schritt beschrieben werden. Dabei könnte die
Hololens auf dem Gerät die Teile hervorheben, die gerade bearbeitet werden müssen (z.B. "Um die
Abdeckung zu entfernen, lösen sie die beleuchteten vier Schrauben mit einem Kreuz-Schraubenzieher")

Eine weitere Anwendung dieses Frameworks wäre, dass die Informationen, die bei einem Smartphone im
Notification Menu vorhanden sind, mit der Hololens über dem Smartphone schweben zu lassen.

### Anforderungen

Nachdem erste Erfahrungen gesammelt werden konnten, wurden folgende Anforderungen an das Framework
und die darauf zu entwickelnde Applikation gestellt.


#### Framework (muss)

* Erkennen und Lesen QR-Code
    * Gerätetyp (mandatory)
    * Geräteid (mandatory)
    * Verbindungsinformationen (IP) (mandatory)
    * statische Zusatzinformationen zum Geräte (optional beliebige Anzahl)
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


#### Framework (kann)

* Darstellen eines Benutzerhandbuchs (pdf, analog Text Information)
    * pdf lokal gespeichert
* Ein-/Ausblenden von Informationen mittels holografischem Menus
* Es kann entschieden werden, ob die Informationen am realen Gerät oder am Hologramm dargestellt
  wird



## Lösungsentwicklung

Hier werden die Erfahrungen und die Schritte beschrieben, die während des Projektes gemacht wurden

### Erste Schritte

Die Hololens ist als Stand-alone Gerät konzipiert. Sie benötigt keine externen Geräte um zu
funktionieren. Das heisst, die Applikationen werden direkt in der Brille ausgeführt und die
Hologramme werden auch von der Brille selber gerendert.
Die Vorteile davon sind:

* Keine zusätzlichen Kosten für weitere Geräte
* Keine Abhängigkeiten und Inkompatibilitäten (PCs, Treiber, Grafikkarten etc.)
* Keine Kabel die von der Brille hängen

Es gibt auch Nachteile:

* Die Brille wiegt schwer nach ein paar Stunden, weil sowohl Rechner, Display als auch Batterie drin
untergebracht ist.
* Die Laufzeit ist limitiert durch die Batterie.
* Die Displays, auf welchen die Hologramme angezeigt werden können, ist klein.

Die ersten Schritte als Anwender mit der Hololens waren erstaunlich einfach und intuitiv. Beim
ersten Verwenden gibt es ein Tutorial, das dem Träger erklärt, wie mit dem neuen Interface
interagiert wird. Als Pointer gibt es im Zentrum des Sichtfelds einen Punkt, Gaze genannt. Mit
diesem Pointer zielt man auf die Objekte, mit welchen man interagieren will.

Als Eingabegerät dient die Hand. Sie erkennt zwei Gesten. Einerseits kann man das Start Menü
aufrufen, indem man die fünf Finger nach oben zusammenhält und danach öffnet wie eine Blume.
Andererseits gibt es den bekannten Klickevent. Für diesen hält man den Zeigefinger wie ermahnend vor
das Gesicht. Der Cursor im Blickfeld ändert daraufhin sein Aussehen um dem Benutzer zu
signalisieren, dass der Finger erkannt wurde. Nun kann der Träger mit dem Zeigefinger auf den Daumen
tippen um ein Klick auszulösen.

Dies funktioniert sehr gut und recht intuitiv.


#### Demoapplikationen

Zu Demonstrationszwecken hat Microsoft bereits einige Applikationen entwickeln lassen. Davon möchten
wir zwei Spiele hervorheben.

1. **RoboRaid**: Dies ist ein Spiel, bei welchem Roboter ein Loch in die Wand brechen und auf den Spieler
   schiessen. Einige krabbeln über die Wand, andere fliegen im Raum herum. Dieses Spiel zeigt schön,
   wie man die reale Umgebung in die Applikation einbinden kann. Die Löcher in der Wand überzeugen.
   Obwohl sich der Spieler in der realen Umgebung bewegt und diese auch wahrnimmt, so ist er dennoch
   in der Virtualität gefangen und kümmert sich kaum um die Umgebung.

2. **Jenga**: Dies ist ein Spiel, bei welchem Holzblöcke zu einem Turm geschichtet werden (je drei
    pro Etage) und die Spieler nacheinander einen Holzblock entfernen und oben auf den Turm legen
    müssen. Dieses Spiel wurde ebenfalls für die Hololens adaptiert. Hier zeigen sich jedoch die
    Limitationen der Virtualität. Man kann zwar die Blöcke mit Gesten bewegen aber es fehlt ein
    haptisches Feedback.


#### Tutorials

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


### Entwicklung "Gerätestatus"

Für die Demonstration des Frameworks haben wir uns für einen Laptop als Gerät entschieden. Der ist
mobil und kann ebenfalls als Webserver dienen. Somit ist kein dediziertes Demogerät und zusätzliche
Infrastruktur notwendig.

Für die Darstellung der Informationen werden folgende Daten benötigt, die wir über die
Webschnittstelle und QR-Code erhalten werden.

- Information die dargestellt werden soll
- Bezeichner der Quelle
- Vektor für die relative Position der Information zur Quelle
- Vektor für die relative Position der Quelle zum Gerät


#### Workflow

Ein Gerät, das mit zusätzlichen Informationen muss sich einmalig bei der Hololens anmelden. Dies
geschieht über einen QR-Code, der mit der Applikation auf der Hololens gescannt werden muss. Mit den
Informationen konfiguriert die Hololens das Gerät, indem es sich merkt, wo es steht und wo es die
zusätzlichen Daten abholen kann.

Wenn sich die Daten dynamisch ändern können (wie Füllstand oder Temperatur oder ähnliches),
holt sich die Hololens periodisch die aktualisierten Daten und stellt sie erneut dar. Wenn es sich
um statische Daten handelt, fällt dies natürlich weg.

#### Datenquelle

Als dynamische Datenquelle haben wir uns für eine REST-Schnittstelle entschieden. Sie

[https://github.com/ledux/pawi-hololens-dummyapi.git]



## Schlussfolgerungen und Ausblick

_--> Notizen TODO: ausformulieren_

- Über die Sicherheit der REST-Schnittstelle haben wir noch keine Gedanken gemacht
- Verschiedene Datenquellen für die Informationen im QR-Code unterbringen
    - http
    - bluetooth
- Update interval im QR-Code
- Ob statisch oder dynamische Daten im QR-Code

## Lessons learned

