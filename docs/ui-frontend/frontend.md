# Benutzerdefiniertes Frontend

## Routing
Im Standard wird beim Aufruf des Root-Ordners der Installation über den Webserver (z.B. https://yourdomain.de) 
das Contentfly CMS User Interface (siehe [Architektur](../allgemein/architektur.md) mit dem Login-Fenster aufgerufen. Dieses Standard-Verhalten
kann aber angepasst werden und ein benutzerdefiniertes Frontend ausgegeben werden. Dazu muss das Contentfly CMS User Interface auf eine andere 
URL-Route "verlegt" werden. Ein benutzerdefiniertes Frontend kann beispielsweise sein:

- **Eine komplette Website**, die sich Daten aus dem Contentfly CMS zieht
- **Eine Landing-Page**, die sich dynamische Inhalte aus dem Contentfly CMS zieht
- **Eine separate Oberfläche** über z.B. das React Framework
- **Eine Webapp** über jQuery Mobile mit Anbindung an die Contentfly CMS API
- ...

!!! tip "Das Contentfly CMS als Web-CMS"
    Mit einem benutzerdefinierten Frontend kann das Contentfly CMS auch als Web-CMS eingesetzt werden. Denn wenn es primär
    um die Verwaltung und Anzeige strukturierter Daten geht (z.B eine Produktdatenbank /-katalog) ist das Contentfly CMS auf
    Grund der strukturierten Datenhaltung und den flexiblen Anpassungsmöglichkeiten oftmals klassischen CMS-Lösungen 
    wie TYPO3 oder Wordpress überlegen.
    
### Contentfly CMS UI Route

Das Umstellen der URL-Route für das Contentfly CMS UI erfolgt in der Konfigurationsdatei.

_/custom/config.php_
```
<?php
$configDefault->FRONTEND_URL  = "admin";
```

!!! note "Hinweis"
    Das AngularJS-Backend ist damit unter **yourdomain.de/admin** erreichbar.
    
### Benutzerdefinierte  Standard-Route

Nach Umstellung der URL-Route des Contentfly CMS UI kann die Ausgabe auf der Standard-Route beliebig angepasst werden.

_/custom/app.php_ 
```
<?php
$app->get('/', function () use ($app) {
    return $app['twig']->render('index.twig', array(
        'foo' => 'bar'
    ));
});
```

### Weitere benutzerdefinierte Routen

Es können zudem beliebige URL-Routen-/Controller definiert werden.

_/custom/app.php_ 
```
$app->get('/test', function () use ($app) {
    return $app['twig']->render('test.twig', array(
        'foo' => 'bar'
    ));
});

$app->get('/hello/{name}', function ($name) use ($app) {
    return $app['twig']->render('test.twig', array(
        'title' => "Hallo $name"
    ));
});
```
### Auslagerung in Controller

Alternativ zu den obigen Methoden, um ein benutzerdefiniertes Frontend auszugeben, 
kann die Logik auch in Controller-Klassen ausgelagert werden.

- siehe [Benutzerdefinierte API-Aufrufe](../backend/api.md)

## Templates und Systemfunktionen

### Twig Template Engine
Die HTML-Templates müssen relativ zum Ordner __/custom/Views__ gespeichert sein und basieren auf der [Twig Template-Engine](http://twig.sensiolabs.org/) und dem [Twig Silex Service Provider](http://silex.sensiolabs.org/doc/providers/twig.html)


### Zugriff auf Systemfunktionen

- siehe [Zugriff auf Systemfunktionen](../backend/api.md#zugriff-auf-systemfunktionen)

## Authentifizierung 

Im PHP-Code können Sie die integrierte Authentifizierung des Contentfly CMS verwenden. Die Authentifizierung erfolgt im Hintergrund über Sessions*

### Methoden

**$app['auth']->login($alias, $pass)**<br>
Ist der Login nicht erfolgreich werden _Exceptions_ mit dem Fehlercode 401 und der entsprechenden Meldung (_$exception->getMessag()_) geworfen.

| Parameter | Typ    | Optional | Beschreibung |
|:----------|:-------|:---------|:-------------|
| $alias    | String | nein     | Benutzername |
| $pass    | String | nein     | Passwort |

Rückgabe: _Areanet\PIM\Entity\User_

**$app['auth']->logout();**<br>
Logout des Benutzers und Beenden der Session.

**$app['auth']->getUser();**<br>
Rückgabe des eingeloggten Benutzers. Gibt _null_ zurück, wenn kein Benutzer eingeloggt ist.

Rückgabe: _Areanet\PIM\Entity\User_

### Beispiel  

_/custom/app.php_ 
```
<?php
$app->get('/secure', function () use ($app) {
    if(!$app['auth']->getUser()){
        return $app->redirect('/login');    
    }
    
    return $app['twig']->render('secure.twig');
});

$app->get('/login', function () use ($app) {
    return $app['twig']->render('login.twig');
});

$app->post('/login', function ($user, $alias) use ($app) {
    
    try{
        $app['auth']->login($user, $alias)
    }catch(Exception $e){
        return $app['twig']->render('login.twig', array(
            'message' => $e->getMessage()
        ));
    }
    
    return $app->redirect('/secure'); 
});
```

## Statische Ressourcen
  
Statische Ressourcen wie Bilder, Javascript oder CSS können/müssen im Ordner '_custom/Frontend_ abgelegt. 
Dieser ist über einen Symlink mit dem Ordner _appcms/public/custom/Frontend_ verlinkt. 
Alle dort abgelegten Ressourcen können also auf der Webseite über _www.domain.de/custom/Frontend/RESSOURCE.css_ 
aufgerufen und erreicht werden.