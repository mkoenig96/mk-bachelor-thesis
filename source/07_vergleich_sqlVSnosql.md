# Vergleich von SQL und NoSQL Datenbanken

Anhand der in den Kapiteln 2.2 und 2.3 geschaffenen Grundlagen zu SQL und NoSQL Datenbanken sollen diese nun unter den darin aufgeführten und weiteren Aspekten verglichen werden.

## ACID, BASE und das CAP-Theorem
„Unter dem Begriff Konsistenz oder Integrität einer Datenbank versteht man den Zustand widerspruchsfreier Daten“ (@MeierKaufmann2016 135).
Für gewöhnlich arbeitet nicht nur eine Person an einer Datenbank, sondern es greifen mehrere Benutzer darauf zu und verändern Daten. Dabei muss immer gewährleistet sein, dass jeder Benutzer stets die aktuellen Daten einsehen kann, keine integritätsverletzenden Änderungen erfolgen und jede gültige Transaktion auch vollständig abgeschlossen wird. Unter einer Transaktion ist der Zugiff auf die Datenbank zu verstehen, bei dem Daten verändert werden.

SQL und NoSQL Datenbanken verfolgen hierbei zwei verschiedene Ansätze: Das ACID und das BASE Verfahren.

**ACID**

Bei allen relationalen und somit SQL Datenbanken hat die Datenkonsistenz höchste Priorität (strong consistency). 
ACID steht für **A**tomicity, **C**onsistency, **I**solation und **D**urability. Diese Anforderungen zur Konsistenzgewährung müssen von einer relationalen Datenbank bei jeder Transaktion stets erfüllt werden.

- _Atomarität (Atomicity):_ Transaktionen müssen entweder vollständig oder garnicht ausgeführt werden. Eine teilweise ausgeführte Transaktion darf nicht stattfinden.

- _Konsistenz (Consistency):_ Sobald eine Transaktion abgeschlossen ist, muss diese immer die Konsistenzbedingungen der Datenbank erfüllen.

- _Isolation (Isolation):_ Bei gleichzeitig ausgeführten Transaktionen dürfen diese keine anderen Auswirkungen haben, als wenn die Transaktion einzeln durchgefürt werden würde.

- _Langlebigkeit (Durability):_ Korrekte Transaktionen müssen auch bei Systemfehlern ausgeführt werden. Des weiteren müssen die dadurch oder eine andere Transaktionen geänderten Daten jederzeit bis zu einer neuerlichen Änderungen in der Datenbank bestehen.

[@MeierKaufmann2016 136-137].

**CAP-Theorem**

„Bei umfangreichen und verteilen Datenhaltungssystemen hat man erkannt, dass die Konsistenzforderung nicht in jedem Fall anzustreben ist, vorallem wenn man auf Verfügbarkeit und Ausfalltoleranz setzen möchte“ [@MeierKaufmann2016 148].

Eric Brewer entwickelte das sogenannte CAP-Theorem, welche aus drei Eigenschaften besteht:

- _Konsistenz (**C**onsistency):_ Alle Knoten im verteilten System erhalten die von einem anderen Knoten gemachten Änderungen.
- _Verfügbarkeit (**A**vailability):_ Das System ist jederzeit mit akzeptablen Antwortzeiten verfügbar. 
- _Ausfalltoleranz (**P**artition Tolerance):_ Das verteilte System kann jederzeit aufrecht erhalten werden, auch wenn Knoten ausfallen oder entfernt werden.

[@MeierKaufmann2016 148-149].

Brewer entdeckte, dass in einem massiv verteilten System nie mehr als zwei Eigenschaften des CAP-Theorems erfüllt werden können. Abhängig vom jeweiligen Anwendungsfall, für den das Datenbanksystem gelten soll, müssen somit zwei der drei Eigenschaften priorisiert werden. 
Da NoSQL Datenbanken sehr häufig in massiv verteilten Systemen angewendet werden und bei diesen Verfügbarkeit sowie Ausfalltoleranz im Vordergrund stehen, kann das ACID Prinzip der relationalen Datenbanken nicht umgesetzt werden. NoSQL Datenbanken halten daher andere Vorgaben ein, um die Datenkonsistenz zu gewährleisten.

**BASE**

NoSQL Datenbanken verfolgen aufgrund des CAP-Theorems und der Tatsache, dass diese zumeist hohe Verfügbarkeit sowie Ausfalltoleranz anstreben, den BASE Ansatz. BASE steht für **B**asically **A**vailable, **S**oft State und **E**ventually Consistency. „Ein einzelner Knoten im Rechnernetz ist meistens verfügbar (Basically Available) und manchmal noch nicht kosistent nachgeführt (Eventually Consistent), d.h. erkann sich in einem weichen Zustand (Soft State) befinden“ [@MeierKaufmann2016 154]. NoSQL Datenbanken gehen daher, im Gegensatz zu relationalen Datenbanken, einer schwachen Datenkonsistenz nach (weak consistency), wodurch es passieren kann, dass bei einem Knoten bereits Änderungen entstanden sind, aber diese noch nicht von anderen Knoten registriert wurden. 

[@MeierKaufmann2016 148-155].

## Pessimistische und Optimistische Verfahren

Zuvor wurde stark auf die Einhaltung von Datenkonsistenz und Integrität eingegangen. Dabei lag der Fokus auf den grundsätzlichen Eigenschaften, die die jeweiligen Datenbanken erfüllen müssen, um den Anforderungen gerecht zu werden. Im weiteren wird darauf eingegangen, wie die Umsetzung von ACID und BASE erfolgt.

