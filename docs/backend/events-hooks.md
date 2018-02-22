# Events und Hooks

Events sind definierte Ereignisse, die im Workflow des Contentfly CMS auftreten bzw. auftreten können. 
Für jedes Event kann ein Eventhandler (oder auch mehrere) reqistriert werden, der automatisch beim Auftreten des Events vom Contentfly CMS aufgerufen wird.

## Eventhandler

###  Registrieren

_in custom/app.php_
```
<?php
use Symfony\Component\EventDispatcher\EventDispatcherInterface;

$app->extend('dispatcher', function (EventDispatcherInterface $dispatcher, $app) {
  
  $dispatcher->addListener('EVENT1', function ($event) {
    //Event-Handling
    $response = $event->getParam('response');
  });
  
  $dispatcher->addListener('EVENT2', function ($event) {
    //Event-Handling
    $response = $event->getParam('response');
  });
  
  return $dispatcher;

});
```

Jedes Event wird vom Contentfly CMS mit unterschiedlichen Parametern aufgerufen. Diese können über _$event->getParam(NAME)_ abgefragt werden. 
Die zum jeweiligen Event verfügbaren Parameter finden Sie in den folgenden Abschnitten. 

### Parameter anpassen

Die vom Event übergebenen Paramater können angepasst werden, um den weitergehende Programmausführung des Contentfly CMS zu beeinflussen. Beispielsweise kann bei der Auflistung der Objekte 
über die Schnittstelle _/api/list_ die Limitierung auf 2 Objekte gesetzt werden.

#### Objekte
Parameter die einen Objekttyp, z.B. _\Symfony\Component\HttpFoundation\Response_ haben, können direkt angepasst werden.
```
<?php
$dispatcher->addListener(EVENT_NAME, function (\Areanet\PIM\Classes\Event $event) {
  $response = $event->getParam('response'); // \Symfony\Component\HttpFoundation\Response
  $response->headers->set('Content-Type', 'text/html');
});
```

#### Arrays und Strings
Parameter, die vom Typ _Array_ oder _String_ sind, müssen nach der Anpassung wieder an die Eventklasse zurückgeschrieben werden.
```
<?php
$dispatcher->addListener(EVENT_NAME, function (\Areanet\PIM\Classes\Event $event) {
  $pushTitle = $event->getParam('pushTitle'); // String
  $pushTitle = 'foo';
  $event->setParam('pushTitle', $pushTitle);
});
```

### Ausführung abbrechen
Sie können eine komplette Ausführung des Contentfly CMS in einem Eventhandler abbrechen, um z.B. den Upload von Dateien für bestimmte Benutzer(gruppen) backend-seitig zu deaktivieren.

```
<?php
$dispatcher->addListener(EVENT_NAME, function (\Areanet\PIM\Classes\Event $event) {
  $user = $event->getParam('user'); // Areanet\Entity/User
  
  if($user->getAlias() == 'Peter Pan'){
    throw new Exception(''Upload für Peter Pan leider nicht erlaubt.');
  }
});
```

## Controller-Events

Die Controller Events sind, wie der Name schon sagt, an die Contentfly CMS-Controller gebunden. 

!!! note "Hinweis"
    Die Controller und entsprechenden Methoden 
    finden Sie im Ordner _appcms/areanet/PIM/Controller_.

### pim.controller.before 

Wird vor der eigentlichen Code-Ausführung von jedem Controller ausgeführt.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| app          |  Silex\Application   |


**Beispiel**
```
<?php
$dispatcher->addListener('pim.controller.before', function(\Areanet\PIM\Classes\Event $event){});
```

### pim.controller.before.CONTROLLER 

Wird vor der eigentlichen Code-Ausführung von jedem Controller mit dem Name _CONTROLLER_ ausgeführt.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| app          |  Silex\Application   |


**Beispiel**
```
<?php
$dispatcher->addListener('pim.controller.before.api', function(\Areanet\PIM\Classes\Event $event){});
```

### pim.controller.before.CONTROLLER.METHOD 
Wird vor der eigentlichen Code-Ausführung von jedem Controller mit dem Name _CONTROLLER_ und der Methode _METHOD_ ausgeführt.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| app          |  Silex\Application   |


**Beispiel**
```
<?php
$dispatcher->addListener('pim.controller.before.api.list', function(\Areanet\PIM\Classes\Event $event){});
```

