# Quantitative Analyse der aktuellen sowie neuen Architektur 

Dieses Kapitel soll Aufschluss darüber geben, welche Änderungen sich hinsichtlich der neuen Multi-Tenant Architektur in Bezug auf das aktuelle System in puncto Kosten und Datenbankabfragen ergeben. 
In die Analyse wurden stets die nachfolgenden fünf Server mit einbezogen.

|           | **Datenbankgröße** | **SSD** |                 |              | 
|-----------|----------------|------------------|-----------------|--------------|-----------|
| **Server**    | *in MB*          | *Auslastung in GB* | *Auslastung in %* | *Gesamt in GB* | 
| Live 1    |           76,5 |             83,8 |            27,7 |        302,3 |         |
| Live 2    |          122,7 |            111,4 |            22,1 |        503,8 |         |
| Live 3    |           83,6 |             76,1 |            15,1 |        503,8 |         |
| Live 4    |           11,4 |             14,3 |             2,8 |        503,8 |          |
| Generator |          184,5 |             29,5 |             9,8 |        302,3 |         |
Tabelle 2: Serverkapazitäten TeamSports2

Auf den Liveservern liegen die Liveinstanzen der Kunden unter einer eigenen Domain. Der Generatorserver beinhaltet dahingegen alle Testinstanzen, welcher durch den TeamSports2 Generator erstellt wurden. Die Unterschiede in der Gesamtgröße der SSD Festplatte bei Live 1 und Generator zu den anderen Servern ergibt sich aus den jeweils unterschiedlich gebuchten Serverpaketen beim Hostinganbieter.

## Kosten 

Aus vorangegangener Tabelle wird deutlich, dass bei Normalauslastung der Server viele Ressourcen ungenutzt sind. Bei keinem Server steigt die Auslastung SSD Festplatte über 30%. Trotzdem müssen die Ressourcen vorgehalten werden um Anfragespitzen zuverlässig verarbeiten und bei jeder Instanz eine schnelle Antwortzeit gewährleisten zu können. Die aktuell gebuchten Serverpakete werden auch aufgrund der damit verbundenen vCores des Prozessors und des RAMs benötigt. Eine geringere SSD Kapazität würde wiederum weniger vCores sowie RAM bedeuten, welche bei der Anzahl der Instanzen auf den Servern zu einer längeren Antwortzeit führt.
Die aktuellen monatlichen Kosten für die angemieteten Server sind als sehr günstig einzustufen. Allerdings ist dem gegenüberstellt, dass einige Kapazitäten ungenutzt sind und keine Skalierung der Ressourcen möglich ist.

| Server    | Instanzen | CPU in vCores | RAM in GB | Kosten in € / p.m.  | 
|-----------|-----------|---------------|-----------|---------------------|
| Live 1    |        34 |             6 |        12 |               16,99 |            
| Live 2    |        60 |            12 |        24 |               24,99 |            
| Live 3    |        61 |            12 |        24 |               24,99 |            
| Live 4    |         5 |            12 |        24 |               24,99 |             
| Generator |        90 |             6 |        12 |               17,00 |             
| **Gesamt** |         |              |         |               **108,96** |       
Tabelle 3: Aktuelle Gesamtkosten pro Server bei TeamSports2

Durch die neue Multi-Tenant Architektur werden weniger Server und Datenbanken benötigt. Dahingegen ist die notwendige Gesamtgröße um alle Tenants auf einem Server mit einer Datenbank betreiben zu können auf eine Ressource, in Form des Servers, konzentriert. Eine vergleichbare on-premise Lösung, mit der die aktuellen Lasten bewältigt werden können, ist in folgender Tabelle unter dem Hosting Service zu finden. Aufgrund der gesammelten Anfragelast auf einen Server werden für den Server dementsprechend mehr Ressourcen benötigt.

| Service | Anzahl in Stk. | CPU in vCores | RAM in GB | SSD in GB | Kosten in € / p.m. |
|---------|----------------|---------------|-----------|-----------|--------------------|
| EC2     |              5 |             8 |        32 |       120 |            1160,96 |
| Hosting |              1 |            32 |        48 |      1000 |              79,99 |
| EC2     |              1 |            32 |        48 |      1000 |             569,97 |   
Tabelle 4: Kosten der AWS EC2-Instanzen

