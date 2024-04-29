### Zusammenfassung der YAML-Syntax

YAML-Dateien folgen bestimmten Syntaxregeln:

- Zeilen können kommentiert werden.
- Text kann in einfachen oder doppelten Hochkommas geklammert werden, vor allem bei Sonderzeichen oder Escape-Sequenzen.
- Listen werden mit einem Minuszeichen und einem Leerzeichen erstellt.
- Key-Value-Paare werden durch ein Doppelpunkt und einen Leerzeichen getrennt.
- Objekte mit mehreren Key-Value-Paaren können durch Zeilenumbrüche gebildet werden.
- Werte können rekursiv aus Objekten, Key-Value-Paaren oder Listen bestehen.
- Die Strukturierung erfolgt durch Einrückungen mit Leerzeichen.

**Online-Validator:** [codebeautify.org/yaml-validator](https://codebeautify.org/yaml-validator)

### Docker-Compose-Schlüsselwörter

Für `docker-compose.yml` gibt es spezifische Schlüsselwörter:

- `version`: Definiert die Version von Docker-Compose.
- `services`: Definiert einzelne Container-Dienste.
- `networks`: Zur Definition eigener Netzwerke.
- `volumes`: Angabe von benannten Volumes.
- `secrets`: Angabe von Passwortdateien.

Ein typischer Aufbau von `docker-compose.yml` sieht so aus:

```yaml
version: "3.8"
services:
  dienstname1:
    schlüsselwort1: einstellung1
    schlüsselwort2: einstellung2
    ...
volumes:
networks:
secrets:
```

### Wichtige Schlüsselwörter unter Services

Unter den Dienstnamen werden folgende Schlüsselwörter verwendet:

- `image`: Basisimage des Dienstes.
- `build`: Verzeichnis mit Dockerfile, falls kein Basisimage verwendet wird.
- `restart`: Definiert die Restart-Policy.
- `environment`: Liste mit Umgebungsvariablen.
- `volumes`: Liste der gemounteten Volumes.
- `ports`: Liste der Portweiterleitungen.
- `expose`: Angabe von Ports für die interne Kommunikation.
- `networks`: Verweis auf definierte Netzwerke.
- `secrets`: Verweis auf definierte Passwortdateien.

### Weitere Details

Einige wichtige Schlüsselwörter im Detail:

- `image`: Direkte Verwendung eines verfügbaren Basisimages.
- `build`: Erstellung eines eigenen Images aus einem Dockerfile.
- `restart`: Definiert die Neustart-Policy des Containers.
- `ports`: Angabe der Portweiterleitungen für den Zugriff vom Host aus.
- `expose`: Ports für die Kommunikation zwischen Containern.
- `networks`: Verweis auf definierte Netzwerke.
- `volumes`: Angabe von gemounteten Volumes.
- `depends_on`: Definiert Abhängigkeiten zwischen Containern.
- `environment`: Definition von Umgebungsvariablen.
- `secrets`: Verwendung von Passwortdateien für Sicherheitszwecke.

### Hinweis

Für detailliertere Informationen und die vollständige Referenz zum Aufbau von `docker-compose.yml` kann die Dokumentation auf [docker.com](https://docs.docker.com/compose/compose-file) konsultiert werden.


```yaml
# docker-compose.yml

Zusätzlich zur allgemeinen Syntax von YAML-Dateien, gibt es bei docker-compose.yml definierte Schlüsselwörter die als Keys verwendet werden müssen:

```yaml
version: zur Zeit "3.8"
services: Zur Definition der einzelnen Container. Die Dienstnamen (Keys) für die Container können freigewählt werden.
networks: Zur Definition eigener Netzwerke
volumes: Angabe von benannten Volumes
secrets: Angabe von Passwortdateien
```

Grundsätzlicher Aufbau von docker-compose.yml

```yaml
version: "3.8"
services:
  dienstname1:
    schlüsselwort1: einstellung1
    schlüsselwort2: einstellung2
    ...
  dienstname2:
    schlüsselwort1: einstellung1
    schlüsselwort2: einstellung2
    ...
volumes:
networks:
secrets:
```

Die wichtigsten Schlüsselwörter unterhalb der Dienstnamen sind:

```yaml
image: Das Basisimage eines Service oder
build: Ein Verzeichnis mit Dockerfile, der Service wird dann nicht aus einem Image gestartet sondern aus einem Dockerfile
restart: always, on-failure oder unless-stopped
environment: Liste mit Umgebungsvariablen für einen Container
volumes: Liste der gemounteten Volumes
ports: Liste der Portweiterleitungen
expose: Ports für die Kommunikation zwischen Containern
networks: Verweis auf ein im Top-Level-Schlüsselwort networks definiertes Netzwerk
secrets: Verweis auf die im Top-Level-Schlüsselwort secrets definierte Passwortdatei
```

Hier nun die Schlüsselwörter im Detail:

### image

Wenn das Basisimage in Dockerhub verfügbar ist, kann es direkt verwendet werden:

```yaml
services: 
  my-service:
    image: ubuntu:latest
    ...
```

### build

Wird ein eigenes Image benötigt, kann dieses aus dem angegebenen Dockerfile erstellt werden:

```yaml
services: 
  my-custom-app:
    build: /path/to/dockerfile/
    ...
```

### restart

Dieser Eintrag definiert die Restart-Policy für einen Container. Folgende Werte sind möglich

- no: Der Standard, Container wird nicht automatisch gestartet
- on-failure: Der Container wird bei einem Fehler (Exit-Code ungleich 0) automatich neu gestartet
- always: Der Container wird immer neu gestartet, insbesondere bei einem Reboot. Wenn der Container manuell gestopped wird, startet er neu, wenn der Dockerdaemon neu gestartet wird.
- unless-stopped: Ähnlich wie always, wird der Container manuell gestoppt, startet er nach einem Neustart des Dockerdaemons oder einem Reboot nicht mehr neu.

```yaml
services: 
  my-custom-app:
    restart: always
    ...
```

### ports

Um einen Service vom Host aus zu erreichen, wird die Angabe der Portweiterleitungen benötigt:

```yaml
services:
  my-custom-app:
    image: myapp:latest
    ports:
      - "8080:3000"
      - "8081:4000"
    ...
```

### expose

Angabe der Ports, welche die Container für die Kommunikation untereinander benötigen. Wenn ein Basisimage bereits einen Port exponiert, ist diese Angabe nicht nötig.

```yaml
services:
  network-example-service:
    image: karthequian/helloworld:latest
    expose:
      - "80"
```

### networks

Hier kann ein unter dem Top-Level-Schlüsselwort definiertes Netzwerk referenziert werden.

```yaml
services:
  network-example-service:
    image: karthequian/helloworld:latest
    networks: 
      - my_network
    ...
  another-service-in-the-same-network:
    image: alpine:latest
    networks: 
      - my_network
    ...

networks:
  my_network: 
    ipam:
      config:
        - subnet: 172.17.0.0/24
          gateway: 172.17.0.1
```

In der Regel werden bei der Definition des Netzwerkes keine weiteren Optionen angegeben. Docker kümmert sich dann selber um die konkrete Definition:

```yaml
services:
  network-example-service:
    image: karthequian/helloworld:latest
    networks: 
      - my_network
    ...

networks:
  my_network: 
```

### volumes

Gibt an wie Containervolumes auf den Host gemountet werden. Benannte Volumes müssen im Top-Level-Schlüsselwort volumes angegeben werden. Somit können benannte Volumes von mehreren Containern gleichzeitig verwendet werden. Der Zusatz :ro mountet ein Verzeichnis read-only.

```yaml
services:
  volumes-example-service:
    image: alpine:latest
    volumes: 
      - my-named-global-volume:/my-volumes/named-global-volume
      - /tmp:/my-volumes/host-volume
      - /home:/my-volumes/readonly-host-volume:ro
    ...
  another-volumes-example-service:
    image: alpine:latest
    volumes:
      - my-named-global-volume:/another-path/the-same-named-global-volume
    ...

volumes:
  my-named-global-volume:
```

Unbenannte Volumes und Volumes in eigene Verzeichnisse müssen nicht im Top-Level volume aufgeführt werden.
Es können auch einzelen Dateien gemountet werden.

### depends_on

Um Abhängigkeiten zu definieren, kann depends_on verwendet werden. Die betreffenden Container werden dann zuerst geladen:

```yaml
services:
  kafka:
    image: wurstmeister/kafka:2.11-0.11.0.3
    depends_on:
      - zookeeper
    ...
  zookeeper:
    image: wurstmeister/zookeeper
    ...
```

### environment

Variablen können sowohl statisch als auch dynamisch mit ${} definiert werden

```yaml
services:
  database: 
    image: "postgres:${POSTGRES_VERSION}"
    environment:
      DB: mydb
      USER: "${USER}"
```

Die dynamischen Variablen können dabei u.a. in einer Datei .env im selben Verzeihnis als Key-Value-Paare definiert werden:

```
POSTGRES_VERSION=alpine
USER=foo
```

Die verfügbaren Umgebungsvariablen müssen in der Dokumentation zu einem Image auf Dockerhub nachgeschaut werden

### secrets

Sollen Passwörter aus sicherheitstechnischen Gründen nicht in der docker-compose Datei aufgeführt werden, kann man secrets verwenden. Das Top-Level-Schlüsselwort gibt an, wo sich die Passwortdatei befindet. Die Einträge bei den Services referenzieren dann die Top

-Level-Definition

```yaml
services:
   db:
     image: mysql:latest
     environment:
       MYSQL_ROOT_PASSWORD_FILE=/run/secrets/db_root_password
       MYSQL_PASSWORD_FILE=/run/secrets/db_password
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

Die in MYSQL_ROOT_PASSWORD_FILE und MYSQL_PASSWORD_FILE angegebenen Dateien innerhalb des Containers enthalten dann die in den unter dem Top-Level-Schlüsselwort secrets angegebenen Passwörter. Die verfügbaren Secretsvariablen müssen in der Dokumentation zu einem Image auf Dockerhub nachgeschaut werden
```