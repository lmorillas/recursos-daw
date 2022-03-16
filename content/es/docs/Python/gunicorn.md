---
title: "Despliegue con Gunicorn"
linkTitle: "gunicorn"
weight: 10
description: >
  
---

{{% pageinfo %}}
## Objetivo
* Despliegue de una web con **gunicorn** y un proxy inverso
* Qué es un [proxy inverso](https://es.wikipedia.org/wiki/Proxy_inverso)? 
* Configurar la aplicación [flask_temperaturas](https://github.com/josedom24/flask_temperaturas) para que sea servida con apache2 + gunicorn
  
* Documentación original: https://fp.josedomingo.org/iaw2122/u03/gunicorn.html
{{% /pageinfo %}}

## Documentación
Gunicorn es un servidor web que permite ejecutar una aplicación en un proceso independiente. Usamos los servidores web como proxies inversos que envían la petición python al servidor WSGI que estemos utilizando

## Configuración Vagrant
Configura el forward del puerto 8080 al puerto 8080 de la máquina virtual

## Instalar Gunicorn
En **/home/vagrant/** 

Activa en entorno virtual
```bash
source env/bin/activate
pip install gunicorn

(env) /home/vagrant/flask_temperaturas$ gunicorn -w 2 -b :8080 wsgi
```
Comprueba con tu navegador que puedes acceder al puerto 8080 y que responde ahora gunicorn

## Sigue la [documentación base](https://fp.josedomingo.org/iaw2122/u03/gunicorn.htm)
*Atento porque puede cambiar alguna dirección*
* Crea un servicio en systemd
* Activa e inicia el servicio
* Configura Apache2 compo un proxy inverso. 

Comprueba con tu navegador que puedes acceder al puerto 8000 y que resonde apache. Elimina el forward del puerto 8080 y reinicia vagrant. Comprueba que la aplicación sigue funcionando.


