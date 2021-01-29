# IST-Zustand von TeamSports2

TeamSports2 ist ein Content-Management-System für Sportvereine. Das System wurde entwickelt, um Sportvereinen den Aufbau einer intuitiven, attraktiven sowie kostengünstigen Vereinshomepage zu ermöglichen. Die Faktoren Zeit und Pflege der Homepage stellen jeweils ein zentrales Kriterium für die Vereine dar, wenn es um den Aufbau oder Relaunch einer Vereinshomepage geht. Durch TeamSports2 kann jeder Verein für sich entscheiden, welche und wieviele Personen die Verwaltung der Homepage übernehmen. Zudem ist die Administration der Homepage für jederman, unabhängig von Ausbildung und Weberfahrung, möglich.

## Grundlegende Produktstruktur

Der Verein erstellt sich über die Homepage teamsports2.de eine Testseite. Dabei gibt der Verein lediglich Kontaktdaten, die anfängliche Seitenfarbe sowie den gewünschten Seitennamen an. Daraufhin wird aus diesen Daten eine Testinstanz mit eigener zugehöriger Datenbank generiert. Ein beispielhaftes Frontend einer neu generierten Testseite ist in Abbildung vier zu sehen.

![](source/figures/TeamSports2_Frontend.png)
Abbildung 4: Ansicht TeamSports2 Frontend

Mit dem Erstellen der Seite werden anfangs einige Testberichte sowie rudimentäre Inhalte erstellt, um dem Nutzer den Einstieg zu erleichtern. Die Testseite befindet sich zu diesem Zeitpunkt unter einer Subdomain, beispielsweise hm.teamsports2.de. Über diese Subdomain sowie die während des Generierens erstellten Zugangsdaten kann der Nutzer die Seite nach seinen Wünschen bearbeiten. Mittels diese Zugangsdaten kann sich der Nutzer in das Backend seiner Instanz einloggen.

![](source/figures/TeamSports2_Backend.png)
Abbildung 5: Ansicht TeamSports2 Backend

Der Nutzer hat bei der Gestaltung seiner Seite sehr viele Optionen, welche über das Auswählen des eigenen Designs bis hin zu vereinsspezifischen Funktionalitäten reichen. Um einen Eindruck des Systems zu vermitteln, werden nachfolgend einige wichtige Funktionen aufgezeigt.

- _Template und Farben:_ Es kann zwischen verschiedenen Templates, die eine jeweils unterschiedliche Anordnung des Menüs, Headerbildes und grundsätzlichen Aufbau der Seitenelemente darstellen, gewählt werden. Des weiteren können die Vereinsfarben auf der Seite mittels verschiedener Optionen zur Gestaltung der Haupt-, Schrift- und Hintergrundfarbe realisiert werden.

- _Seitenaufbau mittels Elementen:_ Jede vom System generierte Seite kann mit verschiedenen Elementen bestückt werden. Ein Element kann wie ein Baustein, welcher aus CakePHP, HTML und CSS Code besteht, gesehen werden. Diese unterscheiden sich jeweils in der Darstellung der Informationen. Dabei können Informationen wie der Mannschaftskader, Vereins- oder Teamberichte sein, welche der Nutzer zuvor im Backend eingegeben hat. 

- _Nutzerrollen:_ Für jede Instanz kann es einen oder mehrere Administrationsbenutzer geben. Diese können auf alle Bereiche, wie in Abbildung fünf oben rechts zu sehen ist, zugreifen. Darunter gibt es aber auch andere Nutzerrollen, welche beispielsweise als Trainer nur ihr jeweiliges Team bearbeiten können.

- _Teams:_ Für jede Abteilung innerhalb des Vereins beziehungsweise der Instanz können beliebig viele Teams angelegt werden. Jedes Team beinhaltet dabei Spielerkader, Bilder sowie die Ergebnisse aus dem Ligabetrieb. All diese Informationen werden auch im Frontend angezeigt.

Sollte sich der Nutzer vor oder nach Ablauf der 90 Tage für den Kauf seiner Testseite entscheiden, wird die Instanz vom Testserver, wo die diese für den Testzeitraum liegt, auf einen Produktivserver kopiert.

## Deployment-Prozess und Infrastruktur

