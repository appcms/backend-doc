# Konfiguration

## Einfache Konfiguration
Die grundlegende Konfigurations erfolgt in der Datei custom/config.php
und muss dort über ein Objekt der Klasse _\Areanet\PIM\Classes\Config()_
an _\Areanet\PIM\Classes\Config\Factory_ übergeben werden. Die
Factory-Klasse stellt dem APP-CMS anhand des Server-Namens die
entsprechenden Konfigurationseinstellungen bereit. Damit ist es möglich
für verschiedene Umgebungen (z.B. Lokal, Testserver, Live-Server,...)
unterschiedliche Konfigurationseinstellungen vorzuhalten. Der einfachste
Aufbau der Konfigurationsdatei sieht wie folgt aus:

**_custom/config.php_**
```
<?php
   
use \Areanet\PIM\Classes\Config\Factory;
$configFactory = Factory::getInstance();

/*
* Lokaler Server
*/

$configDefault = new \Areanet\PIM\Classes\Config();

$configDefault->DB_HOST = 'localhost';
$configDefault->DB_NAME = 'db';
$configDefault->DB_USER = 'user';
$configDefault->DB_PASS = 'pass';
$configDefault->DB_GUID_STRATEGY = true;

$configFactory->setConfig($configDefault);
```
## Multiple Konfigurationen
Innherhalb der Konfigurationsdatei können beliebig viele Umgebungen (z.B. lokale Entwicklungsumgebung, Testserver, Live-Server, ...) anhand des Server-Names verwaltet werden. Die unterschiedllichen Konfigurationseinstellungen können dabei voneinander erben, so dass nur die Unterschiede zwischen zwei Umgebungen angepasst werden müssen.

**_custom/config.php_**
```
<?php
   
use \Areanet\PIM\Classes\Config\Factory;
$configFactory = Factory::getInstance();

/*
* Lokaler Server
*/

$configDefault = new \Areanet\PIM\Classes\Config();
...

/*
 * Staging Server
 */
$configStaging = new \Areanet\PIM\Classes\Config('staging.server.com', $configDefault);

$configStaging->DB_HOST = 'localhost';
$configStaging->DB_NAME = 'staging_db';
$configStaging->DB_USER = 'staging_user';
$configStaging->DB_PASS = 'staging_pass';

$configFactory->setConfig($configStaging);
```
An die Klasse _\Areanet\PIM\Classes\Config($serverName, $parentConfig)_ können zwei optionale Parameter übergeben werden.

| Parameter   | Beschreibung  |
|:--------------|:--|
| **$serverName** |  Server-Name für die die Konfiguration gelten soll. Das APP-CMS ordnet anhand der PHP-Server-Laufzeitvariablen _SERVER_NAME_ die korrekte Konfiguration zu. Wird keine Übereinstimmung gefunden, wir die Standard-Konfiguration geladen. |
| **$parentConfig** | Wird _$parentConfig_ angegeben, werden alle Einstellung aus dieser Konfiguration übernommen und können gezielt überschrieben werden.  |

## Referenz
### Datenbank

| Eigenschaft          | Typ   | Standard | Beschreibung                                                                                                                                                                                         |
|:---------------------|:---|:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DB_HOST**          |  String  | --       | Datenbank-Host                                                                                                                                                                                       |
| **DB_NAME**          |  String  | --       | Datenbank-Name                                                                                                                                                                                       |
| **DB_USER**          |  String  | --       | Datenbank-Benutzer                                                                                                                                                                                   |
| **DB_PASS**          |  String  | --       | Datenbank-Passwort                                                                                                                                                                                   |
| **DB_CHARSET**       |  String  | utf8     | Zeichensatz der Datenbank-Verbindung                                                                                                                                                                 |
| **DB_NESTED_LEVELS** |  Integer  | 3        | Bis zu welcher Ebene/Tiefe werden untergeordnete Objekte über die API mitausgegeben. Die Anzahl wirkt sich auf die Performance aus (je mehr Ebenen geladen werden, desto schlechter die Performance) |
| **DB_GUID_STRATEGY** | Boolean   | true     | Neue Objekte werden standardmäßig mit UUIDs als Strings angelegt (Doctrine: _@ORM\GeneratedValue(strategy=UUID)_). Ansonsten werden Integer als Auto-Increment-Werte verwendet (Doctrine: _@ORM\GeneratedValue(strategy=AUTO)_). Nur bei UUIDs ist die automatische Synchronisations mit Clients (z.B. über das iOS- oder Android-SDK) gewährleistet!   |

