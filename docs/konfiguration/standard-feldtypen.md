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

_TODO_

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
@ORM\OneToMany(targetEntity="Custom\Entity\TARGET_ENTITY_JOIN", mappedBy="join_field_TARGET_ENTITY")
@PIM\ManyToMany(targetEntity="Custom\Entity\TARGET_ENTITY", mappedBy="join_field_TARGET_ENTITY_JOIN")
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