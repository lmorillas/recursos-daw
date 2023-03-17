---
title: "Práctica - Certificado SSL"
linkTitle: "HTTPS"
weight: 35
description: >
   
draft: Trusss

docs: >
 * https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2023.html#letsencrypt-2023
 * https://levelup.gitconnected.com/adding-a-custom-domain-and-ssl-to-aws-ec2-a2eca296facd

---

{{% pageinfo %}}
## HTTPS
* https://en.wikipedia.org/wiki/HTTPS

## Docs AWS
* https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2023.html#letsencrypt-2023
* https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html#letsencrypt

## Docs LetsEncrypt/Certbot
* https://letsencrypt.org/es/getting-started/
* https://certbot.eff.org/

{{% /pageinfo %}}

## Requisitos
* Nombre de dominio
* Configurado con la ip pública de una máquina ECS 
* Servidor web configurado en esa máquina

## Instalación Certbot

```bash
# instalación paquetes
$ sudo dnf install -y python3 augeas-libs pip

# Creación entorno virtual python
$ sudo python3 -m venv /opt/certbot/

# Actualización pip 
$ sudo /opt/certbot/bin/pip install --upgrade pip

# Instalación certbot
$ sudo /opt/certbot/bin/pip install certbot certbot-apache
```

## Creación certificado

```bash
# parar servidor (certbot levanta uno)
$ sudo systemctl stop httpd

$ sudo certbot certonly --standalone

# o también más fácil

$ sudo certbot --apache

# Sigue los pasos

# Instala el certificado si certbot no ha configurado los archivos de httpd
```

Al final tendremos estos archivos de configuración.

###  /etc/httpd/conf.d/ssl.conf 

```apacheconf
# Dentro tendrá unas líneas similares
SSLEngine on
SSLCertificateFile /etc/letsencrypt/live/www.cpilosenlaces.ninja/fullchain.pem
SSLCertificateKeyFile  /etc/letsencrypt/live/www.cpilosenlaces.ninja/privkey.pem
```

### /etc/httpd/conf/httpd.conf 
Configuración del virtualhost y la redirección de http a https

```apacheconf
<VirtualHost *:80>
    DocumentRoot "/var/www/html"
    ServerName "www.cpilosenlaces.ninja"
    ServerAlias "cpilosenlaces.ninja"
    #Redirect HTTP to HTTPS
    RewriteEngine On 
    RewriteCond %{HTTPS} off
    RewriteCond %{HTTP:X-Forwarded-Proto} !https 
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</VirtualHost>
```


## Configuración renovación automática
Puedes configurar la renovación automática del certificado

```bash
$ echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null

$ sudo systemctl restart crond
```

