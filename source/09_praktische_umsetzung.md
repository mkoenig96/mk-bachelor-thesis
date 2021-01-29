# Praktische Umsetzung

Das Kapitel der praktischen Umsetzung zeigt in diesem Fall eine theoretische Annäherung auf, wie TeamSports2 in eine Multi-Tenant Architektur überführt werden kann und welche praktische Herangehensweise für die Migration verwendet wurde.
Ursprünglich war geplant, einen Prototypen für eine Multi-Tenant Archtitektur mit TeamSports2 zu entwickeln. Dabei sollten zwei Instanzen in eine Multi-Tenant Architektur migriert werden. Nach Beginn der Umsetzung wurde allerdings klar, dass dies für diese Arbeit, ein zu umfangreiches Unterfangen ist. Die Komplexität des Systems in den verschiedenen Models, Views und Controllern sowie das Datenbankmodell ließen eine Migration in der vorgegebenen Zeit nicht zu. Daraufhin wurde ein weiterer Versuch unternommen, indem ein kleiner Teil der Komponenten aus dem bestehenden System herausgelöst wurde um damit eine Multi-Tenant Architektur zu erreichen. Auch dieser Versuch scheiterte, da die Instanzen, ohne die fehlenden Komponenten und durch die bestehenden Abhängigkeiten nicht mehr funktioniert hätten. 

## Vorgehensweise

Bevor mit der eigentlichen Migration begonnen wird, ist es wichtig das bestehende System kritisch zu betrachten und sich bewusst zu machen welches Ziel mit der Migration verfolgt werden soll.
Die Analyse des Systems im vorherigen Kapitel hat aufgezeigt, an welchen Stellen das System effizienter gestaltet werden kann. Zudem sollen folgende Leitfragen helfen an den richtigen Stellen anzusetzen und um die neue Architektur planen zu können.

1. Welche Ziele sollen mit der Migration in eine Mulit-Tenant Architektur verfolgt werden?
1. An welchen Stellen ist Code implementiert, welcher für alle Instanzen gleich ist?
1. An welchen Stellen ist Code implementiert, welcher durch die Instanzen individualisiert wird?
1. Welche Strategie soll bei der Datenbankstruktur künftig verfolgt werden?

Diese Fragen können nun auf das TeamSports2-System adaptiert werden.

1. Ziele
    - Schnelleres sowie leichter konfigurierbares Depyloment.
    - Reduzierung von doppelten Code über die verschiedenen Instanzen hinweg gesehen.
    - Reduktion der Kosten durch effizientere Ressourcennutzung
1. Gleicher Code
    - Die Models und Controller sind bei jeder Instanz gleich. Aktuell stellt der jeweilige Ordner in der Instanz einen Symlink dar, welcher auf einen zentralen Ordner auf dem Server verweist. 
    - Einige Views werden ebenso auf allen Instanzen übereinstimmend dargestellt. Beispielsweise ist die Grundstruktur der Backend-Views bei jeder Instanz gleich.
    - Der Ordner view_elements stellt für alle Instanzen dieselben Elemente bereit. 
1. Unterschiedlicher Code
    - Alle Views die wiederum auf die view_elements über einen Symlink verweisen sind bei jeder Instanz unterschiedlich. Wenn der Nutzer seinen Seitenaufbau, beispielsweise bei der Mannschaftsansicht, abwandelt wird die View nach jedem Speichervorgang mit den geänderten Elementen neu geschrieben.
    - Jede Instanz kann eigene Dateien (Mannschaftsfotos, Spielerfotos, PDFs, Bildergalerien) hochladen. Dafür ist in jeder Instanz ein webroot Ordner enthalten, der beispielsweise beim Hochladen eines Mannschaftsfotos um den jeweiligen Teamordner erweitert wird. Gleiches gilt für das Hochladen anderer Dateien.
1. Datenbankstruktur
    - Das in jeder Datenbank-Instanz zugrunde liegende Datenbankmodell (Tabellen, Schlüssel, Attribute) ist gleichbleibend.
    - Die Datenbankstruktur sollte weitestgehend erhalten werden.

Grundsätzlich sollen in der neuen Architektur möglichst viele bestehende Implementierungen erhalten bleiben. Dies ist vorallem durch die von CakePHP vorgegebenen Namenskonvetionen begründet. Eine Umbenennung von beispielsweise Models, Views und Controllern würde demnach auch immer die verknüpften Komponenten betreffen. Um dem vorzubeugen sollen die Kernkomponenten möglichst erhalten bleiben. 

## Datenbankmodell

