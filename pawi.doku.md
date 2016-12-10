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

----------------------------------------------------------------------------------------------------
Table: Definitionen

----------------------------------------------------------------------------------------------------
**Abkürzung**       **Bedeutung**
-----------------   ---------------------------------------------------------------
API                 Application Program Interface, Schnittstelle von Computerprogrammen für
                    Computerprogramme

ReST                Representational State Transfer, Konzept für APIs über http(s)

----------------------------------------------------------------------------------------------------
Table: Abkürzungen

# Ziele und erwartete Resultate

## Einführung

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
dargestellt.

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
untergebracht ist.
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
tippen um ein Klick auszulösen.

Lässt man den Zeigefinger und den Daumen nach dem Tippen zusammen, kann man damit Hologramme
festhalten. Damit kann man je nach Kontext das Hologramm im Raum verschieben oder durch einen langen
Text scrollen.

Dies funktioniert sehr gut und recht intuitiv, solange man sich auf den Cursor konzentriert. Der
verändert sich im Aussehen, je nach dem, mit was die Blickrichtung  kollidiert und ob der
Zeigefinger für den Klick erkannt wurde.


### Demoapplikationen

Zu Demonstrationszwecken hat Microsoft bereits einige Applikationen entwickeln lassen. Davon möchten
wir zwei Spiele hervorheben.

1. **RoboRaid**: Dies ist ein Spiel, bei welchem Roboter ein Loch in die Wand brechen und auf den Spieler
   schiessen. Einige krabbeln über die Wand, andere fliegen im Raum herum. Dieses Spiel zeigt schön,
   wie man die reale Umgebung in die Applikation einbinden kann. Die Löcher in der Wand überzeugen.
   Obwohl sich der Spieler in der realen Umgebung bewegt und diese auch wahrnimmt, so ist er dennoch
   in der Virtualität gefangen und kümmert sich kaum um die Umgebung.

