# IST-Zustand von TeamSports2

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

- _Seitenaufbau mittels Elementen:_ Jede vom System generierte Seite kann mit verschiedenen Elementen bestückt werden. Ein Element kann wie ein Baustein, welcher aus CakePHP, HTML und CSS Code besteht, gesehen werden. Dies unterscheiden sich jeweils in der Darstellung der Informationen. Dabei können Informationen, der Mannschaftskader, Vereins- oder Teamberichte sein, welche der Nutzer zurvor im Backend eingegeben hat. 

- _Nutzerrollen:_ Für jede Instanz kann es einen oder mehrere Administrationsbenutzer geben. Diese können auf alle Bereiche, wie in Abbildung 5 oben rechts zu sehen ist, zugreifen. Darunter gibt es aber auch andere Nutzerrollen, welche beispielsweise als Trainer nur ihr jeweiliges Team bearbeiten können.

- _Teams:_ Für jede Abteilung innerhalb des Vereins beziehungsweise der Instanz können Teams beliebig viele Teams angelegt werden. Jedes Team beinhaltet dabei Spielerkader, Bilder sowie die Ergebnisse aus dem Ligabetrieb. All diese Informationen werden im Frontend angezeigt.

Sollte sich der Nutzer vor oder nach Ablauf der 90 Tage für den Kauf seiner Testseite entscheiden wird die Instanz vom sogenannten Generatorserver, wo die Instanz für den Testzeitraum liegt, auf einen Liveserver kopiert.

## Deployment-Prozess und Infrastruktur

Aktuell umfasst TeamSports2 über 150 Live-Instanzen unter einer jeweils eigenen Domain. Diese sind auf vier Apache-Server mit einem Ubunutbetriebssystem verteilt, welche von einem externen Hostinganbieter gehostet werden. Die Daten jeder Instanz liegen auf jeweils einer eigenen MySQL-Datenbank, welche wiederum auf dem Server gespeichert ist. Aktuell ist TeamSports2 somit auf einer Singel-Tenant Architektur aufgebaut, wobei jede Instanz auf ihre eigene Anwendung sowie Datenbank zugreift.

Anhand nachfolgender Abbildung wird der Entwicklungs- sowie Deploymentprozess dargestellt.

![](source/figures/TeamSports2_Deployment.png)
Abbildung 6: Deployment-Prozess TeamSports2

In mehreren GitHub-Repositories liegt der Code der Produktionsumgebung mit Entwicklungs- sowie Masterbranches. Zum Entwickeln von Code wird ein eingenständiger Entwicklungsserver bereitgestellt, worunter zur Produktonsumgebung adäquate Testseiten erstellt werden können. Dadurch muss nicht auf Liveservern, welche die Kundeninstanzen betreffen würde, getestet werden. Soll neuer Code entwickelt werden kann sich vom Entwicklungs- oder Masterbranch der aktuell Code geholt und ein Featurebranch erstellt werden.
Mithilfe des Webservices  „DeployHQ“ wurde eine Build- und Deployment Pipeline aufgebaut. DeployHQ ist mit GitHub verbunden, wodurch auch alle mit dem Repository verbundenen Featurebranches auf die gewünschte Testinstanz deployed werden können.
Ist der Entwickler mit dem Code fertig wird ein PullRequest gestellt. Sobald dieser geprüft und der PullRequest genehmigt wurde, erfolgt ein Merge in den Entwicklungsbranch. Hier werden bei größeren Releases mehrere Features oder Anpassungen gesammelt um diese dann gemeinsam in den Masterbranch zu mergen.

Sobald der für den Release relevante Code vollständig im Masterbranch liegt kann die Deployment-Pipeline angestoßen werden. Dabei wird vom Masterbranch der aktuelle Code synchronisiert und dann durch das Secure Shell (SSH) Protokoll an die Server übertragen. Jede Instanz hat in DeployHQ eine eigene Konfiguration, wodurch festgelegt werden kann auf welchen Pfad auf dem jeweilgen Server deployed werden soll. Der gesamte Code des Masterbranches muss dabei auf jede Instanz einzeln gespielt werden. 
Beim Deployen werden nur Dateien an die Kundeninstanzen übertragen, welche nicht durch eine Individualisierung, wie die Datei für das Favicon, auf der Instanz betroffen sein können. Bedeutet es werden die Controller, Models und die View deployed, wobei Controller und Model jeweils separat von der View deployed werden. 

