#Architektur APP-CMS

![Architektur](../images/architektur.png)

## Backend

### Silex Microframework
Auf der untersten Ebene agiert das Silex Microframework und ist u.a. für Themen wie Routing, 
MVC und Dependency Injection zuständig.

- [Silex Version 1.3](https://silex.sensiolabs.org/doc/1.3/)

### Symfony Components
Das APP-CMS nutzt Teile der Symfony Components, wie _CONSOLE_, _HTTP KERNEL_, _HTTP FOUNDATION_ oder _ROUTING_.

- [Symfony Components Version 2.x](http://symfony.com/components)

### Doctrine ORM
Für die Datenhaltung kommt das Doctrine ORM (_Object-Relational Mapping_) zum Einsatz.

- [Doctrine ORM Version 2.3](http://www.doctrine-project.org/projects/orm.html)

### APP-CMS Core
Der Kern des APP-CMS befindet sich im Projektordner unter _appcms/areanet/PIM_. Der Ordnername _PIM_ ist dabei 
noch historsisch geschuldet, denn das APP-CMS war urprünglich als individuelles _Produkt-Informations-Management-System_ (kurz PIM) 
entwickelt worden. Das System erwies sich aber als so flexibel und ausbaufähig, dass es letztendlich die Grundlage 
für das APP-CMS diente. Und damit auch bewiesen ist, dass das APP-CMS auch als PIM eingesetzt werden kann.

### APP-CMS API
Das eigentliche Herzstück, die Schnittstelle des APP-CMS. Das APP-CMS ist im Grunde als **API-zentriertes** System konzipiert.
Neudeutsch wird diese Art der Software auch als **Headless** CMS benannt. Damit ist die Datenhaltung komplett unabhängig 
von der letztendlichen Ausgabe. Alle Daten und Inhalte sind über eine Schnittstelle, sowohl lesend als auch schreibend, ansprechbar. 

Der zentrale Code für die API steckt in der Datei _appcms/areanet/PIM/Controller/ApiController.php_

- [Nutzung der API](schnittstelle.md)
- [API-Dokumentation](/doku/api/1.3.0)

### Weitere Komponenten

- [Pimple Version 1.x](https://pimple.sensiolabs.org/)
- [Twig Version 1.x](https://twig.sensiolabs.org/)
- [Doctrine DBAL Version 2.2](http://www.doctrine-project.org/projects/dbal.html)

### Custom
Das APP-CMS kann über benutzerdefinierten Code flexibel angepasst und erweitert werden, ohne die Update-Fähigkeit 
des APP-CMS zu verlieren.

- Event- und Hook-System zur Anpassung von Controller-Ausgaben (_preDispatch/postDispatch_)
- Benutzerdefinierte API-Aufrufe-/Routen und -Ausgaben
- Erweiterung von System-Entitäen (wie z.B. Benutzer oder Gruppen) durch [Traits](http://php.net/manual/de/language.oop5.traits.php)
- Individuelle Frontend-Ausgaben über PHP und Twig-Templates (z.B. für eine Landing-Page)

## User Interface (UI)

### AngularJS

Als Grundlage für das APP-CMS UI dient AngularJS, JQuery und Bootstrap.

- [AngularJS Version 1.4](https://angularjs.org/)
- [jQuery Version 2.1](https://jquery.com)
- [Bootstrap Version Version 3.3](https://getbootstrap.com)

### APP-CMS UI

Der Code des APP-CMS UI befindet sich im Ordner _appcms/areanet/PIM-UI_, dessen Unterordner _assets_ dabei per Symlink 
im Ordner _appcms/public/ui/default_ veröffentlicht ist. Das APP-CMS UI lädt nach dem Einloggen über die Schnittstelle _/api/schema_
die Konfiguration des Systems und generiert dynamisch die Oberfläche, Listen und Formulare.


### Custom
Ebenso wie das Backend, kann auch das APP-CMS UI individuell angepasst werden.