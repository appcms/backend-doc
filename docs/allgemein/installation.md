# Installation

## Installation einer Release-Version

**(1) Download eines Release-Paketes**

Laden Sie sich das gewünschte Paket des Contentfly CMS unter <https://www.contentfly-cms.de> herunter und entpacken es auf Ihrem Server.

**(2) Webserver (Virtual Host) DocumentRoot auf _appcms/public_ stellen**

Oder alternativ einen benutzerdefinierten DocumentRoot-Ordner erstellen
\- siehe nächstes Kapitel.

**(3) URL/Host im Browser aufrufen und Installations-Hinweisen folgen**

Nach der Installation können Sie sich mit Benutzernamen=admin und Passwort=admin am Contentfly CMS anmelden.

!!! note "Sicherheits-Hinweis"
    Denken Sie bitte daran, dass Admin-Passwort nach dem ersten Login abzuändern.
    
!!! tip "Nach der Installation kein Login möglich?"
    Sollte nach der Installtion kein Login möglich sein und ein Datenbankfehler erscheinen, wurde vermutlich die Datenbank nicht korrekt eingerichtet.
    Führen Sie in diesem Fall bitte aus dem nächsten Kapitel _Manuelle Installation/Kompilierung aus Git_ die Schritte (3), (4) und (5) aus. Danach müsste das
    Contentfly CMS im Browser korrekt funktionieren.

## Benutzerdefinierter DocumentRoot

Im Standard sollte der DocumentRoot des Webservers auf den Ordner _appcms/public_ gestellt werden. Dies kann allerdings zu Problemen führen, wenn z.B.

- benutzerdefinierte Angaben in der htaccess-Datei
- benutzerdefinierte Anwendungen in einem eigenen Ordner (z.B. PhpMyAdmin)
- benutzerdefinierte PHP-Skripte

benötigt werden. Diese könnten bei einem Contentfly CMS Update überschrieben werden. Um einen benutzerdefinierten DocumentRoot zu verwenden, legen Sie auf Ihrem Webserver
an beliebiger Stelle einen Ordner mit einer _index.php_ mit folgendem Inhalt an:

**public/index.php**
```
<?php
require_once '../appcms/bootstrap-web.php';
```

!!! note "bootstrap-web.php"
    Die einzige Datei die Sie einbinden müssen, ist die _bootstrap-web.php_ aus
    dem _appcms_-Ordner. Darüber hinaus können Sie Ihre eigene _index.php_ um
    weitere PHP-Befehle (z.B. Error-Handling) ergänzen.

Im Falle eines Apache-Webservers vergessen Sie nicht die htaccess-Datei in Ihrem benutzerdefinierten DocumentRoot anzulegen.

**public/.htaccess**
```
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ index.php [QSA,L]
</IfModule>

<IfModule mod_deflate.c>
     AddOutputFilterByType DEFLATE text/css
     AddOutputFilterByType DEFLATE text/javascript
     AddOutputFilterByType DEFLATE application/x-javascript
     AddOutputFilterByType DEFLATE application/javascript
     AddOutputFilterByType DEFLATE text/x-component
     AddOutputFilterByType DEFLATE text/html
     AddOutputFilterByType DEFLATE text/richtext
     AddOutputFilterByType DEFLATE image/svg+xml
     AddOutputFilterByType DEFLATE text/plain
     AddOutputFilterByType DEFLATE text/xsd
     AddOutputFilterByType DEFLATE text/xsl
     AddOutputFilterByType DEFLATE text/xml
     AddOutputFilterByType DEFLATE image/x-icon
     AddOutputFilterByType DEFLATE application/json
 </IfModule>
```

!!! tip "Symlinks _custom/Frontend_ und _ui/default_"
    Die für das Contentfly CMS User Interface benötigten Ordner _custom/Frontend_ und _ui/default_
    werden als symbolische Links zur Laufzeit automatisch vom System erstellt. Achten Sie darauf, dass
    Ihr Webserver die Verfolgung von symbolischen Links unterstützt.

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