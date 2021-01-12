# Technische Grundlagen 

## Multi-Tenant Architektur 


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