Aktuell umfasst TeamSports2 über 160 Produktivinstanzen unter einer jeweils eigenen Domain. Diese sind auf vier Apache-Server mit einem Ubuntubetriebssystem verteilt, welche von einem externen Hostinganbieter bereitgestellt werden. Anhand nachfolgender Abbildung wird der Entwicklungs- sowie Deploymentprozess dargestellt. 

![](source/figures/TeamSports2_Deployment.png)
Abbildung 6: Deployment-Prozess TeamSports2

\pagebreak

Die Daten jeder Instanz liegen auf jeweils einer eigenen MySQL-Datenbank, welche wiederum auf dem Server gespeichert ist. Aktuell ist TeamSports2 somit auf einer Singel-Tenant Architektur aufgebaut, wobei jede Instanz auf ihre eigene Anwendung sowie Datenbank zugreift. In mehreren GitHub-Repositories liegt der Code der Produktionsumgebung mit Entwicklungs- sowie Masterbranches. Zum Entwickeln von Code wird ein eigenständiger Entwicklungsserver bereitgestellt, worunter zur Produktionsumgebung äquivalente Entwicklungsseiten erstellt werden können. Dadurch muss nicht auf Produktivservern, welche die Produktivinstanzen betreffen würde, getestet werden. Soll neuer Code entwickelt werden, kann sich vom Entwicklungs- oder Masterbranch der aktuelle Code geholt und ein Featurebranch erstellt werden.
Mithilfe des Webservices DeployHQ wurde eine Build- und Deployment Pipeline aufgebaut. DeployHQ ist mit GitHub verbunden, wodurch auch alle mit dem Repository verbundenen Featurebranches auf die gewünschte Entwicklungsinstanz deployed werden können.
Ist der Entwickler mit seinem Code fertig, wird ein PullRequest gestellt. Sobald dieser geprüft und der PullRequest genehmigt wurde, erfolgt ein Merge in den Entwicklungsbranch. Hier werden bei größeren Releases mehrere Features oder Anpassungen gesammelt, um diese dann gemeinsam in den Masterbranch zu mergen.

Sobald der für den Release relevante Code vollständig im Masterbranch liegt, kann die Deployment-Pipeline angestoßen werden. Dabei wird vom Masterbranch der aktuelle Code synchronisiert und durch das Secure Shell (SSH) Protokoll an die Server übertragen. Jede Instanz hat in DeployHQ eine eigene Konfiguration, wodurch festgelegt werden kann, auf welchen Pfad auf dem jeweilgen Server deployed werden soll. Der gesamte Code des Masterbranches muss dabei auf jede Instanz einzeln gespielt werden. 
Beim Deployen werden nur Dateien an die Kundeninstanzen übertragen, welche nicht durch eine Individualisierung, wie die Datei für das Favicon, auf der Instanz betroffen sind. Somit werden Views, Models und Controller deployed, wobei die beiden letztgenannten zu den Views separiert übertragen werden.

\pagebreak

## Architektur 

TeamSports2 ist in PHP geschrieben und nutzt das CakePHP Framework, welches auf dem Model-View-Controller (MVC) Prinzip basiert. Bei diesem Prinzip werden Geschäfts-, Anwendungslogik und die Datenhaltung voneinander getrennt. 

- _Model:_ Enthält die Geschäftslogik der Anwedung, organisiert die Datenhaltung und stellt die jeweiligen Tabellen dar.
- _View:_ Stellt die Inhalte für den Benutzer mittels PHP und HTML Code dar.
- _Controller:_ Enhält die Anwendungslogik und fungiert als Vermittler zwischen View und Model.

[@Ammelburger2008 6-9].

Dadurch sind alle drei Ebenen unabhängig voneinander und logisch getrennt. Dies erleichtert die Wartung der Anwendung sowie die Implementierung neuer Funktionen. 
Mit CakePHP sollen zum einen die Vorteile des MVC Prinzips genutzt und zum anderen nützliche Funktionen seitens des Frameworks bereitgestellt werden. Ein wichtiger Bestandteil dessen ist das „Convention over Configuration“ [@Ammelburger2008 5] Prinzip, welches in CakePHP umgesetzt wird. Dabei müssen keine speziellen Konfigurationen implementiert werden, um Verknüpfungen zwischen dem Model, der View und dem Controller herzustellen. Die Verbindungen zueinander werden von CakePHP automatisch über die jeweilige Benennung der Komponenten erkannt [@Ammelburger2008 5-6]. Nachfolgende Tabelle zeigt, worauf bei der Benennung der jeweiligen Komponenenten geachtet werden muss:

