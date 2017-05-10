# Installation und Einrichtung

## APP-CMS entpacken

Entpacken Sie die Download-Version des APP-CMS auf Ihrem Webserver. Sie finden lediglich den Ordner _appcms_ vor, der den Kern des APP-CMS enthält. Alle weiteren Verzeichnisse werden während der Installation angelegt:

* ROOT-VERZEICHNIS
    * appcms

## Einstellungen Webserver anpassen

Setzen Sie den Document-Root/Web-Root des Webservers oder virtuellen Hosts auf _appcms/public_.

Für den Apache-Webserver wird bereits eine .htaccess mitgeliefert, die alle Anfragen automatisch auf die _index.php_ umschreibt.

Für den Nginx-Webserver müssen Sie in den Einstellungen für die Seite folgenden Eintrag vornehmen:

    location / { 
        try_files $uri $uri/ /index.php?$query_string; 
    }
    
## APP-CMS installieren
  
Rufen Sie die Webseite im Browser aus und folgenden Sie den Anweisungen
des Installationsassistenten. Für die Installation muss der
Webserver-Benutzer Schreibrechte auf das ROOT-VERZEICHNIS haben,
ansonsten können während des Installation die benötigten Verzeichnisse nicht angelegt werden.

## Login in das Backend

Nach der Installation können Sie sich mit Benutzername _admin_ und Passwort _admin_ in das Backend einloggen. Vergessen Sie nicht
    
