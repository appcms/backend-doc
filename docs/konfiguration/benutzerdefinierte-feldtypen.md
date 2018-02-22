# Benutzerdefinierte Feldtypen 

Im Contentfly CMS können die [Standard-Feldtypen](standard-feldtypen.md) durch eigene Feldtypen erweitert werden. Ein Feldtyp besteht dabei aus einer Backend-Komponente (PHP) und einer UI-Komponente (AngularJS-Direktive und Template).

## Backend-Komponente

### Struktur
Anzulegen sind folgende Dateien/Klassen:

* **custom/Classes/Types/[TYPNAME]Type.php**:<br>
  Als Klasse abgeleitet von _Areanet\PIM\Classes\Type\CustomType_.
* **custom/Classes/Annotations/[TYPNAME].php** - optional:<br>
  Als Doctrine-Annotation-Definition, um den Feldtyp weiterführend konfigurierbar zu machen.

### Beispiel

_custom/Classes/Types/MapType.php_
```
<?php
namespace Custom\Classes\Types;

use Areanet\PIM\Classes\Type\CustomType;

class MapType extends CustomType
{
    public function getAlias()
    {
        return 'map';
    }

    public function getAnnotationFile()
    {
        return 'Map';
    }

    public function doMatch($propertyAnnotations){
        if(!isset($propertyAnnotations['Custom\\Classes\\Annotations\\Map'])) {
            return false;
        }

        return true;
    }

    public function processSchema($key, $defaultValue, $propertyAnnotations){
        $schema                 = parent::processSchema($key, $defaultValue, $propertyAnnotations);
        $propertyAnnotations    = $propertyAnnotations['Custom\\Classes\\Annotations\\Map'];
     
        return $schema;
    }


}
```

_custom/Classes/Annotations/Map.php_
```
<?php
namespace Custom\Classes\Annotations;

use Doctrine\Common\Annotations\Annotation;

/**
 * @Annotation
 */
final class Map extends Annotation
{
    /**
     * @var string
     */
     public $showGoogleMap = true;
}
```

##  Frontend-Komponente


### Struktur

Anzulegen sind folgende Dateien:

* **custom/Frontend/ui/default/types/[TYPNAME]/[TYPNAME].directive.js**:<br>
  Als AngularJS-Direktive mit dem Name _pim[TYPNAME]_
* **custom/Frontend/ui/default/types/[TYPNAME]/[TYPNAME].html** - optional:<br>
  HTML-Template

### Beispiel

_custom/Frontend/ui/default/types/test/test.directive.js_
```
(function() {
    'use strict';

    angular
        .module('app')
        .directive('pimTest', pimTest);


    function pimTest(localStorageService){
        return {
            restrict: 'E',
            scope: {
                key: '=', config: '=', value: '=', isvalid: '=', isSubmit: '=', onChangeCallback: '&'
            },
            templateUrl: function(){
                return '/custom/Frontend/ui/default/types/test/test.html'
            },
            link: function(scope, element, attrs){
                scope.writable = parseInt(attrs.writable) > 0;

                if(scope.value === undefined && scope.config.default != null){
                    scope.value = Boolean(scope.config.default);
                }

                scope.$watch('value',function(data){

                    scope.value = Boolean(scope.value);
                    if(scope.writable) scope.onChangeCallback({key: scope.key, value: scope.value ? scope.value : false});
                },true)
            }
        }
    }

})();
```

_custom/Frontend/ui/default/types/test/test.html_
```
<pim-plugin key="TYPE_PREPEND" uiblocks="uiblocks"></pim-plugin>
<div class="col-sm-10 col-sm-offset-2">
    <h1>TEST-TYPE</h1>
    <pim-plugin key="TYPE_FIELD_PREPEND" uiblocks="uiblocks"></pim-plugin>
    <div class="checkbox">
        <label for="{{key}}" class="ng-class: {optional: config.nullable}">
            <input type="checkbox"  id="{{key}}" name="{{key}}" ng-required="!config.nullable && !config.readonly && !config.hide" ng-disabled="config.readonly || !writable" ng-model="value">
            {{config.label}}<pim-plugin key="TYPE_LABEL_APPEND" uiblocks="uiblocks"></pim-plugin></label>
    </div>
    <pim-plugin key="TYPE_FIELD_APPEND" uiblocks="uiblocks"></pim-plugin>
</div>
<div class="col-sm-10 col-sm-offset-2" ng-show="isSubmit && !config.readonly && !config.hide && isValid">
    <p class="error">Bitte fülllen Sie das Feld korrekt aus!</p>
</div>
<pim-plugin key="TYPE_APPEND" uiblocks="uiblocks"></pim-plugin>
</pre>
```

## Einbindung

### Registrieren

Ein eigener Feldtyp muss in der _custom/app.php_ registriert werden
```
<?php
$app['typeManager']->registerType(new \Custom\Classes\Types\MapType($app));
```

### Annotation anwenden

Danach kann der neue Feldtyp für eine Eigenschaft in den Entitäten wie folgt genutzt werden:
```
<?php
use Custom\Classes\Annotations as CUSTOM;

..

/**
  * @ORM\Column(type="string", nullable=true)
  * @PIM\Config(showInList=60, label="Titel")
  * @CUSTOM\Map(showGoogleMap=false)
  */

protected $test;
```
