# So laut ist der Verkehr in der Stadt St.Gallen
Auswertung des Lärmbelastungskataster des Kantons St.Gallen (Layers Lärmemission und Lärmbelastung) in Bezug auf die Stadt St.Gallen


## Inhalt dieses Read.me
* Ziel
* Ausgangsthese
* Vorgenommene Analysen
* Quellen
* Gespräch mit Briefingperson
* Einschätzung von Aufwand und Ertrag vor Beginn des Projekts
* Stolpersteine
* Programmcode
* Arbeitsprotokoll
* Ergebnis
* Dank


## Ziel
In meiner Arbeit analysiere ich Daten aus dem Lärmbelastungskataster des Kantons. Ziel ist es aufzuzeigen, wo der Verkehrslärm wie laut ist.

Dies ist einerseits auf persönlicher Ebene der Leserinnen und Leser relevant:
* Sie erhalten dadurch niederschwellig Zugang zu Informationen. (Der Lärmbelastungskataster dürfte nicht allzu bekannt sein.) Dadurch können sie sich darüber informieren, ob sie theoretisch Anrecht auf Lärmschutzmassnahmen haben.
* Weiter können sie z.B. in Falle der Wohnungssuche prüfen, wie laut es an einer Stelle in etwa ist.

Andererseits ist die Analyse auf politischer Ebene relevant. Der Bund hat die Frist für Lärmsanierungen bis 2024 verlängert (und dürfte die Frist nochmals verlängern). Mit meiner Auswertung zeige ich auf, wie viele Häuser zu viel Strassenlärm ausgesetzt sind. Zwar stammen die aktuellsten Daten von 2018, jene für die Stadt St.Gallen gar von 2017. Doch geben sie eine Vorstellung davon, wo die Stadt St.Gallen in Bezug auf Lärmsanierungen steht.

In Bezug auf meine Artikel möchte ich also, dass mein Publikum ...
* nachschlagen kann, wie laut es an welchen Orten ist (durchsuchbare Tabelle)
* wo die Lärm-Hotspots in der Stadt St.Gallen sind (Karte)
* nachschlagen kann, wo die Grenzwerte überschritten werden (durchsuchbare Tabelle)
* verstehen, welchen Effekt die verschiedenen Lärmschutzmassnahmen haben (Interview, Lauftext)
* erfahren, wo die Stadt St.Gallen in Bezug auf Lärmsanierungen steht und was sie und der Kanton diesbezüglich tun (Interview, Lauftext)


## Ausgangsthese
In der Stadt St.Gallen gibt es Häuser, deren Bewohner:innen zu viel Verkehrslärm ausgesetzt sind, obwohl eine Lärmsanierung notwendig wäre. Diese Häuser dürften an den Hauptachsen liegen.


## Vorgenommene Analysen
Im Zuge meines Projekts habe ich zwei Datensätze ausgewertet:

1. Jenen zu den **Lärmemissionen**. Die in diesem Datensatz enthaltenen Dezibelwerte beziehen sich auf den Verkehrslärm am Strassenrand. Die Werte werden aufgrund des Verkehrsaufkommens mit Modellen errechnet.
2. Jenen zur **Lärmbelastung**. Diese Dezibelwerte beziehen sich auf Punkte an der Fassade. Sie werden mit Hilfe der Dezibelwerte im Datensatz Lärmemissionen errechnet.


