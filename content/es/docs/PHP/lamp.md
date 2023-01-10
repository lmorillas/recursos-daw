---
title: "Instalación base"
linkTitle: "LAMP"
weight: 5
description: >
  Introducción a entornos lamp
---

{{% pageinfo %}}
* Instalación entoro LAMP
* https://es.wikipedia.org/wiki/LAMP
{{% /pageinfo %}}


El acrónimo **LAMP** se refiere a un entorno configurado en un servidor que nos posibilita servir aplicaciones web escritas en PHP.

Para ello usamos las siguientes tecnologías:

* `Linux`, sistema operativo;
* `Apache` o `nginx`, servidor web;
* `MySQL`, `MariaDB`, gestores de bases de datos;
* `PHP`, lenguaje de programación.


## Instalación base

### Partimos de una configuración base

```bash
vagrant init bento/ubuntu-22.04
```
Edita el Vagrantile para hacer el forward del puerto 8080 al puerto 80 por ejemplo

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.network "forwarded_port", guest: 80, host: 8080
end
```

Levanta y actualiza el sistema

```bash 
vagrant up
vagrant ssh
sudo apt update
```

### mariadb
Instalación del servidor de base de datos

```bash
sudo apt install mariadb-server
```

Por defecto el usuario root no tiene contraseña, para acceder necesitaremos hacer con el root del sistema. Es muy recomendable ejecutar el programa `mariadb-secure-installation` para configurar la base de datos de manera segura, por ejemplo para indicar una contraseña al root.

Si necesitas crear una base de datos y un usuario que tenga acceso a la misma:

```bash
 mysql -u root -p
 CREATE DATABASE newdb;
 CREATE USER 'username'@'localhost' IDENTIFIED BY 'userpassword';
 GRANT ALL PRIVILEGES ON newdb.* to 'username'@'localhost';
 FLUSH PRIVILEGES;
 quit
```

### Apache y PHP

Instalación de apache2 y del módulo que permite que apache2 interprete PHP (apache2 hará el papel de servidor web y de servidor de aplicaciones).

```bash
sudo apt install apache2 libapache2-mod-php php php-mysql
```

### Configuración de PHP

Archivos de configuración de PHP (según versión. En este caso 7.4, que serguramente es más antigua):

* `/etc/php/7.4/cli`: Configuración de php para `php7.4-cli` (ejecución de php desde la línea de comandos)
* `/etc/php/7.4/apache2`: Configuración de php para apache2 usado como módulo
* `/etc/php/7.4/apache2/php.ini`: Configuración de php
* `/etc/php/7.4/fpm`: Configuración de php para php-fpm
* `/etc/php/7.4/mods-available`: Módulos disponibles de php


### Comprobación

Creamos un fichero `info.php` en el `documentRoot` (`/var/www/html` ) con el siguiente contenido:

```php
<?php phpinfo(); ?>
```
Accedemos con el navegador. Si no aparece la información de php y vemos el contenido que hemos escrito, es que no se ha instalado correctamente. Tendrías que ver algo similar a esto:

Ahora podemos acceder a la URL: `http://localhost:8080/info.php`

Si tenemos problemas de acceso podemos ver los logs del servidor. El fichero de logs de acceso por “defecto”: `/var/log/apache/access.log` o el de errores: `/var/log/apache/error.log`

También podemos ver los logs del servicio ejecutando: `sudo journalctl -u apache2`

## Instalación desatendida
Si ha funcionado esta instalación manual y atendida, destruye el entorno y haz que se genere toda la instalación de forma automática cuando ejecutas `vagrant up`

> Tienes que usar un `bootstrap.sh` que ejecute tus comandos. Busca ejemplos en internet  o usa ChatGPT si tienes dudas.
> Se trata de que no tengas que perder tiempo ejecutando comandos cuando hagas una nueva instalación.

## README.md

Elimina el contenido del fichero README.md del repositorio y crea uno nuevo con la documentación (recuerda que tiene que estar escrito en markdown) de todo el proceso de instalación. Incluye también las instrucciones para que alguien pueda usar el repositorio y usar tu instalación de php.