Da eine effizientere Nutzung der Ressourcen, als auf herkömmlichen on-premise Servern, mithilfe der neuen Multi-Tenant Architektur in Verbindung mit Cloud Computing möglich ist, werden vergleichbare Cloud Computing Infrastrukturen ebenso beleuchtet.
Als Cloud Computing Anbieter wurde in der gesamten Analyse AWS gewählt, wobei auch andere Anbieter gleichwertige Services zur Verfügung stellen. Alle Kosten der AWS Services wurden mithilfe des AWS Kostenkalkulators ermittelt und die zugehörigen Kostenparameter, wie CPU oder Requests, mit den Werten aus der aktuellen TeamSports2 Architektur gleichgesetzt. 

Mithilfe von EC2 stellt AWS in der Cloud Serverkapazitäten zur Verfügung. Die vollständige Wartung sowie Konfigruation der Server übernimmt AWS. Wie zu sehen ist, sind die bentöigten EC2 Instanzen circa um das Zehnfache teurer, als die gegenwärtige Infrastruktur, wenn diese nahezu gleich in AWS aufgebaut werden würde. Des weiteren wurde in der Kalkulation mit den niedrigst möglichen Ressourcen gerechnet und es sind keine Anfragespitzen mit einberechnet. Zwar ist eine automatische vertikale Skalierung aufgrund von Parametern wie CPU Auslastung über AWS problemlos möglich, allerdings ist damit ein zusätzlicher Anstieg der ohnehin vergleichweise teuren EC2 Infrastrukutr verbunden. Eine einzige EC2 Instanz mit größerem Leistungsumfang wäre zwar günstiger als fünf EC2 Instanzen aber immer noch um einiges teurer als ein on-premise Server mit gleichem Leistungsumfang. 

Dem gegenüber gestellt ist eine vollständige AWS Infrastruktur. Die verwendeten Services werden zum Verständnis kurz vorgestellt:

- _S3:_ Vollständig verwalteter Objektspeicherservice.
- _Lambda:_ Ermöglicht die Ausführung von vollständig serverlosen Code, sogenannte Lambda Functions.
- _Aurora:_ Vollständig verwaltete sowie skalier- und replizierbare MySQL Datenbank.

Mithilfe von S3 werden, durch die Tenants hochgeladenen Dateien, gespeichert. Die Abrechnung erfolgt nach Speicherplatz und Requests an den sogenanten S3 Bucket. Daher können die im folgenden Kapitel dargestellten Daten zu den SELECT Abfragen berücksichtigt werden. Bei Lambda wird nur die Anzahl der Requests und der damit ausgeführte Code berücksichtigt. Finden keine Requests statt, erfolgt auch keine Berechnung. Für Aurora wurde sich an den aktuellen Auslastungen des Systems orientiert.

| AWS Service | Leistung                                               | Kosten in € / p.m. |
|-------------|--------------------------------------------------------|--------------------|
| S3          | 350 GB Speicherplatz  (SELECT Abfragen berücksichtigt) | 211,95             |
| Lambda      | 1.735.586.217 Requests                                 | 344,82             |
| Aurora      | vCPU: 2, RAM: 15.25, Speicher: 500 GB                  | 680,50             |
| Redis Cache | vCPU: 2, RAM: 3.09, Knoten: 3                          | 137,03             |
| **Gesamt**  |                                                        | **1374,30**            |
Tabelle 5: Kosten einer möglichen AWS Infrastruktur

Die Berechnungen zeigen, dass eine reine AWS Infrastruktur im Vergleich zur jetzigen Infrastruktur sehr viel teurer wäre. Allerdings kann dadurch eine bedarfsgerechte Skalierung ermöglicht werden, wodurch wiederum Kosteneinsparungen eintreten. Zudem können auch Entwicklerkosten besser eingesetzt werden, da sich die Entwickler nicht parallel um die Verwaltung der Infrastruktur kümmern müssen. 
Das Betreiben einer oder mehrerer EC2 Instanzen stellt dahingegen, aufgrund der vergleichsweise hohen Kosten keine praktikable Lösung dar. Hier ist der vom Hostinganbieter bereitgestellte Server mit besserem Leistungsumfang, in Verbindung mit der neuen Multi-Tenant Architektur, die günstigere Variante.
Unabhängig davon ob eine Migration in die Cloud durchgeführt wird, können mit der neuen Mulit-Tenant Architektur Kosten- sowie Verwaltungsaufwände, durch die Reduktion der Server, verringert werden.

\pagebreak

## Häufige Queries 

Mit der aktuellen Architektur setzt sich bei allen Liveservern der Hauptteil der Datenbankabfragen aus SELECT Abfragen zusammen. Die in den nachfolgenden Diagrammen festgestellten Werte für den Live eins Server finden sich bei den Liveservern zwei, drei und vier ebenso wieder. Alle Diagramme (Abbildung 16) und Zahlen (Tabelle 6) wurden aus dem MySQL Statistik Dashboard generiert.