### pim.controller.after 
Wird nach der eigentlichen Code-Ausführung von jedem Controller ausgeführt.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| response   |   \Symfony\Component\HttpFoundation\Response  |
| app          |  Silex\Application   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.controller.after', function(\Areanet\PIM\Classes\Event $event){});
```

### pim.controller.after.CONTROLLER 

Wird nach der eigentlichen Code-Ausführung von jedem Controller mit dem Name _CONTROLLER_ ausgeführt.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| response   |   \Symfony\Component\HttpFoundation\Response  |
| app          |  Silex\Application   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.controller.after.api', function(\Areanet\PIM\Classes\Event $event){});
```

### pim.controller.after.CONTROLLER.METHOD 
Wird vor der eigentlichen Code-Ausführung von jedem Controller mit dem Name _CONTROLLER_ und der Methode _METHOD_ ausgeführt.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| response   |   \Symfony\Component\HttpFoundation\Response  |
| app          |  Silex\Application   |


**Beispiel**
```
<?php
//JSON-Response nach /api/schema verändern
$dispatcher->addListener('pim.controller.after.api.schema', function (\Areanet\PIM\Classes\Event $event) {
    $response         = $event->getParam('response');
    $content          = json_decode($response->getContent());
    $content->message = "mySchemaAction";
    $response->setContent(json_encode($content));
});
```

## Entity-Events

Die Entity-Events sind, wie der Name schon sagt, an Statusupdates der Doctrine-Entities im Contentfly CMS gebunden. Die folgenden Entity-Events 
werden sowohl für die Standard-Entities, als auch für die benutzerdefinierten Entities aufgerufen.


### pim.entity.before.list
Wird vor der Auflistung einer Entität in der _list_-Methode im _Api_-Controller aufgerufen. Hier ist beispielsweise die Manipulation der Doctrine-Abfrage über
den _QueryBuilder_ möglich.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| queryBuilder          |  Doctrine\DBAL\QueryBuilder   |
| entity          |  String _Name der Entität_   |


**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.before.list', function(\Areanet\PIM\Classes\Event $event){
   $queryBuilder = $event->getParam('queryBuilder');
   $queryBuilder->setMaxResults(2); //MySQL LIMIT
});
```

### pim.entity.before.insert
Wird vor dem Hinzufügen (Neu Speichern) einer Entität in der _insert_-Methode im _Api_-Controller aufgerufen. Hier können beispielsweise 
die zu speichernden Daten/Eigenschaften über den Parameter _data_ manipuliert werden.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| data          |  Array _zu speicherende Daten_   |
| entity          |  String _Name der Entität_   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.before.insert', function(\Areanet\PIM\Classes\Event $event){
   $data = $event->getParam('data');
   $data['title'] = 'Test';
});
```

### pim.entity.after.insert
Wird nach dem Hinzufügen (Neu Speichern) einer Entität in der _insert_-Methode im _Api_-Controller aufgerufen.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| object          |  Areanet\PIM\Entity _Neue angelegtes Objekt der entsprechenden Entität_   |
| entity          |  String _Name der Entität_   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.after.insert', function(\Areanet\PIM\Classes\Event $event){
   $object = $event->getParam('object');
   $object->getTitle();
});
```

### pim.entity.before.insert
Wird vor dem Aktualisieren (Speichern) einer Entität in der _update_-Methode im _Api_-Controller aufgerufen. Hier können beispielsweise 
die zu speichernden Daten/Eigenschaften über den Parameter _data_ manipuliert werden.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| data          |  Array _zu speicherende Daten_   |
| id          |  String _Objekt-ID_   |
| entity          |  String _Name der Entität_   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.before.update', function(\Areanet\PIM\Classes\Event $event){
   $data = $event->getParam('data');
   $data['title'] = 'Test';
});
```

### pim.entity.after.update
Wird nach dem Aktualisieren (Speichern) einer Entität in der _update_-Methode im _Api_-Controller aufgerufen.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| data          |  Array _zu speicherende Daten_   |
| id          |  String _Objekt-ID_   |
| entity          |  String _Name der Entität_   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.after.update', function(\Areanet\PIM\Classes\Event $event){ });
```

### pim.entity.before.delete
Wird vor dem Aktualisieren (Speichern) einer Entität in der _delete_-Methode im _Api_-Controller aufgerufen. 

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| id          |  String _Objekt-ID_   |
| entity          |  String _Name der Entität_   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.before.delete', function(\Areanet\PIM\Classes\Event $event){});
```

