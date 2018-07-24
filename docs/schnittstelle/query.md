# api/query

## Einführung
Erweiterter API-Endpoint, über den nahezu beliebige Abfragen auf die
Datenbank/Entitäten gestellt werden können. Die Abfragesyntax basiert
dabei auf dem
[DBAL-QueryBuilder](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/query-builder.html)
von Doctrine. Der JSON-Request (siehe Beispiele unten) wird im
Contentfly CMS in eine analoge DBAL-Abfrage über den QueryBuilder umgewandelt.

## Rückgabe

Die Rückgabe der Daten erfolgt im JSON-Format. Durch die DBAL-Abfrage
erfolgt die Rückgabe direkt auf Datenbankebene und nicht auf Doctrine Entitäten.

## Beispiele

###  Einfache Abfrage

```
POST: api/query

REQUEST:
{
	select: '*',
	from: entityName
}
```

### Einfache Abfrage mit Where

```
POST: api/query

REQUEST:
{
	select: '*',
	from: entityName,
	where: {field: value},
}
```

### Abfrage mit Group, Count(), Limit und Offset

```
POST: api/query

REQUEST:
{
	select: ['field1', 'field2', 'COUNT(id) AS users'],
	from: entityName
	where: {field: value},
	groupBy: field,
	having: {field: value},
	orderBy: {field: 'ASC'},
	addOrderBy: {field: 'DESC'},
	setFirstResult: 10, //Offfset
	setMaxResults: 20, //Limit
}
```

### Join mit Where, GroupBy und Having

```
POST: api/query

REQUEST:
{
	select: ['alias1.field1', 'alias1.field2'],
	from: {entity1: alias1},
	innerJoin:['alias1', 'table2', 'alias2', 'alias1.id = alias2.field_id']
	where: {field: value},
	groupBy: field,
	having: {field: value}
}
```

### Join und erweiterte Where-Abfrage

```
POST: api/query

REQUEST:
{
	select: ['alias1.field1', 'alias1.field2'],
	from: {entity1: alias1},
	innerJoin:['alias1', 'table2', 'alias2', 'alias1.id = alias2.field_id']
	where: {'field1 = ? OR field2 = ?': [value1, value2]},
	andWhere: {field3: value3}
	groupBy: field,
	having: {field: value}
}
```

## Referenz

!!! tip "API-Referenz"
    [www.contentfly-cms.de/docs/api/1.4#api-Objekte-Query](http://www.contentfly-cms.de/docs/api/1.4#api-Objekte-Query)