---
title: "Ejercicio 1 Evaluación"
linkTitle: "Ejercicio 1EV"
weight: 20
---


{{% pageinfo %}}
Una propuesta de solución al ejercicio de la primera evaluación: 
https://www.adistanciafparagon.es/mod/assign/view.php?id=46025
{{% /pageinfo %}}


## Objetivo
Configuración de un servidor apache para que muestre las páginas web de 4 empresas

## Descripción

Nos han encargado poner en marcha las webs de 4 empresas. Se trata de seguir todo el proceso necesario para que las webs sean accesibles en nuestro servidor:

* Configuración de virtualbox/vagrant
* Instalación y configuración de apache

Tienes los archivos de cada web en los archivos adicionales de la actividad. 

Sigue todos los pasos necesarios para poner en funcionamiento las sitios web. Configura también el archivo hosts para simular los nombres de dominio.

* Cada web tendrá que verse con la url del nombre (zip) y con la url con www
* Por defecto no se permitirá el listado de archivos o ficheros de las carpetas del servidor. 
* En la web relojesvex.com habrá un directorio /privado que será privado para el usuario admin con password vexReloj .
* En la web relojesvex.com crea un directorio /fotos en el que se podrán colgar fotos y ver el listado de archivos que hay en esa carpeta. 
* Crea un index en la raiz de apache (localhost) que muestre los enlaces a los 4 sitios web.
* Cada sitio tendrá unos log de acceso y error específicos para poder depurar más fácil los errores. 

## Solución

### Preparación del entorno en el host

**Preparación de Vagrant/Virtualbox con Ubuntu 20.04**
```
$ vagrant init bento/ubuntu-20.04
```

**Vagrantfile**
```
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
end
```
**Copiar archivos de las webs**
Descargar y descomprimir:

```
.
├── Vagrantfile
└── webs
    ├── airspace.com
    ├── hugoflotts.com
    ├── jonathon.com
    └── relojesvex.com
```
**Editar archivo *hosts* en el equipo principal**

**/etc/hosts**
```
127.0.0.1       localhost
127.0.0.1       airspace.com  www.airspace.com
127.0.0.1       hugoflotts.com www.hugoflotts.com
127.0.0.1       jonathon.com www.jonathon.com
127.0.0.1       relojesvex.com www.relojesvex.com
```
**Comprobamos que funciona bien**
```
$ ping airspace.com
PING airspace.com (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.032 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.053 ms
...
$ ping www.airspace.com
PING airspace.com (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.049 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.043 ms
...

```

**Levantar máquina virtual y conectar por ssh**
``` bash
$ vagrant up

$ vagrant ssh
```
El directorio **/vagrant** de la máquina virutal mapea el directorio de nuestro vagrant en el host.

**Instalar apache**

```bash
$ sudo apt update
$ sudo apt install apache2
$ sudo systemctl  status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2022-03-11 15:47:43 UTC; 29s ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 13490 (apache2)
      Tasks: 55 (limit: 1071)
     Memory: 5.5M
     CGroup: /system.slice/apache2.service
             ├─13490 /usr/sbin/apache2 -k start
             ├─13491 /usr/sbin/apache2 -k start
             └─13492 /usr/sbin/apache2 -k start
```

**Preparo enlace en /var/www**
``` 
ln -s /vagrant/webs/relojesvex.com /var/www/relojesvex.com
``` 

**Configurar VirutalHosts**

```apache2
<VirtualHost *:80>
	DocumentRoot /var/www/relojesvex.com
    ServerName relojesvex.com
    ServerAlias www.relojesvex.com
</VirtualHost>
```
$ sudo systemctl reload apache2

**Comprobamos el funcionamiento**
 
Desde el host apuntamos con el navegador a las webs:

* http://localhost:8080
* http://relojesvex.com:8080
* http://www.relojesvex.com:8080
