# Quantitative Analyse der aktuellen sowie neuen Architektur 

Dieses Kapitel soll Aufschluss darüber geben, welche Änderungen sich hinsichtlich der neuen Multi-Tenant Architektur in Bezug auf das aktuelle System in puncto Kosten und Datenbankabfragen ergeben. 
In die Analyse wurden stets die nachfolgenden fünf Server mit einbezogen.

|           | **Datenbankgröße** | **SSD** |                 |              | 
|-----------|----------------|------------------|-----------------|--------------|-----------|
| **Server**    | *in MB*          | *Auslastung in GB* | *Auslastung in %* | *Gesamt in GB* | 
| Produktiv 1    |           76,5 |             83,8 |            27,7 |        302,3 |         |
| Produktiv 2    |          122,7 |            111,4 |            22,1 |        503,8 |         |
| Produktiv 3    |           83,6 |             76,1 |            15,1 |        503,8 |         |
| Produktiv 4    |           11,4 |             14,3 |             2,8 |        503,8 |          |
| Test |          184,5 |             29,5 |             9,8 |        302,3 |         |
Tabelle 2: Aktuelle Serverkapazitäten bei TeamSports2

Auf den Produktivservern liegen die Instanzen der Kunden unter einer eigenen Domain. Der Testserver beinhaltet dahingegen alle Testinstanzen, welche durch den TeamSports2 Generator erstellt wurden. Die Unterschiede in der Gesamtgröße der SSD Festplatten bei Produktiv eins und Testserver zu den anderen Servern ergibt sich aus den jeweils unterschiedlich gebuchten Serverpaketen beim Hostinganbieter.

\pagebreak

## Kosten 

Aus vorangegangener Tabelle wird deutlich, dass bei Normalauslastung der Server viele Ressourcen ungenutzt sind. Bei keinem Server steigt die Auslastung SSD Festplatte über 30 Prozent. Trotzdem müssen die Ressourcen vorgehalten werden, um Anfragespitzen zuverlässig verarbeiten und bei jeder Instanz eine schnelle Antwortzeit gewährleisten zu können. Die aktuell gebuchten Serverpakete werden auch aufgrund der damit verbundenen vCores des Prozessors und des RAMs benötigt. Eine geringere SSD Kapazität würde wiederum weniger vCores sowie RAM bedeuten, welche bei der Anzahl der Instanzen auf den Servern zu einer längeren Antwortzeit führt.
Die aktuellen monatlichen Kosten für die angemieteten Server sind als sehr günstig einzustufen. Allerdings ist dem gegenüberzustellen, dass einige Kapazitäten ungenutzt sind und keine Skalierung der Ressourcen möglich ist.

| Server    | Instanzen | CPU in vCores | RAM in GB | Kosten in € / p.m.  | 
|-----------|-----------|---------------|-----------|---------------------|
| Produktiv 1    |        34 |             6 |        12 |               16,99 |            
| Produktiv 2    |        60 |            12 |        24 |               24,99 |            
| Produktiv 3    |        61 |            12 |        24 |               24,99 |            
| Produktiv 4    |         5 |            12 |        24 |               24,99 |             
| Test |        90 |             6 |        12 |               17,00 |             
| **Gesamt** |         |              |         |               **108,96** |       
Tabelle 3: Aktuelle Gesamtkosten pro Server bei TeamSports2

Durch die neue Multi-Tenant Architektur werden weniger Server und Datenbanken benötigt. Dahingegen ist die notwendige Gesamtgröße, um alle Tenants auf einem Server mit einer Datenbank betreiben zu können, auf eine Ressource in Form des Servers konzentriert. Eine vergleichbare on-premise Lösung, mit der die aktuellen Lasten bewältigt werden können, ist in folgender Tabelle unter dem Hosting Service zu finden. Aufgrund der gesammelten Anfragelast auf einen Server wird dementsprechend mehr Leistung benötigt.

| Service | Anzahl in Stk. | CPU in vCores | RAM in GB | SSD in GB | Kosten in € / p.m. |
|---------|----------------|---------------|-----------|-----------|--------------------|
| EC2     |              5 |             8 |        32 |       120 |            1160,96 |
| Hosting |              1 |            32 |        48 |      1000 |              79,99 |
| EC2     |              1 |            32 |        48 |      1000 |             569,97 |   
Tabelle 4: Kosten der AWS EC2-Instanzen und eines on-premise Servers

