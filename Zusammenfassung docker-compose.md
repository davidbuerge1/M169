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