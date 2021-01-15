# Vergleich von SQL und NoSQL Datenbanken

Anhand der in den Kapiteln 2.2 und 2.3 geschaffenen Grundlagen zu SQL und NoSQL Datenbanken sollen diese nun unter den darin aufgeführten und weiteren Aspekten verglichen werden.

## ACID, BASE und das CAP-Theorem
„Unter dem Begriff Konsistenz oder Integrität einer Datenbank versteht man den Zustand widerspruchsfreier Daten“ (@MeierKaufmann2016 135).
Für gewöhnlich arbeitet nicht nur eine Person an einer Datenbank, sondern es greifen mehrere Benutzer darauf zu und verändern Daten. Dabei muss immer gewährleistet sein, dass jeder Benutzer stets die aktuellen Daten einsehen kann, keine integritätsverletzenden Änderungen erfolgen und jede gültige Transaktion auch vollständig abgeschlossen wird. Unter einer Transaktion ist der Zugiff auf die Datenbank zu verstehen, bei dem Daten verändert werden.

SQL und NoSQL Datenbanken verfolgen hierbei zwei verschiedene Ansätze. Das ACID und das BASE Verfahren.

**ACID**

[@MeierKaufmann2016 136-137].

Bei allen relationalen und somit SQL Datenbanken hat die Datenkonsistenz höchste Priorität (strong consistency). 
ACID steht für **A**tomicity, **C**onsistency, **I**solation und **D**urability. Diese Anforderungen zur Konsistenzgewährung müssen von einer relationalen Datenbank bei jeder Transaktion stets erfüllt werden.

- _Atomarität (Atomicity):_ Transaktionen müssen entweder vollständig oder garnicht ausgeführt werden. Eine teilweise ausgeführte Transaktion darf nicht stattfinden.

- _Konsistenz (Consistency):_ Sobald eine Transaktion abgeschlossen muss diese immer die Konsistenzbedingungen der Datenbank erfüllen.

- _Isolation (Isolation):_ Bei gleichzeitig ausgeführten Transaktionen dürfen diese keine anderen Auswirkungen haben, als wenn die Transaktion einzeln durchgefürt werden würde.

- _Langlebigkeit (Durability):_ Korrekte Transaktionen müssen auch bei Systemfehlern ausgeführt werden. Des weitern müssen die dadurch oder eine andere Transaktionen geänderten Daten jederzeit, bis zu einer neuerlichen Änderungen, in der Datenbank bestehen.

**CAP-Theorem**

„Bei umfangreichen und verteilen Datenhaltungssystemen hat man erkannt, dass die Konsistenzforderung nicht in jedem Fall anzustreben ist, vorallem wenn man auf Verfügbarkeit und Ausfalltoleranz setzen möchte“ [@MeierKaufmann2016 148].

[@MeierKaufmann2016 148-149].
Eric Brewer entwickelte das sogenannte „CAP-Theorem“, welche aus drei Eigenschaften besteht:
- _Konsistenz (**C**onsistency):_ Alle Knoten im verteilten System erhalten die von einem anderen Knoten gemachten Änderungen.
- _Verfügbarkeit (**A**vailability):_ Das System ist jederzeit mit  akzeptablen Antwortzeiten verfügbar. 
- Ausfalltoleranz (**P**artition Tolerance):_ Das verteilte System kann jederzeit aufrecht erhalten werden, auch wenn Knoten ausfallen oder entfernt werden.

Brewer entdeckte, dass in einem massiv verteilten System nie mehr als zwei Eigenschaften des CAP-Theorems erfüllt werden können. Abhängig vom jeweiligen Anwendungsfall, für den das Datenbanksystem gelten soll, müssen somit zwei der drei Eigenschaften priorisiert werden. 
Da NoSQL Datenbanken sehr häufig in massiv verteilten Systemen angewendet werden und bei diesen Verfügbarkeit sowie Ausfalltoleranz im Vordergrund stehen, kann das ACID Prinzip der relationalen Datenbanken nicht umgesetzt werden. NoSQL Datenbanken halten daher andere Vorgaben ein um die Datenkonsistenz zu gewährleisten.

**BASE**

[@MeierKaufmann2016 148-155].





## Pessimistische und Optimistische Verfahren

## Datenmodellierung

## Praxisorientierung



