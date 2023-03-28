---
title: "Introducción a AWS"
linkTitle: "AWS"
weight: 100
description: >
  Introducción al despliegue en la nube AWS
---

{{% pageinfo %}}
## Documentación general
* https://josejuansanchez.org/iaw/taller-aws/

{{% /pageinfo %}}



## EC2
### Configuración apache
```bash
# isntalar apache
$ sudo yum -y install httpd
# iniciar el servicio
$ sudo service httpd start  
```
#### Virtualhosts
Configura `/etc/httpd/conf.d/vhosts.conf`
* https://httpd.apache.org/docs/2.4/vhosts/name-based.html