## Architektur 

TeamSports2 ist in PHP geschrieben und nutzt das CakePHP Framework, welches auf dem Model-View-Controller (MVC) Prinzip basiert. Bei diesem Prinzip werden Geschäfts-,  Anwendungslogik und die Datenhaltung voneinander getrennt. 

- _Model:_ Enthält die Geschäftslogik der Anwedung, organisiert die Datenhaltung und stellt die jeweiligen Tabellen dar.
- _View:_ Darstellung der Inhalte für den Benutzer in PHP und HTML Code.
- _Controller:_ Enhält die Anwendungslogik und fungiert als Vermittler zwischen View und Model.

[@Ammelburger2008 6-9].

Dadurch sind alle drei Ebenen unabhängig voneinander und logisch getrennt. Dies erleichtert zum einen die Wartung der Anwendung, als auch die Implementierung neuer Funktionen. 
Mit CakePHP sollen zum einen die Vorteile des MVC Prinzips genutzt und zum anderen nützliche Funktionen seitens des Frameworks bereitgestellt werden. Ein wichtiger Bestandteil dessen ist das „Convention over Configuration“ [@Ammelburger2008 5] Prinzip, welches in CakePHP umgesetzt wird. Dabei müssen keine speziellen Konfigurationen implementiert werden um beispielsweise eine Verbindung zwischen dem Model, der View und dem Controller herzustellen. Die Verbindungen zueinander werden von CakePHP automatisch über die jeweilige Benennung der Komponenten erkannt [@Ammelburger2008 5-6]. Nachfolgende Tabelle zeigt, worauf bei der Benennung der jeweiligen Komponenenten geachtet werden muss.

| Komponente | Schreibweise | Numerus  | Beispiel        |
|------------|--------------|----------|-----------------|
| DB Tabelle | klein        | Plural   | teams           |
| Model      | groß         | Singular | Team            |
| Controller | groß         | Plural   | TeamsController |
| View       | groß         | Plural   | Teams           |

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

Der übergebene Parameter eins in der URL steht für die departmentId, welche der Action übergeben werden muss. Als Department wird in TeamSports2 eine Abteilung des Vereins bezeichnet. Dadurch, dass die View seniors.ctp  wie die Action im TeamsController benannt ist, kann die View diese zuordnen. Dass die View seniors.ctp auf den TeamsController zugreifen muss, ist durch die Verzeichnisstruktur im Projekt festgelegt. Alle Views die im View-Ordner und dort wiederum im Teams-Ordner liegen, wissen dass sie mit dem TeamsController verbunden sind. In diesem Fall enthält die seniors View ein Element mit der Nummer 33. Prinzipiell kann in der View auch PHP und HTML Code stehen. An dieser Stelle wurde ein Element verwendet, dass der Nutzer auch selbständig über den Administrationsbereich abändern kann. Grundsätzlich enthält jede Instanz Elemente, die zu Darstellungen in den Views dienen. Damit diese nicht bei jeder Instanz einzeln abgelegt werden müssen, enthält jede Instanz im View Ordner den Ordner view_elements. Dieser ist ein Symlink zum Pfad */opt/elements/view* auf demselben Server. Unter diesem Pfad findet sich dann auch das passende Element. In der View seniors.ctp wird das Element 33 mit folgendem Code aufgerufen.

```
<?php
echo $this->element('view_elements/33');
?>
```

CakePHP erkennt mithilfe von *$this->element*, dass auf den View Ordner, worin der Elements Ordner liegt zugegriffen werden muss. Die gleiche Ordnerstruktur ist im Controller Ordner auch auf dem Server vorzufinden mit der Ergänzung des view_elements Ordners. Somit wird im Ordner View der Unterordner Elements gesucht. In letzerem ist dann der view_elements Symlink enthalten welcher das Element 33 enthält.
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

## Datenbankmodell 

Aufgrund der verwendeten MySQL Datenbank ist das gesamte Datenbankmodell relational. Die nachfolgende Abbildung zeigt einen Ausschnitt des aktuellen Datenbankmodells mit einigen Haupttabellen, welches in der dritten Normalform vorliegt.

![](source/figures/TS2_AusschnittDB-Modell.png)
Abbildung 9: Ausschnitt Datenbankmodell TeamSports2

