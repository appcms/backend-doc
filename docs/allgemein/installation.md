# Installation

## Installation einer Release-Version

**(1) Download eines Release-Paketes**

Laden Sie sich das gewünschte Paket des Contentfly CMS unter <https://www.contentfly-cms.de> herunter und entpacken es auf Ihrem Server.

**(2) Webserver (Virtual Host) DocumentRoot auf _appcms/public_ stellen**

**(3) URL/Host im Browser aufrufen und Installations-Hinweisen folgen**

Nach der Installation können Sie sich mit Benutzernamen=admin und Passwort=admin am Contentfly CMS anmelden.

!!! note "Sicherheits-Hinweis"
    Denken Sie bitte daran, dass Admin-Passwort nach dem ersten Login abzuändern.
    
!!! tip "Nach der Installation kein Login möglich?"
    Sollte nach der Installtion kein Login möglich sein und ein Datenbankfehler erscheinen, wurde vermutlich die Datenbank nicht korrekt eingerichtet.
    Führen Sie in diesem Fall bitte aus dem nächsten Kapitel _Manuelle Installation/Kompilierung aus Git_ die Schritte (3), (4) und (5) aus. Danach müsste das
    Contentfly CMS im Browser korrekt funktionieren.

   

## Manuelle Installation/Kompilierung aus Git

**(1) Git-Repository laden**

```
#!/bin/bash
git clone https://github.com/area-net-gmbh/contentfly-cms.git
```


**(2) Systemumgebung über Ant-Buildskript[^1] im Root-Ordner[^2] erstellen**

```
#!/bin/bash
ant
```

**(3) Datenbankzugangsdaten in custom/config.php eintragen**

**(4) Datenbank über Doctrine im Ordner appcms generieren.**

```
#!/bin/bash
php console.php orm:schema:update --force
```

Beachten Sie die Hinweise zur den [Contentfly CMS Konsolenbefehlen](konsole.md)

**(5) Datenbank im Ordner appcms initalisieren/einrichten**

```
#!/bin/bash
php console.php appcms:setup
```

**(6) Webserver (Virtual Host) DocumentRoot auf _appcms/public_ stellen**

**(7) URL/Host im Browser aufrufen**

Für das erste Login verwenden Sie den Benutzernamen=admin und das Passwort=admin.

!!! note "Sicherheits-Hinweis"
    Denken Sie bitte daran, dass Admin-Passwort nach dem ersten Login abzuändern.
    
## Apache Webserver

Für den Apache-Webserver wird bereits eine _.htaccess-Datei_ mitgeliefert, die alle Anfragen automatisch auf die _index.php_ umschreibt.

## Nginx Webserver

Für den Nginx-Webserver entfernen Sie am besten die mitgelieferte _.htaccess-Datei_ und konfigurieren den Nginx wie folgt:

    location / { 
        try_files $uri $uri/ /index.php?$query_string; 
    }

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

[^1]: <http://ant.apache.org/>
[^2]: Im Root-Ordner des Contentfly CMS befindet sich z.B. die Datei _build.xml_