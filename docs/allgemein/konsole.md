# Konsolenbefehle

## Einführung

Die Aktualisierung der Datenbank aus das Ausführen von bestimmten Aktionen und Befehlen erfolgt im Contentfly CMS in der Regel über die Konsole. Alle folgenden Konsolenbefehle müssen im Ordner _appcms_ (in dem sich die Datei _console.php_) befindet ausgeführt werden.

!!! Hinweis
    Um den vollen Funktionsumfang nutzen zu können, müssen Sie Konsolenzugriff auf Ihre Contentfly CMS Instanz, z.B. per SSH haben.

Alle folgenden Konsolenbefehle müssen über einen PHP-Interpreter auf der Kommandozeile ausgeführt werden. Je nach Systemumgebung können die Aufrufe entsprechend variieren, z.B.

```
#!/bin/bash

#Aufruf mit global installierten PHP
php ...

#Aufruf mit einer speziellen CLI-Variante (z.B. bei Mittwald)
php_cli ..

#Aufruf über absoluten Pfad
/usr/bin/php

```

Unter Linux/Mac können Sie den Pfad zur ausführbaren PHP-Version mit folgendem Befehl ermitteln:
```
#!/bin/bash
whereis php
```

## Konfiguration

Wird das Contentfly CMS über den Browser aufgerufen, ermittelt das System über die Umgebungsvariable _SERVER_NAME_ automatisch die passende [Konfiguration](../konfiguration/system.md#multiple-konfigurationen). Auf der Konsole kann/muss die Umgebungsvariable _SERVER_NAME_ manuell gesetzt werden:

```
#!/bin/bash
SERVER_NAME="staging.server.com" php ...
```
!!! danger "Achtung bei multipen Konfigurationen"
    Wenn Sie mehrere Konfigurationen (z.B. lokales Testsystem, Staging-System, Produktiv-System) gesetzt haben, geben Sie immer den _SERVER_NAME_ mit an, um die Befehle auf der richtigen Datenbank auszuführen.

## Doctrine-Befehle

Alle Doctrine-Befehle zur Aktualisierung/Wartung der Datenbank sind über das Skript _console.php_ aufzurufen.

**Aktualisierung der Datenbank**
```
#!/bin/bash
php console.php orm:schema:update --force
```

**Ausgabe der Aktualisierung als SQL**
```
#!/bin/bash
php console.php orm:schema:update --dump-sql
```

## Setup und Einrichtung

Wenn Sie das Contentfly CMS nicht über den browserbasierten Installationsassistenten aufgerufen haben oder das Contentfly CMS zurücksetzen müssen, können Sie über die Konsole den Setup-Befehl aufrufen. Dieser führt folgende Aktionen aus:

* Hinzufügen/Zurücksetzen des Benutzers _admin_ mit dem Passwort _admin_
* Setzen der Standard-Bildgrößen

**Einrichtung/Rücksetzung starten**
```
#!/bin/bash
php console.php appcms:setup
```

## Eigene Befehle

Im Contentfly CMS können auch eigene Konsolenbefehle (z.B. für Daten-Imports oder Routine-Aufgaben) hinzugefügt werden. Diese können über folgenden Aufruf gestartet werden:

**Benutzerdefinierten Befehl ausführen**
```
#!/bin/bash
php console.php custom:BEFEHL
```

* [Hinzufügen von eigenen Konsolenbefehlen](../backend/konsole.md)

**Eine Auflistung aller konfigurierten Konsolenbefehle erhalten Sie über**
```
#!/bin/bash
php console.php
```
