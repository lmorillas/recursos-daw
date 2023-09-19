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



## Práctica Inicial: Instalación de NGINX

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