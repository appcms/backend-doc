# Erweiterung Standard-Entitäten

## PHP-Traits
Folgende Entities des Contentfly CMS können über [Traits](http://php.net/manual/de/language.oop5.traits.php) erweitert werden:

- **Areanet\PIM\Entity\User**
- **Areanet\PIM\Entity\Group**
- **Areanet\PIM\Entity\File**
- **Areanet\PIM\Entity\Folder**

Die entsprechenden Traits liegen im Ordner _/custom/Traits_

!!! danger "Achtung!"
    Die folgenden Traits müssen vorhanden sein, ansonsten wirft PHP einen Fatal-Error - die Dateien dürfen nicht gelöscht werden!

- **custom/Traits/User.php**
- **custom/Traits/Group.php**
- **custom/Traits/File.php**
- **custom/Traits/Folder.php**

!!! note "Datenbank-Aktualisierung"
    Bei Änderungen an der **User-Entity** muss die Datenbank per Konsole aktualisiert werden.
    
## Beispiel

Im Beispiel wird die Benutzer-Entität um die Eigenschaft, bzw. das Feld _lastName_ erweitert.

_custom/Traits/User.php_
```
<?php

namespace Custom\Traits;

trait User{
    /**
     * @ORM\Column(type="string", nullable=true)
     * @PIM\Config(showInList=60, label="Nachname")
     */
    protected $lastName;
    
    /**
     * @return mixed
     */
    public function getLastName()
    {
        return $this->lastName;
    }

    /**
     * @param mixed $lastName
     */
    public function setLastName($lastName)
    {
        $this->lastName = $lastName;
    }
}
        
```