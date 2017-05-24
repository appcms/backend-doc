h1. Custom-Repository

Standardmäßig kann auf die Entities über das Standard Doctrine-Repository "Doctrine\ORM\EntityRepository":http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/working-with-objects.html#querying zugegriffen werden:
<pre>
$repo = $app['orm.em']->getRepository('Custom\\Entity\\ENTITY_NAME');
$repo->findAll();
$repo->findBy...()
...
</pre>
 
Zu jeder Entity kann aber auch eine eigene Repository-Klasse mit eigenen Abfragen implementiert werden. Dazu muss die Repository-Klasse in der Entity-Annotation bekannt und entsprechend implementiert werden. Die eigene Repository wird dann automatisch bei obigem Aufruf im Hintergrund geladen.

h2. Beispiel

_custom/Entity/Test.php_
<pre>
namespace Custom\Entity;

use Areanet\PIM\Entity\Base;
use Areanet\PIM\Classes\Annotations as PIM;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity(repositoryClass="Custom\Classes\Repository\TestRepository")
 * @ORM\Table(name="test")
 * @PIM\Config(label = "Test")
 */
class Test extends Base
{
...
}
</pre>

_custom/Classes/Repository/TestRepository.php_
<pre>
<?php
namespace Custom\Classes\Repository;

use Doctrine\ORM\EntityRepository;

class TestRepository extends EntityRepository{

    public function findByTest(){
       ...
    }
}

</pre>

*Aufruf*
<pre>
$app['orm.em']->getRepository('Custom\\Entity\\Test')->findByTest();
</pre>