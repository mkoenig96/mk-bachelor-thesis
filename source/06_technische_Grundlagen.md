# Technische Grundlagen 

## Multi-Tenant Architektur 


## SQL 

SQL ist eine deskriptive Abfragesprache und steht für *Structured Query Language* [@MeierKaufmann2016 7]. Diese wird für die Abfrage von relationalen Datenbanksystemen verwendet, welche in Tabellen organisiert sind. Somit ist es in der Praxis gebräuchlich, von einer SQL-Datenbank zu sprechen, wenn sich dahinter ein relationales Datenbanksystem handelt.

### Relationales Datenbanksystem
[@MeierKaufmann2016 10].

Ein relationales Datenbanksystem besteht aus der Datenbank an sich sowie einem Datenbankmanagementsystem. Dabei umfasst die Datenbank eine Sammlung aller Tabellen mit deren Beziehungen untereinander. Mithilfe des Datenbankmanagementsystem können dann An- und Abfragen an die Datenbank realisiert werden.
Dabei gibt es verschiedene Datenbankmanagementsystem, welche aber bei relationalen Datenbanken allesamt SQL als Abfragesprache nutzen.
Teilweise unterscheiden sich die in den Datenbankmanagementsystemen angewandten SQL Abfragen allerdings in der Syntax.
Nachfolgend werden gebräuche Datenbankmanagementsysteme kurz vorgestellt, wobei gesagt sei, dass diese nur einen Ausschnitt der verfügbaren Systeme darstellen

**MySQL**


### Relationale Datenbankmodelle
Relationale Datenbankmodelle zeichnen sich dadurch aus, dass sie vollständig in Tabellen und Spalten dargestellt werden.
Jede Tabelle enthält einen eindeutigen Bezeichner, den Primärschlüssel. Anhand dieses Schlüssel kann jeder in der Tabelle enthaltene Datensatz eindeutig indentifiziert werden. In jeder Tabelle sind Attribute enthalten, welche die Eigenschaften einer Tabelle darstellen. An nachfolgender MySQL Angabe die eine beispielhafte Tabelle samt ihrer Attributte erstellt, wird dies verdeutlicht.


### Beispielhafte Abfragen
_MySQL_
```
CREATE TABLE teams (
    id int,
    name varchar(255),
    shortname varchar(255),
    description varchar(255),
    departmentId int,
    PRIMARY KEY (id)
);
```
Die Attribute der Tabelle sind demnach,bei Ausführung der Angabe, „id“ (=festgelegter Primärschlüssel), „name“, „shortname“ und „description“.
Oftmals ist es gewollt, dass ein Attribut auf eine andere Tabelle referenzieren soll. Hierfür muss dann ein Fremdschlüssel angegeben werden, wodurch die Angabe wie folgt ergänzt werden müsste.

```
FOREIGN KEY (departmentId) REFERENCES departments(departmentId)
```
Mit dieser Angabe wird das Attribut „departmentId“ als Fremdschlüssel deklariert und referenziert dabei auf das Attributt „departmentId“ in der “departments“ Tabelle. 

Die vorangeganenen SQL Befehle sind in MySQL geschrieben. MySQL stellt dabei Es gibt aber auch andere 




## NoSQL 

<!--Das ist ein Unterabschnitt des Mittelteils. Quisque sit amet tempus arcu, ac suscipit ante. Cras massa elit, pellentesque eget nisl ut, malesuada rutrum risus. Nunc in venenatis mi. Curabitur sit amet suscipit eros, non tincidunt nibh. Phasellus lorem lectus, iaculis non luctus eget, tempus non risus. Suspendisse ut felis mi.

## Zusammenfassung der Kapitel

<!--
kursiv: * auf beiden Seiten des Textes
fett: **
kursiv und fett: ***
-->

<!--Dies ist ein kurzer Überblick darüber, was in jedem Kapitel geschrieben wurde. **Kapitel 1** gibt einen Hintergrund über duis tempus justo quis arcu consectetur sollicitudin. **Kapitel 2** diskutiert morbi sollicitudin gravida tellus in maximus. **Kapitel 3** diskutiert vestibulum eleifend turpis id turpis sollicitudin aliquet. **Kapitel 4** zeigt wie phasellus gravida non ex id aliquet. Proin faucibus nibh sit amet augue blandit varius.-->




