# Entitäten
## Doctrine

Die Datenmodellelierung im APP-CMS erfolgt objektorientiert über sogenannte Entitäten. Eine Entität repräsentiert eine
Datenbank-Tabelle und wird als PHP-Klasse angelegt. Die Verbindung von objektorientierter Entität und der relationalen 
Datenbank erfolgt über Doctrine ORM (_Object Relational Mapping_). 

!!! tip "Konfiguation der Entitäten"
    Im Doctrine ORM gibt es verschiedene Möglichkeiten die Entitäten zu konfigurieren (z.B. Welche Eigenschaft/Property 
    in der Entität soll auf welches Datenbank-Feld von welchem Typ gemappt werden?): XML, YAML oder PHP-Annotations. 
    
    Im APP-CMS werden nur PHP-Annotations unterstützt. Doctrine-basierte Annotationen müssen dabei mit _@ORM\_ angesprochen werden:
    
    ````
    @ORM\Table(name="testtabelle")
    ````

Weiterführende Informationen zu Doctrine ORM:

- <http://www.doctrine-project.org/>

### Standard-Entitäten

Das APP-CMS bringt bereits einige vordefinierte Entitäten mit, die beispielsweise für die Benutzerverwaltung zuständig sind. 
Die Standard-Entitäten finden Sie im Ordner _appcms/areanet/PIM/Entity_
  
- **User** Benutzer, kann durch PHP-Traits erweitert werden
- **Group** Benutzergruppe, kann durch PHP-Traits erweitert werden
- **Permission** Berechtigungen einer Gruppe
- **Token** Aktive Tokens von eingeloggten Benutzern oder statischen API-Tokens
- **PushToken** Token von iOS- und Android-Geräten für Push-Notifications
- **File** Dateien, kann durch PHP-Traits erweitert werden
- **Folder** Dateiordner, kann durch PHP-Traits erweitert werden 
- **Nav** Benutzerdefinierter Navigationsbereich im APP-CMS UI
- **NavItem** Benutzerdefinierter Navigationseintrag unterhalt eines Navigationsbereiches im
  APP-CMS UI
- **ThumbnailSetting** Bildgrößen, die automatisch im Dateisystem für
  jede Bilddatei angelegt werden


### Benutzerdefinierte Entitäten

Benutzerdefinierte Enitäten müssen im Ordner _custom/Entity_ gespeichert
werden und von der Klasse _Areanet\PIM\Entity\Base_,
_Areanet\PIM\Entity\BaseTree_ oder _Areanet\PIM\Entity\BaseSortable_
erben. Für jede Eigenschaft/Property müssen in der Entität entsprechende
Getter- und Setter-Methoden vorhanden sein.

## Arten von Entitäten

### Einfache Entität

```
<?php
namespace Custom\Entity;

use Areanet\PIM\Entity\Base;
use Areanet\PIM\Classes\Annotations as PIM;
use Custom\Classes\Annotations as CUSTOM;
use Areanet\PIM\Entity\BaseSortable;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="test")
 * @PIM\Config(label="Test")
 */
class Test extends Base
{
    /**
     * @ORM\Column(type="string", unique=false)
     * @PIM\Config(showInList=40, label="Titel")
     */
    protected $titel;

    /**
     * @return mixed
     */
    public function getTitel()
    {
        return $this->titel;
    }

    /**
     * @param mixed $titel
     */
    public function setTitel($titel)
    {
        $this->titel = $titel;
    }

}
```

### Sortierbare Entität

```
<?php
namespace Custom\Entity;

use Areanet\PIM\Entity\BaseSortable;
use Areanet\PIM\Classes\Annotations as PIM;
use Custom\Classes\Annotations as CUSTOM;
use Areanet\PIM\Entity\BaseSortable;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="test_sortable")
 * @PIM\Config(label="Test sortierbar")
 */
class Test extends BaseSortable
{
    /...
}
```

### Entität mit Baumstruktur

```
<?php
namespace Custom\Entity;

use Areanet\PIM\Entity\BaseTree;
use Areanet\PIM\Classes\Annotations as PIM;
use Custom\Classes\Annotations as CUSTOM;
use Areanet\PIM\Entity\BaseSortable;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="test_tree")
 * @PIM\Config(label="Baumtest")
 */
class Test extends BaseTree
{
    /...
}
```

## Annotations
### Klassen-Annotationen

Die Klassen-Annotationen gelten für die gesamte Entität. Auf Doctrine-Ebene muss hier zwingend die Annotation _@ORM\Entity_ gesetzt und 
zusätzlich der entsprechende Tabellennamm in der Datenbank definiert werden. Im Folgenden sind die zusätzlichen Konfigurationsmöglichkeiten
des APP-CMS _@PIM\Config_ aufgeführt:

``` hl_lines="6"
<?php
...
/**
 * @ORM\Entity
 * @ORM\Table(name="TABLE_NAME")
 * @PIM\Config(label="LABEL", sortBy="SORT_PROPERTY_NAME", sortOrder="ASC/DESC", sortRestrictTo="PROPERTY_NAME", labelProperty="PROPERTY_NAME", hide=false, readonly=false)
 */
class Test extends Base
{
...
```

| Eigenschaft    | Typ    | Standard | Beschreibung                                                                                                                                                                                    |
|:---------------|:-------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **label**          | String |          | Name der Entität im APP-CMS UI                                                                                                                                                                  |
| **sortBy**         | String | id       | Eigenschaft, nach er in den Listen im Standard sortiert werden soll                                                                                                                                     |
| **sortOrder**      | String | ASC      | ASC / DESC                                                                                                                                                                                      |
| **sortRestrictTo** | String |          | Begrenzt die Sortierung bei _BaseSortable-Entities_ auf die eingetragene Eigenschaft (z.B. "Eigenschaften" sollen nur sortiert werden können, wenn im Filter eine "Kategorie" ausgewählt wurde) |
| **labelProperty**  | String |          | Feld, das in den APP-CMS UI Formularen anstatt "Objekt ID bearbeiten" angezeigt werden soll                                                                                                     |
|      **hide**          |    Boolean    |   false       |   Entität wird nicht in der APP-CMS UI Navigation aufgelistet                                                                                                                                                                                              |
|      **readonly**          |    Boolean    |   false       |   Entität kann im APP-CMS UI nicht bearbeitet werden                                                                                                                                                                                              |


### Eigenschaften-Annotationen
#### Allgemein
``` hl_lines="4"
<?php
/**
  * @ORM\Column(type="string", unique=false)
  * @PIM\Config(showInList=40, label="Titel")
  */
protected $titel;
```
#### Optionale Felder
# Standard-Entitäten erweitern
## User-Entität
## File-Entität
## Folder-Entität