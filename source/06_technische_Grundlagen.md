# Technische Grundlagen 

## Multi-Tenant Architektur 

Multi-Tenant ist ein Architekturkonzept in der Softwareentwicklung. Dieses sieht vor, dass die Anwendung nicht für jeden Nutzer in einer eigenen Instanz ausgespielt wird, sondern alle Nutzer auf der gleichen Plattform arbeiten können.

Das Konzept besteht bereits seit den späten 90er Jahren und fand in den letzten Jahren im Zusammenhang mit der immer mehr voranschreitenden Digitalisierung wieder deutlich mehr Aufmerksamkeit. Dies ist zum einen darin begründet, dass die Anzahl an Software- sowie Webanwendungen massiv zugenommen hat und zum anderen, durch den Fortschritt der Cloud Computing Technologie, neue Möglichkeiten für dieses Architekturkonzept entstanden.
Dabei gibt es im Cloud Computing drei verschiedene Arten:

- _Infrastructure as a Service (IaaS):_ Bereitstellung der notwendigen Ressourcen wie beispielsweise Serverkapazitäten, Datenspeicher etc. die Entwickler, abgestimmt auf ihre Bedürfnisse, nutzen können. Die Ressourcen müssen selbständig verwaltet werden.
- _Platform as a Service (PaaS):_ Hierbei kümmert sich der Cloudanbieter um die Verwaltung sowie Bereitstellung der Ressourcen. Die Entwickler müssen sich lediglich um die Anwendungsprogrammierung kümmern.
- _Software as a Service (SaaS):_ Dieser Service ist in der Regel für den Endnutzer gedacht. Es wird lediglich die fertig Anwendung für den Nutzer bereitgestellt, welcher sich weder um die Ressourcen, noch um die Entwicklung der Anwendung kümmern muss.

Letztgenannte Form findet man in Verbindung mit Multi-Tenant Architekturen recht häufig, da SaaS von Multi-Tenancy sehr starkt profitieren kann. 
Nachfolgende Grafik zeigt eine 

![Abbildung 1: Gegenüberstellung Single- und Multi-Tenant Architektur \label{Krass}](source/figures/SingleVsMulti-Tenant.png) { width=50% }
Abbildung 1

Und immer weiter.


## SQL 

SQL ist eine deskriptive Abfragesprache und steht für *Structured Query Language* [@MeierKaufmann2016 7]. Diese wird für die Abfrage von relationalen Datenbanksystemen verwendet, welche in Tabellen organisiert sind. Somit ist es in der Praxis gebräuchlich, von einer SQL-Datenbank zu sprechen, wenn sich dahinter ein relationales Datenbanksystem handelt.

### Relationales Datenbanksystem
[@MeierKaufmann2016 10].

Ein relationales Datenbanksystem besteht aus der Datenbank an sich sowie einem Datenbankmanagementsystem (DBMS). Dabei umfasst die Datenbank eine Sammlung aller Tabellen mit deren Beziehungen untereinander. Mithilfe des Datenbankmanagementsystem können dann An- und Abfragen an die Datenbank realisiert werden.
Dabei gibt es verschiedene Datenbankmanagementsystem, welche aber bei relationalen Datenbanken allesamt SQL als Abfragesprache nutzen.
Teilweise unterscheiden sich die in den Datenbankmanagementsystemen angewandten SQL Abfragen allerdings in der Syntax.
Jedes Datenbankmanagementsystem erfüllt dabei einige Aufgaben, die jedes DBMS wiederum anderweitig realisieren kann. Die wichtigsten Anforderungen werden nachfolgend kurz vorgestellt. [@Adams2020, 5-6]
WIE AM BESTEN ZITIEREN? (DIREKT/INDIREKT)
„
- Sprachinterpreter: Übersetzt die Befehle in einen ausführbaren Code.
- Optimierer: Ermöglicht schnelle Ausführung der Befehle.
- Datenschutz: Schreib- und Lesezugriffe an den jeweiligen Nutzer richtig vergeben.
- Datensicherheit: Verhindern von Datenverlusten.
- Application Programming Interface (API): Dient der Anwendung zum Zugriff auf das DBMS.