| Komponente | Schreibweise | Numerus  | Beispiel        |
|------------|--------------|----------|-----------------|
| DB Tabelle | klein        | Plural   | teams           |
| Model      | groß         | Singular | Team            |
| Controller | groß         | Plural   | TeamsController |
| View       | groß         | Plural   | Teams           |

Tabelle 1: CakePHP Namenskonventionen

Anhand eines beispielhaften Aufrufs einer TeamSports2-Seite soll der Ablauf des MVC Prinzips dargestellt werden.

![](source/figures/MVC_TS2.png)
Abbildung 7: Model-View-Controller Architektur bei TeamSports2

Alle TeamSports2 Instanzen laufen auf einem Apache Server unter Ubuntu als Betriebssystem. Sendet ein Nutzer eine Anfrage an die Seite hm-teamsports.de, so wird auf das zur Domain zugehörige Verzeichnis auf dem Server zugegriffen. Für den Aufbau der URLs nutzt CakePHP das „[...] Representational State Transfer Schema, kurz REST, das für den Aufbau verteilter Informationssysteme definiert wurde“ [@Ammelburger2008 46]. „Das URL-Schema ist dabei normalerweise so aufgebaut: _http://domain/controller/action/parameter1/parameter2_“ [@Ammelburger2008 45].

Im Beispiel wird der TeamsController angesprochen, welcher die Action seniors enthält.

```
function seniors($departmentId = null)
	{
		$title = $this->getTitle("seniors", $departmentId);
		$this->set('title_for_layout', $title);
		$this->set('own_team_id', "0");
		$teams = $this->getseniors($departmentId);
		$this->set('teams', $teams);
	}
```

Der übergebene Parameter eins in der URL steht für die departmentId, welche der Action übergeben werden muss. Als Department wird in TeamSports2 eine Abteilung des Vereins bezeichnet. Dadurch, dass die View seniors.ctp wie die Action im TeamsController benannt ist, kann die View diese zuordnen. Dass die View seniors.ctp auf den TeamsController zugreifen muss, ist durch die Verzeichnisstruktur im Projekt festgelegt. Alle Views, die im View-Ordner und dort wiederum im Teams-Ordner liegen, wissen, dass sie mit dem TeamsController verbunden sind. In diesem Fall enthält die seniors View ein Element mit der Nummer 33, wobei direkt in der View auch PHP und HTML Code stehen kann. An dieser Stelle wurde ein Element verwendet, dass der Nutzer auch selbständig über den Administrationsbereich abändern kann. Grundsätzlich enthält jede Instanz Elemente, die zu Darstellungen in den Views dienen. Damit diese nicht bei jeder Instanz einzeln abgelegt werden müssen, enthält jede Instanz im View Ordner den Ordner view_elements. Dieser ist ein Symlink zum Pfad */opt/elements/view* auf demselben Server. Unter diesem Pfad findet sich dann auch das passende Element. In der View seniors.ctp wird das Element 33 mit folgendem Code aufgerufen.

```
<?php
echo $this->element('view_elements/33');
?>
```

CakePHP erkennt mithilfe von *$this->element*, dass auf den View Ordner, worin der Elements Ordner liegt, zugegriffen werden muss. Die gleiche Ordnerstruktur ist im Controller Ordner auch auf dem Server vorzufinden mit der Ergänzung des view_elements Ordners. Somit wird im Ordner View der Unterordner Elements gesucht. In letzterem ist dann der view_elements Symlink enthalten, welcher das Element 33 enthält.
Der TeamsController steht, ebenfalls durch die Namenskonvention, in Beziehung mit dem Team Model. CakePHP erkennt auch an dieser Stelle, dass in der Datenbank eine Tabelle namens teams besteht und verküpft das Model mit der Tabelle. Im Model werden dann unter anderem die relationalen Beziehungen der Models untereinander definiert.
Beispielsweise drückt $belongsTo aus, dass zwischen dem AgeBracket sowie Department Model eine n:1 Beziehung besteht.

```
public $belongsTo = array(
        "AgeBracket" => array(
            "className" => "AgeBracket"
        ),
        "Department" => array(
            "className" => "Department"
        )
    );
```
Weitere relationale Beziehungen können wie folgt dargestellt werden.

- 1:1 mit hasOne
- 1:n mit hasMany
- m:n mit hasAndBelongsToMany

