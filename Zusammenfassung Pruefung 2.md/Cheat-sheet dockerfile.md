# Cheat-Sheet Dockerfiles

## Dockerfile

| Anweisung   | Beschreibung                                                  |
|-------------|---------------------------------------------------------------|
| ADD         | Kopiert Dateien in das Dateisystem des Images.               |
| CMD         | Führt das angegebene Kommando beim Container-Start aus.      |
| COPY        | Kopiert Dateien aus dem Projektverzeichnis in das Image.      |
| ENTRYPOINT  | Führt das angegebene Kommando immer beim Container-Start aus.|
| ENV         | Setzt eine Umgebungsvariable.                                 |
| EXPOSE      | Gibt die aktiven Ports des Containers an.                    |
| FROM        | Gibt das Basis-Image an.                                     |
| LABEL       | Legt eine Zeichenkette fest.                                  |
| RUN         | Führt das angegebene Kommando aus.                            |
| USER        | Gibt den Account für RUN, CMD und ENTRYPOINT an.              |
| VOLUME      | Gibt Volume-Verzeichnisse an.                                 |
| WORKDIR     | Legt das Arbeitsverzeichnis für RUN, CMD, COPY etc. fest.     |


## Console

Das Image kann mit folgendem Befehl erstellt werden.
```Bash
docker build -t meine-webseite-image .
```
Docker images können mit folgendem Befehl aufgelistet werden.
```Bash
docker images
```
Folgender Befehl um das Image zu löschen.
```Bash
docker rmi meine-webseite-image -f
```

## Bsp Dockerfile:

```Dockerfile
# Set base image
FROM ubuntu:latest

# Set environment variables
ENV APP_HOME=/app
ENV PORT=8080

# Set working directory
WORKDIR $APP_HOME

# Copy files from the project directory to the image
COPY . $APP_HOME

# Install dependencies
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip

# Set a label
LABEL maintainer="Your Name <your.email@example.com>"

# Expose port
EXPOSE $PORT

# Set user
USER nobody

# Define entrypoint
ENTRYPOINT ["python3", "app.py"]

# Define command
CMD ["--debug"]
```

In diesem Beispiel:

- `FROM`: Gibt das Basis-Image (Ubuntu) an.
- `ENV`: Setzt Umgebungsvariablen für das Arbeitsverzeichnis und den Port.
- `WORKDIR`: Legt das Arbeitsverzeichnis fest.
- `COPY`: Kopiert Dateien aus dem Projektverzeichnis in das Image.
- `RUN`: Führt das Installieren von Python und Pip aus.
- `LABEL`: Legt eine Beschreibung für das Image fest.
- `EXPOSE`: Gibt den aktiven Port an.
- `USER`: Legt den Benutzer fest, der die folgenden Befehle ausführt.
- `ENTRYPOINT`: Führt das angegebene Kommando (hier Python-App) beim Container-Start aus.
- `CMD`: Gibt zusätzliche Argumente für das Entry Point-Kommando an.

