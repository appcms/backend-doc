# api/list

## Einführung
API-Endpoint zum Abruf von Objekten einer Entität.

## Rückgabe

Die Rückgabe der Daten erfolgt im JSON-Format auf
Basis des Doctrine ORM. Joins (1:n) und Multijoins (n:m) werden
automatisch umgewandelt und als Unterobjekte zurückgegeben.

## Beispiele

###  Einfache Abfrage

Gibt alle Objekte der Entität "Products" zurück. Dabei werden alle
Properties inklusive Joins und Multijoins ausgegeben.

```
POST: api/list

REQUEST:
{
	entity: 'Products'
}

RESPONSE
[
    {
        id: 1,
        name: 'Test',
        category: {
            id: 1,
            name: 'Testkategorie',
            active: true
        },
        price: 19.99
    },
    {
        id: 2,
        name: 'Test2',
        ...
    }
]
```

!!! Note "Performance"
    Kann bei vielen Objekten mit Joins/Multijoins zu
    Performance-Problemen führen. Abhilfe bietet der Parameter _flatten_.

###  Einfache Abfrage als flache Liste

Gibt alle Objekte der Entität "Products" als flache Liste zurück. Von verknüpften Joins/Multijoins werden lediglich die IDs zurückgegeben.

```
POST: api/list

REQUEST:
{
	entity: 'Products',
	flatten: true
}

RESPONSE
[
    {
        id: 1,
        name: 'Test',
        category: {
            id: 1
        },
        price: 19.99
    },
    {
        id: 2,
        name: 'Test2',
        ...
    }
]
```

###  Einfache Abfrage als flache Liste von bestimmten Properties

Gibt alle Objekte der Entität "Products" als flache Liste zurück. Es werden nur die angegebenen Properties zurückgegeben.

```
POST: api/list

REQUEST:
{
	entity: 'Products',
	flatten: true,
	properties: [id, name]
}

RESPONSE
[
    {
        id: 1,
        name: 'Test'
    },
    {
        id: 2,
        name: 'Test2'
    }
]
```

## Referenz

!!! tip "API-Referenz"
    [www.contentfly-cms.de/docs/api/1.4#api-Objekte-List](http://www.contentfly-cms.de/docs/api/1.4#api-Objekte-List)