[@Ammelburger2008 73].

Durch den Parameter in der URL können alle Seniorenteams der Abteilung mit department_id = 1, in diesem Fall die Fußballabteilung, aufgerufen werden. Die unter *hm-teamsports.de/teams/seniors/1* aufgerufene Seite wird dann wie folgt angezeigt.

![](source/figures/SeniorsView.png)
Abbildung 8: Seniors.ctp View bei TeamSports2

\pagebreak

## Datenbankmodell 

Aufgrund der verwendeten MySQL Datenbank ist das gesamte Datenbankmodell relational. Die nachfolgende Abbildung zeigt einen Ausschnitt des aktuellen Datenbankmodells mit einigen Haupttabellen, welches in der dritten Normalform vorliegt.

![](source/figures/TS2_AusschnittDB-Modell.png)
Abbildung 9: Ausschnitt Datenbankmodell TeamSports2

In der teams Tabelle sind alle Daten zum Team zu finden. Jede Abteilung kann beliebig viele Teams beinhalten, weswegen die department_id ebenso in der teams Tabelle hinterlegt ist. Das Team kann zudem einen Team- oder Spielbericht schreiben. Hierfür besteht eine Verknüpfung mit der articles Tabelle. Die departmens Tabelle stellt alle Informationen zu einer Abteilung dar. Auch hier kann eine beliebige Anzahl von Abteilungen angelegt werden. Zudem gibt es in jedem Team einen oder mehrere Trainer sowie einen oder mehrere Spieler. Diese werden jeweils über die Zuordnungstabellen team_players sowie teams_trainers dem jeweiligen Team zugeordnet. Prinzipiell ist jeder Spieler oder Trainer immer auch ein Nutzer aus der users Tabelle. In dieser sind alle nutzerspezifischen Daten, wie der Name oder das Passwort, enthalten. Wird einem Nutzer beispielsweise eine Trainer- oder Spielerrolle zugewiesen, entsteht eine Verknüpfung zur players oder trainers Tabelle. 

## Analyse

TeamSports2 ist ein Software as a Service Produkt, welches eine einfache sowie schnelle Erstellung einer voll funktionsfäigen Webseite ermöglicht. Dabei soll die zugrundeliegende Infrastruktur und Architektur kritisch hinterfragt werden.

**MVC Prinzip mit CakePHP**

Das MVC Prinzip bietet eine hohe Flexibilität hinsichtlich der Wartung sowie Neuimplementierung von neuen Softwarekomponenten. Neue Funktionalitäten und damit einhergehende neue Views, Models oder Controller können mithilfe der CakePHP Namenkonventionen ohne zusätzlichen Implementierungsaufwand hinzugefügt werden. Auch die Wartung einzelner Komponenten innerhalb der drei Ebenen gestaltet sich durch die Separierung dieser als vergleichsweise einfach. Tritt beispielsweise ein Darstellungsfehler in einer der Views auf, kann dieser in der View direkt beziehungsweise im zugehörigen Element behoben werden. Das Antasten von Controller oder Model ist nicht weiter notwendig. Dies spart Entwicklungszeit und minimiert das Risiko, dass andere Komponenten beim Beheben des Fehlers betroffen sein können. 
Des weiteren sind die in dieser Arbeit dargestellten CakePHP Funktionen nur ein Bruchteil dessen, was das Framework anbietet. Dieses bietet sehr viel mehr Funktonen, um die Entwicklungsarbeit effizienter sowie schneller zu gestalten.

**Wartung der Infrastruktur**

Jeder Apache Server, auf dem die Instanzen der Vereine liegen, wird aktuell vollständig selbst verwaltet. Lediglich die Ressourcen werden von einem Hostinganbieter bereitgestellt. Die Instandhaltung obliegt den Entwicklern. Dies beinhaltet unter anderem das Updaten des Betriebssystems, Sicherstellung einer rebootsicheren Konfiguration für Updates seitens des Hostinganbieters sowie das Verwalten einer Backupstrategie.

**Skalierbarkeit**

Im aktuellen Zustand ist das System sehr schwerfällig zu skalieren. Die Einrichtung eines neuen Servers muss komplett händisch von einem Entwickler durchgeführt werden. Es gibt hierbei keine automatisierten Prozesse, die die Einrichtung erleichtern. Bei einem großen Neukundenandrang müsste sich vorerst auf die Bereitstellung der Ressourcen anstatt um das Betreuen der Neukunden gekümmert werden.
Des weiteren sind die Ressourcen sehr statisch verteilt. Unabhängig der Nutzeranfragen bleiben diese immer konstant. Dabei haben Auswertungen die folgende Höhe der Seitenaufrufe über die Woche verteilt ergeben.

