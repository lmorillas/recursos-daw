---
title: "Instalación mediawiki"
linkTitle: "Práctica 2"
weight: 15
description: >
  
---

{{% pageinfo %}}
## Uso con Vagrant
* Vagrantfile inicial: https://github.com/lmorillas/vagrant_docker/blob/main/Vagrantfile
> Este `Vagrantfile` crea una máquina virtual con `docker` y `docker-compose` instalados.
{{% /pageinfo %}}

## Instalación de mediawiki con contenedores.

> Sigue las instrucciones de https://josedom24.github.io/curso_docker_2022/sesion2/mediawiki.html

Las instrucciones anteriores instalan mediwiki con la base de datos sqlite. 

Si quieres usar `MySQL`/`MariaDB` es mejor que uses docker-compose.

Tienes instrucciones en https://hub.docker.com/_/mediawiki Aquí tienes una versión más detallada usando también `docker-compose`: https://blog.programster.org/deploy-your-own-mediawiki-wiki o https://culturalibre.ar/2022/06/12/montar-una-wiki-de-lo-que-quieras-y-libre/

Información más detallada y específica hay en https://www.mediawiki.org/wiki/MediaWiki-Docker

## Ejemplo del docker-compose.yml de mediawiki
```yaml
# MediaWiki with MariaDB
#
# Access via "http://localhost:8080"
#   (or "http://$(docker-machine ip):8080" if using docker-machine)
version: '3'
services:
  mediawiki:
    image: mediawiki
    restart: always
    ports:
      - 8080:80
    links:
      - database
    volumes:
      - images:/var/www/html/images
      # After initial setup, download LocalSettings.php to the same directory as
      # this yaml and uncomment the following line and use compose to restart
      # the mediawiki service
      # - ./LocalSettings.php:/var/www/html/LocalSettings.php
  # This key also defines the name of the database host used during setup instead of the default "localhost"
  database:
    image: mariadb
    restart: always
    environment:
      # @see https://phabricator.wikimedia.org/source/mediawiki/browse/master/includes/DefaultSettings.php
      MYSQL_DATABASE: my_wiki
      MYSQL_USER: wikiuser
      MYSQL_PASSWORD: example
      MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
    volumes:
      - db:/var/lib/mysql

volumes:
  images:
  db:
```

## Si no quieres usar docker-compose

Tienes que montar los contenedores de `MariaDB` y `MediaWiki` en la misma red para que puedan comunicarse.

```bash
docker network create miwiki
```

```bash
docker run -d \
    --network miwiki \
    --network-alias mariadb \
    -v todo-mysql-data:/var/lib/mysql \
    -e MARIADB_USER=userwiki \
    -e MARIADB_PASSWORD=userwikipwd \
    -e MARIADB_ROOT_PASSWORD=my-secret-pw \
    -e MYSQL_DATABASE=miwiki \
    mariadb:latest
```

```bash
 docker run \
    --network miwiki \
    --name mimediawiki  \
    -p 8080:80 \
    -d mediawiki    
```
En la instalación de mediawiki tendrás que usar como host de la base de datos `mariadb` y como usuario `userwiki` y contraseña `userwikipwd` (o lo que configures en el entorno de docker)

Una vez instalado, descarga el archivo `LocalSettings.php` y guárdalo. Para el contenedor de `mediawiki` y vuélvelo a lanzar añadiendo el volumen con el archivo `LocalSettings.php`:

```bash
docker run \
    --network miwiki \
    --name mimediawiki  \
    -v ./LocalSettings.php:/var/www/html/LocalSettings.php \
    -p 8080:80 \
    -d mediawiki    
```
