# Entitäten
## Doctrine

Die Datenmodellelierung im APP-CMS erfolgt objektorientiert über sogenannte Entitäten. Eine Entität repräsentiert eine
Datenbank-Tabelle und wird als PHP-Klasse angelegt. Die Verbindung von objektorientierter Entität und der relationalen 
Datenbank erfolgt über Doctrine ORM (_Object Relational Mapping_). 

!!! tip "Konfiguation der Entitäten"
    Im Doctrine ORM gibt es verschiedene Möglichkeiten die Entitäten zu konfigurieren (z.B. Welche Eigenschaft/Property 
    in der Entität soll auf welches Datenbank-Feld von welchem Typ gemappt werden?): 
    XML, YAML oder PHP-Annotations. 
    
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

!!! tip "Anpassung Standard-Entitäten"
    Die Standard-Entitäten _User_, _Group_, _File_ und Folder_ können durch PHP-Traits erweitert werden, siehe [Anpassung Standard-Entitäten](../backend/standard-entitaeten.md)

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

Optionen für @PIM\Config:

| Eigenschaft    | Typ    | Standard | Beschreibung                                                                                                                                                                                    |
|:---------------|:-------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **label**          | String |          | Name der Entität im APP-CMS UI                                                                                                                                                                  |
| **sortBy**         | String | id       | Eigenschaft, nach er in den Listen im Standard sortiert werden soll                                                                                                                                     |
| **sortOrder**      | String | ASC      | ASC / DESC                                                                                                                                                                                      |
| **sortRestrictTo** | String |          | Begrenzt die Sortierung bei _BaseSortable-Entities_ auf die eingetragene Eigenschaft (z.B. "Eigenschaften" sollen nur sortiert werden können, wenn im Filter eine "Kategorie" ausgewählt wurde) |
| **labelProperty**  | String |          | Feld, das in den APP-CMS UI Formularen anstatt "Objekt ID bearbeiten" angezeigt werden soll                                                                                                     |
|      **hide**          |    Boolean    |   false       |   Entität wird nicht in der APP-CMS UI Navigation aufgelistet                                                                                                                                                                                              |
|      **readonly**          |    Boolean    |   false       |   Entität kann im APP-CMS UI nicht bearbeitet werden                                                                                                                                                                                              |
| **tabs**                 |     JSON-String    |          |  Optional können die Eigenschaften in den Bearbeitungs-Formularen des APP-CMS UI auf mehrere Tabs aufgeteilt werden: _{'tabKey1' : 'Tab Title 1', 'tabKey2' : 'Tab Title 2'}_                                                                                                                                                                                              |


### Eigenschaften-Annotationen

Anhand den Standard-Doctrine-Annotationen ermitteld das APP-CMS automatisch das passende Formularfeld und Darstellung im APP-CMS UI. 

- Typ _string_ = Einzeiliges Textfeld
- Typ _text_ = Mehrzeiliges Textfeld
- Typ _boolean_ = Checkbox
- ...

Dieses Standard-Verhalten kann aber durch spezielle Annotationen des APP-CMS (_@PIM_) ergänzt und/oder angepasst werden.

#### Allgemein
``` hl_lines="4"
<?php
/**
  * @ORM\Column(type="string")
  * @PIM\Config(showInList=40, label="Titel")
  */
protected $titel;
```
Optionen für @PIM\Config:

| Eigenschaft    | Typ     | Standard | Beschreibung                                                                                                                                                                                   |
|:---------------|:--------|:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **label**      | String  |          | Name der Eigenschaft im APP-CMS UI                                                                                                                                                             |
| **showInList** | Integer |          | Zeigt den Inhalt dieser Eigenschaft in der Auflistung der Entität an der an eingegebenen Position an. Im obigen Beispiel wird die Eigenschaft _titel_ in der Liste in der Spalte 40 angezeigt. |
| **hide**       | Boolean | false    | Das Formularfeld ist im APP-CMS UI Formular versteckt                                                                                                                                          |
| **readonly**   | Boolean | false    | Das Formularfeld kann im APP-CMS UI Formular nur gelesen werden                                                                                                                                |
| **tab**               |   String      |          |       Schlüssel des Tabs, in dem die Eigenschaft optional im APP-CMS UI angezeigt werden soll. Dieser Schlüssel/Key muss in der Klassen-Annotation _@PIM\Config_ in der Option _tabs_ entsprechend konfiguriert sein.                                                                                                                                                                                         |
| **isFilterable** | Boolean | false    | Die Einträge aus dieser Eigenschaft werden als Filter angezeigt                                                                                                                                |
| **isDatalist**   | Boolean | false    | Rendert eine Filterliste einer Eigenschaft mit _isFilterable=true_ als Auto-Suggest-Feld dar                                                                                                   |
| **encoded**      | Boolean | false    | Automatische Verschlüsselung der Inhalte in der Datenbank                                                                                                                                      |

!!! note "Hinweis für _isFilterable_"
    - Bei _join/multijoin/virtualjoin_-Eigenschaften werden die entsprechenden Einträge aus verknüpften Entität geladen
    - Bei normalen Eigenschaften (z.B. vom Typ _String_) werden alle eindeutigen, gespeicherten Werte aufgelistet
    - _Select_- und _Boolean_-Felder werden automatisch in den Filtern angezeigt

!!! note "Hinweis für _isDatalist_"
    Die Option _isDatalist_ ist nur für _join/multijoin_-Eigenschaften vefügbar
  
!!! note "Hinweis für _encoded_"
    - nur für normale Text-Eigenschaften (Type _String_ und _Text_) verfügbar
    - Konfigurationsvariable _SECURITY_CIPHER_KEY_ muss gesetzt sein.
    
#### Pflichtfeld

Das APP-CMS UI erkennt automatisch, ob eine Eigenschaft einen _NULL_-Wert enthalten darf und stellt das gerenderte Formularfeld entsprechend als Pflichtfeld oder optionales Feld dar. Ob eine Eigenschaft einen _NULL_-Wert enthalten darf wird über die Doctrine-Annotation _@ORM\Column_ konfiguriert

```hl_lines="3 9"
<?php
/**
  * @ORM\Column(type="string", nullable=true)
  * @PIM\Config(showInList=40, label="Kein Pflichtfeld")
  */
protected $keinPflichtfeld;

/**
  * @ORM\Column(type="string", nullable=false)
  * @PIM\Config(showInList=50, label="Pflichtfeld")
  */
protected $pflichtfeld;
```

!!! note "Hinweis" 
    Ohne Angabe der Option _nullable_ sind alle Eigenschaften standardmäßig Pflichtfelder!

#### Feldtypen

Das APP-CMS ermittelt aus den angegebenen Annotations automatisch den entsprechenden Feldtyp, also auch wie das Feld im APP-CMS UI im Formular ausgegeben wird. 

* [Auflistung Standard-Feldtypen](standard-feldtypen.md)

