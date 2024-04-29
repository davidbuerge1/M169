# Zusammenfassung

## Einleitung
Dies ist eine Zusammenfassung zur 2. Prüfung in M169



### Sie kennen die Syntax von Dockerfiles.
| Schlüsselwort | Bedeutung                                                    |
|---------------|--------------------------------------------------------------|
| ADD           | Kopiert Dateien in das Dateisystem des Images.               |
| CMD           | Führt das angegebene Kommando beim Container-Start aus.      |
| COPY          | Kopiert Dateien aus dem Projektverzeichnis in das Image.      |
| ENTRYPOINT    | Führt das angegebene Kommando immer beim Container-Start aus.|
| ENV           | Setzt eine Umgebungsvariable.                                 |
| EXPOSE        | Gibt die aktiven Ports des Containers an.                    |
| FROM          | Gibt das Base-Image an.                                      |
| LABEL         | Legt eine Zeichenkette fest.                                 |
| RUN           | Führt das angegebene Kommando aus.                           |
| USER          | Gibt den Account für RUN, CMD und ENTRYPOINT an.             |
| VOLUME        | Gibt Volume-Verzeichnisse an.                                 |
| WORKDIR       | Legt das Arbeitsverzeichnis für RUN, CMD, COPY etc. fest.    |


### Sie können das Beispiel von Dockerfiles nachvollziehen.

```Dockerfile
# Verwende das Basisimage Ubuntu 20.04
FROM ubuntu:20.04

# Setze den Wartungsverantwortlichen für das Image
LABEL maintainer "name@somehost.com"

# Führe Befehle aus, um das Image zu konfigurieren:
# - Aktualisiere die Paketlisten und installiere 'joe' (Texteditor)
# - Bereinige nach der Installation die Paketlisten und entferne überflüssige Dateien
RUN apt-get update && \
    apt-get install -y joe && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Setze den Standardbefehl, der beim Starten des Containers ausgeführt wird, auf /bin/bash
CMD ["/bin/bash"]
```

### Sie können eigene Dockerfiles erstellen, testen und dokumentieren.
Bsp:
```Bsp-Dockerfile
# Verwenden eines Basis-Images
FROM ubuntu:latest

# Installieren von Paketen
RUN apt-get update && apt-get install -y \
    package1 \
    package2

# Ausführen von Befehlen
CMD ["executable", "param1", "param2"]
```

### Sie kennen den Einsatzzweck von Docker compose.
- Docker Compose ist ein Tool, mit dem Entwickler mehrere Docker-Container gleichzeitig verwalten können.
- Es verwendet eine YAML-Datei, um die Konfiguration der Container, Netzwerke und Volumes zu definieren.
- Docker Compose ist besonders nützlich für die Entwicklungsumgebungen, in denen eine Anwendung aus mehreren Diensten besteht, die miteinander kommunizieren müssen.

### Sie verstehen den Aufbau einer YAML-Datei und können eigene YAML-Dateien erstellen.
- YAML steht für "YAML Ain't Markup Language" und ist ein menschenlesbares Datenformat.
- YAML-Dateien werden häufig für die Konfiguration und Datenstrukturierung verwendet.
- YAML verwendet Einrückungen, um die Struktur der Daten zu definieren.


### Sie können Docker compose anwenden.

#### Syntax

| Regel                                                                 | Beschreibung                                                                                       |
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| Kommentare                                                            | Kommentare werden mit einem Hash-Symbol (#) eingeleitet.                                           |
| Texteinklammern                                                      | Text kann in einfachen (') oder doppelten (") Anführungszeichen eingeschlossen werden.            |
| Listen                                                                | Listen werden mit einem Bindestrich (-) und einem Leerzeichen als Trenner für die Elemente definiert. |
| Key-Value-Paare                                                      | Key-Value-Paare werden durch ein Doppelpunkt-Zeichen (:), gefolgt von einem Leerzeichen, gebildet. |
| Strukturierung von Objekten                                          | Objekte, die aus mehreren Key-Value-Paaren bestehen, können durch Zeilenumbrüche strukturiert werden. |
| Rekursive Werte                                                      | Ein Wert kann rekursiv aus Objekten, Key-Value-Paaren oder Listen bestehen.                        |
| Einrückungen                                                         | Die Strukturierung erfolgt durch Einrückungen mit Leerzeichen. Tabulatoren am Zeilenende führen zu Fehlern. |



Ein einfaches Beispiel für eine Docker-Compose-Datei:

```yaml
version: '3'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: example_password
```

### Sie verstehen wie in Docker compose mit Passwörtern oder sonstigen geheimen Informationen umgegangen wird.

#### Umgang mit sensiblen Informationen:
- Sensible Informationen wie Passwörter sollten nicht direkt in der YAML-Datei gespeichert werden, insbesondere wenn sie in einem Versionskontrollsystem wie Git verwaltet werden.
- Docker Compose bietet Mechanismen wie Umgebungsvariablen oder externen Dateien, um sensible Informationen sicher zu verwalten.
- Ein gängiges Verfahren besteht darin, Umgebungsvariablen zu verwenden und sie zur Laufzeit einzusetzen, anstatt sie direkt in die YAML-Datei einzubetten.

Hier ist ein Beispiel, wie du Umgebungsvariablen in Docker Compose verwenden könntest:

```yaml
version: '3'

services:
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
```

Dann kannst du die Umgebungsvariable `DB_PASSWORD` in deiner Umgebung setzen, ohne sie direkt in die YAML-Datei einzufügen.

### Wichtigste Befehle
Natürlich, hier ist der Markdown-Text:

---

Um deine `docker-compose.yml` Datei zu verwenden, navigiere zunächst in das entsprechende Verzeichnis:

```bash
cd myapp
```

Dann starte die gesamte Anwendung im Hintergrund mit:

```bash
docker-compose up -d
```

Das `-d` Flag bewirkt, dass die Anwendung im Hintergrund läuft. Um die Anwendung zu beenden, führe einfach aus:

```bash
docker-compose down
```

Normalerweise läuft alles reibungslos. Sollte jedoch ein Fehler auftreten, ist es ratsam, `docker-compose up` ohne das `-d` Flag auszuführen. Auf diese Weise werden die Protokollausgaben aller Container direkt in der Konsole angezeigt.

--- 

Lass mich wissen, ob das so passt oder ob du noch Änderungen möchtest!