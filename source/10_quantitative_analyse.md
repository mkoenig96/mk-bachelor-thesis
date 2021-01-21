# Quantitative Analyse der aktuellen sowie neuen Architektur 


Hier sollen die Kosten der aktuellen und möglichen neuen Architektur ggü. stellen. Bei häufigen Queries soll analysiert werden wie diese durch MultiTenant verbesser bzw. performanter gestaltet werden können.

Ein Gedanke (hattest du ja auch schon angebracht) war noch ein weiteres Kapitel miteiner Qualitativen Analyse zu machen. Hier möchte ich analysieren welche Verbesserungsmöglichkeiten es beim aktuellen System gibt und wie diese durch MultiTenant realisiert werden können. Aber auch ob es evtl. Nachteile gibt TS2 auf Multi Tenant umzustellen. 

## Kosten 




## Häufige Queries 

|           | DB-Verbindungen |              |               |             |
|-----------|--------------|--------------|---------------|-------------|
| **Server**    | *ø pro Stunde* | *ø pro Minute* | *ø pro Sekunde* | *Gesamt*      |
| Live 1    |      210.691 |        3.512 |            59 | 354.673.454 |
| Live 2    |      363.582 |        6.060 |           101 | 747.092.582 |
| Live 3    |      364.066 |        6.068 |           101 | 747.007.146 |
| Live 4    |       38.543 |          642 |            11 | 80.165.996  |
| Generator |       15.544 |          259 |             4 | 16.326.900  |