“

Je nach Datenbankmanagemensystem können sich die Aufgaben beziehungsweise Anforderungen an das DBMS unterscheiden. Inzwischen gibt es sehr viele DBMS, wovon die gebräuchlisten kurz aufgezählt werden. [@Adams2020, 7]

- Oracle
- MySQL
- Microsoft SQL Server
- MariaDB

Die Wahl auf des bevorzugten DBMS hängt dabei vom Anwendungsfall sowie den Anforderungen an das System ab.


**Eigenschaften**

Ein relationales Datenbanksystem ist durch ein relationales Datenbankmodell abgebildet, welches mittels SQL abgefragt und modifiziert werden kann. Dabei gewährleistet das gesamte System zusätzlich folgende Eigenschaften [@MeierKaufmann2016 10-11]:

- _Datenunabhängigkeit_: Die „Daten und Anwendungsprogramme bleiben weitgehend voneinander getrennt“, was durch die Entkoppelung des Datenbankmanagementsystems zur physischen Datenbank realisiert wird. Dadurch können, abhängig von der Anwendungsarchitektur, Änderungen an der Datenbank, ohne Einfluss auf die Anwendung, vorgenommen werden.
- _Mehrbenutzerbetrieb_: Durch diesen ist gewährleistet, dass „mehrere Benutzer gleichzeitig ein und dieselbe Datenbank abfragen oder bearbeiten“ können. Dabei stellt das System sicher, dass alle durch die Benutzer vorgenommen Aktionen korrekt verarbeitet werden
- _Konsistenzgewährung_: Das Datenbanksystem unterstützt das Erfüllen der Datenintegrität, womit „die fehlerfreie und korrekte Speicherung der Daten“ gewährleistet ist.
- _Datensicherheit und Datenschutz_: Mithilfe des Datenbanksystems werden „Daten vor Zerstörung, vor Verlust und vor unbefugten Zugriff“ geschützt. 


### Relationale Datenbankmodelle
Relationale Datenbankmodelle zeichnen sich dadurch aus, dass sie vollständig in Tabellen und Spalten dargestellt werden.
Jede Tabelle enthält einen eindeutigen Bezeichner, den Primärschlüssel. Anhand dieses Schlüssel kann jeder in der Tabelle enthaltene Datensatz eindeutig indentifiziert werden. In jeder Tabelle sind Attribute enthalten, welche die Eigenschaften einer Tabelle darstellen. An nachfolgender MySQL Angabe die eine beispielhafte Tabelle samt ihrer Attributte erstellt, wird dies verdeutlicht.

_MySQL_
```
CREATE TABLE teams (
    id int NOT NULL,
    name varchar(255),
    shortname varchar(255),
    description varchar(255),
    departmentId int,
    PRIMARY KEY (id)
);
```
Die Attribute der Tabelle sind demnach,bei Ausführung der Angabe, „id“ (= festgelegter Primärschlüssel), „name“, „shortname“ und „description“. Das  NOT NULL Statement sorgt dafür, dass das Attribut „id“ niemals NULL werden kann.
Dies ist gerade bei Schlüsselattributen wichtig, damit dieses auch wirklich gesetzt werden und als solches agieren kann.
Zudem ist es in relationalen Datenbankmodellen notwendig, dass Fremdschlüsselbeziehungen gesetzt werden. Dadurch werden Tabellenbeziehungen über Attribute hergestellt. Diese sind in mehreren Tabellen als Fremdschlüsselattribute gesetzt und referenzieren dabei von einer Tabelle zu einer anderen. 
Die Angabe des Fremdschüssel mit der zugehörigen Referenz muss dann auch in der SQL Ausführung angegeben beziehungsweise ergänzt werden.

