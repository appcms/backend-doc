# api/update

## Einführung
API-Endpoint zum Ändern eines Objektes einer Entität.

## Beispiel


```
POST: api/update

REQUEST:
{
 entity: "Product",
 id: 1,
 data: {
     name: "Ein neueer Name",
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
    [www.contentfly-cms.de/docs/api/1.4#api-Objekte-Update](http://www.contentfly-cms.de/docs/api/1.4#api-Objekte-Update)