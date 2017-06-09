# Standard-Feldtypen

Folgende Typen werden automatisch aus den dargestellten Annotations gemappt und im APP-CMS UI entsprechend dargestellt. Im Backend sind die im Standard mitgelieferten Eigenschaften-Typen 
unter _appcms/areanet/PIM/Classes/Types_ definiert, die korrespondierende AngularJS-Directive im APP-CMS UI unter _appcms/areanet/PIM-UI/default/assets/types_

## Checkbox
```
<?php
@ORM\Column(type="boolean")
```

## Dateiupload 1:n (einfach)
```
<?php
@ORM\ManyToOne(targetEntity="Areanet\PIM\Entity\File")
@ORM\JoinColumn(onDelete="SET NULL")
```

## Dateiupload n:m (mehrfach)
```
<?php
@ORM\ManyToMany(targetEntity="Areanet\PIM\Entity\File")
@ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")})
```

## Dateiupload n:m (mehrfach sortierbar)
```
<?php
@ORM\OneToMany(targetEntity="Custom\Entity\ProductImages", mappedBy="product")
@PIM\ManyToMany(targetEntity="Areanet\PIM\Entity\File", mappedBy="image")
protected $images;
```

Für sortierbare, verknüpfte Entitäten muss eine "Zwischen-Entität" (vgl. n-m-Join bei einer Datenbank angelegt werden) vom Typ _BaseSortable_ angelegt werden. 
Die Doctrine Annotatin _@ORM\OneToMany_ ist die eigentlich relevante Konfiguration für die Datenbank. Die Annotation _@PIM\ManyToMany_ dient dem APP-CMS UI dazu, 
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
@ORM\Column(type="datetime")
```

## Dezimalzahl
 
```
<?php
@ORM\Column(type="decimal", precision=10, scale=2)
```

## Entity-Auswahlliste

Listet alle konfiguierten Entitäten in einer Select-Auswahlliste auf:
 
```
<?php
@ORM\Column(type="string")
@PIM\EntitySelector()
```

## Ganzzahl

```
<?php
@ORM\Column(type="integer")
```

## Join 1:1

```
<?php
@ORM\OneToOne(targetEntity="Custom\Entity\TARGET_ENTITY")
@ORM\JoinColumn(onDelete="SET NULL")
```

## Join 1:n
```
<?php
@ORM\ManyToOne(targetEntity="Custom\Entity\TARGET_ENTITY")
@ORM\JoinColumn(onDelete="SET NULL")
```

## Join n:m
```
<?php
@ORM\ManyToMany(targetEntity="Custom\Entity\TARGET_ENTITY")
@ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")})
```

## Join n:m (Selbstreferenzierend)

```
<?php
@ORM\ManyToMany(targetEntity="Custom\Entity\TARGET_ENTITY")
@ORM\JoinTable(name="join_table_name", joinColumns={@ORM\JoinColumn(onDelete="CASCADE")}, inverseJoinColumns={@ORM\JoinColumn(onDelete="CASCADE", name="REFERENZ_ID", referencedColumnName="id")})
```

## Join n:m (sortierbar)
```
<?php
@ORM\OneToMany(targetEntity="Custom\Entity\ProductNotes", mappedBy="product")
@PIM\ManyToMany(targetEntity="Custom\Entity\Note", mappedBy="note")
protected $notes;
```

Für sortierbare, verknüpfte Entitäten muss eine "Zwischen-Entität" (vgl. n-m-Join bei einer Datenbank angelegt werden) vom Typ _BaseSortable_ angelegt werden. 
Die Doctrine Annotatin _@ORM\OneToMany_ ist die eigentlich relevante Konfiguration für die Datenbank. Die Annotation _@PIM\ManyToMany_ dient dem APP-CMS UI dazu, 
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
@ORM\Column(type="string")
```

## Textfeld mehrzeilig

```
<?php
@ORM\Column(type="text")
@PIM\Textarea(lines=10)
```
                                                                                                               
!!! note "Hinweis"
    Die Annoation _@PIM\Textarea_ ist optional, die Erkennung eines mehrzeiligen Textfeldes erfolgt bereits durch _type="text"_

## Textfeld mit RTE
```
<?php
@ORM\Column(type="text")
@PIM\Rte()
```

## Select-Feld
```
<?php
@ORM\Column(type="string")
@PIM\Select(options="VALUE=LABEL, VALUE=LABEL, VALUE=LABEL")
```

## Uhrzeit

Standardformat Stunden:Minute
```
<?php
@ORM\Column(type="time")
```

Benutzerdefiniertes Format, siehe <http://php.net/manual/de/function.date.php>
```
<?php
@ORM\Column(type="time")
@PIM\Time(format='H:i:s')
```