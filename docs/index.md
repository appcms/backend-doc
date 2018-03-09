# Einführung
- **Lizenz**: Duale Lizenz AGPL v3 / Properitär
- **Webseite**: <http://www.contentfly-cms.de>

## Die Contentfly Plattform

- **Server**: <https://github.com/area-net-gmbh/contentfly-cms>
- **Ionic SDK**: <https://github.com/area-net-gmbh/contentfly-ionic>
- **iOS SDK**:  <https://github.com/area-net-gmbh/contentfly-ios>

## Tools und mehr

- Vagrant-Umgebung für das CMS: <https://github.com/area-net-gmbh/contentfly-vagrant>
- Download + Dokumentation: <http://www.contentfly-cms.de>

## Einführung und Überblick

Mit der Contentfly Plattform können mobile Apps für iOS und Android unter dem Einsatz von nativen und webbasierten Technologien inklusive Synchronisations-Anbindung an einen Server entwickelt werden. 

Der Server basiert dabei auf PHP und MySQL und kann auf nahezu jedem Standard-Hosting-Provider eingesetzt werden. Die SDKs für iOS und Android unterstützen Entwickler bei der Datenhaltung, Synchronisation und Darstellung von Inhalten. Zudem enhalten die SDKs eine Template-Engine, mit der Oberflächen einfach und plattformübergreifen in HTML erstellt werden können.

Es ist dem Entwickler völlig freigestellt, ob Apps komplett nativ oder in Kombination mit webbasierten Bereichen umgesetzt werden.

!!! note "Contentfly != Baukasten-System"
    Die Contentfly Plattform ist kein Baukasten-System, mit dem sich Apps ohne Vorkenntnisse zusammenklicken lassen - sondern bietet ein technisches Rahmenwerk, mit sich mobile Apps effizient, kosten-nutzenoptimiert, aber dennoch frei von Zwängen anderer (hybrider) Frameworks wie Titanium Mobile oder PhoneCap.

Für die Entwicklung von Apps mit dem Contentfly Framework sind folgende Kentnisse erforderlich:

- **CMS**: PHP und optimalerweise Doctrine und MySQL
- **Ionic**: Typescript/Javascript und gegebenfalls HTML5
- **iOS**: Swift oder Objective-C und gegebenfalls HTML5

## Das Contentfly CMS

Mit dem CMS können serverseitig beliebige Inhalte gespeichert und verwaltet werden. Der Server kann letztendlich auch losgelöst von mobilen Apps betrieben werden. Über eine Schnittstelle kann auf alle auf dem Server gespeicherten Daten zugegriffen werden. Damit kann der Server auch zum Beispiel als zusätzliches PIM (Product Information Managament) für eine Webseite in TYPO3 oder Wordpress eingesetzt werden.

##  Technologien

- [PHP](http://www.php.net/) und [MySQL](https://www.mysql.de/)
- [Silex](http://silex.sensiolabs.org/) als Micoframework mit [Symfony Components](http://symfony.com/components)
- [Doctrine](http://www.doctrine-project.org/) als ORM für die Datenhaltung
- [AngularJS](https://angularjs.org/) für die Oberfläche

## Systemvoraussetzungen

- Apache oder Nginx Webserver
    - mit mod_rewrite Module
    - FollowSymLinks aktiviert
    - mod_deflate empfohlen
- PHP 
    - mindestens Version 7.1
- PHP-Module
    - open_ssl
    - gd_lib
    - pdo_mysql
- Empfohlene PHP-Einstellungen
    - PHP auch vom der Konsole aufrufbar
    - OPcache und APC/APCU-Cache
- MySQL 
    - mindestens Version 5.5

## Lizenz

Die Contentfly Plattform ist unter eine dualen Lizenz (AGPL v3 und properitär) verfügbar. Die genauen Lizenzbedingungen sind in der Datei _licence.txt_ zu finden.

**Die Contentfly Plattform ist ein Produkt der AREA-NET GmbH**

AREA-NET GmbH
Öschstrasse 33
73072 Donzdorf

**Kontakt**

- Telefon: 0 71 62 / 94 11 40
- Telefax: 0 71 62 / 94 11 18
- http://www.area-net.de
- http://www.app-agentur-bw.de
- http://www.contentfly-cms.de


**Geschäftsführer**
Gaugler Stephan, Köller Holger, Schmid Markus

**Handelsregister**
HRB 541303 Ulm
Sitz der Gesellschaft: Donzdorf
UST-ID: DE208051892