1. **Jenga**: Dies ist ein Spiel, bei welchem Holzblöcke zu einem Turm geschichtet werden (je drei
    pro Etage) und die Spieler nacheinander einen Holzblock entfernen und oben auf den Turm legen
    müssen. Dieses Spiel wurde ebenfalls für die Hololens adaptiert. Hier zeigen sich jedoch die
    Limitationen der Virtualität. Man kann zwar die Blöcke mit Gesten bewegen aber es fehlt ein
    haptisches Feedback. Dadurch wird es schwierig, die Blöcke im  richtigen Winkel aus dem Turm zu
    ziehen.


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
Das Framework besteht aus einem UnityPackage und einer Anleitung wie die einzelnen Komponenten genutzt werden sollen. Zusätzlich wird das Microsoft HoloToolkit UnityPackage benötigt. Dieses ist verfügbar unter [^holotoolkit-unity]:[HoloToolkit-Unity](https://github.com/Microsoft/HoloToolkit-Unity) und enthällt ein Readme.md mit der Installationsanleitung. 

#### Die Assets
Als Assets werden alle Dateien bezeichnet welche in einer Unity App benutzt werden. 
- 3D-Modelle:	Unity-Objekte mit Meshes, Positionierung, Collider und Materialien.
- Scripts:		C# Klassen welche Unity-Objekten angehängt werden können. (Javascript ist auch möglich)
- Materialien:	Oberfläche welche mittels Texturen und Shader auf 3D-Modelle angewendet wird.
- Prefabs:		Eine Gruppierung von Assets welche zur Laufzeit instanziert werden kann.
- Weiter gibt es Bilder, Sprites, Audio, Lichtquellen, Physikmaterialien und Animationen

Im erstellten Framework befinden sich hauptsächlich Scripts und Prefabs.
**DeviceManager.cs**
Der DeviceManager behandelt die Erstellung und das Entfernen von Geräten. ????????????

**DeviceBehaviour.cs**
Jedes Gerät wird durch das DeviceBehaviour Script gesteuert. Es enthällt alle offline Informationen zum Gerät und die URL zur Webschnittstelle. In regelmässigen Abständen, konfigurierbar durch 'PollRateInSec' werden die Informationen abgefragt und dargestellt. 
 
**Device Prefab**
Für jedes neue Gerät muss ein Device Prefab erstellt werden. Es enthällt das 3D-Modell, den Collider und das DeviceBehaviour Skript mit der gewünschten Konfiguration aus DeviceName, DeviceInformationUrl und PollRateInSec. Weiter müssen die TextInformationPrefab und ImageInformationPrefab sowie die Materialien (Normal und Selektiert) für die 3D-Linien gesetzt werden.

**InformationBaseScript.cs**
Um Informationen wie Text oder Bild sinnvoll darzustellen, wird dynamisch eine "3D-Linie" von einem Ort des Gerätes (Anker) zum Ort an welchem die Information dargestellt wird (Ziel) erzeugt. Diese "3D-Linie" besteht aus einer Kugel beim Anker, einem langen Zilinder als Verbindung und einem breiten Zilinder als Podest für die Information. Es ermöglicht die Information mit der Press-Gestik zu verschieben und wechselt das Material der "3D-Linie" falls die Information fokussiert wird.

**TextInformation.cs**
Diese Ableitung des InformationBaseScript ermöglicht es Text mittels SetText darzustellen.

**TextInformation Prefab**
Damit der Benutzer den Text sehen kann sind in diesem Prefab nebst dem TextInformation Scripts mehrere Assets nötig. Ein Billboard Script, aus dem HoloToolkit, richtet das Objekt relativ zum Blickwinkel der Kamera aus. Ein BoxCollider wird verwendet damit registriert werden kann ob der Benutzer das Objekt fokussiert. Der Kollider passt sich mittels dem InformationBaseScript dem Inhalt des Prefabs an. Auch die Unity Scripts HorizontalLayerGroup und ContentSizeFitter werden benötigt damit sich die Grösse dynamisch dem Inhalt anpasst. Hierarchisch enthällt das Prefab die Unity UI Objekte Canvas, Panel und Text. Das Panel besitzt ein Image Script ohne Bild aber mit der Farbe blau um einen Hintergrund für den Text zu haben. Ein Padding im HorizontalLayerGroup des Panels erleichter die Lesbarkeit des Textes. Auf der untersten Ebene befindet sich das Text Objekt welches vom TextInformation Script aktualisiert wird.

**ImageInformation.cs**
Vergleichbar mit dem TextInformation Script wird stattdessen ein Bild durch SetImage gesetzt. Zusätzlich wird die Grösse des Prefabs der Grösse des Bildes Angepasst. Dies war nötig da der ContentSizeFitter nicht wie bei dem Text objekt funktioniert hat.

**ImageInformation Prefab**
Dieses Prefab gleicht dem TextInformation Prefab bis auf zwei Änderungen. Der ContentSizeFitter wird nicht verwendet und es wird das Text Objekt mit RawImage ersetzt. 


### QR-Code

Der QR-Code ist sozusagen der Einstiegspunkt für die Applikation. Darauf ist die Datenquelle
vermerkt, wo sich die Applikation die Informationen für das entsprechende Gerät abholen kann. In
unserem Fall ist dies eine URL zu einer ReST-Schnittstelle.

Je nach Version ist die Kapazität auf einem QR-Code sehr limitiert[^qr-capacity].

[^qr-capacity]:[Kapazitäten von QR-Codes](http://www.qrcode.com/en/about/version.html)

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



# Schlussfolgerungen und Ausblick

Da diese Arbeit nur ein Prototyp ist und höchstens als proof of concept gelten kann, gibt es
einiges, das wir ausgespart haben.

## Sicherheit

Die Informationen, die die Hololens über die Geräte darstellt, sind potentiell kritisch und dürfen
nicht in jedem Fall frei verfügbar sein. In unserem Szenario haben wir diesen Aspekt völlig
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
bluetooth auf der Hand.

Diese Art und Weise, wie die Applikation zu seinen Daten kommt, müsste auch noch im QR-Code
untergebracht werden.

_--> Notizen TODO: ausformulieren_

- Update interval im QR-Code
- Ob statisch oder dynamische Daten im QR-Code

# Lessons learned

