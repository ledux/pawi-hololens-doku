
# Anforderungen Hololens

* Framework (muss)
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


* Framework (kann)
    * Darstellen eines Benutzerhandbuchs (pdf, analog Text Information)
        * pdf lokal gespeichert
    * Ein-/Ausblenden von Informationen mittels holografischem Menus
    * Es kann entschieden werden, ob die Informationen am realen Gerät oder am Hologramm dargestellt
      wird

Für die Demonstration des Frameworks haben wir uns für einen Laptop als Gerät entschieden. Der ist
mobil und kann ebenfalls als Webserver dienen. Somit ist kein dediziertes Demogerät und zusätzliche
Infrastruktur notwendig.

Für die Darstellung der Informationen werden folgende Daten benötigt, die wir über die
Webschnittstelle und QR-Code erhalten werden.

- Information die dargestellt werden soll
- Bezeichner der Quelle
- Vektor für die relative Position der Information zur Quelle
- Vektor für die relative Position der Quelle zum Gerät


