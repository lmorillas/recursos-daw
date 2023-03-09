---
title: "Práctica Apache con VirtualHosts"
linkTitle: "Práctica 1 - Apache"
weight: 10
description: >
  Práctica de instalacion de apache con 2 virtualhosts
---

{{% pageinfo %}}
## Learner Lab
* Curso: https://awsacademy.instructure.com/courses/33805
* Learner Lab: https://awsacademy.instructure.com/courses/33805/modules/items/282557

### Ayuda configuración apache:
Recuerda que la configuración en AMI LINUX es diferente a la de Ubuntu. En este caso, el archivo de configuración de apache es `/etc/httpd/conf/httpd.conf` y el directorio de los virtualhosts es `/etc/httpd/conf.d/` https://thewebhacker.com/setting-up-apache-vhosts-on-aws-linux/
{{% /pageinfo %}}

## Objetivo:
Instalar apache en una instancia ECS y configurar 3 virtualhosts: uno para la página principal, otro para la página de relojesvex y otro para airspace. Las dos últimas webs están en la tarea de moodle. 

## Tareas:
* Instala apache en la instancia EC2 y configura los vhosts
* Tendrás que configurar el archivo hosts de tu máquina local para que puedas acceder a las páginas web desde tu navegador.
* La web principal (a la que accedes por la ip pública de la instancia EC2) debe mostrar un mensaje de bienvenida y un enlace a las otras dos páginas.
* Los archivos de las webs los puedes subir por `scp` (revisa cómo se configura el `ssh` en los Learner Lab) o si lo subes a un sitio público, puedes descargarlo con `wget` y descomprimirlo dentro de la máquina en la nube