_MySQL_
```
FOREIGN KEY (departmentId) REFERENCES departments(departmentId)
```
Mit dieser Angabe wird das Attribut „departmentId“ als Fremdschlüssel deklariert und referenziert dabei auf das Attributt „departmentId“ in der “departments“ Tabelle. 

Zum Vergleich werden die beiden SQL Anfragen jeweils in Oracle geschrieben.

_Oracle_
```
CREATE TABLE teams (
    id int NOT NULL PRIMARY KEY,
    name varchar(255),
    shortname varchar(255),
    description varchar(255),
    departmentId int FOREIGN KEY REFERENCES departments(departmentId),
);
```

Selbstverständlich hätte bei MySQL der Befehl für den Fremschlüssel genau so in der ersten CREATE TABLE Anweisung stehen können.
Dies soll hervorheben, dass zwar alle DBMS auf SQL als Abfragesprache beruhen, es allerdings, abhängig vom DBMS, unterschiedliche Syntax gibt.

### Datenmodellierung

Bevor ein relationes Datenbanksystem aufgesetzt wird, muss ein Datenmodell entworfen werden um redudante Daten zu verhindern sowie Abhängkeiten zwischen den Tabellen inklusive aller notwendigen Attributen und Datentypen zu berücksichtigen.
„Ein Merkmal einer Tabelle ist redundant, wenn einzelne Werte dieses Merkmals innerhalb der Tabelle ohne Informationsverlust weggelassen werden können.“ [@MeierKaufmann2016 36]
Um solche Redundanzen und Datenanomalien zu vermeiden werden Datenmodelle auf Basis der Normalformen erstellt. In der Praxis finden dabei am häufigsten die erste bis dritte Normalform Anwendung. Auf die Normalformen vier und fünf wird nicht eingegangen, da damit einhergehende „[...] Mehrwert- oder Verbundabhängigkeiten kaum in in Erscheinung treten [...]“ [@MeierKaufmann2016 38].

**Erste Normalform (1. NF)**

[@MeierKaufmann2016 39].
Die erste Normalfrom ist erfüllt, wenn alle Tabellenfelder atomare Werte vorweisen. Bedeutet in jedem Tabellenfeld darf exakt ein Wert vorkommen; „Mengen, Aufzählungen oder Wiederholungsgruppen“ [@MeierKaufmann2016 39] sind nicht zulässig.

**Zweite Normalform (2. NF)**

[@MeierKaufmann2016 39].
Voraussetzung für die zweite Normalform ist, dass die 1. NF bereits erfüllt ist. Zudemm muss „jedes Nichtschlüsselmerkmal von jedem Schlüssel voll funktional abhängig“ [@MeierKaufmann2016 39] sein. Dies bedeutet, dass ein Wert einen anderen Wert exakt bestimmt.
Wird ein Nichtschlüsselattribut von einem zusammengesetzten Schlüssel bestimmt, so darf nur allein der zusammengesetzte Schlüssel das Nichtschlüsselattribut bestimmen.

**Dritte Normalform (3. NF)**

[@MeierKaufmann2016 42-43]
Wiederum die erste Bedingung für die dritte Normalform ist die Erfüllung der 2. NF. Des weiteren muss gewährleistet sein, dass „kein Nichtschlüsselmerkmal von irgendeinem Schlüssel transitiv abhängig ist“ [@MeierKaufmann2016 42].
Eine transitive Abhängigkeit liegt dann vor wenn Nichtschlüsselattribute voneinander „[...] über Umwege funktional abhängig [sind]" [@MeierKaufmann2016 42].

Die Vermeidung von Redundanzen und Anomalien ist daher wichtig um eine performante Datenbank zu implementieren. 
[@Adams2020 39].
Doppelt vorhandende Daten kosten unnötigen Speicherplatz welcher zum einen die Arbeitsgeschwindigkeit des Datenbanksystems negativ beeinträchtigen kann und zum anderen mehr Speicherplatz in Anspruch nimmt, was sich wiederum auf die monetären Kosten einer Datenbank auswirkt. Des weiteren sind bei redudant vorhandenen Daten „teuer, da diese in vielen Zeilen durchgeführt werden müssen“ [@Adams2020 39].


