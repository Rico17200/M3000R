# LB3

## Inhaltsverzeichnis
- [LB3](#lb3)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Docker](#docker)
  - [Befehle](#befehle)
  - [Netzwerkplan](#netzwerkplan)
    - [Dockerfile](#dockerfile)
      - [Container starten](#container-starten)
    - [Service Überwachung](#service-überwachung)
    - [Container Sicherheit](#container-sicherheit)
      - [Read-Only](#read-only)
  - [Persönliche Lernentwicklung](#persönliche-lernentwicklung)
    - [Vergleich Vorwissen - Wissenszuwachs](#vergleich-vorwissen---wissenszuwachs)
      - [Vorwissen im Bezug zum Modul und LB3](#vorwissen-im-bezug-zum-modul-und-lb3)
      - [Neues Wissen](#neues-wissen)
    - [Reflexion](#reflexion)
      - [Tag 5](#tag-5)
      - [Tag 6](#tag-6)


## Docker
Docker ist eine Freie Software zur Isolierung von Anwendungen mit Hilfe von Containervirtualisierung. Docker vereinfacht die Bereitstellung von Anwendungen, weil sich Container, die alle nötigen Pakete enthalten, leicht als Dateien transportieren und installieren lassen.
<br>

## Befehle
| Befehl            | Funktion                                             |
| -------------     | ---------------------------------------------------- | 
| ```docker pull```      | Holt ein Image. |
| ```docker run```      | Started VM mit dem ausgewähltem Image. |
| ```docker ps```      | Zeigt laufende Maschinen. |
| ```docker version```      | Zeigt die Docker Version von Echo-Client und Server an. |
| ```docker images```        | Listet alle Docker Images auf. |
| ```docker exec```       | Führt einen Befehl in einem laufenden Container aus. |
| ```docker search```    | Durchsucht das Docker Hub nach Images. |
| ```docker attach```      | Hängt etwas an einen laufenden Container an. |
| ```docker commit```   | Erstellt ein neues Image mit den Änderungen, die an einem Container vorgenommen worden sind. |
| ```docker stop```   | Haltet die gewünschte Maschine an. |

<br>

## Netzwerkplan

### Dockerfile

```
#
#	  Einfache Apache Umgebung
#
FROM ubuntu:14.04
RUN apt-get update
RUN apt-get -q -y install apache2 
# Konfiguration Apache
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
RUN mkdir -p /var/lock/apache2 /var/run/apache2
EXPOSE 80
VOLUME /var/www/html
CMD /bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"
```

<br>

#### Container starten
Zuerst zum Verzeichnis wechseln:
```
cd M3000R\LB3
```

Zuerst wird ein Image erstellt:
```
docker build .
```

Danach das Image umbennen, damit es einfacher zu erkennen ist:
``` 
docker tag <ID> apache2
```

Nun kann man die VM starten (hier wird der Port 8080 weitergeleitet):
```
docker run --rm -d -p 8080:80 -v web:/var/www/html --name apache2 <ID>
```

So könnte man im nachhinein auf die Shell zugreifen:
```
winpty docker exec -it apache2 bash
```

So könnte man auch die Webseite abändern:
```
docker cp index.html apache2:/var/www/html/
```

<br>

### Service Überwachung
Dieser Service ist sehr schwer einzurichten:
```
docker run -d --name cadvisor -v :/rootfs:ro -v var-run:/var/run:rw -v sys:/sys:ro -v var-lib-docker-:/var/lib/docker:ro -p 8090:8080 google/cadvisor:latest
```
Somit kann man nun im Browser mit "localhost:8080" auf den Service zugreifen.

<br>

### Container Sicherheit
Hiermit kann der normale User keine sudo Befehle ausführen:
Das Dockerfile muss folgendes beinhalten:
```
RUN useradd -ms /bin/bash Username
USER Username
WORKDIR /homedir
```

<br>

#### Read-Only
Wenn man den Docker mit der Option read-only startet, können keine Änderungen am Dateisystem vorgenommen werden (auch mit sudo nicht):
```
docker run --read-only -d -t --name Apache2 Image
```
<br>



## Persönliche Lernentwicklung

### Vergleich Vorwissen - Wissenszuwachs

#### Vorwissen im Bezug zum Modul und LB3
- Schon genau 1 Mal Docker verwendet, aber schon über 1 Jahr her und dadurch keine Kenntnisse mehr

#### Neues Wissen
- Aufbau von Docker
- Docker Befehle
- Das man nie Daten in Container speichern soll
- Begriff DevOps 

### Reflexion
#### Tag 5
Zuerst wollte ich Docker wieder einmal öffnen, aber da bekam ich nur Fehlermeldungen. Also dachte ich, ich installiere Docker neu, da es sowieso viele neue Versionen gibt. Bei der Installation gab es aber wieder Fehler, aber es lief durch. Das öffnen von Docker Desktop ging aber nicht. Also löschte ich Docker Desktop ganz und installierte danach Docker erneut. Danach konnte ich Docker Desktop endlich öffnen. Dies verbrauchte schon viel Zeit und Nerven.
Zudem lernte ich die Basic Docker Befehle wieder kennen und wie Docker genau funktioniert.
Ich lernte auch den genauen unterschied von Virtuellen Maschinen und Docker kennen.

#### Tag 6
DOCKER_BUILDKIT=0  docker build .
docker tag 06d77f556a7e apache2
docker run --rm -d -p 8080:80 -v web:/var/www/html --name apache2 06d77f556a7e
winpty docker exec -it apache2 bash

docker run -d --name cadvisor -v :/rootfs:ro -v var-run:/var/run:rw -v sys:/sys:ro -v var-lib-docker-:/var/lib/docker:ro -p 8090:8080 google/cadvisor:latest

run -d --name cadvisor -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -p 8080:8080 google/cadvisor:latest