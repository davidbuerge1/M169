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