# Analyse des IST-Zustands von TeamSports2

TeamSports2 ist ein Content-Management-System für Sportvereine. Das System wurde entwickelt um Sportvereinen den Aufbau einer leicht bedienbaren, attraktiven sowie kostengünstigen Vereinshomepage zu ermöglichen. Die Idee dafür entstand aufgrund des Bedarfs einer neuen Homepage im eigenen Handballverein. 
Durch die eigene Vereinsarbeit war man sich um die Strukturen, welche in den Vereinen herrschen durchaus bewusst. Zudem war durch die Erfahrung auch klar, welche Funktionen und Möglichkeiten die Vereine benötigen um einen guten Webauftritt kreieren zu können. Basierend auf diesem Wissen sollte ein System entstehen, dass zum einen keinerlei Programmierkenntnisse der Anwender erfordert und zum anderen mehrere Benutzer zulässt, die gleichzeitig am System arbeiten können.
Diese beiden Punkte stellen jeweils ein zentrales Kriterium für die Vereine dar, wenn es um den Aufbau oder Relaunch einer Vereinshomepage geht. Die Aufbauarbeit sowie Pflege soll nicht lediglich einer Person obliegen, die den Aufwand alleine betreiben muss. Des weiteren ist auch nicht davon auszugehen, dass es im Verein ausgebildete Programmierer oder ähnlich technikaffine Personen gibt. Zuletzt ist natürlich auch der zeitliche Aufwand ein wichtiger Faktor, weswegen auch hier eine unkompliziert sowie einfache Bedienung wichtig ist.

## Grundlegende Produktstruktur

Der Verein erstellt sich über die Homepage teamsports2.de eine Testseite. Dabei gibt der Verein lediglich persönliche Daten, die anfängliche Seitenfarbe sowie den gewünschten Seitennamen an. Daraufhin wird eine aus diesen Daten eine Testinstanz mit eigener zugehöriger Datenbank generiert. Die Seite enthält anfangs einige Testberichte sowie rudimentären Inhalt um den Nutzer den Einstieg zu erleichtern. Ein beispielhaftes Frontend einer neue generierten Testseite ist in der nachfolgendenden Abbildung zu sehen.

![Abbildung 4: Ansicht TeamSports2 Frontend](source/figures/TeamSports2_Frontend.png) { width=50% }
Abbildung 4

Der Server, worauf sich die Testseite des Vereins unter einer Subdomain, wie testseite.teamsports2.de befindet ist ein Apache-Server mit Ubuntu-Betriebssystem. Über die Subdomain sowie die mit dem generieren der Seite erstellten Zugangsdaten kann der Nutzer die Seite nach seinen Wünschen bearbeiten. Der Aufbau des Backends ist in Abbildung fünf aufgezeigt.

![Abbildung 5: Ansicht TeamSports2 Backend](source/figures/TeamSports2_Backend.png) { width=50% }
Abbildung 5

Der Nutzer hat bei der Gestaltung seiner Seite sehr viele Optionen, welche vom auswählen des eigenen Designs bis hin zu vereinsspezifischen Funktionalitäten reichen. Um einen Eindruck des Systems zu vermitteln werden nachfolgend einige wichtige Funktionen aufgezeigt.

- _Template und Farben:_ Es kann zwischen verschiedenen Templates, die eine jeweils unterschiedliche Anordnung des Menüs, Headerbildes und grundsätzlichen Aufbau der Seitenelemente darstellen, gewählt werden. Des weiteren können die Vereinsfarben auf der Seite mittels verschiedener Optionen zur Gestaltung der Haupt-, Schrift- und Hintergrundfarbe realisiert werden.

- _Seitenaufbau mittels Elementen:_ Jede vom System generierte Seite kann mit verschiedenen Elementen bestückt werden. Ein Element kann wie ein Baustein, welcher aus cakePHP, HTML und CSS Code besteht, gesehen werden. Dies unterscheiden sich jeweils in der Darstellung der Informationen. Dabei können Informationen, der Mannschaftskader, Vereins- oder Teamberichte sein, welche der Nutzer zurvor im Backend eingegeben hat. 

- _Nutzerrollen:_ Für jede Instanz kann es einen oder mehrere Administrationsbenutzer geben. Diese können auf alle Bereiche, wie in Abbildung 5 oben rechts zu sehen ist, zugreifen. Darunter gibt es aber auch andere Nutzerrollen, welche beispielsweise als Trainer nur ihr jeweiliges Team bearbeiten können.

- _Menü und Navigation:_ Die Navigationsleiste mit den entsprechenden Menüpunkte wird vom System erstellt. Der Nutzer kann dabei für ihn interessante Module, wie die Anzeige eines Heimspielplans, welcher dann im Menü angezeigt wird wählen oder eigene Seiten individuell gestalten. Dabei kann ich die Anordnung der verschiedenen Menüpunkte weitstegehend verändert werden.

- _Teams:_ Für jede Abteilung innerhalb des Vereins beziehungsweise der Instanz können Teams beliebig viele Teams angelegt werden. Jedes Team beinhaltet dabei Spielerkader, Bilder sowie die Ergebnisse aus dem Ligabetrieb. All diese Informationen werden im Frontend angezeigt.

- _Vereins- und Teamberichte:_ Sowohl für Teams, als auch für den Verein an sich können Bericht in Form von Spielberichten oder anderen Vereinsinternen Informationen erstellt werden. Für jeden Bericht gibt es die Möglichkeit, diesen parallel aus TeamSports2 heraus auch auf der Facebookseite des Vereins zu veröffentlichen. Dabei muss der Nutzer im vorhinein seine TeamSports2-Seite mit der Facebookseite des Vereins über FacebookConnect verknüpfen. Diese Funkton stellt ebenso das System bereit.

Während der 90 tägigen Testphase kann der Nutzer diese und viele weitere Funktionen kostenlos testen. Dabei gibt, wie sonst oft üblich, keine Einschränkungen bei der Testversion. Sollte sich der Nutzer vor oder nach Ablauf der 90 Tage für den Kauf seiner Testseite entscheiden wird die Instanz vom vom sogenannten Generatorserver, wo die Instanz für den Testzeitraum liegt, auf einen Liveserver umgezogen.
Hierbei wird eine exakte Kopie der Instanz erstellt und mittels eines Skripts auf den Liveserver übertragen. Der Nutzer kann dann auch seine Domain über TeamSports2 registrieren lassen, welche dann wiederum mit der Instanz auf dem Liveserver per DNS verknüpft wird. Die Instanz auf dem Testserver wird nach erfolgten Umzug gelöscht.
Auch auf dem Liveserver erhält jeder Nutzer eine eigene Instanz mit einer eigenen Datenbank.

## Verwendete Technologien 


## Architektur 


## Datenbankmodell 

