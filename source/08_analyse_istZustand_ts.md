# Analyse des IST-Zustands von TeamSports2

TeamSports2 ist ein Content-Management-System für Sportvereine. Das System wurde entwickelt um Sportvereinen den Aufbau einer leicht bedienbaren, attraktiven sowie kostengünstigen Vereinshomepage zu ermöglichen. Die Idee dafür entstand aufgrund des Bedarfs einer neuen Homepage im eigenen Handballverein. 
Die Faktoren Zeit und Pflegen stellen jeweils ein zentrales Kriterium für die Vereine dar, wenn es um den Aufbau oder Relaunch einer Vereinshomepage geht. Die Aufbauarbeit sowie Pflege soll nicht lediglich einer Person obliegen, die den Aufwand alleine betreiben muss. Des weiteren ist auch nicht davon auszugehen, dass es im Verein ausgebildete Programmierer oder ähnlich technikaffine Personen gibt. Zuletzt ist natürlich auch der zeitliche Aufwand ein wichtiger Faktor, weswegen auch hier eine unkompliziert sowie einfache Bedienung wichtig ist.

## Grundlegende Produktstruktur

Der Verein erstellt sich über die Homepage teamsports2.de eine Testseite. Dabei gibt der Verein lediglich persönliche Daten, die anfängliche Seitenfarbe sowie den gewünschten Seitennamen an. Daraufhin wird eine aus diesen Daten eine Testinstanz mit eigener zugehöriger Datenbank generiert. Die Seite enthält anfangs einige Testberichte sowie rudimentären Inhalt um den Nutzer den Einstieg zu erleichtern. Ein beispielhaftes Frontend einer neue generierten Testseite ist in der nachfolgendenden Abbildung zu sehen.

![](source/figures/TeamSports2_Frontend.png)
Abbildung 4: Ansicht TeamSports2 Frontend

Der Testseite befindet sich dann unter einer Subdomain, wie hm.teamsports2.de, mit einer eigenen Datenbank auf einem Apache-Server. Über die Subdomain sowie die mit dem generieren der Seite erstellten Zugangsdaten kann der Nutzer die Seite nach seinen Wünschen bearbeiten. Der Aufbau des Backends ist in Abbildung fünf aufgezeigt.

![](source/figures/TeamSports2_Backend.png)
Abbildung 5: Ansicht TeamSports2 Backend

Der Nutzer hat bei der Gestaltung seiner Seite sehr viele Optionen, welche vom auswählen des eigenen Designs bis hin zu vereinsspezifischen Funktionalitäten reichen. Um einen Eindruck des Systems zu vermitteln werden nachfolgend einige wichtige Funktionen aufgezeigt.

- _Template und Farben:_ Es kann zwischen verschiedenen Templates, die eine jeweils unterschiedliche Anordnung des Menüs, Headerbildes und grundsätzlichen Aufbau der Seitenelemente darstellen, gewählt werden. Des weiteren können die Vereinsfarben auf der Seite mittels verschiedener Optionen zur Gestaltung der Haupt-, Schrift- und Hintergrundfarbe realisiert werden.

- _Seitenaufbau mittels Elementen:_ Jede vom System generierte Seite kann mit verschiedenen Elementen bestückt werden. Ein Element kann wie ein Baustein, welcher aus cakePHP, HTML und CSS Code besteht, gesehen werden. Dies unterscheiden sich jeweils in der Darstellung der Informationen. Dabei können Informationen, der Mannschaftskader, Vereins- oder Teamberichte sein, welche der Nutzer zurvor im Backend eingegeben hat. 

- _Nutzerrollen:_ Für jede Instanz kann es einen oder mehrere Administrationsbenutzer geben. Diese können auf alle Bereiche, wie in Abbildung 5 oben rechts zu sehen ist, zugreifen. Darunter gibt es aber auch andere Nutzerrollen, welche beispielsweise als Trainer nur ihr jeweiliges Team bearbeiten können.