Durch die neue Multi-Tenant Architektur können Ressourcen in Verbindung mit Cloud Computing effizienter als auf herkömmlichen on-premise Servern genutzt werden. Daher wird eine vergleichbare Cloud Computing Infrastruktur ebenso vorgestellt.
Als Cloud Computing Anbieter wurde in der gesamten Analyse AWS gewählt, wobei auch andere Anbieter gleichwertige Services zur Verfügung stellen. Alle Kosten der AWS Services wurden mithilfe des AWS Kostenkalkulators [@AmazonWebServices2021e] ermittelt und die zugehörigen Kostenparameter, wie CPU oder Requests, mit den Werten aus der aktuellen TeamSports2 Architektur gleichgesetzt. 

Mithilfe von EC2 stellt AWS in der Cloud Serverkapazitäten zur Verfügung. Die vollständige Wartung sowie Konfiguration der Server übernimmt AWS [@AmazonWebServices2021d]. Es ist zu sehen, dass die bentöigten EC2 Instanzen circa um das Zehnfache teurer sind als die gegenwärtige Infrastruktur, wenn diese nahezu gleich in AWS aufgebaut werden würde. Des weiteren wurde in der Kalkulation mit den niedrigst möglichen Ressourcen gerechnet und es sind keine Anfragespitzen mit einberechnet. Zwar ist eine automatische vertikale Skalierung aufgrund von Parametern wie CPU Auslastung über AWS problemlos möglich, allerdings ist damit ein zusätzlicher Anstieg der ohnehin vergleichweise teuren EC2 Infrastruktur verbunden. Eine einzige EC2 Instanz mit größerem Leistungsumfang wäre zwar günstiger als fünf EC2 Instanzen, aber immer noch um einiges teurer als ein on-premise Server mit ähnlichem Leistungsumfang. 

Dem gegenüber gestellt ist eine vollständige AWS Infrastruktur. Die verwendeten Services werden zum Verständnis kurz vorgestellt:

- _S3:_ Vollständig verwalteter Objektspeicherservice [@AmazonWebServices2021].
- _Lambda:_ Ermöglichung der Ausführung von Code ohne die Konfiguration der verwendeten Ressourcen, was auch als „serverless„ bezeichnet wird [@AmazonWebServices2021a].
- _Aurora:_ Vollständig verwaltete sowie skalier- und replizierbare MySQL Datenbank [@AmazonWebServices2021b].

Mithilfe von S3 werden durch die Tenants hochgeladenen Dateien gespeichert. Die Abrechnung erfolgt nach Speicherplatz und Requests an den sogenanten S3 Bucket. Daher können die im folgenden Kapitel dargestellten Daten zu den SELECT Abfragen berücksichtigt werden. Bei Lambda wird nur die Anzahl der Requests und der damit ausgeführte Code berücksichtigt. Finden keine Requests statt, erfolgt auch keine Berechnung. Für Aurora wurde sich an den aktuellen Auslastungen des Systems orientiert.

\pagebreak

| AWS Service | Leistung                                               | Kosten in € / p.m. |
|-------------|--------------------------------------------------------|--------------------|
| S3          | 350 GB Speicherplatz  (GET-Requests berücksichtigt) | 211,95             |
| Lambda      | 1.735.586.217 Requests                                 | 344,82             |
| Aurora      | vCPU: 2, RAM: 15.25, Speicher: 10 GB                  | 212,75             |
| Redis Cache | vCPU: 2, RAM: 3.09, Knoten: 3                          | 137,03             |
| **Gesamt**  |                                                        | **906,55**         |
Tabelle 5: Kosten einer möglichen AWS Infrastruktur

Die Berechnungen zeigen, dass eine reine AWS Infrastruktur im Vergleich zur jetzigen Infrastruktur sehr viel teurer wäre. Allerdings kann dadurch eine bedarfsgerechte Skalierung ermöglicht werden, wodurch wiederum Kosteneinsparungen eintreten. Zudem können auch Entwicklerkosten besser eingesetzt werden, da sich die Entwickler nicht parallel um die Verwaltung der Infrastruktur kümmern müssen. 
Das Betreiben einer oder mehrerer EC2 Instanzen stellt dahingegen aufgrund der vergleichsweise hohen Kosten keine praktikable Lösung dar. Hier ist der vom Hostinganbieter bereitgestellte Server mit besserem Leistungsumfang, in Verbindung mit der neuen Multi-Tenant Architektur, die günstigere Variante.
Unabhängig davon, ob eine Migration in die Cloud durchgeführt wird, können mit der neuen Mulit-Tenant Architektur Kosten- sowie Verwaltungsaufwände durch die Reduktion der Server eingespart werden.

\pagebreak

## Häufige Queries 

Mit der aktuellen Architektur setzt sich bei allen Produktivservern der Hauptteil der Datenbankabfragen aus SELECT Abfragen zusammen. Die in den nachfolgenden Diagrammen festgestellten Werte für den Produktiv eins Server finden sich bei den Produktivservern zwei, drei und vier ebenso wieder. Alle Diagramme (Abbildung 14 und 15) und Zahlen (Tabelle 6) wurden aus dem MySQL Statistik Dashboard generiert.