Der Hintergrund, die aktuelle Datenbankstruktur weitestgehend zu erhalten, liegt an der Verknüpfung der Tabellen mit den Models. Müsste das gesamte Datenbankmodell neu gestaltet werden, so würde damit auch ein Rebuild aller knapp 60 Models einhergehen. Nicht nur die Models müssten aufgrund von anderen Tabellennamen umbenannt werden, auch die zugehörigen Controller sowie alle Beziehungen zwischen den Models müssten neu gesetzt werden. 
Die Umstellung auf eine NoSQL Datenbank, welche die aktuelle relationale Datenbank ersetzt, ist in diesem Falle nicht sinnvoll, da sowohl Ausfalltoleranz als auch die hohe Verfügbarkeit bei Homepages von Sportvereinen nur bedingt gegeben sein müssen. Dahingegen sind Integrität sowie Konsistenz ausschlaggebende Argumente, um weiterhin eine SQL Datenbank in Form von MySQL zu nutzen. Da mehrere Benutzer auf die Datenbank zugreifen und bei jeder Instanz mehrere Benutzer im Backendbereich Änderungen vornehmen können, ist die strenge Datenbankkonsistenz eine Funktionalität, welche erfüllt sein muss und durch eine NoSQL Datenbank nicht gegeben wäre. Nichtzuletzt ist das CakePHP Framework allein in seiner Grundfunktionalität auf eine relationale Datenbank ausgelegt. Dies zeigt sich unter anderem an dem von CakePHP bereitgestellten CRUD-Prinzip, wonach die SQL-Befehle INSERT, SELECT, UPDATE und DELETE als SQL-Datenbankoperationen direkt vom Framework unterstützt sowie angewendet werden, um Daten während der Laufzeit zu selektieren [@Ammelburger2008 5]. Nichtdestotrotz wurde in der neuen Architektur eine NoSQL Datenbank in Form eines Redis Cache Key/Value Stores implementiert. Da mit der neuen Architektur sehr viele Tenants auf eine Datenbank zugreifen, sollen die Datenbankabfragen durch das clientseitige Caching für die Dauer der Nutzersession reduziert werden.     
Redis wird dabei so konfiguriert, dass die vom Besucher genutzten Primärschlüssel durch den Server gespeichert und bei Änderungen in der Datenbank nur die Primärschlüssel aktualisiert werden, welche auch wirklich vom Nutzer benötigt werden. Dies kann über die von Redis standardmäßig zur Verfügung gestellte Methodik beim clientseitigen Caching realisiert werden. [@Redis.io].

Bei den notwendigen Tabellen soll eine neue Tabellenspalte hinzugefügt werden, um den richtigen Tenant identifizieren zu können. 
Hierfür muss bei allen nötigen Tabellen folgender Befehl ausgeführt werden:

```
ALTER TABLE teams 
DROP PRIMARY KEY, 
ADD COLUMN tenantId int(11) NOT NULL, 
ADD PRIMARY KEY (id,tenantId)
```

Die Primärschlüssel der angesprochenen Tabelle müssen zuerst entfernt und dann mit dem neuen zusammengesetzten Primärschlüssel wieder angelegt werden. Dies wirkt sich nicht auf das Löschen der Fremschlüssel in der jeweiligen Tabelle aus. Der Befehl 

```
ALTER TABLE teams 
ADD COLUMN tenantId int(11) NOT NULL PRIMARY KEY
```
gibt eine Fehlermeldung unter MySQL zurück, dass mehrere Primärschlüssel definiert sind. 
Mithilfe des neuen Attributes tenantId in der jeweiligen Tabelle kann schließlich die zugehörige Instanz eindeutig identifiziert werden. Die tenantId fungiert in der Tabelle mit dem bereits bestehenden Primärschlüssel als zusammengesetzter Primärschlüssel, da nur durch tenantId und beispielsweise die teamId die richtige Zeile in der Tabelle eindeutig bestimmt weden kann. Die Position der tenantId ist bewusst an erster Stelle gesetzt, da die Reihenfolge eine Auswirkungen auf die Indizierung hat und in der Praxis eher nach allen Tenants gesucht wird, die eine gewisse id haben. Nachdem allen Tabellen die neue Spalte hinzugefügt wurde, sieht das Datenbankmodell wie folgt aus:

![](source/figures/TS2_AusschnittDB-Modell_MultiTenant.png)
Abbildung 12: Ausschnitt Datenbankmodell für Multi-Tenant Architektur bei TeamSports2

\pagebreak

