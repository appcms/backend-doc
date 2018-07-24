# api/single

## Einführung
API-Endpoint zum Abruf eines einzelnen Objektes einer Entität.

## Rückgabe

Die Rückgabe der Daten erfolgt im JSON-Format auf
Basis des Doctrine ORM. Joins (1:n) und Multijoins (n:m) werden
automatisch umgewandelt und als Unterobjekte zurückgegeben.

## Beispiele

###  Einfache Abfrage anhand der ID

Gibt ein Objekt der Entität "Products" anhand der ID zurück. Dabei
werden alle Properties inklusive Joins und Multijoins ausgegeben.

```
POST: api/single

REQUEST:
{
	entity: 'Products',
	id: 1
}

RESPONSE
{
    id: 1,
    name: 'Test',
    category: {
        id: 1,
        name: 'Testkategorie',
        active: true
    },
    price: 19.99
}
```


###  Einfache Where-Abfrage

Gibt ein Objekt der Entität "Products" anhand einer einfachen Abfrage zurück.

```
POST: api/query

REQUEST:
{
	entity: 'Products',
	where: {name: 'Test'}
}

RESPONSE
{
    id: 1,
    name: 'Test',
    category: {
        id: 1,
        name: 'Testkategorie',
        active: true
    },
    price: 19.99
}
```

!!! Note "Umfangreiche Abfragen"
    Umfangreiche Abfragen können Sie über
    den Endpoint [api/query](query.md) realisieren.

## Referenz

!!! tip "API-Referenz"
    [www.contentfly-cms.de/docs/api/1.4#api-Objekte-List](http://www.contentfly-cms.de/docs/api/1.4#api-Objekte-List)