![](source/figures/Queries-diagram_Live1.png)
Abbildung 14: Queries gegen die Datenbank auf dem Produktivserver 1

Einzig der Testserver weist einen vergleichsweise geringeren Prozentsatz bei den SELECT Abfragen auf. Die 21 Prozent an SET OPTION Abfragen lassen sich mit der Tatsache erklären, dass beim Generieren einer neuen Seite die Datenbank für die jeweilige Instanz neu erstellt wird und somit Optionen, wie das Passwort, gesetzt werden müssen [@OracleCorporation2021]. 

![](source/figures/Queries-diagram_Generator.png)
Abbildung 15: Queries gegen die Datenbank auf dem Testserver

Über alle fünf Server verteilt sich die genaue Anzahl der SELECT Abfragen wie in folgender Tabelle zu sehen ist.

|           | SELECT Abfragen |            |             |
|-----------|-----------------|------------|-------------|
| Server    |    ø pro Stunde |  ø pro Tag | ø pro Monat |
| Produktiv 1    | 173.500         | 4.164.000  | 124.920.000 |
| Produktiv 2    | 302.700         | 7.264.800  | 217.944.000 |
| Produktiv 3    | 308.300         | 7.399.200  | 221.976.000 |
| Produktiv 4    | 24.900          | 298.800    | 8.964.000   |
| Test | 9.388           | 225.312    | 6.759.360   |
|           |                 |            |             |
| Gesamt    | 818.788         | 19.352.112 | 580.563.360 |
Tabelle 6: SELECT Abfragen der einzelnen Server in Stück

Der hohe Anteil der SELECT Abfragen im Vergleich zu anderen Abfragen ist in diesem Fall nicht verwunderlich, da in TeamSports2 hauptsächlich Daten aus bestimmten Tabellen angefragt und keine komplexen Operationen vorgenommen werden müssen. 
Mit der neuen Multi-Tenant Architektur kann besagter Anteil nicht reduziert werden, da sowohl an den Datenbankabfragen in den Controllern als auch an der Anzahl der Tabellen keine Änderungen vorgesehen sind. Hierfür müsste zum einen das Datenbankmodell von Grund auf überdacht werden, um gegebenenfalls überflüssige Tabellen zu entfernen und die Datenstruktur für die Abfragen zu verbessern. Zum anderen wäre auch das Überarbeiten der Abfragen an die Datenbank in den Controllern notwendig, ob beispielsweise an Stellen alle Daten aus einer Tabelle geladen werden, anstatt nur die benötigten.   
Dahingegen kann mit der neuen Architektur die Anzahl der SELECT Abfragen reduziert werden. Zwar findet zu Beginn einer jeden Session ein zusätzlicher Request an die Datenbank für die Ermittlung der tenantId statt. Allerdings werden häufig angefragte Daten mittels Redis gecached und somit die Anzahl der Abfragen durch SELECT auf die Datenbank reduziert. Das Caching ist auch aufgrund der Konzentration aller Daten in einer Datenbank hilfreich, um die Last auf die Datenbank bei vielen gleichzeitigen Abfragen durch die Tenants zu reduzieren. Bereits gecachte Daten müssen somit nicht mehr von der Datenbank abgefragt werden. Der Einsatz von Redis Cache wäre zwar auch in der jetzigen Architektur möglich, zumal CakePHP diesen untersützt. Allerdings darf nicht vergessen werden, dass ein RedisCache für jede einzelne Datenbank in einer Single-Tenant Anwendung hohe Kosten sowie Wartungs- und Implementierungsaufwände verursacht, wodurch sich die Performancevorteile wieder relativieren würden.  
Des weiteren kann durch Multi-Tenant eine leichtere horizontale Skalierung der Datenbank erwirkt werden, was ebenso eine Verringerung der Datenbankabfragen ermöglicht. Mithilfe von Aurora Replikationen können Kopien der Daten aus der primären Datenbank erstellt werden, worauf die einzelnen Tenants zugreifen können. Somit müssen die Daten bei gleichen Abfragen nicht immer neu aus der primären Datenbank geladen werden. [@AmazonWebServices2021c].


<!--

|           | **DB-Verbindungen** |              |               |             |
|-----------|--------------|--------------|---------------|-------------|
| **Server**    | *ø pro Stunde* | *ø pro Minute* | *ø pro Sekunde* | 
| Produktiv 1    |      210.691 |        3.512 |            59 | 
| Produktiv 2    |      363.582 |        6.060 |           101 | 
| Produktiv 3    |      364.066 |        6.068 |           101 | 
| Produktiv 4    |       38.543 |          642 |            11 | 
| Test |       15.544 |          259 |             4 | 
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