- _Teams:_ Für jede Abteilung innerhalb des Vereins beziehungsweise der Instanz können Teams beliebig viele Teams angelegt werden. Jedes Team beinhaltet dabei Spielerkader, Bilder sowie die Ergebnisse aus dem Ligabetrieb. All diese Informationen werden im Frontend angezeigt.

Sollte sich der Nutzer vor oder nach Ablauf der 90 Tage für den Kauf seiner Testseite entscheiden wird die Instanz vom sogenannten Generatorserver, wo die Instanz für den Testzeitraum liegt, auf einen Liveserver kopiert.

## Deployment-Prozess und Infrastruktur

Aktuell umfasst TeamSports2 über 150 Live-Instanzen unter einer jeweils eigenen Domain. Diese sind auf vier Apache-Server mit einem Ubunutbetriebssystem verteilt, welche von einem externen Hostinganbieter gehostet werden. Die Daten jeder Instanz liegen auf jeweils einer eigenen MySQL-Datenbank, welche wiederum auf dem Server gespeichert ist.

Anhand nachfolgender Abbildung wird der Entwicklungs- sowie Deploymentprozess dargestellt.

![](source/figures/TeamSports2_Deployment.png)
Abbildung 6: Deployment-Prozess TeamSports2

In mehreren GitHub-Repositories liegt der Code der Produktionsumgebung mit Entwicklungs(Dev)- sowie Masterbranches. Zum Entwickeln von Code wird ein eingenständiger Entwicklungsserver bereitgestellt, worunter zur Produktonsumgebung adäquate Testseiten erstellt werden können. Dadurch muss nicht auf Liveservern, welche die Kundeninstanzen betreffen würde, getestet werden. Soll neuer Code entwickelt werden kann sich vom Entwicklungs- oder Masterbranch der aktuell Code geholt und ein Featurebranch erstellt werden.
Mithilfe des Webservices  „DeployHQ“ wurde eine Build- und Deployment Pipeline aufgebaut. DeployHQ ist mit GitHub verbunden, wodurch auch alle mit dem Repository verbundenen Featurebranches auf die gewünschte Testinstanz deployed werden können.
Ist der Entwickler mit dem Code fertig wird ein PullRequest gestellt. Sobald dieser geprüft und der PullRequest genehmigt wurde, erfolgt ein Merge in den Entwicklungsbranch. Hier werden bei größeren Releases mehrere Features oder Anpassungen gesammelt um diese dann gemeinsam in den Masterbranch zu mergen.

Sobald der für den Release relevante Code vollständig im Masterbranch liegt kann die Deployment-Pipeline angestoßen werden. Dabei wird vom Masterbranch der aktuelle Code synchronisiert und dann durch das Secure Shell (SSH) Protokoll an die Server übertragen. Jede Instanz hat in DeployHQ eine eigene Konfiguration, wodurch festgelegt werden kann auf welchen Pfad auf dem jeweilgen Server deployed werden soll. Der gesamte Code des Masterbranches muss dabei auf jede Instanz einzeln gespielt werden. 
Beim Deployen werden nur Dateien an die Kundeninstanzen übertragen, welche nicht durch eine Individualisierung, wie die Datei für das Favicon, auf der Instanz betroffen sein können. Bedeutet es werden die Controller, Models und die View deployed, wobei Controller und Model jeweils separat von der View deployed werden. 

## Architektur 


![](source/figures/MVC_TS2.png)
Abbildung 7: Model-View-Controller Architektur bei TeamSports2


Insbesondere soll hier das MVC Prinzip mit cakePHP erklärt werden und wie dieses in TeamSports umgesetzt wurde. Die Zusammenhänge sollen auch in er Abbildung dargestellt werden.


## Datenbankmodell 

Ursprünglicher Plan war, das gesamte Datenbankmodell von TeamSports abzubilden. Mit allen Beziehungen, Tabellen etc. Da dies aber m.E. zu umfangreich ist, soll auf die wichtigsten Tabellen (temas, departments etc.) und deren Struktur sowie Beziehungen eingegangen werden. Auch hierfür ist eine Abbildung vsl sinnvoll.
