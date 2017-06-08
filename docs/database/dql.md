#  Doctrine DQL

Als Alternative zu den [Doctrine Repositories](repositories.md) kann auch über die [Doctrine Query Language](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/dql-doctrine-query-language.html) zugegriffen werden.

**Beispiel**
```
<?php
$query   = $app['orm.em']->createQuery("SELECT e FROM Custom\\Entit\\ENTITY_NAME e");
$objects = $query->getResult();
```