### Backend

| Eigenschaft                 | Typ     | Standard      | Beschreibung                                                                                                                                                                    |
|:----------------------------|:--------|:--------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **APP_DEBUG**               | Boolean | false         | Aktiviert Debug-Ausgabe bei API-Aufrufen. Sollte in der Produktivumgebung deaktiviert werden.                                                                                   |
| **APP_ENABLE_SCHEMA_CACHE** | Boolean | true          | Das Generieren der Schema-Dateien aus den Entity-Annotations erfordert einiges an Rechenaufwand. Aus Performance-Gründen sollte das Caching im Produktivmodus aktiviert werden. |
| **$APP_CACHE_DRIVER** | String | filesystem          | Mögliche Werte _filesystem_, _apc_, _memcached_ - der Cache wird nur verwendet wenn APP_DEBUG auf _false_ gesetzt ist. |
| **APP_TOKEN_TIMEOUT**       | Integer | 1800          | Standard-Gültigkeitsdauer für API-TOKEN in ms. Kann über die Benutzer-/Gruppeneinstellung individuell angepasst werden                                                          |
| **APP_CHECK_TOKEN_TIMEOUT** | Boolean | true          | Überprüfung Gültigkeitsdauer bei API-TOKEN                                                                                                                                      |
| **APP_MAILTO**              | String  |               | Standard-E-Mail-Adresse für eventuellen PHP-Mailversand                                                                                                                         |
| **APP_MAILFROM**            | String  |               | Standard-E-Mail-Adresse für eventuellen PHP-Mailversand                                                                                                                         |
| **APP_TIMEZONE**            | String  | Europe/Berlin | Standard-PHP-Zeitzone                                                                                                                                                           |
| **APP_ENABLE_XSENDFILE**        | Boolean | false         | Aktivier x_sendfile (Apache/nginx-Modul) für die Auslieferung von Dateien über _file/get/ID_. Kann mit zusätzlicher Konfigurations-/Anpassungsaufwand am Server verbunden sein. |

### CORS 

Cross-Origin Resource Sharing: Erlaubt den Zugriff von Webclients über andere Domains.

| Eigenschaft                 | Typ     | Standard      | Beschreibung                                                                                                                                                                    |
|:----------------------------|:--------|:--------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **APP_ALLOW_ORIGIN**               | String | null         | Setzt eine Domain, über die CORS-Requests erlaubt sind, z.B. 'domain.de' oder '*'                                                                                   |
| **APP_ALLOW_CREDENTIALS** | Boolean | false          | Erlaubt die Übergabe von Cookies bei CORS-Requests. Zusätzlich muss im Client _withCredentials_ aktiviert sein. |
| **APP_ALLOW_METHODS**       | String | post,get          | Erlaubte HTTP-Methoden für einen CORS-Request                                                      |
| **APP_ALLOW_HEADERS**       | String | content-type, x-xsrf-token          | Erlaubte HTTP-Header für einen CORS-Reques                                                     |
| **APP_MAX_AGE**       | Integer | 0         | Zeitdauer in der Prefligth-Request bei CORS-Anfragen gecached werden.                                                     |

### Frontend