![](source/figures/TS2_SeitenaufrufeWoche.png)
Abbildung 10: Seitenaufrufe im Wochenverlauf von TeamSports2

Die Schwankungen sind darin begründet, dass zu Beginn und Ende der Woche die Vorbereitungen für die jeweiligen Spieltage am Wochenende beginnen. Es werden Vorberichte eingepflegt, letzte Informationen zum kommenden Spieltag weitergegeben und die Fans informieren sich über die Spielzeiten ihrer Mannschaften. Zu Beginn der Woche werden dann die Spielberichte des vergangenen Spieltags auf die Webseiten gestellt, welche wiederum von den Fans gelesen werden. Am Wochenende findet dann der eigentlich Spieltag statt, wobei Ergebnisse und erste Spiel- oder Vorberichte auf den Seiten eingestellt werden. 
Über das Jahr hinweg gesehen sind ebenso relativ eindeutige Schwankungen, gemessen an den Seitenaufrufen, festzustellen.

![](source/figures/TS2_SeitenaufrufeJahr.png)
Abbildung 11: Seitenaufrufe im Jahresverlauf von TeamSports2

\pagebreak

Aufgrund der relativ langen Winterpause von Fußballvereinen, welche 65 Prozent der Instanzen bei TeamSports2 ausmachen, findet auf den Seiten weniger Aktivität statt. Gleiches gilt für die Zeit zwischen Juni und Juli, wenn sich viele Vereine in der Sommerpause befinden. Zu Saisonbeginn im Herbst steigt dann wiederum das Nutzeraufkommen. 

Anhand der Auswertungen wird deutlich, dass es große Unterschiede in der Lastenverteilung gibt. Die aktuellen Ressourcen können die höhere Anfragelast in den Spitzenzeiten bisher problemlos bewältigen, allerdings laufen die gleichen Ressourcen auch in niedrig frequentierten Zeiten. 

**Deployment**

Für das Bereitstellen eines neuen Releases oder Updates muss der gesamte Code aus dem GitHub-Projekt auf jede Produktivinstanz einzeln deployed werden. Der Deployment Prozess nimmt somit nicht nur mehr Zeit in Anspruch, es muss auch ein erheblicher Konfigurationsaufwand betrieben werden. Jede Instanz ist in DeployHQ einzeln angelegt und mit dem richtigen Pfad auf dem Server versehen. Zwar muss die Einrichtung grundsätzlich nur einmal erfolgen, allerdings stellen die Konfigurationen eine potenzielle Fehlerquelle dar. Ist beispielsweise der Pfad nicht richtig, bekommt die Instanz das Update nicht. Zu dem Deployment auf die Produktivinstanzen kommen noch alle Testinstanzen hinzu, deren Anzahl auf einen hohen zweistelligen bis niedrigen dreistelligen Bereich beziffert werden kann.
Des weiteren befindet sich auf den Servern sehr viel doppelter Code, da alle Models, Views und Controller auf jeder Instanz gleich sind.

**Testing**

Um neu entwickelten Code zu testen, steht ein eigener Entwicklungsserver mit Entwicklungsinstanzen, welche äquivalent zu den Produktivinstanzen sind, zur Verfügung. Da die Produktiv- und Entwicklungsserver separiert voneinander sind, ist es allerdings nicht möglich unter den exakten Voraussetzungen des Produktivsystems zu testen. Nach dem erfolgten Deploy des neuen Codes kann es, beispielsweise durch abweichende Serverkonfigurationen, auf den Produktivinstanzen zu Fehlern kommen. Um den Fehler zu beheben, muss wieder ein neuer Deploymentprozess über alle Instanzen angestoßen werden. In DeployHQ werden zwar Models und Controller sowie die View separat voneinander deployed, aber auch dies muss jeweils fortlaufend über alle Instanzen geschehen. Zuletzt ist auch der Kundeneindruck nicht unerheblich, wenn neue Updates auf den Produktivseiten unterwünschtes Verhalten oder im schlimmsten Fall einen Ausfall der Seite hervorrufen.


