# Benutzerdefinierte Konsolen-Befehle

Im APP-CMS können eigene Konsolen-Befehle erstellt und über die Konsole gestartet werden.

## Konsolen-Befehl anlegen

Anlegen eines Befehles unter _custom/Commands/[CommandName].php als abgeleitete Klasse von _Areanet\PIM\Classes\Command\CustomCommand_

**Beispiel**

_custom/Commands/DatabaseImport.php_
```
<?php
namespace Custom\Command;

use Areanet\PIM\Classes\Command\CustomCommand;

class DatabaseImport extends CustomCommand
{

    protected function configure()
    {
        parent::configure();

        $this
            ->setName('test:start')
            ->setDescription('Test-Befehl')
        ;
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $app        = $this->getSilexApplication();
        $this->em   = $app['orm.em'];
        
        //....
        
        $output->writeln("<info>Der Import wurde erfolgreich durchgeführt.</info>");
    }
}
```

!!! Hinweis
    Weitere Informationen finden Sie unter [symfony.com/doc/current/console.html](http://symfony.com/doc/current/console.html)

## Konsolen-Befehl registrieren

Der angelegte Konsolen-Befehl muss zum Abschluss noch in der _custom/app.php_ bekannt gemacht werden.

```
<?php
$app['consoleManager']->addCommand(new \Custom\Command\DatabaseImport());
```


## Konsolen-Befehl ausführen

In der Konsole (Shell) im Ordner _appcms/_
```
php console.php custom:test:start
```

## Alternative Konfiguration laden

Im Standard verwendet die Konsole die Default-Konfiguration. Um eine andere Konfiguration (z.B. Datenbank) zu verwenden, müssen Sie den Server-Namen _custom/config.php_ mit übergeben:

```
SERVER_NAME="[SERVER_NAME]" php console.php custom:test:start
```