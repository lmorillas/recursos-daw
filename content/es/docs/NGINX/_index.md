---
title: "NGINX"
linkTitle: "NGINX"
weight: 15
description: >
  NGINX
docs: > 
    * https://logongas.es/doku.php?id=clase:daw:daw:2eval:tema08
    * https://www.alvarovf.com/categories.html

    https://awsacademy.instructure.com/courses/57608/modules
    guia https://awsacademy.instructure.com/courses/57608/modules/items/5062392
    lab https://awsacademy.instructure.com/courses/57608/modules/items/5062395
    start lab
    AWS (verde)

    EC2
    Crear instancia
    nginx
    par clasves
    ver instancias
    clic en id de instancia
    conectar -> conectar

    public IP
    sudo dnf update
    sudo dnf install nginx -y
    cat /etc/nginx/nginx.conf
    sudo systemctl enable nginx
    sudo systemctl start nginx
    sudo systemctl status nginx


    seguridad > grupos de seguridad > editar reglas de entrada
    agregar regla > 
    tcp > HTTP > anywhere 

    http://<ip pública de mi ec2>

    cambio datos de mi index.html
---

{{% pageinfo %}}
## Documentación
* https://www.nginx.com/resources/wiki/start/
* https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04
* https://linuxhint.com/install-nginx-ubuntu-22-04/
* https://www.alvarovf.com/servicios/2020/11/07/servidor-web-nginx.html 
{{% /pageinfo %}}



## Práctica Inicial: Instalación de NGINX en una instancia EC2

* Acceder a Lab y a la consola
* EC2
* Crear instancia
* >  par claves
* ver instancias
* clic en id de instancia
* conectar -> conectar
* > public IP
```bash
sudo dnf update
sudo dnf install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```
* Comprobamos si se puede acceder a esa ip desde el navegador
* No tiene abierto el puerto http (80)
* Seguridad > grupos de seguridad > editar reglas de entrada
* agregar regla > tcp > HTTP > anywhere 

* Vuelve a comprobar si puedes acceder a esa ip desde el navegador

* test: cambio datos de mi index.html
> Dónde está el `index.html`? 
> `$ cat /etc/nginx/nginx.conf`


## Práctica 1: Subir la web que hemos hecho con Hugo

* Genera la web con Hugo
```bash
$ hugo --minify
```
* Conecta con tu learner lab y descárgate la clave del lab `labsuser.pem`. En las instrucciones del lab tienes la explicación detallada para cada sistema operativo.
* Comprime la carpeta `public` de tu web
* Sube el fichero comprimido a tu servidor EC2
```bash
$ scp -i labuser.pem <fichero_comprimido> ec2-user@<ip_de_tu_ec2>:/home/ec2-user
```
* Descomprímelo en la carpeta `/usr/share/nginx/html`
```bash
$ sudo unzip <fichero_comprimido> -d /usr/share/nginx/html
```
* Comprueba si puedes acceder a tu web desde el navegador

## Práctica 2: Crear un virtual host
> Objetivo: queremos servir otra web distinta en la misma instancia EC2

{{<youtube _LQv96MdtCk>}}

> En nginx se llaman `server blocks`

### Pasos:
#### 1. Estructura de Directorios
Por convención, se suele colocar el contenido de cada sitio web en su propio directorio dentro de `/var/www/` o `/usr/share/nginx`. Por ejemplo:
```bash
/usr/share/nginx/site1.com
/usr/share/nginx/site2.com
```
o

```bash
/var/www/site1.com/public_html
/var/www/site2.com/public_html
```

#### 2. Crear los directorios y añadir contenido de prueba:

```bash
sudo mkdir -p /usr/share/nginx/site1.com
sudo mkdir -p /usr/share/nginx/site2.com
```

```bash

echo "Bienvenido a site1.com" | sudo tee /var/www/site1.com/public_html/index.html
echo "Bienvenido a site2.com" | sudo tee /var/www/site2.com/public_html/index.html
```

#### 3. Configurar los Server Blocks

Por convención, cada server block (o virtual host) se define en su propio archivo dentro de `/etc/nginx/sites-available/` y luego se crea un enlace simbólico hacia `/etc/nginx/sites-enabled/`.

Crearemos dos archivos de configuración:

```bash
sudo nano /etc/nginx/sites-available/site1.com
```

Y añade lo siguiente (ajustando donde sea necesario):

```nginx
server {
    listen 80;
    server_name site1.com www.site1.com;

    root /usr/share/nginx/site1.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Guarda y cierra el archivo, luego crea el segundo server block:

```bash
sudo nano /etc/nginx/sites-available/site2.com
```

Y añade:

```nginx
server {
    listen 80;
    server_name site2.com www.site2.com;

    root /usr/share/nginx/site2.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

#### 4. Activar los Server Blocks

Para activar estos server blocks, crea un enlace simbólico de estos archivos hacia `sites-enabled`:

```bash
sudo ln -s /etc/nginx/sites-available/site1.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/site2.com /etc/nginx/sites-enabled/
```

#### 5. Verificar la Configuración de Nginx

Es recomendable verificar la configuración de Nginx para asegurarte de que no hay errores sintácticos:

```bash
sudo nginx -t
```

Si todo está bien, deberías ver algo como:

```
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

#### 6. Reiniciar Nginx

Por último, reinicia Nginx para que los cambios surtan efecto:

```bash
sudo systemctl restart nginx
```
#### 7. Crea las entradas en tu archivo `hosts` local
> En linux: `/etc/hosts`
> En Windows: `C:\Windows\System32\drivers\etc\hosts`

con las siguientes líneas:
```
<ip_pública_de_tu_ec2> site1.com
<ip_pública_de_tu_ec2> site2.com
```
#### 8. Comprueba si puedes acceder a tus webs desde el navegador

O desde la consola con `curl`:
```bash
$ curl site1.com
$ curl site2.com
```

Comprueba también los registros de log del servidor:
```bash
$ sudo tail -f /var/log/nginx/access.log
$ sudo tail -f /var/log/nginx/error.log
```

## Práctica 3: Crear un virtual host con un certificado SSL


![En construcción](/recursos-daw/img/under-construction.gif)