| Eigenschaft                            | Typ     | Standard              | Beschreibung                                                                                                                                                                                                                                                                                                                                                                          |
|:---------------------------------------|:--------|:----------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FRONTEND_TITLE**                     | String  | APP-CMS               | Zeichenkette, die als Titel angezeigt werden soll                                                                                                                                                                                                                                                                                                                                     |
| **FRONTEND_WELCOME**                   | String  | Willkommen im APP-CMS | Zeichenkette, die als Willkommensseite nach dem Login angezeigt werden soll                                                                                                                                                                                                                                                                                                           |
| **FRONTEND_URL**                       | String  | /                     | Virtueller Pfad, unter dem das APP-CMS über eine URL erreichbar sein soll. Im Standard ist das Login des APP-CMS unter der installierten Domain direkt zu erreichen, z.B. _www.das-app-cms.de_. Mit der Angabe von _admin_ wäre das Login/Frontend unter _www.das-app-cms.de/admin_ zu erreichen. Somit könnte unter _www.das-app-cms.de/_ eine individuelle Programmierung erfolgen. |
| **FRONTEND_CUSTOM_NAVIGATION**               | Boolean | false                 | Ist der Wert auf _true_, kann über das APP-CMS-UI eine benutzerdefinierte Navigation für die Entitäten erstellt werden.                                                                                                                                                                                                                                                                       |
| **FRONTEND_CUSTOM_LOGO**               | Boolean | false                 | Ist der Wert auf _true_, wird im Frontend das Logo vom Pfad _/custom/Frontend/ui/default/img/logo.png_ geladen.                                                                                                                                                                                                                                                                       |
| **FRONTEND_CUSTOM_LOGIN_BG**           | Boolean | false                 | Ist der Wert auf _true_, wird im Loginfenster des Frontends das Hintergrundbild vom Pfad _/custom/Frontend/ui/default/img/bg_login.jpg_ geladen.                                                                                                                                                                                                                                      |
| **FRONTEND_FORM_IMAGE_SQUARE_PREVIEW** | Boolean | true                  | Alle Vorschaubilder in Listen und Formularen werden quadratisch zugeschnitten. Bei _false_ werden die original Dimensionen beibehalten                                                                                                                                                                                                                                                |
| **FRONTEND_ITEMS_PER_PAGE**                                       |  Integer       |      20                 |     Anzahl von Einträgen pro Seite in den Auflistungen                                                                                                                                                                                                                                                                                                                                                                                  |
| **FRONTEND_UI**                        | String  | default               | Legt fest, welches Frontend als Benutzeroberfläche geladen werden soll. Standardmäßig ist das das mitgelieferte AngularJS-Frontend im Ordner _default_ im Pfad _public/ui_                                                                                                                                                                                                            |
|                                        |         |                       |                                                                                                                                                                                                                                                                                                                                                                                       |

### Dateien

| Eigenschaft               | Typ     | Standard                                            | Beschreibung                                                                                                            |
|:--------------------------|:--------|:----------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| **FILE_PROCESSORS**       | Array   | array('\Areanet\PIM\Classes\File\Processing\Image') | Welche Dateiverarbeitungen sind im System registriert. Diese generieren abhängig vom Dateityp z.B. Vorschaubilder. Mitgeliefert werden automatische Bildgenerierungen über _\Areanet\PIM\Classes\File\Processing\Image' für GDLib und _\Areanet\PIM\Classes\File\Processing\ImageMagick_ für ImageMagick    |
| **FILE_HASH_MUST_UNIQUE** | Boolean | false                                               | Legt fest, ob eine Datei (anhand des Datei-Hashes) nur einmal physikalisch vorhanden sein darf.                         |
| **FILE_CACHE_LIFETIME**   | Integer | 604800                                              | Laufzeit für den HTTP-Cache bei Dateiabrufen über z.B. _/file/get/12_ in Sekunden - der Standard ist auf 7 Tage gesetzt |
| **IMAGEMAGICK_EXECUTABLE**                          |       String  |   convert                                                  |             Pfad zum ausführbaren ImageMagick-Befehl auf dem Server, sollte _FILE_PROCESSORS_ auf _array('\Areanet\PIM\Classes\File\Processing\ImageMagick')_ gestellt sein.                                                                                                       |

### Push-Notifcations

| Eigenschaft         | Typ    | Standard | Beschreibung                                                                              |
|:--------------------|:-------|:---------|:------------------------------------------------------------------------------------------|
| **PUSH_GOOGLE_KEY** | String | null     | API-Key von Google zum Versenden von Push-Notifications                                   |
| **PUSH_APPLE_CERT** | String | null     | Relativer Pfad/Dateiname des Push-Zertifikates unter _/custom_                            |
| **PUSH_APPLE_SANDBOX**                    |  Boolean      |    false      |     Versendung von Test-Push-Notifications über das Sandbox-Gateway von Apple                                                                             |

### Sicherheit

Nur relevant, wenn String-/Text-Properties mit der Option encoded = true konfiguriert sind.

| Eigenschaft         | Typ    | Standard | Beschreibung                                                                              |
|:--------------------|:-------|:---------|:------------------------------------------------------------------------------------------|
| **SECURITY_CIPHER_KEY** | String |      | Wenn Verschlüsselung bei Textfeldern verwendet wird, muss ein Schlüssel/Passwort zwingend gesetzt und darf im nachhinein nicht mehr geändert werden!                           |
| **SECURITY_CIPHER_METHOD** | String | AES-128-ECB     | Verschlüsselungsmethode, sollte nicht geändert werden                                |

### Konsole

| Eigenschaft         | Typ    | Standard | Beschreibung                                                                              |
|:--------------------|:-------|:---------|:------------------------------------------------------------------------------------------|
| **SYSTEM_PHP_CLI_COMMAND** | String | php     | Pfad zur ausführbaren CLI-Variante von PHP auf dem Server.                                 |