## Quellen
### Lärmemissionen
Die Daten zu den Lärmemissionen habe ich über den [Web Feature Service](https://metadata.geo.sg.ch/geo_records/217)(WFS) des Kantons St.Gallen abgefragt. Der Datensatz ist auch auf dem kantonalen Open-Data-Portal in einer teilweise [aufbereiteten Form](https://daten.sg.ch/explore/dataset/larmemission-kanton-stgallen/table/?disjunctive.street_nam&disjunctive.emodel_str&disjunctive.belaggkorr&disjunctive.vsignt_str&disjunctive.vsignn_str&disjunctive.tunnel&disjunctive.bridge&disjunctive.emiss_tram) abrufbar.  Die darin enthaltenen Geodaten konnte ich in der Form aber nicht mit Geopandas lesen. Daher beschloss ich, den Datensatz via WFS abzufragen.

Später zeigte sich, dass sich Geopandas für meine Auswertung gar nicht eignet und ich auf den Multilines (den Strassenzügen) Punkte setzen muss (siehe *Stolpersteine*). Zu diesem Zeitpunkt waren meine Auswertungen aber schon so weit fortgeschritten, dass ich beschloss, für diesen Datensatz weiterhin nicht auf das Open-Data-Portal zurückzugreifen.


### Lärmbelastung
Die Daten zur Lärmbelastung lagen auch als shp-file vor. Jedoch konnte ich für diese Auswertung abermals nicht auf Geopandas zurückgreifen. Stattdessen musste ich auch für diese Analyse aus den Geodaten (geometry) die Koordinaten ziehen. Ich beschloss also, dieses Mal die Daten aus dem [Open-Data-Portal](https://daten.sg.ch/explore/dataset/larmbelastung-kanton-stgallen/information/?disjunctive.es&disjunctive.comm_use_d&disjunctive.exp_lim&disjunctive.exp_lim_d&disjunctive.exp_lim_n) zu verwenden


### shp-files
Bevor ich definitiv zum Schluss kam, dass Geopandas mich nicht weiterbringt, hatte ich die Daten zu den Lärmemissionen auf diverse Karten der Stadt geplottet. Dazu verwendete ich den [Gemeindenstrassenplan](https://daten.sg.ch/explore/dataset/gemeindestrassenplan%40stadt-stgallen/information/?disjunctive.strassenkl&disjunctive.strassenna&disjunctive.strassennr&location=12,47.42403,9.36333&basemap=jawg.streets), die Karte der [PLZ](https://daten.stadt.sg.ch/explore/dataset/plz_verzeichnis/information/?disjunctive.postleitzahl) sowie den Datensatz für die Schweiz von [Open Street Map](http://download.geofabrik.de/europe/switzerland.html).


## Gespräch mit Briefingperson
**Andreas Kästli, Fachstelle Immissionen Mobilität und Planung beim Kanton St.Gallen**

*Was muss ich bei einer solchen Auswertung beachten?*

Je näher ein Gebäude an der Strasse steht, umso eher wird der Grenzwert überschritten. Ist eine Häuserzeile nahe an der Strasse, kann man davon ausgehen, dass alle viel Lärm ausgesetzt sind.

Das ganze Kataster ist ein 3D-Model. Mit einem Tool nehmen wir die Werte von den Verkehrszählungen und berechnen den Lärm. Dabei wird dieser für einen Punkt an der Fassade berechnet. Hat es an diesem Punkt z.B. kein Fenster, weicht der berechnete Wert vom tatsächlichen Wert ab. Es ist also kein ganz genaues Instrument, sondern gibt Grössenangaben an.

Auch die Verkehrszahlen, die hinter den Berechnungen stehen, sind nicht überall im Kanton ganz genau. Die Abweichung kann plus/minus 20 Prozent betragen. Das scheint eine grosse Abweichung, aber auf die Lärmberechnungen hat sie keine so grosse Auswirkung: Es braucht doppelt so grosses Verkehrsaufkommen, damit der Lärm um drei Dezibel steigt.

*Welche Wirkung haben die verschiedenen Lärmsanierungsmassnahmen?*

Ein Schallschutzfenster kann den Lärm um bis zu 15 Dezibel senken. Aber hier kommt es drauf an, was für Fenster man vorher drin hatte. Waren es moderne Scheiben dann sinkt der Lärm mit den Schallschutzfenstern vielleicht nur noch um 4,5 Dezibel.
Bei Lärmschutzwänden kommt es drauf an, wie viel man noch von der Strasse sieht. Umso höher die Wand, umso besser. Dann kann sie den Lärm um bis zu 10 Dezibel senken. Wenn man die Auto noch sieht, wirkt sie noch um 5 Dezibel. Sieht man auch die Strasse wirkt sie kaum noch.

Lärmarme Strassenbeläge senken den Lärm anfangs um 6 bis 7 Dezibel. Aber sie verlieren schnell ihre Wirkung, weil die Hohlräume verschmutzen. Auch normale Beläge werden durch diese Abnutzung lauter. Extrem lärmarme Strassenbeläge senken den Lärm anfangs um 8 Dezibel, aber weil ihre Lebensdauer kurz ist, bauen wir sie nicht ein. Einer der nur um 3 bis 5 Dezibel wirkt hat eine längere Lebensdauer.

Wenn man einen neuen Belag ausprobiert, dauert es immer 10 Jahre, bis man ein Ergebnis hat. Bis man die Lebensdauer weiss, wie er sich abnutzt und wie gut er wirkt.

In letzter Zeit und auch wegen Bundesgerichtsurteilen sind auch Geschwindigkeitsbegrenzungen ein Thema. Senkt man das Tempo von 50 auf 30 Kilometer die Stunde gewinnt man 3 Dezibel. Aber wenn die Autos auf der mit 50 signalisierten Strecke schon 40 fahren, weil schneller nicht geht, dann ist die Wirkung durch die Reduktion auf 30 Stundenkilometer nicht mehr so gross. Man gewinnt nur noch 1 Dezibel.


## Einschätzung von Aufwand und Ertrag vor Beginn des Projekts
### Aufwand
Das Auslesen der Daten an sich dürfte einfach sein. Das Problem wird sein, dass ich Daten für den ganzen Kanton habe, nicht aber nur für jene zur Stadt St.Gallen interessiere. Eine Filtermöglichkeit gibt es nicht, weil in den Daten zu den Lärmemissionen vom WFS die Angaben zur Gemeinde oder Postleitzahl fehlen.

Ich muss also entweder eine Lösung finden, dies automatisiert zu tun (schwierig), oder die Lärm-Hotspots von Auge ablesen (noch schwieriger bis unmöglich). Wenn ich die Dezibelwerte auf Strassenstücke bzw. auf einzelne Häuser zurückführen möchte, muss ich auf jeden Fall eine Möglichkeit finden, die Koordinaten rauszufiltern und auf die Adressen zurückzuführen.

Die Analyse danach dürfte einfach werden. Bei der Darstellung für den Online-Artikel dürfte es auf eine Datawrapperkarte hinauslaufen, in der ich die Lärmzonen mit Polygonen einzeichne. Wie genau ich das mache bzw. welche Daten ich dafür benötige, muss ich vor Bereinigung und Analyse klären.

Alles in allem denke ich, der Zeitrahmen von fünf Tagen könnte knapp werden. Zumal ich mit Rückschlägen rechnen muss.


### Ertrag
Auf einer Skala von 0 (= keinen) bis 5 (= maximum):

* Journalistischer Impact: 4
Eventuell nimmt eine Politikerin oder ein Politiker das Thema in einer Form auf, da es viele Leute betrifft. Ausserdem dürfte diese Auswertung die Glaubwürdigkeit des "Tagblatts" stärken und Leser:innen vom Wert des Abos überzeugen bzw. zu einem Kauf bewegen.

* Mögliche Reichweite: 4
Weil es viele Leute betrifft (Stadtbevölkerung und jene, die in St.Gallen arbeiten), dürfte die Analyse auf grosses Interesse stossen.

* Knowhow-Aufbau: 5
Ich werde sehr viele erworbenen Fähigkeiten einsetzen und sicher auch neues Wissen aufbauen, das mir auch in Zukunft von Nutzen sein wird.

* Mehrmaliger Nutzen des Codes: 5
Für die Städte Wil, Gossau und Rorschach könnte ich dieselbe Auswertung machen. Ich könnte sie zudem mit demselben Code für den ganzen Kanton machen. Die Daten werden ausserdem von Gesetzes wegen alle fünf Jahre aktualisiert. Die nächste Aktualisierung steht 2023 an. Das bietet mir die Möglichkeit, dieselbe Analyse nochmals vorzunehmen um festzustellen, wie viel sich getan hat.

* Innovation: 3
Eine solche Auswertung hat beim "Tagblatt" noch nie jemand gemacht. Die Idee ist aber nicht ganz neu.


## Stolpersteine
Wie es in der Natur eines solchen Projektes liegt, begegnete ich etlichen Hindernissen. Die wichtigsten liste ich hier auf.

### Setzen eines Geo-Punktes
Sehr früh hatte sich gezeigt, dass ich für die Auswertung - besonders für die Filterung der Daten - alle Datenpunkte mit den Koordinaten ergänzen musste. Im  Datensatz Lärmemission bezogen sich die Datenpunkte aber auf mehr oder weniger lange Strassen(stücke).

Da diese nicht alle aus gleich vielen Koordinatenpaaren bestanden, fand ich keine Möglichkeit, diese Tatsache zu berücksichtigen. Stattdessen entschied ich mich für ein Koordinatenpaar auf den "Multilines". Später zeigte sich, dass der Verlust (zum Glück) weniger gross war als befürchtet.

### Umrechnen der Koordinaten von EPSG2056 (LV95) zu EPSG4326 (WSG84)
Die Koordinaten lagen im Schweizer Koordinatensystem vor. Für die Adressabfrage musste ich sie aber ins System WSG84 überführen. Es brauchte einige Anläufe und viele Googleabfragen bis ich eine Lösung gefunden hatte.

### Nominatim
Ich fragte mit den Koordinaten der Lärmemissionen Nominatim, die API von Open Street Map ab. Dies führte auf mehreren Ebenen zu Problemen:
* Die Daten in Nominatim sind extrem lückenhaft. Stellenweise konnte ich für Koordinaten weder Strasse, Hausnummer, PLZ noch Gemeinde abfragen. Mir war bewusst, dass die Angaben auf Open Street Maps lückenhaft- und teilweise fehlerhaft sind. Das Ausmass der Lücken überraschte mich aber trotzdem.
* Nominatim ist nicht für grössere Abfragen geeignet. Abfragen dauern entsprechend. Ab einer gewissen Zahl sind gar keine Abrufe mehr möglich. Hier hätte ich mich zu Beginn vertiefter in die Dokumentation einlesen müssen.

Für die Koordinaten zur Lärmbelastung versuchte ich es nochmals mit Nominatim, da es sich um einen kleineren Datensatz handelte. Doch wichen die daraus gewonnenen Adressen teilweise um mehrere Strassenzüge von der tatsächlichen Adresse ab.

-> Ich wich daher für beide Datensätze auf die [Geocoding API](https://developers.google.com/maps/documentation/geocoding/overview) von Google aus. Diese ist zwar kostenpflichtig, aber verlässlich.


## Programmcode
Die Codes habe ich in sechs Jupyter-Notebooks auf diesem Repository hinterlegt. Die ersten fünf beinhalten die Codes für die **Auswertung Lärmemissionen**, das letzte die Codes für die **Auswertung Lärmbelastung**.

In den Zip-Ordner **Daten** und **Ergebnisse** sind alle für den Code nötigen Files abgelegt. Darunter auch Zwischenergebnisse, sodass es zum Beispiel nicht nötig ist, die Google API abzufragen, um die Analyse nachvollziehen zu können.

Da die Datenmenge sehr gross war, sind für das Ausführen des Codes mit allen Dateien folgende Schritte nötig:

1. Alle Notebooks und die beiden Zip-Ordner lokal abspeichern
2. Die Zip-Ordner entpacken
3. Den Ordner "Ergebnisse" in den Ordner "Daten" verschieben

Wenn Sie Anmerkungen, Verbesserungsvorschläge oder Ideen haben, freue ich mich über eine [Rückmeldung](marlen.haemmerli@chmedia.ch)


## Arbeitsprotokoll

|   Wann                             |   Was                                                                                                                         |   Zeitaufwand (in Stunden)  |
|:------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|:-----------------------------|
|   2. Dezember                      |   Daten sichten, oberflächlich analysieren und Probleme grob identifzieren                                                    |   2                         |
|   7. Dezember                      |   Fragen an Experten und Datenlieferanten festlegen, Recherche zu Experten                                                    |   0.5                       |
|   8. Dezember                      |   Gespräch mit Briefingperson und Person von Open-Data-St.Gallen                                                              |   0.75                      |
|   9. Dezember                      |   Einlesen in Funktion von WFS-Diensten, Suche nach Python-Library und erster Testlauf                                        |   1                         |
|   10. Dezember                     |   Zeitplan aufstellen, Briefing verschriftlichen                                                                              |   0.5                       |
|   11. Dezember                     |   Datensätze analysieren, festgestellt, das Funktion nicht alle Daten holt, versucht Problem zu lösen, erfolglos              |   3                         |
|   12. Dezember                     |   Versuche, Geo Point zu extrahieren, geschafft,dann versucht, Adressen aus Geo Points extrahiert                             |   2                         |
|   13. Dezember                     |   Daten aus WFS weiter verarbeitet.   Versucht, Koordinaten umzurechnen.                                                      |   1.5                       |
|   14. Dezember                     |   Koordinaten umgerechnet,       daran gearbeitet, die Adressdaten über die Koordinaten auszulesen                            |   8                         |
|   15. Dezember                     |   Adressen fertig ausgelesen, Bugs entdeckt, versucht diese zu lösen                                                          |   6                         |
|   17. Dezember                     |   Problem der Bugs gelöst, alle Adressen ausgelesen                                                                           |   5                         |
|   19. Dezember                     |   Fehlerhafte Addressdaten bereinigt                                                                                          |   1                         |
|   20. Dezember                     |   Erste Auswertungen gemacht und weitere geplant                                                                              |   2                         |
|   22. Dezember                     |   Weitere Auswertungen gemacht                                                                                                |   1                         |
|   24. Dezember                     |   Problem mit Kartenplot gelöst                                                                                               |   2                         |
|   27. Dezember                     |   Weitere Auswertungen im Datensatz Lärmemission gemacht, wäre Datenbank sinnvoll?                                            |   1                         |
|   29. Dezember                     |   Den Datensatz Lärmbelastung nach demselben Prinzip bereinigt und angefangen, auszuwerten                                    |   3                         |
|   2. Januar                        |   Letze Auswertungen in beiden Datensätzen, interessante Ergebnisse notiert, Doppelte Datenreihen bemerkt und Lösung gesucht  |   3                         |
|   5. Januar                        |   Storytelling-Besprechung mit Alexandra Stark                                                                                |   0.5                       |
|   7. Januar                        |   Weiter an den Doppelten gearbeitet und Notebook „aufgeräumt“                                                                |   2                         |
|   8. Januar                        |   Problem mit den Doppelten weiter gelöst                                                                                     |   1                         |
|   9. Januar                        |   Problem mit den Doppelten bei den Lärmemission fertig gelöst.                                                               |   1                         |
|   10. + 12. Januar                 |   Erster Artikel geschrieben und Grafiken erstellt                                                                            |   4                         |
|   16. Januar                       |   Fehler bei Adressen und Doppelte in Datensatz Lärmbelastung bemerkt, beide Fehler behoben                                   |   3                         |
|   17. Januar                       |   Interview geführt für zweiten Artikel, Grafiken erstellt                                                                    |   3                         |
|   18. Januar                       |   Zweites Interview für zweiten Artikel geführt, Bilder für zweiten Artikel bestellt.                                         |   0.75                      |
|   19. Januar                       |   Drittes Interview für den zweiten Artikel geführt, Artikel geschrieben, Grafik für Print bestellt.                          |   4                         |
|   21. Januar                       |   Beide Artikel nach Rückmeldungen fertig gestellt gestellt.                                                                          |   0.5                       |
|   TOTAL                            |                                                                                                                               |   63                        |
|   In Arbeitstagen von 8,5 Stunden  |                                                                                                                               |   7.41176470588235          |



## Ergebnis
[Erster Artikel: Lärmemission](https://www.tagblatt.ch/laermschutz-wie-ein-rasenmaeher-an-diesen-zwoelf-orten-droehnt-der-verkehr-in-der-stadt-stgallen-am-lautesten-so-sieht-es-an-ihrer-strasse-aus-ld.2236753)

[Zweiter Artikel: Lärmbelastung](https://www.tagblatt.ch/laerm-ueber-70-dezibel-diese-haeuser-in-der-stadt-stgallen-sind-dem-meisten-strassenlaerm-ausgesetzt-ld.2232059)

-> Für die Stadt Wil habe ich bereits eine Auswertung zur Lärmbelastung vorgenommen und der zuständigen Redaktorin zugestellt. Der Artikel dürfte im Verlaufe des Februars erscheinen.



### Mögliche Nachzüge
* Interview, was macht Lärm mit uns?
* Feature von der lautesten Strasse in der Stadt St.Gallen
* Porträt einer/der Person, die im Haus mit der höchsten Lärmbelastung wohnt
* Dieselbe Analyse für weitere Städte im Kanton St.Gallen oder anderen Kantonen (die Daten müssen gesetzlich erhoben werden, liegen also überall vor, die Frage ist in welcher Form)


#### Dank
Ich danke Barnaby Skinner, Simon Schmid, Thomas Ebermann und Alexandra Stark für die gute Einführung in die Welt des Datenjournalismus mit all ihren Stolpersteinen.

Ich danke meinen Vorgesetzten, dass sie mir die Weiterbildung ermöglichten, mir Zeit gaben und in mich vertrauten.

Ich danke meinen Teamkolleginnen und -kollegen fürs Mitdenken, wenn ich den Lärm vor Daten nicht mehr sah.
