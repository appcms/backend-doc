# Benutzerdefinierte API-Aufrufe

Sie können das APP-CMS jederzeit um benutzerdefinierte Schnittstellen (API-Aufrufe) ergänzen.

## Controller anlegen

Benutzerdefinierte Controller, die die Logik für die eigene Schnittstelle beinhaltet, sind im Ordner _custom/Controller_ abzulegen und von _Areanet\PIM\Classes\Controller\BaseController_ 
abzuleiten.

_custom/Controller/TestController.php_
```
<?php
namespace Custom\Controller;

use Areanet\PIM\Classes\Controller\BaseController;

class TestController extends BaseController
{
    public function testAction(){
        $data = array();
        
        return new JSONResponse($data);
    }
    
    public function secureAction(){
        $data = array();
            
        return new JSONResponse($data);
    }

    public function hallo($name){
        $data = array('welcomeMessage', "Hallo $name");
        
        return new JSONResponse($data);
    }
}
```

## Controller einbinden

### Route-Manager

Der benutzerdefinierte Controller muss noch mit entsprechenden URL-Routen verknüpft werden, um diesen auch von "außen" erreichbar zu machen. 
Dies wird über den _Areanet\PIM\Classes\Manager\RouteManager_ erreicht, der über _$app['routeManager']_ angesprochen werden kann.

Über den Befehl _mount_ wird der benutzerdefinierte Controller mit der Basis-Route _custom_ verknüpft und kann über _CustomControllerProvider_ weiter konfiguriert werden.

_custom/app.php_
```
<?php
$controllerProvider = $app['routeManager']->mount('custom', 'Custom\Controller\TestController');
```

**mount($baseRoute, $controller)**

| Parameter | Typ | Optional | Beschreibung |
|:----------|:----|:---------|:-------------|
|         $baseRoute  |   String  |       nein   |         Name der Basis-Route. Alle weiteren Aufrufe sind dann unter _www.domain.de/BASIS_ROUTE/*_ erreichbar     |
|          $controller |   Areanet\PIM\Classes\Controller\BaseController  |      nein    |              |

- Rückgabe: _Areanet\PIM\Classes\Controller\Provider\Base\CustomControllerProvider_

!!! note "Basis-Route"
    Im obigen Beispiel sind alle im Controller aufgerufenen Aktionen über die Basis-URL _www.domain.de/custom_ erreichbar.
    
    **Beachten Sie, dass alle aufrufbaren Aktionen im Controller aber über die folgenden Schritten noch manuell konfiguriert werden müssen!**
    
    **Die "Basic-Route" $baseRoute muss eindeutig sein!

### Controller-Provider

Über den Controller-Provider können die einzelnen URL-Routen zu den Aktionen im Controller konfiguriert werden.

**get($route, $isSecure = false, $action_name = null)**

Hinzufügen einer URL-Route über HTTP-GET.

| Parameter | Typ                                           | Optional | Beschreibung                                                                                         |
|:----------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $route    | String                                        | nein     | URL-Route 'routename' wird automatisch auf die Methode 'routenameAction' im Contoller gemappt, wenn keine dritter Parameter _action_name_ angegeben ist. |
| $isSecure | Boolean | ja     |                                                                                                      | Erfordert Authentifizierung per Header-Token (Absichern von API-Aufrufen), andere Authentifizierungen werden derzeit noch nicht unterstützt.
|        $action_name   |   String                                            |     ja     |      anstatt automatischem Mapping wird die URL-Route auf die hier angegebene Methode im Controller gemappt.                                                                                                 |

- Rückgabe/Fluent Interface: _Areanet\PIM\Classes\Controller\Provider\Base\CustomControllerProvider_

```
<?php
$controllerProvider->get('secure', true);
// Ohne weitere Parameter wird automatisch die Methode _secureAction_
// in Ihrem benutzerdefinierten Controller aufgerufen.
```


**post($route, $isSecure = false, $action_name = null)**

Hinzufügen einer URL-Route über HTTP-POST.

| Parameter | Typ                                           | Optional | Beschreibung                                                                                         |
|:----------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $route    | String                                        | nein     | URL-Route 'routename' wird automatisch auf die Methode 'routenameAction' im Contoller gemappt, wenn keine dritter Parameter _action_name_ angegeben ist. |
| $isSecure | Boolean | ja     |                                                                                                      | Erfordert Authentifizierung per Header-Token (Absichern von API-Aufrufen), andere Authentifizierungen werden derzeit noch nicht unterstützt.
|        $action_name   |   String                                            |     ja     |      anstatt automatischem Mapping wird die URL-Route auf die hier angegebene Methode im Controller gemappt.                                                                                                 |

- Rückgabe/Fluent Interface: _Areanet\PIM\Classes\Controller\Provider\Base\CustomControllerProvider_

```
<?php
$controllerProvider->post('test', true);
```

### Beispiele

_custom/app.php_

```
<?php
$app['routeManager']->mount('custom', 'Custom\Controller\TestController')
    ->get('secure', true)
    ->get('test', false)
    ->get('hallo/{name}', false, 'hallo');
```

Die benutzerdefinierte Schnittstellen sind damit wie folgt zu erreichen:

- www.domain.de/custom/secure _mit gesetztem X-XSRF-TOKEN Header-Token_
- www.domain.de/custom/test
- www.domain.de/hallo/peter

!!! note "Silex URL-Routing"
    Weitere Informationen zum Routing unter [Silex URL-Routing](https://silex.sensiolabs.org/doc/1.3/usage.html#routing)

## Zugriff auf Systemfunktionen

Innerhalb Ihrer benutzerdefinierten Controller können Sie über Dependency Injection auf verschiedene Systemkomponenten und -funktionen zugreifen.

### Silex-Application

```
<?php

function controllerAction(){
    $app = $this->app; // Silex\Application
}
```


### Doctrine-ORM

```
<?php

function controllerAction(){
    $em = $this->app['orm.em']; // Doctrine\ORM\EntityManager
    
    $categories = $em->getRepository('Custom\Entity\Category')->findBy(array('isDeleted' => false), array('sorting'=>'asc'));
    $questions  = $em->getRepository('Custom\Entity\Question')->findBy(array('isDeleted' => false), array('sorting'=>'asc'));
}
```

### Doctrine-DBAL

```
<?php

function controllerAction(){
    $dbal = $this->app['database']; // Doctrine\DBAL\Connection
}
```

### Eingeloggter Benutzer
```
<?php

function controllerAction(){
    $user = $this->app['auth.user']; // Doctrine\ORM\EntityManager
}
```

### Hilfsfunktionen
```
<?php

function controllerAction(){
    $helper = $this->app['helper']; // Areanet\PIM\Classes\Helper
}
```

Die Helper-Klasse stellt derzeit lediglich eine Methode zur Verfügung.

**getFullEntityName($shortName)**

Gibt aus der Kurzschreibweise (z.B. Produkt oder _PIM\User_) den kompletten Pfad zurück (z.B. _Custom\Entity\Produkt_ oder _Areanet\PIM\Entity\User_)

| Parameter | Typ                                           | Optional | Beschreibung                                                                                         |
|:----------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $shortName    | String                                        | nein     | Kurzschreibweise des Entitäten-Namen. |

- Rückgabe: _String_



