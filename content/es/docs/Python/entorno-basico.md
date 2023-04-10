---
title: "Entorno básico con apache2 + mod_wsgi"
linkTitle: "Entorno básico"
weight: 5
description: >
  
---

{{% pageinfo %}}
## Objetivo
* ¿Qué es WSGI?
* Configurar la aplicación [flask_temperaturas](https://github.com/lmorillas/flask_temperaturas) para que sea servida con apache2 + mod_wsgi. Explica los pasos más importantes y entrega una prueba de funcionamiento.

* Documentación original: https://fp.josedomingo.org/iaw2122/u03/modwsgi.html
{{% /pageinfo %}}

## Documentación
**¿Qué es WSGI?**
* https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface
* https://entrenamiento-frameworks-web-python.readthedocs.io/es/latest/leccion4/introduccion_wsgi.html

## Pasos instalación
Levanta una máquina vagrant Ubuntu (la Ubuntu-22.04 que estás usando por ejemplo) e instala `apache2, git, python3 y python3-venv` si no está instalado.

### Clonar el repositorio
En **/var/www/** 
```bash
$ sudo git clone https://github.com/lmorillas/flask_temperaturas.git
```

### Prepara máquina virtual para entornos de python
En **/var/www/flask_temperaturas** ejecuta:
```bash
$ python3 -m venv env   # Crea un entorno virtual
$ source env/bin/activate  # Activa el entorno virtual
$ cd flask_temperaturas
$ pip install -r requirements.txt  # Instala dependencias
```

### Instala y configura el módulo wsgi para apache2
```bash
$ sudo  apt install libapache2-mod-wsgi-py3
```

## Activar el módulo wsgi
Nuestra aplicación se encuentra en `/var/www/flask_temperaturas`.
Nuestro entorno virtual está en `/var/www/flask_temperaturas/env`.

### Creación del fichero wsgi

Lo primero que vamos a hacer es crear el fichero WSGI, que vamos a llamar `wsgi.py` estará en `/var/www/flask_temperaturas` y tendrá el siguiente contenido:
```python
    from app import app as application
```
Explicación:

* El primer `app` corresponde con el nombre del módulo, es decir del fichero del programa, en nuestro caso se llama `app.py`.
* El segundo `app` corresponde a la aplicación flask creada en `app.py`:  `app = Flask(__name__)`.
* Importamos la aplicación flask, pero la llamamos `application` necesario para que el servidor web pueda enviarle peticiones.

### Configuración de apache2

Yo he utilizado el virtualhost por defecto, si usamos otro virtualhost esta configuración ira en el fichero correspondiente:
```apache2
    WSGIDaemonProcess flask_temp python-path=/var/www/flask_temperaturas:/var/www/flask_temperaturas/env/lib/python3.10/site-packages
    WSGIProcessGroup flask_temp
    WSGIScriptAlias / /var/www/flask_temperaturas/wsgi.py process-group=flask_temp
    <Directory /var/www/flask_temperaturas>
            Require all granted
    </Directory>
```
> Fíjate en la ruta `/var/www/flask_temperaturas/env/lib/python3.10/site-packages` Si usas otra versión de python, tendrás que cambiarla.

Vamos a explicar la configuración:

* El `DocumentRoot`se indica el directorio donde está la aplicación. Realmente el servidor web siempre va a llamar al fichero WSGI `wsgi.py`, pero el DocumentRoot es necesario por si hay contenido estático.
* La directiva `WSGIDaemonProcess`: Se define un grupo de procesos que se van a encargar de ejecutar la aplicación (servidor de aplicaciones). A estos procesos se le ponen un nombre (`flask_temp`) y se indica los directorios donde se encuentran la aplicación y los paquetes necesarios (`python-path`), como puedes observar se pone el directorio donde esta la aplicación y el directorio donde se encuentran los paquetes en el entorno virtual, separados por dos puntos.
* `WSGIProcessGroup`: Nos permite agrupar procesos. Se pone el misimo nombre que hemos definido en la directiva anterior.
* La directiva `WSGIScriptAlias` nos permite indicar que programa se va a ejecutar (el fichero WSGI: `/var/www/flask_temperaturas/wsgi.py`) cuando se haga una petición a la url `/` y que proceso lo va a ejecutar.

La carpeta `/var/www/flask_temperaturas` debe tener los siguientes permisos:
```bash
$ sudo chown -R www-data:www-data /var/www/flask_temperaturas
```

Reinicia el servicio web y prueba el funcionamiento. Si te da algún erro 500 puedes ver los errores, en `/var/log/apache2/error.log`.