### pim.entity.after.delete
Wird nach dem Löschen einer Entität in der _delete_-Methode im _Api_-Controller aufgerufen.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| id          |  String _Objekt-ID_   |
| entity          |  String _Name der Entität_   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.after.delete', function(\Areanet\PIM\Classes\Event $event){ });
```

##File-Events

Die File-Events sind, wie der Name schon sagt, an Dateifunktionen wie z.B. Uploads gebunden.


### pim.file.before.upload

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.before.upload', function(\Areanet\PIM\Classes\Event $event){ });
```

### pim.file.after.upload

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| fileObject          |  Areanet\PIM\Entity\File   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.after.upload', function(\Areanet\PIM\Classes\Event $event){ });
```

### pim.file.before.get

Wird vor 

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| fileObject          |  Areanet\PIM\Entity\File   |

**Beispiel**
```
<?php
$dispatcher->addListener('pim.entity.after.upload', function(\Areanet\PIM\Classes\Event $event){ });
```

## Push-Events

Die Push-Events sind, wie der Name schon sagt, an Push-Nachrichten gebunden.

### pim.push.before.send

Wird vor dem Versenden einer Push-Nachricht an registrierte iOS- und Android-Clients geworfen.

| Parameter | Typ |
|:----------|:----|
| request   |   \Symfony\Component\HttpFoundation\Request  |
| user          |  Areanet\PIM\Entity\User   |
| app          |  Silex\Application   |
| data          |  Array _Eingegebene Formulardaten der zu sendenden Push-Nachricht_   |
| pushTitle          |  String _Titel der Push-Nachricht_   |
| pushText          |  String _text der Push-Nachricht_   |
| additionalData          |  Array _Assoziatives Array it optionalen zusätzlichen Daten, das im Payload der Push-Nachricht mitgesendet wird._   |
**Beispiel**
```
<?php
$dispatcher->addListener('pim.push.before.send', function(\Areanet\PIM\Classes\Event $event){
    $user                       = $event->getParam('user');
    
    $additionalData             = $event->getParam('additionalData');
    $addtionalData['userAlias'] = $user->getAlias();
    
    $event->setParam('additionalData',$additionalData);
});
```

## Schema-Events

Wird aufgerufen, nachdem eine Klassen-Annotation verarbeitet wurde. Kann genutzt werden, um benutzerdefinierte Annotationen in das Schema aufzunehmen.
### pim.schema.after.classAnnotation

Wird nach jeder Verarbeitung einer Klassen-Annotation geworfen.

| Parameter | Typ |
|:----------|:----|
| classAnnotation   |    Doctrine\Common\Annotations\Annotation  |
| settings          |  Bisherige Schema-Eigenschaften der Entität als Array, wird wieder zurückgeschrieben (muss dafür im Eventhandler mit $event->setProperty('settings', $settings) gesetzt werden)   |
**Beispiel**
```
<?php
$dispatcher->addListener('pim.schema.after.classAnnotation', function(\Areanet\PIM\Classes\Event $event){
    $settings          = $event->getParam('settings');
    $classAnnotation   = $event->getParam('classAnnotation');
    
    if ($classAnnotation instanceof \Custom\Classes\Annotations\Test) {
        $settings['test'] = true;
        $event->setParam('settings', $settings);
    }
    
});
```

### pim.schema.after.propertyAnnotation

Wird aufgerufen, nachdem eine Property-Annotation verarbeitet wurde. Kann genutzt werden, um benutzerdefinierte Annotationen in das Schema aufzunehmen.

| Parameter | Typ |
|:----------|:----|
| propertyAnnotation   |    Doctrine\Common\Annotations\Annotation  |
| properties          |  Bisherige benutzerdefinierte Schema-Eigenschaften zur Property als Array, wird wieder zurückgeschrieben (muss dafür im Eventhandler mit $event->setProperty('properties', $properties) gesetzt werden)   |
**Beispiel**
```
<?php
$dispatcher->addListener('pim.schema.after.propertyAnnotation', function(\Areanet\PIM\Classes\Event $event){
    $properties         = $event->getParam('properties');
    $propertyAnnotation = $event->getParam('propertyAnnotation');
    
    if ($propertyAnnotation instanceof \Custom\Classes\Annotations\Map) {
        $properties['googleMap'] = true;
        $event->setParam('properties', $properties);
    }
    
});
```