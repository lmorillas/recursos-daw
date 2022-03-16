---
title: "Entorno básico con mod_wsgi"
linkTitle: "Entorno básico"
weight: 5
description: >
  
---

{{% pageinfo %}}
## Objetivo
Configurar la aplicación [flask_temperaturas](https://github.com/josedom24/flask_temperaturas) para que sea servida con apache2 + mod_wsgi. Explica los pasos más importantes y entrega una prueba de funcionamiento.

* Documentación original: https://fp.josedomingo.org/iaw2122/u03/modwsgi.html
{{% /pageinfo %}}


## Pasos instalación
Levanta una nueva máquina vagrant Ubuntu-20.04 e instala apache2, git, python3 y python3-venv

### Clonar el repositorio
En **/home/vagrant/** 
```bash
$ git clone https://github.com/josedom24/flask_temperaturas.git
```

### Prepara máquina virtual para entornos de python
En **/home/vagrant/** ejecuta:
```bash
$ python3 -m venv env   # Crea un entorno virtual
$ source env/bin/activate  # Activa el entorno virtual
$ cd flask_temperaturas
$ pip install -r requirements.txt  # Instala dependencias
```

### Instala y configura el módulo wsgi para apache2
    apt install libapache2-mod-wsgi-py3

Activa el módulo wsgi
Nuestra aplicación se encuentra en `/home/vagrant/flask_temperaturas`.
Nuestro entorno virtual está en `/home/vagrant/env`.

### Creación del fichero wsgi

Lo primero que vamos a hacer es crear el fichero WSGI, que vamos a llamar `wsgi.py` estará en `/home/vagrant/flask_temperaturas` y tendrá el siguiente contenido:

    from app import app as application

Explicación:

* El primer `app` corresponde con el nombre del módulo, es decir del fichero del programa, en nuestro caso se llama `app.py`.
* El segundo `app` corresponde a la aplicación flask creada en `app.py`:  `app = Flask(__name__)`.
* Importamos la aplicación flask, pero la llamamos `application` necesario para que el servidor web pueda enviarle peticiones.

### Configuración de apache2

Yo he utilizado el virtualhost por defecto, si usamos otro virtualhost esta configuración ira en el fichero correspondiente:
```apache2
    WSGIDaemonProcess flask_temp python-path=/home/vagrant/flask_temperaturas:/home/vagrant/env/lib/python3.9/site-packages
    WSGIProcessGroup flask_temp
    WSGIScriptAlias / /home/vagrant/flask_temperaturas/wsgi.py process-group=flask_temp
    <Directory /home/vagrant/flask_temperaturas>
            Require all granted
    </Directory>
```
Vamos a explicar la configuración:

* El `DocumentRoot`se indica el directorio donde está la aplicación. Realmente el servidor web siempre va a llamar al fichero WSGI `wsgi.py`, pero el DocumentRoot es necesario por si hay contenido estático.
* La directiva `WSGIDaemonProcess`: Se define un grupo de procesos que se van a encargar de ejecutar la aplicación (servidor de aplicaciones). A estos procesos se le ponen un nombre (`flask_temp`) y se indica los directorios donde se encuentran la aplicación y los paquetes necesarios (`python-path`), como puedes observar se pone el directorio donde esta la aplicación y el directorio donde se encuentran los paquetes en el entorno virtual, separados por dos puntos.
* `WSGIProcessGroup`: Nos permite agrupar procesos. Se pone el misimo nombre que hemos definido en la directiva anterior.
* La directiva `WSGIScriptAlias` nos permite indicar que programa se va a ejecutar (el fichero WSGI: `/home/debian/flask_temperaturas/wsgi.py`) cuando se haga una petición a la url `/` y que proceso lo va a ejecutar.

Reinicia el servicio web y prueba el funcionamiento. Si te da algún erro 500 puedes ver los errores, en `/var/log/apache2/error.log`.

