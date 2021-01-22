# Quantitative Analyse der aktuellen sowie neuen Architektur 

Ich habe hier jetzt einfach mal die aus meiner Sicht wichtigsten Daten gesammelt. Zur neuen Architektur muss ich mal schauen, wie ich die Kosten/Queries da am besten angebe. Das kann, aufgrund der theoretischen Annäherung, eben nicht allzu genau gesagt werden. Daher auch der Vergleich zu AWS, da ich auf das Cloud-Thema auch recht stark bei den technischen Grundlagen eingegangen bin und ich dadurch auf jedne Fall einen Vergleich zu einer alternativen Cloud-Architektur habe.
Bei dem Kapitel zu den häufigsten Queries werde ich nicht alle Diagramm einsetzen, da Live 1-3 nahezu gleiche Werte aufweisen.


|           | **Datenbankgröße** | **SSD** |                 |              | 
|-----------|----------------|------------------|-----------------|--------------|-----------|
| **Server**    | *in MB*          | *Auslastung in GB* | *Auslastung in %* | *Gesamt in GB* | 
| Live 1    |           76,5 |             83,8 |            27,7 |        302,3 |         |
| Live 2    |          122,7 |            111,4 |            22,1 |        503,8 |         |
| Live 3    |           83,6 |             76,1 |            15,1 |        503,8 |         |
| Live 4    |           11,4 |             14,3 |             2,8 |        503,8 |          |
| Generator |          184,5 |             29,5 |             9,8 |        302,3 |         |
Tabelle X: Serverkapazitäten TeamSports2




## Kosten 

Bei den Kosten soll bewusst der Vergleich zwischen den drei Optionen gezogen werden um klarzustellen, dass  eine "halbe" Migration (nur EC2-Istnanzen anmieten) kostentechnisch mit einer vollen Migration gleich zusetzen ist. Bei der vollen Migration aber der Verwaltungsaufwand sowie die Möglichkeit der Skalierung um einiges flexibler ist.
Zudem soll darauf eingegangen werden, dass eben der größte Vorteil bei der Mulit-Tenancy in der guten Skalierbarkeit in Verbindung mit der Cloud liegt.
Zuletzt auch noch ein kleiner Abstecher darauf, dass die Kosten in der Cloud zwar höher im Vergleich zur jetzigen on-premise Infrastruktur, aber man auch den niedrigeren Verwaltungsaufwand sehen muss, was wiederum Entwicklerkosten einspart die anderen Stellen eingesetzt werden können.

Mir ist bewusst, dass ich aufpassen muss nicht zu sehr in die Cloudrichtung abzudriften. Das werde ich auch in der theoretischen Architektur beachten.


| Server    | Instanzen | CPU in vCores | RAM in GB | Kosten in € / p.m.  | 
|-----------|-----------|---------------|-----------|---------------------|
| Live 1    |        34 |             6 |        12 |               16,99 |            
| Live 2    |        60 |            12 |        24 |               24,99 |            
| Live 3    |        61 |            12 |        24 |               24,99 |            
| Live 4    |         5 |            12 |        24 |               24,99 |             
| Generator |        90 |             6 |        12 |               17,00 |             
| **Gesamt** |         |              |         |               108,96 |       
Tabelle X: Aktuelle Gesamtkosten pro Server bei TeamSports2

Hier soll verglichen werden, was die aktuelle Infrastruktur quasi einfach in die Cloud adaptiert Kosten würde.

| Service | CPU in vCores | RAM in GB | SSD in GB | Kosten in € / p.m.  | 
|---------|---------------|-----------|-----------|---------------------|
| EC2     |             8 |        32 | 120       |            1.160,96 |     
Tabelle X: Kosten für fünf EC2-Instanzen bei AWS

| AWS Service | Leistung                                               | Kosten in € / p.m. |
|-------------|--------------------------------------------------------|--------------------|
| S3          | 350 GB Speicherplatz  (SELECT Abfragen berücksichtigt) | 211,95             |
| Lambda      | 1.735.586.217 Requests                                 | 344,82             |
| Aurora      | vCPU: 2, RAM: 15.25, Speicher: 500 GB                  | 680,50             |
|             |                                                        |                    |
| Gesamt      |                                                        | 1237,27            |
Tabelle X: Kosten einer möglichen vollständigen AWS Infrastruktur



## Häufige Queries 

|           | **DB-Verbindungen** |              |               |             |
|-----------|--------------|--------------|---------------|-------------|
| **Server**    | *ø pro Stunde* | *ø pro Minute* | *ø pro Sekunde* | 
| Live 1    |      210.691 |        3.512 |            59 | 
| Live 2    |      363.582 |        6.060 |           101 | 
| Live 3    |      364.066 |        6.068 |           101 | 
| Live 4    |       38.543 |          642 |            11 | 
| Generator |       15.544 |          259 |             4 | 
Tabelle X: Datenbank-Verbindungen TeamSports2


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
Tabelle X: SELECT Abfragen in Stück



![](source/figures/Queries-diagram_Generator.png)
Abbildung X: Queries gegen die Datenbank auf dem Generator Server

![](source/figures/Queries-diagram_Live1.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 1 Server

![](source/figures/Queries-diagram_Live2.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 2 Server

![](source/figures/Queries-diagram_Live3.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 3 Server

![](source/figures/Queries-diagram_Live4.png)
Abbildung X: Queries gegen die Datenbank auf dem Live 4 Server

