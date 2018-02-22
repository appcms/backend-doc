# Standard-Feldtypen

Folgende Typen werden automatisch aus den dargestellten Annotations gemappt und im Contentfly CMS UI entsprechend dargestellt. Im Backend sind die im Standard mitgelieferten Eigenschaften-Typen 
unter _appcms/areanet/PIM/Classes/Types_ definiert, die korrespondierende AngularJS-Directive im Contentfly CMS UI unter _appcms/areanet/PIM-UI/default/assets/types_

## Checkbox
```
<?php
/**
 * @ORM\Column(type="boolean")
 */
protected $checkboxProperty;
```

## Dateiupload 1:n (einfach)
```
<?php
/**
 * @ORM\ManyToOne(targetEntity="Areanet\PIM\Entity\File")
 * @ORM\JoinColumn(onDelete="SET NULL")
 */
protected $images;
```

## Dateiupload n:m (mehrfach)
```
<?php
/**
 * @ORM\ManyToMany(targetEntity="Areanet\PIM\Entity\File")
 * @ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")})
 */
protected $images;
```

## Dateiupload n:m (mehrfach sortierbar)
```
<?php
/**
 * @ORM\OneToMany(targetEntity="Custom\Entity\ProductImages", mappedBy="product")
 * @PIM\ManyToMany(targetEntity="Areanet\PIM\Entity\File", mappedBy="image")
 */
protected $images;
```

Für sortierbare, verknüpfte Entitäten muss eine "Zwischen-Entität" (vgl. n-m-Join bei einer Datenbank angelegt werden) vom Typ _BaseSortable_ angelegt werden. 
Die Doctrine Annotatin _@ORM\OneToMany_ ist die eigentlich relevante Konfiguration für die Datenbank. Die Annotation _@PIM\ManyToMany_ dient dem Contentfly CMS UI dazu, 
um die Oberfläche korrekt darstellen zu können.

```
<?php
/**
 * @ORM\Entity
 * @ORM\Table(name="product_images")
 * @PIM\Config(hide=true)
 */
class ProductImages extends Areanet\PIM\Entity\BaseSortable
{

    /**
     * @ORM\ManyToOne(targetEntity="Custom\Entity\Product", inversedBy="images")
     * @ORM\JoinColumn(onDelete="CASCADE")
     */
    protected $product;

    /**
     * @ORM\ManyToOne(targetEntity="Areanet\PIM\Entity\File")
     * @ORM\JoinColumn(onDelete="CASCADE")
     * @PIM\Config(label="Detailbilder", accept="image/*")
     */
    protected $image;
    
    ...;
}    
```

## Datum
```
<?php
/**
 * @ORM\Column(type="datetime")
*/
protected $datetimeProperty;
```

## Dezimalzahl
 
```
<?php
/**
 * @ORM\Column(type="decimal", precision=10, scale=2)
*/
protected $decimalProperty;
```

## Entity-Auswahlliste

Listet alle konfiguierten Entitäten in einer Select-Auswahlliste auf:
 
```
<?php
/**
 * @ORM\Column(type="string")
 * @PIM\EntitySelector()
*/
protected $entitySelectorProperty;
```

## Ganzzahl

```
<?php
/**
 * @ORM\Column(type="integer")
 */
protected $integerProperty;
```

## Join 1:1

```
<?php
/**
 * @ORM\OneToOne(targetEntity="Custom\Entity\TARGET_ENTITY")
 * @ORM\JoinColumn(onDelete="SET NULL")
*/
protected $targetObject;
```

## Join 1:n

**Custom\Entity\SOURCE_ENTITY**
```
<?php
/**
 * @ORM\ManyToOne(targetEntity="Custom\Entity\TARGET_ENTITY")
 * @ORM\JoinColumn(onDelete="SET NULL")
*/
protected $targetObject;
```

**Bidirektional in Custom\Entity\TARGET_ENTITY**

Bei einer bidirektionalen Verbindung wird in der Datenbank-Tabelle der TARGET_ENTITY kein Feld angelegt, die Verbindung ist lediglich "virtuell" in Doctrine vorhanden.

```
<?php
/**
 * @ORM\OneToMany(targetEntity="Custom\Entity\SOURCE_ENTITY", mappedBy="targetObject")
 */
protected $sourceObjects;
```


## Join n:m

**Custom\Entity\SOURCE_ENTITY**
```
<?php
/**
 * @ORM\ManyToMany(targetEntity="Custom\Entity\TARGET_ENTITY")
 * @ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")})
*/
protected $targetObjects;
```