## NoSQL 

[@MeierKaufmann2016 222].
Mit NoSQL werden alle Datenbank bezeichnet, „die über keinen SQL-Zugang verfügen“ [@MeierKaufmann2016 222]. Des weiteren werden bei NoSQL-Datenbanken keine Tabellen zur Speicherung der Daten verwendet.
Trotzalledem stößt man in der Praxis häufig auf „Not only SQL“, wenn von einer NoSQL Datenbank die Rede ist. Dies hat den Hintergrund, dass Anwendungen unter dem Konzept der „mehrsprachigen Persistenz“ [@MeierKaufmann2016 222] entwickelt werden, was bedeutet, dass SQL- in Verbindung NoSQL Datenbanken genutzt werden.

NoSQL Datenbanken zeichnen sich dadurch aus, dass sin in der Regel als verteiltes System implementiert werden. Des weiteren geben NoSQL Datenbanken keine bestimmte Datenstruktur vor, wie es bei relationalen Datenbanken der Fall ist. 
Die Notwendigkeit zur Nutzung von nicht relationalen Datenbanken ist aufgrund der in den letzten Jahren vorangeschrittenen Digitalisierung begründet. Mit dieser ging auch ein starkes Ansteigen der zu verarbeitenden Datenmengen, vorwiegend im Webbereich, einher. Aufgrund dieses höheren Datenaufkommen mussten Datenbankalternativen ausgemacht werden um die Daten schneller sowie effektiver speichern und abfragen zu können.
Vor allen Dingen durch das damit entstandene Themegebiet „Big Data“ gewannen NoSQL Lösungen immer mehr an Bedeutung.
[@MeierKaufmann2016 13].
Die Definition von Big Data beruht auf der Erfüllung der „3 V's“, welche erfüllt sein müssen damit Datensammlungen als Big Data angesehen werden.

- _Volume_: Die Größe der Datenbestände wird mindestens auf den Terabytebereich beziffert.
- _Variety_: Hierunter wird verstanden, dass die Daten sowohl geordnet, als auch ungeordnet vorliegen können.
- _Velocity_: Die Daten müssen „in Echtzeit ausgewertet und analysiert werden können“ [@MeierKaufmann2016 13].

Dabei werden NoSQL Datenbanken bestimmte Eigenschaften zugeordnet, welche erfüllt sein müssen um als solche bezeichnet werden zu können.

**Eigenschaften**

[@MeierKaufmann2016 20]

- _Modell und Schema:_ Das Datenbankmodell ist nicht relational und folgt keinem vorgegebenen Schema.
- _Erfüllung der drei V's:_ Die im Datenbanksystem gespeicherten Daten weisen in ihren Eigenschaften die drei V's auf. 
- _Architektur:_ Eine horizontale Skalierbarkeit sowie verteile Webanwendungen müssen unterstützt werden.
- _Replikation:_ Das Datenbanksystem muss die Replikation aller Daten ermöglichen.
- _Datenkonsistenz:_ Es darf keine starke Datenkonsistenz vorliegen um eine geringe Ausfalltoleranz sowie hohe Verfügbarkeit zulassen zu können.

### Arten von NoSQL-Datenbanken
„Die Forscher und Betreiber des NoSQL-Archivs listen auf ihrer Webplattform 150 NoSQL-Datenbankprodukte“ [@MeierKaufmann2016 20]. Daraus kann geschlossen werden, dass der Bedarf von NoSQL-Technologien heutzutage stark gewachsen sowie notwendig ist. Nachfolgend werden die in der Praxis gebräuchlichsten Technologien näher erläutert

#### Key/Value Stores
[@Fasel2016 113-115].

