# Benutzerdefinierter Login-Manager

Über einen benutzerdefinierten Login-Manager können Sie das standardmäßige Login-Verhalten des Contentfly CMS erweitern oder anpassen. Folgende Beispiele können mit einem eigenen Login-Manager umgesetzt werden:

- Benutzerlogin nur durch einen Pincode
- Benutzerlogin gegenüber einem Drittsystem (z.B: ERP-System), SSO (Single Sign On)

## Login über API

Benutzerdefinierte Login-Manager können über die API verwendet werden. Dafür ist der Authentifizierung lediglich der entsprechende Name des Login-Managers anzugeben.

```
POST: auth/login

REQUEST:
{
  "user:" : "username",
  "pass": "password",
  "loginManager": "CustomLoginManager"
}
```

## Login-Manager erstellen

Um einen benutzerdefinierten Login-Manager verwenden zu können, muss dieser im Ordner _custom/Classes_ von der Klasse _Areanet\PIM\Classes\Manager\LoginManager_ abgeleitet werden. Zwingend implementiert werden muss dabei die Mathode _auth()_, die einen gültigen Benutzer vom Typ _Areanet\PIM\Entity\User_ zurückgeben muss.

_custom/Classes/CustomLoginManager.php_
```
<?php
namespace Custom\Classes;

use Areanet\PIM\Classes\Manager\LoginManager;

class CustomLoginManager extends LoginManager
{
    public function auth(){
        $user = ...
        return $user; //Areanet\PIM\Entity\User
    }

}
```


## Beispiel: SSO

### API-Aufruf

```
POST: auth/login

REQUEST:
{
  "user:" : "username",
  "pass": "password",
  "loginManager": "ERPLoginManager"
}
```

### Login-Manager

_custom/Classes/ERPLoginManager.php_
```
<?php
namespace Custom\Classes;

use Areanet\PIM\Classes\Manager\LoginManager;

class ERPLoginManager extends LoginManager
{
    public function auth(){
        $username = $this->request->get('user');
        $password = $this->request->get('pass');
        if(!$this->erpAuth($username, $password)){
            throw new \Exception('Authentifizeriung fehlgeschlagen.');
        }

        return $this->createManagedUser($username);
    }

    protected function erpAuth($username, $password){
        //Prüfe gegen ERP-Drittsystem per Curl
        return true;
    }
}
```
Im obigen Beispiel prüft der Login-Manager die Authentifizeriung über Benutzername und Passwort gegenüber einem externen ERP-System. Ist die Authentifizeriung über das ERP-System erfolgreich, legt die Hilfsmtethode createManagedUser() einen Dummy-Benutzer (einen sogenannten "Managed User") im Contentfly CMS an und gibt das User-Objekt entsprechend wieder zurück.

!!! Note "Managed Users"
    Managed Users können sich nicht über das normale UI-Login im Backend anmelden. Eine Authentifizeriung ist nur über die Login-API und der Angabe des Login-Managers (s.o.) möglich. Somit erfolgt die Authentifizeriung immer gegenüber dem Drittsystem.

## Beispiel: Login über Pincode

### API-Aufruf

```
POST: auth/login

REQUEST:
{
  "pin:" : "1234",
  "loginManager": "PincodeLoginManager"
}
```

### Login-Manager

_custom/Classes/PincodeLoginManager.php_
```
<?php
namespace Custom\Classes;

use Areanet\PIM\Classes\Manager\LoginManager;

class PincodeLoginManager extends LoginManager
{
    public function auth(){
        $pin = $this->request->get('pin');
        
        $user = $this->app['orm.em']->getRepository('Areanet\PIM\Entity\User')->findOneBy(array('pin' => $pin));
        if (!$user) {
            throw new \Exception('Ungültiger Pincode.', 401);
        }

        return $user;
    }
}
```