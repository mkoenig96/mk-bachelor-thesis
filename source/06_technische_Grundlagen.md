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
Die Attribute der Tabelle sind demnach,bei Ausführung der Angabe, „id“ (=festgelegter Primärschlüssel), „name“, „shortname“ und „description“. Das  NOT NULL Statement sorgt dafür, dass das Attribut „id“ niemals NULL werden kann.
Dies ist gerade bei Schlüsselattributen wichtig, damit dieses auch wirklich gesetzt werden und als solches agieren kann.
Oftmals ist es gewollt, dass ein Attribut auf eine andere Tabelle referenzieren soll. Hierfür muss dann ein Fremdschlüssel angegeben werden, wodurch die Angabe wie folgt ergänzt werden müsste.

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
Dies soll hervorheben, dass zwar alle DBMS auf SQL als Abfragesprache beruhen, es allerdings durchaus unterschiedliche Syntax gibt.

**Datenmodellierung**




## NoSQL 

<!--Das ist ein Unterabschnitt des Mittelteils. Quisque sit amet tempus arcu, ac suscipit ante. Cras massa elit, pellentesque eget nisl ut, malesuada rutrum risus. Nunc in venenatis mi. Curabitur sit amet suscipit eros, non tincidunt nibh. Phasellus lorem lectus, iaculis non luctus eget, tempus non risus. Suspendisse ut felis mi.

## Zusammenfassung der Kapitel

<!--
kursiv: * auf beiden Seiten des Textes
fett: **
kursiv und fett: ***
-->

<!--Dies ist ein kurzer Überblick darüber, was in jedem Kapitel geschrieben wurde. **Kapitel 1** gibt einen Hintergrund über duis tempus justo quis arcu consectetur sollicitudin. **Kapitel 2** diskutiert morbi sollicitudin gravida tellus in maximus. **Kapitel 3** diskutiert vestibulum eleifend turpis id turpis sollicitudin aliquet. **Kapitel 4** zeigt wie phasellus gravida non ex id aliquet. Proin faucibus nibh sit amet augue blandit varius.-->




