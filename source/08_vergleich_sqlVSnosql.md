# Vergleich von SQL und NoSQL Datenbanken

Anhand der in den Kapiteln 2.2 und 2.3 geschaffenen Grundlagen zu SQL und NoSQL Datenbanken sollen diese nun unter den darin aufgeführten und weiteren Aspekten verglichen werden.

## ACID und BASE
„Unter dem Begriff Konsistenz oder Integrität einer Datenbank versteht man den Zustand widerspruchsfreier Daten“ (@MeierKaufmann2016 135).
Für gewöhnlich arbeitet nicht nur eine Person an einer Datenbank, sondern es greifen mehrere Benutzer darauf zu und verändern Daten. Dabei muss immer gewährleistet sein, dass jeder Benutzer stets die aktuellen Daten einsehen kann, keine integritätsverletzenden Änderungen erfolgen und jede gültige Transaktion auch vollständig abgeschlossen wird. Unter einer Transaktion ist der Zugiff auf die Datenbank zu verstehen, bei dem Daten verändert werden.

SQL und NoSQL Datenbanken verfolgen hierbei zwei verschiedene Ansätze. Das ACID und das BASE Verfahren.

**ACID**

[@MeierKaufmann2016 136-137].

Alle relationalen und somit SQL Datenbanken erfüllen die ACID Anforderung. 
ACID steht für **A**tomicity, **C**onsistency, **I**solation und **D**urability. Diese Anforderungen zur Konsistenzgewährung müssen von einer relationalen Datenbank bei jeder Transaktion stets erfüllt werden.

- _Atomarität (Atomicity):_ Transaktionen müssen entweder vollständig oder garnicht ausgeführt werden. Eine teilweise ausgeführte Transaktion darf nicht stattfinden.

- _Konsistenz (Consistency):_ Sobald eine Transaktion abgeschlossen muss diese immer die Konsistenzbedingungen der Datenbank erfüllen.

- _Isolation (Isolation):_ Bei gleichzeitig ausgeführten Transaktionen dürfen diese keine anderen Auswirkungen haben, als wenn die Transaktion einzeln durchgefürt werden würde.

- _Langlebigkeit (Durability):_ Korrekte Transaktionen müssen auch bei Systemfehlern ausgeführt werden. Des weitern müssen die dadurch oder eine andere Transaktionen geänderten Daten jederzeit, bis zu einer neuerlichen Änderungen, in der Datenbank bestehen.

**BASE**



## Pessimistische und Optimistische Verfahren

## Datenmodellierung

## Praxisorientierung



