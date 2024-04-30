# Cheat-Sheet Docker Compose

## Commands - Schlüsselwörter
| Kategorie  | Beschreibung                                                  |
|------------|---------------------------------------------------------------|
| Version    | Zur Zeit "3.8"                                                |
| Services   | Zur Definition der einzelnen Container. Die Dienstnamen (Keys) für die Container können freigewählt werden. |
| Networks   | Zur Definition eigener Netzwerke                               |
| Volumes    | Angabe von benannten Volumes                                   |
| Secrets    | Angabe von Passwortdateien                                     |

## Commands nach Schlüsselwörtern
| Kategorie    | Beschreibung                                                                      |
|--------------|-----------------------------------------------------------------------------------|
| Image        | Das Basisimage eines Service                                                      |
| Build        | Ein Verzeichnis mit Dockerfile, der Service wird dann nicht aus einem Image gestartet sondern aus einem Dockerfile |
| Restart      | always, on-failure oder unless-stopped                                             |
| Environment  | Liste mit Umgebungsvariablen für einen Container                                   |
| Volumes      | Liste der gemounteten Volumes                                                      |
| Ports        | Liste der Portweiterleitungen                                                      |
| Expose       | Ports für die Kommunikation zwischen Containern                                    |
| Networks     | Verweis auf ein im Top-Level-Schlüsselwort networks definiertes Netzwerk            |
| Secrets      | Verweis auf die im Top-Level-Schlüsselwort secrets definierte Passwortdatei        |

## Aufbau eines YAML
```YAML
version: '3.8'  # Die Version von Docker Compose, die du verwendest

services:  # Definiert die verschiedenen Container für deine Anwendung
  service1:  # Ein Beispielname für einen Service/Container
    depends_on: # Abhängigkeit definieren
    restart: always # Erlaubt restart zu jeder zeit
    image: image1:tag  # Die Docker-Image-Informationen für diesen Service
    ports:  # Die Ports, die diesem Service zugeordnet sind
      - "host_port:container_port"
    environment:  # Umgebungsvariablen für den Service
      - KEY=VALUE
    volumes:  # Volumes, die für diesen Service konfiguriert sind
      - host_path:container_path
    networks:  # Netzwerke, denen dieser Service angehört
      - network1
      - network2

  service2:  # Ein weiterer Service/Container
    # Konfiguration für service2 ...

networks:  # Definition der Netzwerke für die Anwendung
  network1:  # Ein Beispielname für ein Netzwerk
    # Konfiguration für network1 ...
  network2:  # Ein weiterer Beispielname für ein Netzwerk
    # Konfiguration für network2 ...

volumes:  # Definition von Volumes für die Anwendung
  volume1:  # Ein Beispielname für ein Volume
    # Konfiguration für volume1 ...
  volume2:  # Ein weiterer Beispielname für ein Volume
    # Konfiguration für volume2 ...

```

# Secrets in Docker-Compose

Wenn aus Sicherheitsgründen Passwörter nicht in der `docker-compose`-Datei angegeben werden sollen, können Secrets verwendet werden. Das Top-Level-Schlüsselwort gibt an, wo sich die Passwortdatei befindet. Die Einträge bei den Services referenzieren dann die Top-Level-Definition.

```yaml
services:
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
    secrets:
      - db_root_password
      - db_password
    ...
secrets:
  db_password:
    file: db_password.txt
  db_root_password:
    file: db_root_password.txt
```
Die in MYSQL_ROOT_PASSWORD_FILE und MYSQL_PASSWORD_FILE angegebenen Dateien innerhalb des Containers enthalten dann die in den unter dem Top-Level-Schlüsselwort secrets angegebenen Passwörter. Die verfügbaren Secretsvariablen müssen in der Dokumentation zu einem Image auf Dockerhub nachgeschlagen werden.