![](source/figures/Queries-diagram_Live1.png)
Abbildung 14: Queries gegen die Datenbank auf dem Live 1 Server

Einzig der Generator Server weist einen vergleichsweise geringeren Prozentsatz bei den SELECT Abfragen auf. Die 21 % an SET OPTION Anfragen lassen sich mit der Tatsache erklären, dass beim Generieren einer neuen Seite die Datenbank für die jeweilige Instanz neu erstellt wird und somit Optionen, wie das Passwort gesetzt werden müssen. Zudem passiert es des öfteren, dass Nutzer ein neue Seite erstellen, dieser aber schon nach kurzer Zeit nicht mehr aktiv nutzen. 

![](source/figures/Queries-diagram_Generator.png)
Abbildung 15: Queries gegen die Datenbank auf dem Generator Server

Über alle fünf Server verteilt sich die genaue Anzahl der SELECT Anfragen wie in folgender Tabelle zu sehen ist.

|           | SELECT Abfragen |            |             |
|-----------|-----------------|------------|-------------|
| Server    |    ø pro Stunde |  ø pro Tag | ø pro Monat |
| Live 1    | 173.500         | 4.164.000  | 124.920.000 |
| Live 2    | 302.700         | 7.264.800  | 217.944.000 |
| Live 3    | 308.300         | 7.399.200  | 221.976.000 |
| Live 4    | 24.900          | 298.800    | 8.964.000   |
| Generator | 9.388           | 225.312    | 6.759.360   |
|           |                 |            |             |
| Gesamt    | 818.788         | 19.352.112 | 580.563.360 |
Tabelle 6: SELECT Abfragen aller Server in Stück

Der hohe Anteil der SELECT Abfragen im Vergleich zu anderen Abfragen ist in dieem Fall nicht verwunderlich, da in TeamSports2 hautpsächlich Daten aus bestimmten Tabellen angefragt werden und keine komplexen JOIN Operationen oder vergleichbares vorgenommen werden müssen. 
Mit der neuen Multi-Tenant Architektur kann besagter Anteil nicht reduziert werden; da sowohl an den Datenbankabfragen in den Controllern, als auch an der Anzahl der Tabellen keine Änderungen vorgesehen sind. Hierfür müsste zum einen das Datenbankmodell von Grund auf überdacht werden um gegebenenfalls überflüssige Tabellen zu entfernen und die Datenstruktur für die Abfragen zu verbessern. Zum anderen wäre auch das Überarbeiten der Abfragen an die Datenbank in den Controllern notwendig, ob beispielsweise an Stellen überflüssigerweisen alle Daten aus einer Tabelle geladen werden, anstatt nur die benötigten.   
Dahingegen kann mit der neuen Architektur die Anzahl der SELECT Anfragen reduziert werden. Zwar findet zu Beginn einer jeden Session ein zusätzlicher Request an die Datenbank für die Ermittlung der tenantId statt; wohingegen häufig angefragte Daten mittels Redis gecached werden und somit die Anzahl der Anfragen durch SELECT auf die Datenbank reduziert wird. Dies ist auch aufgrund der Konzentration aller Daten in einer Datenbank hilfreich um die Last auf die Datenbank bei vielen gleichzeitige Anfragen durch die Tenants zu reduzieren. Der Einsatz von Redis Cache wäre zwar auch in der jetzigen Architektur möglich, zumal CakePHP diesen untersützt. Allerdings darf hierbei nicht vergessen werden, dass ein RedisCache für jede einzelne Datenbank in einer Single-Tenant Anwendung hohe Kosten sowie Wartungs- und Implementierungsaufwände verursacht, wodurch die Performancevorteile wieder relativiert werden würden.


<!--

|           | **DB-Verbindungen** |              |               |             |
|-----------|--------------|--------------|---------------|-------------|
| **Server**    | *ø pro Stunde* | *ø pro Minute* | *ø pro Sekunde* | 
| Live 1    |      210.691 |        3.512 |            59 | 
| Live 2    |      363.582 |        6.060 |           101 | 
| Live 3    |      364.066 |        6.068 |           101 | 
| Live 4    |       38.543 |          642 |            11 | 
| Generator |       15.544 |          259 |             4 | 
Tabelle X: Datenbank-Verbindungen TeamSports2
-->

<!--![](source/figures/Queries-diagram_Live1.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 1 Server

![](source/figures/Queries-diagram_Live2.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 2 Server

![](source/figures/Queries-diagram_Live3.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 3 Server

![](source/figures/Queries-diagram_Live4.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 4 Server-->