Nicht bei allen Tabellen muss eine neue Spalte zur Identifikation des Tenants hinzugefügt werden, da die tenantId für einige Tabellen irrelevant ist. Ein Beispiel dafür ist die age_brackets Tabelle, worin die aktuellen Jahrgänge für die Teams enthalten sind. Die darin enthaltenen Daten sind für alle Instanzen gleich und müssen nicht separiert werden.
Da der Datentyp bei den meisten Schlüsselattributen int(11) ist, wurde dies auch für die tenantId übernommen.
An den Fremdschlüsseln sowie den zugehörigen Beziehungen in den Tabellen müssen keine weiteren Änderungen vorgenommen werden, da sich durch die Migration in Multi-Tenant keine Änderungen an den Beziehungen zwischen den Tabellen ergeben.    
CakePHP setzt standardmäßig in jeder Tabelle das Attribut id als Primärschlüssel. Daher muss in den jeweiligen Models explizit der neue zusammengesetzte Primärschlüssel aus tenantId und id angegeben werden. Dies kann mithilfe der sogenannten CakePHP Rules realisiert werden, welche es jedem Model ermöglichen von den CakePHP Konventionen abweichende Konfigurationen vorzunehmen. In diesem Fall wird der neue zusammengesetzte Primärschlüssel in einem Array an die ExistsIn Regel übergeben. [@Cookbook2021].         
Eine weitere Möglichkeit, um mit individuellen Primärschlüsseln in CakePHP zu arbeiten, bietet das Migrations Plugin an. Hierbei werden von den CakePHP Konventionen abweichende Implementierungen durch Methoden dargestellt und ausgeführt. Außerdem muss keine SQL Syntax verwendet werden [@CakePHPMigrationsCookbook2020].   
Für die bestehenden Instanzen müssen eindeutige tenantIds an die jeweiligen Tabellenattribute vergeben werden. Bei allen neu angelegten Seiten muss zudem das Ausstellen einer tenantId in den Prozess der Generierung einer neuen Seite mit eingebunden werden.
Des weiteren wird die Settings Tabelle um die tenantId erweitert. In der name Spalte, welche zugleich den Primärschlüssel der Settings Tabelle darstellt, ist die URL des Tenants im Eintrag base_url zu finden. Mithilfe folgender SQL Abfrage kann dann die passende tenantId zur jeweiligen Domain des Tenants identifiziert werden.

```
SELECT tenantId FROM `settings` 
WHERE name= 'base_url' AND value= 'domain'
```

\pagebreak

## Neue Architektur

Um den Tenant, welcher auf die Anwendung zugreifen möchte, eindeutig identifizieren zu können, soll die Domain in Verbindung mit der tenantId genutzt werden. Beim ersten Aufruf der Seite wird die Domain der aktuellen Session gespeichert und ein Request an die Settings Tabelle gesendet. In dem Request ist die URL der Seite enhalten und es wird in der Settings Tabelle nach der passenden URL gesucht. Ist diese gefunden, kann die tenantId wieder zurück an die Session geschickt werden. Während der gesamten Session bleibt die tenantId gespeichert, um nicht bei jedem Neuaufruf einer View eine Datenbankabfrage senden zu müssen. 
Über die von CakePHP bereitgestellte Session Component können die Einstellungen für jede Session sowohl allgemein als auch in jedem Controller extra gesetzt werden. Für jede Session wird von CakePHP eine Session-ID vergeben, worunter für die Gültigkeit der Session auch die tenantId zu finden ist [@Ammelburger2008 173-176]. 
Die aus der Session stammende URL soll zudem auch für die Zuordnung der richtigen View zum Controller dienen. 

![](source/figures/MultiTenantTS2.png)
Abbildung 13: Multi-Tenant Architektur TeamSports2

Nahezu der gesamte Code liegt mit der neuen Architektur nicht mehr in jeder einzelnen Instanz, sondern zentral auf dem Server. Die Instanz greift darauf zu und ruft im Controller die jeweilige Action auf. Je nachdem, ob es sich um eine für alle Instanzen gültige oder individuelle View handelt, die durch die Action aufgerufen werden soll, wird der Pfad für die View über die Action angepasst.
Wird eine allgemeine View angesprochen, kann auf den zentralen View Ordner zugegriffen werden, welcher wiederum im App Ordner liegt. Handelt es sich um eine individuelle View der Instanz, muss der Pfad zu der View in der Action explizit neu gesetzt werden. 
Das Setzen des neuen View Pfades muss auf diese Wiese nur bei den Actions geschehen, wo die View individueller Natur ist. Weitere Abfragen, um auf eine allgemeine View zu prüfen, sind im Controller nicht weiter notwendig. Die aus der Session stammende Domain der Instanz wird im Controller übergeben und als Parameter in dem Pfad, welcher zur View der Action führt, gesetzt. 

```
$sessionDomain = 'hm-teamsports2.de';

function seniors ($departmendId = null) {
    $this->viewPath = '/Tenants/$sessionDomain/app/View';
}
```
Somit ruft die seniors Action nun die passende View im zugehörigen Ordner der Instanz und nicht mehr im allgemeingültigen Ordner auf. Die Bennenung der Ordner der Tenants ist serverseitig immer gleich zur Domain der jeweiligen Instanz. Damit die in den Views der Tenants bestehenden Verweise auf die View Elements weiterhin gültig sind, wird im View Ordner der Instanz ebenso ein view_elements Ordner erstellt. Dieser stellt einen Symlink zum zentralen Pfad der View Elements auf dem Server dar. Um dem Fall vorzubeugen, dass auch allgemeingültige Views Elemente haben können, wurde im View Ordner der App ebenso ein Symlink zu den View Elements erstellt. Da die Models von den jeweiligen Controllern aufgerufen werden und für alle Tenants gleich sind, können diese im allgemeinen App Ordner verbleiben. Die Pfade für die im Tenant zugehörigen Daten im Webroot Ordner, beispielsweise PDFs und Bilder, werden mithilfe der Domain aus der Session ebenso in den zugehörigen Komponenten angepasst.