**Bidirektional in Custom\Entity\TARGET_ENTITY**
```
<?php
/**
 * @ORM\ManyToMany(targetEntity="Custom\Entity\SOURCE_ENTITY", mappedBy="targetObjects")
*/
protected $sourceObjects;
```


## Join n:m (Selbstreferenzierend)

```
<?php
/**
 * @ORM\ManyToMany(targetEntity="Custom\Entity\TARGET_ENTITY")
 * @ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")}, inverseJoinColumns={@ORM\JoinColumn(onDelete="CASCADE", name="REFERENZ_ID", referencedColumnName="id")})
*/
protected $sourceObjects;
```

!!! note "Beispiel"
    Es sollen zu Produkten jeweilige Alternativprodukte zugewiesen werden können. Beide Objekte sind jeweils eine Entität von _Custom\Entity\Product_, die intern in der Datenbank in der 
    Tabelle _product_ abgelegt sind. Bei einem n:m-Join würde Doctrine im Standard eine Tabelle _product_alt_ mit den beiden Feldern _product_id_ (Original-Produkt) und noch einmal
    _produkt_id_ (Alternaiv-Produkt) anlegen, was zwangsläufig zu einem Datenbankfehler führt.
    
    In diesem Fall muss also in der Doctrine Annotation _@ORM\JoinTable_ der Datenbank-Feldname des Alternativ-Produktes name="REFERENZ_ID" entsprechend angepasst werden.
    
    ```
    <?php
    @ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")}, inverseJoinColumns={@ORM\JoinColumn(onDelete="CASCADE", name="alternativprodukt_id", referencedColumnName="id")})
    ```

## Join n:m (sortierbar)
```
<?php
/**
 * @ORM\OneToMany(targetEntity="Custom\Entity\ProductNotes", mappedBy="product")
 * @PIM\ManyToMany(targetEntity="Custom\Entity\Note", mappedBy="note")
*/
protected $notes;
```

Für sortierbare, verknüpfte Entitäten muss eine "Zwischen-Entität" (vgl. n-m-Join bei einer Datenbank angelegt werden) vom Typ _BaseSortable_ angelegt werden. 
Die Doctrine Annotatin _@ORM\OneToMany_ ist die eigentlich relevante Konfiguration für die Datenbank. Die Annotation _@PIM\ManyToMany_ dient dem Contentfly CMS UI dazu, 
um die Oberfläche korrekt darstellen zu können.

```
<?php
/**
 * @ORM\Entity
 * @ORM\Table(name="product_notes")
 * @PIM\Config(hide=true)
 */
class ProductNotes extends Areanet\PIM\Entity\BaseSortable
{

    /**
     * @ORM\ManyToOne(targetEntity="Custom\Entity\Product", inversedBy="notes")
     * @ORM\JoinColumn(onDelete="CASCADE")
     */
    protected $product;

    /**
     * @ORM\ManyToOne(targetEntity="Custom\Entity\Note")
     * @ORM\JoinColumn(onDelete="CASCADE")
     * @PIM\Config(label="Detailbilder")
     */
    protected $note;
    
    ...;
}    
```

##  Textfeld einzeilig

```
<?php
/**
 * @ORM\Column(type="string")
*/
protected $stringProperty;
```

## Textfeld mehrzeilig

```
<?php
/**
 * @ORM\Column(type="text")
 * @PIM\Textarea(lines=10)
*/
protected $textProperty;
```
                                                                                                               
!!! note "Hinweis"
    Die Annoation _@PIM\Textarea_ ist optional, die Erkennung eines mehrzeiligen Textfeldes erfolgt bereits durch _type="text"_

## Textfeld mit RTE
```
<?php
/**
 * @ORM\Column(type="text")
 * @PIM\Rte()
*/
protected $rteProperty;
```

## Select-Feld
```
<?php
/**
 * @ORM\Column(type="string")
 * @PIM\Select(options="VALUE=LABEL, VALUE=LABEL, VALUE=LABEL")
*/
protected $selectProperty;
```

## Uhrzeit

Standardformat Stunden:Minute
```
<?php
/**
 * @ORM\Column(type="time")
*/
protected $timeProperty;
```

Benutzerdefiniertes Format, siehe <http://php.net/manual/de/function.date.php>
```
<?php
/**
 * @ORM\Column(type="time")
 * @PIM\Time(format='H:i:s')
*/
protected $timeProperty;
```