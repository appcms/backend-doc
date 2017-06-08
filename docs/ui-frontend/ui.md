# Anpassungen APP-CMS UI

## Anpassungen AngularJS UI

### HTML-Code

Die Oberfläche kann an bestimmten Stellen mit eigenem HTML-Code erweitert werden. Diese Stellen sind im Original-Angular-HTML-Quellcode wie folgt gekennzeichnet:

```
<pim-plugin key="BLOCK_NAME" uiblocks="uiblocks"></pim-plugin>
```

Über den *BLOCK_NAME* kann an der gewünschten Stelle in _custom/app.php_ eigener Code eingefügt werden:

```
<?php
$app['uiManager']->addBlock('NAVIGATION_PREPEND', 'blocks/deploy.html');
```

Die einzubindenden Block-Templates müssen im Ordner _/custom/Frontend/ui/default/views_ liegen. In den Block-Templates besteht der Zugriff auf alle Variablen im Scope des entsprechend übergeordneten Templates/Bereiches.

**Beispiel**

_custom/app.php_
```
<?php
$app['uiManager']->addBlock('LIST_TABLE_BODY_COL_APPEND', 'blocks/col.html');
```

Fügt an jede Spalte in der Auflistung von Objekten folgenden HTML-Code ein:

_/custom/Frontend/ui/default/views/blocks/col.html_
```
<button title="Infos zu Objekt ID {{object.id}}"><span class="glyphicon glyphicon-info-sign"></span></button>
```

!!! note "Hinweis"
    Die jeweiligen Blöcke, bzw. die entsprechenden Block-Namen sind aus den HTML-Dateien unter _/appcms/public/ui_ oder dem HTML-Quelltext des APP-CMS UI auszulesen.

### Logo

_custom/config.php_
```
<?php
$configDefault->FRONTEND_CUSTOM_LOGO = true;
```
!!! note "Hinweis"
    Das Logo muss in der entsprechenden Auflösung (185x44px) unter folgendem Pfad abgelegt werden: _custom/Frontend/ui/default/img/logo.png_

### Login-Hintergrund

_custom/config.php_
```
<?php
$configDefault->FRONTEND_CUSTOM_LOGIN_BG = true;
```
!!! note "Hinweis"
    Das Logo muss in der entsprechenden Auflösung (185x44px) unter folgendem Pfad abgelegt werden: _custom/Frontend/ui/default/img/bg_login.jpg_

### Titel 

_custom/config.php_
```
<?php
$configDefault->FRONTEND_TITLE          = "MaxMustermann APP-CMS";
```
 
### Willkommens-Meldung

_custom/config.php_
```
<?php
$configDefault->FRONTEND_WELCOME  = "Willkommen im MaxMustermann APP-CMS";
```

## Benutzerdefinierte UI-Routes  

### UI-Manager
Eigene AngularJS-Routen können über den UI-Manager _Areanet\PIM\Classes\Manager\UIManager_ in der _custom/app.php_ hinzugefügt werden. 
Der UI-Manager ist unter _$app['uiManager']_ erreichbar.

**addRoute($route, $templatePath, $controllerName)**

| Parameter     | Typ                                           | Optional | Beschreibung                                                                                         |
|:--------------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $route        | String                                        | nein     | URL-Route |
| $templatePath | String | nein     |                Pfad zum Template relativ zu _custom/Frontend/ui/default/views_                                                                                      |
| $controllerName       String       |                                               |      nein    |    AngularJS Controller-Name. Benutzerdefinierte Controller sollten unter _custom/Frontend/ui/default/scripts/controllers_ liegen.                                                                                                |

**addJSFile($filePath)**

| Parameter     | Typ                                           | Optional | Beschreibung                                                                                         |
|:--------------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $filePath        | String                                        | nein     | Pfad zur einzubindenden JS-Datei relativ zu _custom/Frontend/ui/default/scripts_  |

**addCSSFile($filePath)**

| Parameter     | Typ                                           | Optional | Beschreibung                                                                                         |
|:--------------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $filePath        | String                                        | nein     | Pfad zur einzubindenden CSS-Datei relativ zu _ccustom/Frontend/ui/default/styles/_  |


**addAngularModule($moduleName, $path)**

| Parameter     | Typ                                           | Optional | Beschreibung                                                                                         |
|:--------------|:----------------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------|
| $moduleName        | String                                        | nein     | Name des einzubindenden, externen AngularJS-Moduls  |
| $path        | String                                        | nein     | Pfad zum einzubindenden, externen AngularJS-Moduls relativ zu _ccustom/Frontend/ui/default/scripts/_  |



###  Beispiel UI-Route

_custom/app.php_
```
<?php
$app['uiManager']->addRoute('/custom/deploy', 'deploy.html', 'DeployCtrl');
$app['uiManager']->addJSFile('controllers/list-button.controller.js');
```

_custom/Frontend/ui/default/scripts/controllers/list-button.controller.js_
```
(function() {
    'use strict';

    angular
        .module('app')
        .controller('DeployCtrl', DeployCtrl);

    function DeployCtrl($scope, $rootScope, $location, localStorageService, $cookies){

    }

})();
```

### Beispiel Angular-Module

Bindet das Moule [Angular File Saver](http://alferov.github.io/angular-file-saver/) zur Verwendung im JS-Code ein.

_custom/app.php_
```
<?php
$app['uiManager']->addJSFile('FileSaver.js');
$app['uiManager']->addAngularModule('ngFileSaver', 'angular-file-saver.js');
```

_custom/Frontend/ui/default/scripts/controllers/download.controller.js_
```
(function() {
    'use strict';

    angular
        .module('app')
        .controller('DownloadCtrl', DownloadCtrl);

    function DownloadCtrl($routeParams, localStorageService, FileSaver){
        
        $http({
            method: 'GET',
            url: '/file/get/'  + $routeParams['fileId'],
            headers: { 'X-Token': localStorageService.get('token') },
            responseType: 'blob'
        }).then(function successCallback(response) {

            var data = new Blob([response.data], { type: 'application/pdf' });
            FileSaver.saveAs(data, $routeParams['fileId'] + '.pdf');
            
        }, function errorCallback(response) {
            
            //Error
            
        });
        
    }
}
```

## Filter vorbelegen   

In den Auflistung (list-Controller) können Filter von folgende Feldtypen durch URL-Parameter vorbelegt werden:

- **join**
- **multijoin**


Der URL-Aufruf lautet wie folgt:

```
#/list/[ENTITY?]?f_[PROPERTY]=[VALUE]
```

Um z.B. direkt die Ansprechpartner einer Firma mit der ID=23 anzuzeigen:
```
#/list/Ansprechpartner?f_firma=23
```