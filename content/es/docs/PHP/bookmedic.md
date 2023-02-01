---
title: "Instalación de Bookmedic"
linkTitle: "Bookmedic"
weight: 8

---

{{% pageinfo %}}

Fuente: https://fp.josedomingo.org/iaw2223/2_php/t3.html
    
{{% /pageinfo %}}

## Objetivo:

* Desplegar una aplicación PHP construida a medida en tu servidor LAMP.

## Recuerda

Como ya sabemos tenemos 3 maneras principales para ejecutar código PHP:

1. Desde la línea de comandos (**cli**).
1. Con el servidor web **Apache2** y el módulo **libapache2-mod-php**. 
1. Con un servidor web y el servidor de aplicaciones **fpm-php**. 

La configuración de php está dividida según desde donde se use (este ejemplo es para php 7.4):

* `/etc/php/7.4/cli`: Configuración de php para `php7.4-cli`, cuando se utiliza php desde la línea de comandos.
* `/etc/php/7.4/apache2`: Configuración de php para apache2 cuando utiliza el módulo.
* `/etc/php/7.4/fpm`: Configuración de php para php-fpm.
* `/etc/php/7.4/mods-available`: Módulos disponibles de php que puedes estar configurados en cualquiera de los escenarios anteriores.

Si nos fijamos en la configuración de php para apache2 con el módulo:

* `/etc/php/7.4/apache2/conf.d`: Módulos instalados en esta configuración de php (enlaces simbólicos a `/etc/php/7.4/mods-available`).
* `/etc/php/7.4/apache2/php.ini`: Configuración de php para este escenario.


## ¿Qué tienes que hacer?
A partir de una instalación básica de Apache + MariaDB + PHP, tienes que instalar la aplicación BookMedik, una aplicación web a medida para llevar el control de citas médicas, pacientes, médicos, historiales de citas, áreas médicas y mucho mas, pensado para centros médicos, clínicas y médicos independientes. Puedes encontrar la aplicación en [https://github.com/evilnapsis/bookmedik](https://github.com/evilnapsis/bookmedik).

Para realizar la instalación sigue los siguientes pasos:

1. Crea la base de datos y las tablas necesarias recuperando la copia de seguridad de la base de datos que encuentras en el fichero `schema.sql`. Se creará una base de datos llamada `bookmedik`. Crea un usuario que tenga privilegios sobre dicha base de datos.
2. Crea un virtualhost con el que accederás con el nombre `bookmedik.tunombre.org`. Copia en el *DocumentRoot* los ficheros de la aplicación (podrías clonar el repositorio en el *DocumentRoot*).
3. Vamos a configurar el acceso a la base de datos desde la aplicación, para ello cambia el fichero `core\controller\Database.php` indicando el usuario de acceso (el que has creado en el punto 1), su contraseña, la base de datos que se llama `bookmedik` y la dirección donde se encuentra la base de datos, que en este caso es `localhost`.
4. Accede a la aplicación web usando la URL configurada en el virtualhost (usa el usuario `admin` y contraseña `admin`).
5. Para esta aplicación no es necesario, pero en determinadas aplicaciones es posible que necesitemos cambiar la memoria RAM que puede utilizar. Cambia la memoria máxima de uso de un script PHP (parámetro `memory_limit`) a 256Mb. ¿En qué fichero lo tienes que cambiar?. Investiga.

Una vez que esté la aplicación funcionando, configura un archivo `bootstrap.sh` que instale la aplicación automáticamente. 



    