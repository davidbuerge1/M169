# Docker Befehlszusammenfassung

## Hilfreiche Commands
* **docker version** - provides full description of docker version
* **docker info** - display system wide information
* **docker -v** - provides a short description of docker version
* **docker run hello-world** - pull hello-world container from registry and run it

# Container erstellen und ausführen

## Image herunterladen
```bash
docker pull <image>
```
Die Images können auf der Ofiziellen docker Seite gefunden werden
```URL
https://hub.docker.com
```

## Container von einem Image erstellen und ausführen:
generell gilt
```bash
docker run <opiton> <Name> <Port> <Image>   
```
Bsp.
```bash
docker run -d --name bsp-apache-php-container -p 8080:80 bsp-apache-php
```
gibt an, dass der Container im Hintergrund läuft (daemon)
```bash
-d
```
gibt en Namen des Containers an, dieser kann anders lauten als das Image

```bash
--name
```
Verknüpft den Port 80 innerhalb des Containers mit dem Port 8080 des Hosts
```bash
-p 8080:80
```
Syntax
```bash
-p [host_port]:[container_port]
```
der Name des Image aus dem der Container gestartet wird
```bash
bsp-apache-php
```
dazu kann noch --force hinzugefügt werden

## root shell öffnen
```bash
docker exec -it bsp-apache-php-container /bin/bash
```
### Container restar
```bash
docker restart my_container
```
# Portweiterleitung
## Syntax
```bash
-p [host_port]:[container_port]
```

## BSP
```bash
-p 8080:80
```

# Docker Volumes
## Unbenanntes Volumen
Um herauszufinden wo nun die Daten aus /var/lib/mysql gelandet sind, kann folgeder Commmand verwendet werden
```bash
docker inspect -f '{{.Mounts}}' mariadb-test
```
Ist schwierig das Volumen nun wieder zu löschen, da ansonsten beim löschen oder bearbeiten eines Volumen der Zufüllige Namen eingegeben werden müsste.

## Benanntes Volumen
Mit folgendem Befehl kann der Name des Volumen beim erstellen eines Container angepasst werden
### Syntax
```bash
-v volumename:containerverzeichnis
```
### Bsp.
```bash
docker run -d --name mariadb-test2 -v myvolume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=geheim mariadb
```

## Volumes in eigenen Verzeichnissen
Anstelle eines Namens für das Hostvolume kann auch ein Verzeichnis angegeben werden
### Syntax
```bash
-v hostverzeichnis:containerverzeichnis
```
### Bsp.
```bash
mkdir /home/vmadmin/databases
docker run -d --name mariadb-test3 -v /home/vmadmin/databases:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=geheim mariadb
```
# Netzwerke
Folgender Befehl um alle Netzwerke anzuzeigen
```bash
docker network ls
```
## Aufbau des Standartnetzwerkes
![Alt-Text](https://gbssg.gitlab.io/m169/img/kap1/7-1.PNG)

Aufrufen der Bridge-Konfiguration
```bash
vmadmin@ubuntu:~$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "65593a9ebb3b25cb368166ff8dc0f3556cd5edf29cbc88286fc38dfa5068594c",
        "Created": "2022-06-25T07:03:44.321727785+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "5fe8760946479f29c3b59470a7a7a23e80ea8c0689f04d1fa4d40c5f668b51c8": {
                "Name": "ubuntu_1",
                "EndpointID": "194b76a3cd7a3360c054ad752a322822bf122d1544fd3266b49e55e116eda643",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

## Eigenes Netzwerk erstellen
Mit folgendem Befehl können neue Netzwerkkomponenten hinzugefügt werden. 
Alle Container landen standardmässig im selben Netzwerk, dem bridge-Netzwerk. Dies ist aus sicherheitstechnischen Gründen nciht ideal, wenn unterschiedliche Anwendungen voneinander isoliert sein sollen. Es lassen sich deshalb eigene Netzwerke definieren und diese den Containern zuordnen.
```bash
docker network create \
--driver=bridge \
--subnet=10.10.10.0/24 \
--gateway=10.10.10.1 \
my_net
```
kann danach überprüft werden mit
```bash
docker network ls
```
Ausgabe
```bash
NETWORK ID     NAME                                 DRIVER    SCOPE
65593a9ebb3b   bridge                               bridge    local
364521a9eaa2   host                                 host      local
049d1ccb15e7   my_net                               bridge    local
```

## Container einem Netzwerk zuordnen
Der Container kann nur beim ausführen einem Netzwerk zugeordnet werden.
```bash
docker run -it --name ubuntu_2 --network=my_net ubuntu:latest
```
Mithilfe des Folgenden befehls, kann die Konfiguration des Netzwerks angezeigt werden
```bash
docker network inspect my_net
```
## IP zuordnen
Normalerweise sind Ip-Adressen per DHCP definiert. Mit folgendem Befehl kann man eine eigene Adresse eingeben
```bash
docker run -it --name ubuntu_2 --ip="10.10.10.10" --network=my_net ubuntu:latest
```
## Ein Container zu einem anderen Netzwerk hinzufügen$
Zurst muss der Contaier von der Bridge disconected werden.
```bash
docker network disconnect bridge ubuntu_1
```
Neues Netzwerk definieren
```bash
docker network connect my_net ubuntu_1
docker start -i ubuntu_1
```
Danach kann wieder mit dem Befehl die Konfiguration gepfüft werden
```bash
docker network inspect my_net
```
Um danach die Maschinen untereinander zu Pingen muss zuerst auf dem Container zu pingen muss zuerst folgende tool installiert werden
```bash
apt update
apt install iputils-ping
```
Um das Netzwerk zu löschen, darf keine Maschine mehr im Netzwerk sein und danach mit folgendem Befehl gelöscht werden.
```bash
docker network rm my_net
```