Hierbei gibt es zwei verschiedene Verfahren, nach denen in einer Datenbank Transaktionen abgearbeitet und Objekte verändert werden.

**Pessimistisch**

Pessimistische Verfahren sperren Objekte, sobald diese verändert werden sollen. Dies gilt sowohl für Lese- als auch Schreibzugriffe. Andere Transaktionen werden während der gesamten Sperrzeit nicht zugelassen. Hierfür wurde das Zweiphasensperrprotokoll entwickelt. Dabei gibt es zwei Phasen, in denen zuerst Sperren aufgebaut und im Anschluss wieder nacheinander abgebaut werden. Dies dauert im Vergleich zu optimistischen Verfahren zwar länger, garantiert dafür aber jederzeit die Integrität sowie Konsistenz der Daten.
Neben dem Zweiphasensperrprotokoll gibt es auch Verfahren, die Transaktionen mit Zeitstempeln versehen und chronologisch abarbeiten.

[@MeierKaufmann2016 141-144].

**Optimistisch**

Bei optimistischen Verfahren werden keine Sperren gesetzt, da davon ausgegangen wird, „[...] dass Konflikte konkurrierender Transaktionen selten vorkommen“ [@MeierKaufmann2016 144]. Stattdessen wird die Transaktion in drei verschiedene Phasen eingeteilt: Lese-, Validierungs- und Schreibphase. 
In der Lesephase werden alle Objekte gelesen, welche mit einer Transaktion bearbeitet werden sollen. Danach wird in der Validierungsphase festgestellt, ob zwischen den zu bearbeitenden Objekten Konflikte entstehen können. Ist dies nicht der Fall, werden in der Schreibphase die Änderungen in die Datenbank überführt. Sollte es doch zu Überschneidungen kommen, hat immer das Objekt Vorrang, welches sich bereits in der Schreibphase befindet. Die Abarbeitung in der Validierungsphase erfolgt chronologisch. Dieses Verfahren hat den Vorteil, dass schnellere sowie gleichzeitige Lesezugriffe möglich sind, da nicht wie beim Zweiphasensperrprotokoll bereits in der Lesephase gesperrt wird. 
[@MeierKaufmann2016 144-146].


## Einsatz in der Praxis

Relationale respektive SQL Datenbanken erfordern, aufgrund des relationalen Datenmodells und der damit in Verbindung stehenden Normalisierung der Daten, im Vorhinein ein sehr durchdachtes Datenkonzept. Dies inkludiert, dass alle Beziehungen der einzelnen Relationen untereinander sowie Datentypen, Tabellen und Attribute festgelegt werden müssen. Jener Prozess kann zu einer geringeren Flexibilität führen, wenn das Datenmodell später, beispielsweise aufgrund eines Relaunches, geändert werden muss. Des weiteren muss bekannt sein, in welcher Form die Daten vorliegen müssen, um mit dem Datenmodell übereinzustimmen. Andererseits sorgt genau dieses Vorgehen dafür, redundanzfreie Daten zu erhalten und Anomalien zu verhindern. Dies wiederum spart Speicherplatz, was Kosten verringert und die Rechenleistung der Datenbank nachhaltig verbessert. Dem gegenüber steht die Tatsache, 
dass SQL Datenbanken vollwertige Datenintegrität mittels ACID gewährleisten, was für viele Unternehmen ein ausschlaggebendes Argument ist. Zudem steht mit SQL eine etablierte Abfragesprache zur Verfügung, welche sich auch in der Syntax zwischen den einzelnen DBMS sehr ähnlich ist. Für die Wissensbildung im Entwicklerteam ist es vorteilhaft, da der Aufwand überschaubar ist, bei Einführung eines neu eingführten DBMS die neue Syntax zu erlernen. 
NoSQL Technologien verwenden dagegen unterschiedliche Abfragesprachen, wie bei den Beispielen MongoDB, Cypher und RedisCache zu sehen ist. Allesamt beinhalten unterschiedliche Syntax, wodurch sich auch Entwickler, welche im jeweiligen Themenbereich noch nicht so viel Erfahrung haben, neu einlesen müssen. Andererseits fällt bei NoSQL Datenbanken die Normalisierung durch das Datenbankmodell weg. Die Daten können ohne weitere Struktur, ungeordnet, aber auch geordnet in die Datenbank eingespielt werden. Gleiches gilt für Datentypen, welche ebenso nicht von vornherein festgelegt werden müssen. Somit können sehr viele Daten in verschiedenen Formen gesammelt und danach analysiert werden. Ein weiterer Vorteil der NoSQL Technologie ist das gute Zusammenspiel in massiv verteilten Systemen, wobei diese hinsichtlich Ausfalltoleranz sowie hoher Verfügbarkeit effektiv eingesetzt werden können.

Die Frage, ob eine SQL oder NoSQL Datenbank nun „besser“ ist, kann und wird nicht beantwortet werden können. Die Anforderungen stellen hier das entscheidende Kriterium dar. Dass eine Bank eine NoSQL Datenbank verwendet, scheint zum jetzigen Zeitpunkt mehr als unwahrscheinlich. Man stelle sich vor, dass aufgrund des BASE Verfahrens einer NoSQL Datenbank ein Kunde Geld abheben kann, obwohl sein Kontostand bereits bei null ist und dieser Zustand aufgrund einer noch nicht registrierten Abbuchung aufgetreten ist. Umgekehrt könnte ein SocialGraph, wie ihn Facebook verwendet, auch nicht mit einer relationalen Datenbank konstruiert werden. 
Letztendlich geht es auch in diesem Fall darum zu wissen, welche Eigenschaften die jeweilige Technologie mit sich bringt und danach die passende Auswahl zu treffen.