Wie der Name bereits vermuten lässt gibt es bei dieser NoSQL-Technolgoie Schlüssel- und Wertpaare. Dabei haben weder der Schlüssel, noch die zugehörigen Werte komplexe Datentypen inne. Der Schlüssel ist immer eindeutig um dem entsprechenden Wert zugeordnet werden zu können und es werden keine Indizes gebildet. Key/Value Stores ist für einfache und wenig komplexe Datensätze gedacht, mit denen keine komplexen Operationen, wie Vergleiche zwischen den Datensätzen durchgeführt werden können. Die Stärken dieser Technologie liegen im schnellen Lese- sowie Schreibzugriff auf die jeweiligen Paare. Dabei fungieren Key/Value Stores meist In-Memory und je nach Anbieter werden Daten auch auf der Festplatte gespeichert. In der Praxis trifft man häufig auf Redis Store und Memcached als Anbieter solcher Key/Value Stores. Ein näheres Eingehen auf die jeweiligen Eigenschaften würde den Rahmen an dieser Stelle überschreiten. Trotzalledem soll ein kurzes Beispiel von Redis Store gezeigt werden.
Hiermit wird der Schlüssel „teamId“ mit dem zugehörigen Wert „name“ gesetzt.
```
SET teamId "name"
```
Durch Ausführen des Befehls
```
GET teamId
```
können die Werte des Schlüssel „teamId“ aufgerufen werden.
Zudem können auch Listen und Sets erstellt werden, die dann mehrere Werte unter einem eindeutigen Schlüssel enthalten.
```
//List
RPUSH teamId "name", "description"

//Set
SADD teamId "name" "description"
```
Zudem kann Redis Store festlegen wie lange ein Schlüssel bestehen soll und festgelegt werden, wie lange dieser Schlüssel bestehen bleiben soll. In diesem Fall wird der Schlüssel „teamId“ in drei Sekunden wieder gelöscht. Die Time To Live (TTL) kann dabei für jeden Schlüssel ebenso angepasst werden.
```
EXPIRE teamId 3
```

Key/Value Stores sind sehr gut für einfach Datenstrukturen sowie Zwischenspeicher für Webapplikationen geeignet. Somit können, aufgrund der In-Memory Speicherung, beispielsweise auf einer Webseite Daten, welche später wieder benötigt werden vorgehalten werden und müssen durch Einsatz eines Key/Value Stores nicht nochmals geladen werden. 

#### Document Stores
[@Fasel2016 115-118].

Document Stores folgen ebenso der Logik von Schlüssel-/Wertepaaren. Allerdings können dabei um einiges komplexere Datenstrukturen dargestellt werden, als dies bei Key/Value Stores der Fall ist. Dies kann durch Dokumente ermöglicht werden, in denen die entsprechenden Attribute gespeichert und indexiert werden können. Des weiteren sind Beziehungen zwischen den einzelnen Dokumenten möglich.
Der Schlüssel eines jeden Dokuments ist über die ganze Datenbank eindeutig und gilt somit nicht nur im Dokument selbst.
Sicherlich einer der bekanntestens Vertreter der Document Stores Technologie ist mongoDB, welche „die Dokumente in JSON ähnlichen Objekten“ [@Fasel2016 115] speichert. 
Das Anlegen eines Dokuments kann ich MongoDB wie folgt realsiert werden.

```
db.teamDocument.insert(
    {
        name: "Hochschule München",
        sport: "football",
        description: "Semi-professional team with students",
        teammates : {
            name: "Alice Arkansas",
            name: "Bob Baltimore"
        },
        league: "UniversityBowl"
    }
)
```
Der Schlüssel für dieses Dokument wird dabei automatisch von MongoDB festgelegt.
Möchte man nun einen weiteren Eintrag hinzufügen, kann dies analog zu obigen Beispiel getan werden.
```
db.teamDocument.insert(
    {
        name: "Hochschule Augsburg,
        league: "UniversityBowl"
    }
)
```
Zu beachten ist hierbei, dass beide Codebeispiele unterschiedliche Datenstrukturen aufweisen, welche von mongoDB ohne Probleme aktzeptiert werden. 
Durch die Indexierung bieten Document Store Datenbanken eine deutlich besser Abfragemöglichkeit der Daten, als es bei Key/Value Store Datenbanken der Fall ist. Lediglich komplexere Beziehungen sind mit Dokument Stores ebenso nicht allzu leicht darzustellen.

