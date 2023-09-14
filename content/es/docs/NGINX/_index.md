---
title: "NGINX"
linkTitle: "NGINX"
weight: 10
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



