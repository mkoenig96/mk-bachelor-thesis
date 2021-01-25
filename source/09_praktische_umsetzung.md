# Praktische Umsetzung

Das Kapitel der praktischen Umsetzung zeigt in diesem Fall eine theoretische Annäherung auf, wie TeamSports2 in eine Multi-Tenant Architektur überführt werden kann, welche Stellen im System bei der Migration kritisch umzusetzen sind und wie eine praktische Herangehensweise an die Migration aussehen kann.
Ursprünglich war geplant, einen Prototypen für eine Multi-Tenant Archtitektur mit TeamSports2 zu entwickeln. Dabei sollten zwei Instanzen in eine Multi-Tenant Architektur migriert werden. Nach Beginn der Umsetzung wurde allerdings klar, dass dies für diese Arbeit, ein zu umfangreiches Unterfangen ist. Die Komplexität des Systems in den verschiedenen Models, Views und Controllern sowie das Datenbankmodell ließen eine Migration in der vorgegebenen Zeit nicht zu. Daraufhin wurde ein weiterer Versuch unternommen, indem ein kleiner Teil der Komponenten aus dem bestehenden System herausgelöst wurde um damit eine Multi-Tenant Architektur zu erreichen. Auch dieser Versuch scheiterte, da die Instanzen, ohne die fehlenden Komponenten und durch die bestehenden Abhängigkeiten nicht mehr funktioniert hätten. 

## Vorgehensweise

Bevor mit der eigentlichen Migration begonnen wird, ist es wichtig das bestehende System kritisch zu betrachten und sich bewusst zu machen welches Ziel mit der Migration verfolgt werden soll.
Die Analyse des Systems im vorherigen Kapitel hat aufgezeigt, an welchen Stellen das System effektiver gestaltet werden kann. Zudem sollen folgende Leitfragen helfen an den richtigen Stellen anzusetzen um die neue Architektur planen zu können.

1. Welche Ziele sollen mit der Migration in eine Mulit-Tenant Architektur verfolgt werden?
1. An welchen Stellen ist Code implementiert, welcher für alle Instanzen gleich ist?
1. An welchen Stellen ist Code implementiert, welcher durch die Instanzen individualisiert wird?
1. Welche Strategie soll bei der Datenbankstruktur künftig verfolgt werden?

Die Fragen können nun auf das TeamSports2-System adaptiert werden.

1. Ziele
    - Schnelleres sowie leichter konfigurierbares Depyloment
    - Reduzierung von doppelten Code über die verschiedenen Instanzen
    - Reduktion der Kosten durch effizientere Ressourcennutzung
1. Gleicher Code
    - Die Models und Controller sind bei jeder Instanz gleich. Aktuell stellt der jeweilige Ordner in der Instanz einen Symlink dar, welcher auf einen zentralen Ordner auf dem Server verweist. 
    - Teile der Views werden ebenso auf allen Instanzen gleich dargestellt. Beispielsweise ist die Grundstruktur der Backend-Views bei jeder Instanz gleich.
1. Unterschiedlicher Code
    - Alle Views die wiederum auf die view_elements über einen Symlink verweisen sind bei jeder Instanz unterschiedlich. Wenn der Nutzer seinen Seitenaufbau, beispielsweise bei der Mannschaftsansicht, abwandelt wird die View nach jedem Speichervorgang mit den geänderten Elementen neu geschrieben.
    - Jede Instanz kann eigene Dateien (Mannschaftsfotos, Spielerfotos, PDFs, Bildergalerien) hochladen. Dafür ist in jeder Instanz ein webroot Ordner enthalten, der beispielsweise beim Hochladen eines Mannschaftsfotos um den jeweiligen Teamordner erweitert wird. Gleiches gilt für das Hochladen anderer Dateien.
1. Datenbankstruktur
    - Das in jeder Datenbank-Instanz zugrunde liegende Datenbankmodell (Tabellen, Schlüssel, Attribute) ist adäquat.
    - Die Datenbankstruktur sollte weitestgehend erhalten werden.

Grundsätzlich sollen in der neuen Architektur möglichst viele bestehende Komponenten der jetzigen Implementierung erhalten bleiben. Dies ist vorallem durch die von CakePHP vorgegebenen Namenskonvetionen begründet. Eine Umbenennung von Actions, Models, Views, Controllern und anderen Komponenten würde demnach auch immer die verknüpften Komponenten betreffen. Um dem vorzubeugen sollen die Kernkomponenten möglichst unberührt bleiben. 

## Datenbankmodell

Der Hintergrund, die aktuelle Datenbankstruktur weitestgehend zu erhalten liegt an der Verknüpfung der Tabellen mit den Models. Müsste das gesamte Datenbankmodell neu gestaltet werden, so würde damit auch ein Rebuild aller knapp 60 Models einhergehen. Nicht nur die Models müssten, aufgrund von anderen Tabellennamen, umbenannt werden, auch die zugehörigen Controller sowie alle Beziehungen zwischen den Models müssten neu gesetzt werden. 
Die Umstellung auf eine NoSQL Datenbank ist in diesem Falle nicht sinnvoll, da sowohl Ausfalltoleranz als auch die hohe Verfügbarkeit bei Homepages von Sportvereinen nur bedingt gegeben sein müssen. Dahingegen ist die Integrität sowie Konsistenz ein ausschlaggebendes Argument um weiterhin eine SQL Datenbank, in Form von MySQL, zu nutzen. Da mehrere Benutzer auf die Datenbank zugreifen und auch bei jeder Instanz stets mehrere Benutzer im Backendbereich Änderungen vornehmen können ist die Datenbankkonsistenz eine Funktionalität, welche erfüllt sein muss und durch eine NoSQL Datenbank nicht gegeben wäre. Nichtzuletzt ist das CakePHP Framework allein in seiner Grundfunktionlität für eine relationale Datenbank ausgelegt. Dies zeigt sich unter anderem an dem von CakePHP bereitgestellten CRUD-Prinzip, wonach die SQL-Befehle INSERT, SELECT, UPDATE und DELETE als SQL-Datenbankoperationen direkt vom Framework unterstützt sowie angewendet um Daten während der Laufzeit zu selektieren [@Ammelburger2008 5].

Bei den notwendigen Tabellen, soll eine neue Tabellenspalte hinzugefügt werden um den richtigen Tenant identifizieren zu können. 
Der Ausschnitt des Datenbankmodells wurde dementsprechend angepasst.

![](source/figures/TS2_AusschnittDB-Modell_MultiTenant.png)
Abbildung 12: Ausschnitt Datenbankmodell mit Multi-Tenant TeamSports2



```
ALTER TABLE teams 
ADD COLUMN tenantId int(11) NOT NULL PRIMARY KEY
```

```
ALTER TABLE teams 
DROP PRIMARY KEY, 
ADD COLUMN tenantId varchar(11) NOT NULL, 
ADD PRIMARY KEY (id,tenantId)
```








## Implementierung



