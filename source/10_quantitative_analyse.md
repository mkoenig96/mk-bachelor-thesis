# Quantitative Analyse der aktuellen sowie neuen Architektur 


Hier sollen die Kosten der aktuellen und möglichen neuen Architektur ggü. stellen. Bei häufigen Queries soll analysiert werden wie diese durch MultiTenant verbesser bzw. performanter gestaltet werden können.

Ein Gedanke (hattest du ja auch schon angebracht) war noch ein weiteres Kapitel miteiner Qualitativen Analyse zu machen. Hier möchte ich analysieren welche Verbesserungsmöglichkeiten es beim aktuellen System gibt und wie diese durch MultiTenant realisiert werden können. Aber auch ob es evtl. Nachteile gibt TS2 auf Multi Tenant umzustellen. 


| Server    | SUM DBs in MB | Festplattenauslastung in GB | Festplattenauslastung in % | Festplattenspeicher Gesamt in GB | Anzahl Instanzen |
|-----------|---------------|-----------------------------|----------------------------|----------------------------------|------------------|
| Live 1    |          76,5 |                        83,8 |                       27,7 |                            302,3 |               34 |
| Live 2    |         122,7 |                       111,4 |                       22,1 |                            503,8 |               60 |
| Live 3    |          83,6 |                        76,1 |                       15,1 |                            503,8 |               61 |
| Live 4    |          11,4 |                        14,3 |                        2,8 |                            503,8 |                5 |
| Generator |         184,5 |                        29,5 |                        9,8 |                            302,3 |               90 |

## Kosten 

```
SELECT table_schema "localhost", ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" FROM information_schema.tables GROUP BY table_schema
```


## Häufige Queries 

|           | DB-Verbindungen |              |               |             |
|-----------|--------------|--------------|---------------|-------------|
| **Server**    | *ø pro Stunde* | *ø pro Minute* | *ø pro Sekunde* | *Gesamt*      |
| Live 1    |      210.691 |        3.512 |            59 | 354.673.454 |
| Live 2    |      363.582 |        6.060 |           101 | 747.092.582 |
| Live 3    |      364.066 |        6.068 |           101 | 747.007.146 |
| Live 4    |       38.543 |          642 |            11 | 80.165.996  |
| Generator |       15.544 |          259 |             4 | 16.326.900  |