In der teams Tabelle sind alle Daten zum Team, wie der Teamname und eine Beschreibung, zu finden. Jede Abteilung kann beliebig viele Teams beinhalten, weswegen die department_id ebenso in der teams Tabelle hinterlegt ist. Jedes Team kann zudem einen Team- oder Spielbericht schreiben, der beliebige Informationen enthält. Hierfür besteht eine Verknüpfung mit der articles Tabelle. Die departmens Tabelle stellt alle Informationen zu einer Abteilung dar. Auch hier kann eine beliebige Anzahl von Abteilungen angelegt werden. Zudem gibt es in jedem Team einen oder mehrere Trainer sowie einen oder mehrere Spieler. Diese werden jeweils über die Zuordnungstabellen team_players sowie teams_trainers dem jeweiligen Team zugeordnet. Prinzipiell ist jeder Spieler oder Trainer immer auch ein Nutzer aus der users Tabelle. In dieser sind alle Nutzerspezifischen Daten, wie der Name, das Passwort, welches in verschlüsselter Form in der Datenbank gespeichert wird, enthalten. Sobald einem Nutzer über das Backend (siehe Abb. 5), beispielsweise eine Trainer- oder Spielerrolle zugewiesen wird entsteht eine Verknüpfung zur players oder trainers Tabelle. 

## Analyse

TeamSports2 ist ein Software as a Service Produkt welches eine einfache sowie schnelle Erstellung einer voll funktionsfäigen Webseite ermöglicht. Dabei sind in der Infrastruktur sowie Architektur einige nicht mehr zeitgemäßge Implementierungen auszumachen 

**MVC Prinzip mit CakePHP**

Das Model View Controller Prinzip bietet eine hohe Flexibilität was die Wartung sowie Neuimplementierung von neuen Softwarekomponenten angeht. Neue Funktionalitäten und damit einhergehende neue Views, Models oder Controller können mithilfe der CakePHP Namenkonventionen ohne zusätzlichen Implementierungsaufwand hinzugefügt werden. Auch die Wartung einzelner Komponenten innerhalb der drei Ebenen gestaltet sich durch die Separierung dieser als vergleichsweise einfach. Tritt beispielsweise ein Darstellungsfehler in einer der Views auf kann dieser in der View direkt beziehungsweise im zugehörigen Element behoben werden. Das Antasten von Controller oder Model ist nicht weiter notwendig. Dies spart Entwicklungszeit und minimiert das Risiko, dass andere Komponenten beim Beheben des Fehlers betroffen sein können. 
Des weiteren sind die in dieser Arbeit dargestellten CakePHP Funktionen nur ein Bruchteil dessen, was das Framework anbietet. Dieses bietet sehr viel mehr Funktonen um die Entwicklungsarbeit effizienter sowie schneller zu gestalten.

**Wartung der Infrastruktur**

Jeder Apache Server, auf dem die zurzeit Instanzen der Vereine liegen, wird aktuell vollständig selbst verwaltet. Lediglich die Ressourcen werden von einem Hostinganbieter bereitgestellt. Die Instandhaltung obliegt den Entwicklern. Dies beinhaltet unter anderem das Updaten des Betriebssystem, Sicherstellung einer Rebootsicheren Konfiguration für Updates seitens des Hostinganbieters sowie die vollständige Verwaltung.

**Skalierbarkeit**

Im aktuellen Zustand ist das System sehr schwerfällig zu skalieren. Die Einrichtung eines neuen Servers muss komplett händisch von einem Entwickler durchgeführt werden. Es gibt hierbei keine automatisierten Prozess die die Einrichtung erleichtern. Bei einem großen Neukunden Andrang müsste sich vorerst auf die Bereitstellung der Ressourcen anstatt um das Betreuen der Neukunden gekümmert werden.
Des weiteren sind die Ressourcen sehr statisch verteilt. Unabhängig der Nutzeranfragen bleiben die Kapazitäten immer konstant. Dabei ist am Wochendende und zu Beginn sowie Ende Woche stets ein erhöhtes Nutzeraufkommen festzustellen. Dies ist dadurch begründet, dass der Spielbetrieb der Vereine sich größtenteils am Wochenende

![](source/figures/TS2_SeitenaufrufeJahr.png)
Abbildung 10: Seitenaufrufe in der Woche von TeamSports2

![](source/figures/TS2_SeitenaufrufeWoche.png)
Abbildung 11: Seitenaufrufe im Jahr von TeamSports2


**Deployment**



**Testing**