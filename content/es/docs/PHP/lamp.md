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
vagrant init bento/ubuntu-20.04
```
Edita el Vagrantile para hacer el forward del puerto 8080 al puerto 80

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
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

### Apache y PHP

Instalación de apache2 y del módulo que permite que apache2 interprete PHP (apache2 hará el papel de servidor web y de servidor de aplicaciones).

```bash
sudo apt install apache2 libapache2-mod-php php php-mysql
```

### Configuración de PHP

Archivos de configuración de PHP (según versión. En este caso 7.4):

* `/etc/php/7.4/cli`: Configuración de php para `php7.4-cli` (ejecución de php desde la línea de comandos)
* `/etc/php/7.4/apache2`: Configuración de php para apache2 usado como módulo
* `/etc/php/7.4/apache2/php.ini`: Configuración de php
* `/etc/php/7.4/fpm`: Configuración de php para php-fpm
* `/etc/php/7.4/mods-available`: Módulos disponibles de php


### Comprobación

Creamos un fichero `info.php` en el `documentRoot` (`/var/www/html` ?) con el siguiente contenido:

```php
<?php phpinfo(); ?>
```
Accedemos con el navegador. Si no aparece la información de php y vemos el contenido que hemos escrito, es que no se ha instalado correctamente. Tendrías que ver algo similar a esto:

![phpinfo](/Resources/phpinfo.png)