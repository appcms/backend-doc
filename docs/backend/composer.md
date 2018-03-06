# Benutzerdefinierte Composer Bibliotheken

Sollten Sie in Ihrem Projekt weiterführende PHP-Bibliotheken benötigen, können diese über Composer[^1] im Ordner __custom__ installiert und verwendet werden. 

Wechseln Sie dazu in den Ordner __custom__ und installieren Sie das gewünschte Paket (z.B. PHPMailer) über Composer:

```
composer require phpmailer/phpmailer
```

Composer installiert daraufhin alle benötigten Abhängikeiten im Ordner __custom/vendor__. Das Contentfly CMS lädt automatisch - ohne weitere Konfiguration - 
die entsprechenden Autoload-Skripte. Sie können die installierte Bibliothek im Anschluss direkt in Ihrem Code verwenden.

**Beispiel custom/app.php**
```
<?php
$app['mailer'] = function (\Silex\Application $app) {

    $mailer = new \PHPMailer();
    
    ...

    return $mailer;
};
```

[^1]: https://getcomposer.org
