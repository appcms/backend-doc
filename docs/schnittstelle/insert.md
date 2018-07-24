# api/insert

## Einführung
API-Endpoint zum Hinzufügen eines neues Objektes einer Entität.

## Beispiel


```
POST: api/insert

REQUEST:
{
 entity: "Product",
 data: {
     name: "Ein neues Produkt",
     // Join 1:n
     category: {
        id: 1
     }
     // Datum im Format yyyy-mm-dd hh:ii:ss
     active_from: "2016-02-18 15:30:00",
     // Multijoin n:m
     cross_selling: [
        {
            id: 2
        },
        {
            id: 6
        }
     ]
 }

```


## Referenz

!!! tip "API-Referenz"
    [www.contentfly-cms.de/docs/api/1.4#api-Objekte-Insert](http://www.contentfly-cms.de/docs/api/1.4#api-Objekte-Insert)