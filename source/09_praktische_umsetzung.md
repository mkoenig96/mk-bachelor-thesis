# Praktische Umsetzung

Das Kapitel der praktischen Umsetzung zeigt in diesem Fall eine theoretische Annäherung auf, wie TeamSports2 in eine Multi-Tenant Architektur überführt werden kann, welche Stellen im System bei der Migration kritisch umzusetzen sind und wie eine praktische Herangehensweise an die Migration aussehen kann.
Ursprünglich war geplant, einen Prototypen für eine Multi-Tenant Archtitektur mit TeamSports2 zu entwickeln. Dabei sollten zwei Instanzen in eine Multi-Tenant Architektur migriert werden. Nach Beginn der Umsetzung wurde allerdings schnell klar, dass dies für diese Arbeit, ein zu umfangreiches Unterfangen ist. Die Komplexität des Systems in den verschiedenen Models, Views und Controllern sowie das Datenbankmodell ließen eine Migration in der vorgegebenen Zeit nicht zu. Daraufhin wurde ein weiterer Versuch unternommen, indem ein kleiner Teil der Komponenten aus dem bestehenden System herausgelöst wurde um damit eine Multi-Tenant Architektur zu erreichen. Auch dieser Versuch scheiterte, da die Instanzen, ohne die fehlenden Komponenten und durch die bestehenden Abhängigkeiten nicht mehr funktioniert hätten. 

## Vorgehensweise

Bevor mit der eigentlichen Migration begonnen wird, ist es wichtig das bestehende System kritisch zu betrachten und sich bewusst zu machen welches Ziel mit der Migration verfolgt werden soll.
Die Analyse des Systems im vorherigen Kapitel hat aufgezeigt, an welchen Stellen das System effektiver gestaltet werden kann. Zudem sollen folgende Leitfragen helfen an den richtigen Stellen anzusetzen um die neue Architektur planen zu können.

1. Welche Ziele sollen mit der Migration in eine Mulit-Tenant Architektur verfolgt werden?
1. An welchen Stellen ist Code implementiert, welcher für alle Instanzen gleich ist?
1. An welchen Stellen ist Code implementiert, welcher durch die Instanzen individualisiert wird?
1. Wie sieht die aktuelle Datenbankstruktur aus und wie soll diese für Multi-Tenant gestaltet werden?

Die Fragen können nun auf das TeamSports2-System adaptiert werden.

 1. Ziele
    - Schnelleres sowie 




## Neue Architektur



## Implementierung