#### Column Family Stores
Colum Families „speichern Datensätze in multidimensionalen Maps ab, die relationalen Objekten ähnlich sind“ [@Fasel2016 118].
Bekannte Anbieter hierbei sind Google Big Tables, Cassandra und HBase, was wiederum auf Big Tables passiert. Grundsätzlich verfolgen alle denselben Ansatz der Column Family Stores. Allerdings gibt es im Detail leichte Abweichungen was Datenmodellierung sowie Namenskonventionen angeht.
Daher wird im folgenden anhand von Cassandra das Prinzip von Column Family Stores erklärt und mit passenden Codebeispielen ergänzt.

Cassandra arbeitet mit sogenannten Keyspaces, in diesem können ein oder mehrere Spalten enthalten sein. Die Gesamtheit aller Spalten in einem Keyspace wird als Spaltenfamilie bezeichnet, welche einen Schlüssel besitzt.
In jeder Spaltenfamilie gibt es wiederum Zeilen, die eine eindeutigen Schlüssel aufweisen. Jede Zeile enthält dabei Spalten, mit einem eindeutigen Wert.
Dabei enthält die Spalte in einer Zeile der Spaltenfamilie zudem einen Zeitstempel, wodurch eine Versionierung ermöglicht wird.
Eine Erweiterung der Spalten in einer Spaltefamilie ist die sogenannte „SuperColumn“. In dieser werden keine einzelnen Werte sondern eine Sammlung von Werten gespeichert.
Zudem ermöglicht Cassandra eine Replikation der Daten, was auch dringend so empfohlen wird. Dabei werden die Kopien auf unterschiedlichen Knoten gespeichert. In diesem Fall werden alle Zeilen zweimal gespeichert, wobei jede Replikation der Daten auf einem gesonderten Knoten liegt.

Column Family Stores bieten somit die Möglichkeit sehr komplexe Datenmodelle zu erstellen ohne dabei auf eine bestimmte Datenstruktur achten zu müssen. Durch die Anordnung in Spalten können Lese- und Schreibvorgänge schnell erfolgen.

#### Graphen Datenbanken

[@Fasel2016 122-124].

Wie der Name bereits vermuten lässt, nutzen Graphen Datenbanken die Graphentheorie um Daten abzubilden. Dabei stellt jeder Knoten eine Entität dar und steht mit einem anderen Knoten, verknüpft über Kanten, in einer Beziehung; wobei sowohl Knoten als auch Kanten Attribute besitzen können. Des weiteren können Knoten und Kanten jederzeit, ohne größeren Aufwand hinzugefügt oder entfernt werden.

Neo4j wird von Weltunternehmen wie Microsoft, IBM oder Adobe als Graphen Datenbanken genutzt. Dabei setzt Neo4j auf eine „eigene deskriptive Graphen-Abfragesprache namens Cypher“ [@Fasel2016 123].
```
CREATE (t:team { name:'Hochschule München', sport:'football'})
CREATE (p:player { name:'Bob Burton'})

```
Hierdurch werden die Knoten „team“ und „player“ mit den zugehörigen Variablen „t“ und „p“ erstellt.
Um nun eine Beziehung zwischen diesen beiden Knoten herzustellen muss folgender Code ausgeführt werden.
```
MATCH (t:team), (p:player)
WHERE t.name = 'Hochschule München' AND p.name = 'Bob Burton'
CREATE (p)-[r:plays_in]->(t)
```
Somit kann nun ausgedrückt werden, dass der Spieler „Bob Burton“ im Team der Hochschule München spielt.

Graphen Datenbanken eignen sich besonders gut um Graphen-ähnliche Strukturen darzustellen. „Ein typischer Graph ist beispielsweise ein Social Graph, wie ihn Facebook oder LinkedIn nutzt“ [@Fasel2016 122].

