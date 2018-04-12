# Running Laravel with xDebug on Windows
## Setting up Docker on windows
- [Install Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)
- Start docker App

## Vessel setup
Bevor wir Vessel installieren müssen muss [Composer](https://getcomposer.org/Composer-Setup.exe) und [php](https://windows.php.net/download/) auf dem PC installiert werden.

### Requirements
**PHP**
- [php download page](https://windows.php.net/download/)
- Non-Thread-safe Version Zip auswählen
- unter `C:\php7.2\` entpacken
- Der Systemvariable PATH den `C:\php7.2` hinzufügen
- Power Shell öffnen und mit `php -v` die installation verifizieren

**Composer**
- download [Composer](https://getcomposer.org/Composer-Setup.exe) and execute
- Power Shell öffnen und mit `composer` die installation verifizieren

**Git Bash**
- download and install [Git Bash](https://gitforwindows.org/) => [Erklärung](https://github.com/shipping-docker/vessel#supported-systems)

**Laravel Projekt**
- über den Laravel installer oder `composer create-project laravel/laravel` ein Laravel Projekt anlegen

**DockerHost User**
Um Docker auf Windows mit "Shared Drives" unabhängig vom Standort zu verwenden, sollte ein "DockerHost" user angelegt werden. [Quelle](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/)

- Win + I -> Konten -> Andere Personen -> Diesem PC eine andere Person hinzufügen
- Ich kenne die Information für diese Person nicht -> Benutzer ohne Microsoft-Konto hinzufügen
=> Benutzername: DockerHost + beliebiges PW

- Auf "DockerHost" Konto klicken -> "Kontotyp ändern" in "Administrator"
- Auf DockerHost Konto anmelden und auf das den eigenen User Ordner zugreifen -> fortsetzen
- Auf eigenem Konto anmelden -> Docker Settings öffnen -> shared Drives -> Alle Haken setzen -> Apply -> DockerHost user + pw eingeben

### Install
- Docker starten
- im erstellten Laravel Projekt `composer require shipping-docker/vessel` && `php artisan vendor:publish --provider="Vessel\VesselServiceProvider"`
- `bash vessel init`
- `./vessel start`
    + Falls ein Error ` Cannot start service app: b'driver failed programming external connectivity on endpoint` auftritt muss möglicherweise der `.env` Datei `APP_PORT=8080` hinzugefügt werden.
- Laravel unter [localhost:APP_PORT](http://localhost:8080) aufrufen

### Enable xDebug
#### in VsCode
- install [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug) & restart vs code
- Anzeigen -> Debuggen anzeigen (strg + shift + d) -> Einstellungen (Zahnrad)

```json
    // edit first config to like like this
    {
        "name": "Listen for XDebug",
        "type": "php",
        "request": "launch",
        "port": 9000,
        "pathMappings": {
            "/var/www/html": "${workspaceRoot}",
        },
        "ignore": [
            "**/vendor/**/*.php"
        ]
    },
```

#### in PhpStorm
- Configuring a Server: `Preferences > Languages & Frameworks > PHP > Servers`
    + Add "docker-server" with Post of `APP_PORT` set "Debugger" to Xdebug
    + select "User path mappings"
- Configuring a new PHP Remote Debugger: `Run > Edit Configurations click in + and PHP Remote Debugger`
    + Name "Docker"
    + Server "docker-server"
    + ide key "docker"

For image as guides go to [Example Settings](https://github.com/petronetto/php7-alpine#phpstorn)

---

- set Breakpoint z.B. in Web & Starte Debugging

### Docs
[Vessel Docs](https://vessel.shippingdocker.com/docs/)