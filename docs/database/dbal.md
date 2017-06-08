#  Doctrine DQL

Da sich das APP-CMS als flexible Lösung in Bezug auf die Datenhaltung bezeichnet, kann auf die Daten auch über [Doctrine DBAL](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/) direkt in SQL zugegriffen werden.

**Beispiel**
```
<?php
$statement = $app['database']->prepare("SELECT id FROM entityTable WHERE fieldName = :value LIMIT 1");
$statement->bindValue("value", $value);
$statement->execute();

$object = $stmt_ersteller->fetch();

```