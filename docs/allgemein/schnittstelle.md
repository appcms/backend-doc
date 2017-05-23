# Schnittstelle

##  Headless CMS

Das APP-CMS wurde ursprünglich als **Headless CMS** konzipiert. Also als Content Management System, 
dass ohne eigene Oberfläche auskommt und lediglich für die Datenhaltung zuständig ist. Da allerdings 
der einfache Zugriff auf die Daten elementar wichtig ist, haben wir auf Basis der Schnitstelle das 
APP-CMS UI ergänzt.

Das APP-CMS ist damit nicht mehr wirklich "headless", die API bleibt aber die zentrale Einheit des APP-CMS. 
Das APP-CMS ist ein **API-zentriertes Content Management System**.

##  Datenformat

Die API nimmt alle Anfragen im JSON-Format entgegen und gibt entsprechend auch wieder Daten im JSON-Format zurück.

## HTTP-Methoden

Im Gegensatz zu REST-Webservices nach Lehrbuch mit GET-, PUT-, DELETE- und POST-Aufrufen, besteht die APP-CMS API
in großen Teilen aus POST-Aufrufen.

```
POST: api/single

REQUEST:
{
  "entity:" : "Mandant",
  "id": "232323-23afdsdf-23fdfss"
}
```

!!! Note "Hinweis"
    In einer nächsten Version des APP-CMS werden beispielsweise auch GET-Aufrufe wie
    _api/single/Mandant/232323-23afdsdf-23fdfss_ möglich sein.

## HTTP-Header

Um die APP-CMS API nutzen zu können, müssen folgende HTTP-Header gesetzt sein:

* **Content-Type**: application/json
* **X-XSRF-TOKEN**: Token zur Authentifizierung

## X-XSRF-TOKEN
Einen Token erhalten Sie zurück, wenn Sie sich über die Login-API per Benutzername und Passwort authentifizieren:

```
POST: auth/login

RESPONSE:
{
    "message" : "Login successful",
    "token" : "69ee5a5eaeea63a6feda2a61ebbb5fcf01b520165948c681bb2dbd5e5f9aae979c829df22aebb3e4948e9816e994756f1f35a63f18eb88f30ab0bc359c70b581",
    "user" : {
        ...
    }
}
```

Der Token ist an den Benutzer gebunden. Damit greifen bei allen API-Aufrufen, die entsprechend für die Benutzergruppe definierten Berechtigungen. Jeder Token
hat eine Laufzeit (_Token-Timeout_). Diese kann über das APP-CMS UI über _Administration->Gruppen_ eingestellt werden.

Alternativ kann über das APP-CMS UI auch ein fester Token für z.b. API-Aufrufe unter _Administration/Einstellungen_ aus einer App angelegt werden:

![Token ](../images/screen_token.png)

Jeder Token muss einem Benutzer zugeordnet werden. Das Feld _Referrer_ dient als Titel und wird derzeit nicht weiter ausgewertet.
        
## API-Dokumentation

- [API-Dokumentation](/doku/api/1.3.0)

[^1]: http://docs.doctrine-project.org/projects/doctrine-common/en/latest/reference/annotations.html
