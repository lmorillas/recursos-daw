---
title: "Práctica - Flask App en EC2"
linkTitle: "Práćtica 5: Flask App"
weight: 40
description: >
   
draft: False

docs: >
 
---

{{% pageinfo %}}
## Pasos 
* Instalar app flask
* Ejectar con Gunicorn
* Configurar Gunicorn como servicio
* Configurar Apache o Nginx como proxy inverso
## Gunicorn y Flask
* https://flask.palletsprojects.com/en/2.2.x/deploying/gunicorn/
* https://docs.gunicorn.org/en/stable/deploy.html
## Conexión con Putty
* https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/putty.html

{{% /pageinfo %}}

## Requisitos
* App a instalar https://github.com/DAW-distancia/Weather-App

```bash
# /etc/systemd/system/temperaturas.service
[Unit]
Description=Gunicorn daemon to serve my flaskapp
After=network.target

[Service]
User=ec2-user
Group=apache
WorkingDirectory=/home/ec2-user/Weather-App
ExecStart=/home/ec2-user/Weather-App/env/bin/gunicorn -w 2 -b 127.0.0.1:8080 run:app
ExecReload=/bin/kill -s HUP $MAINPID
```

```bash
sudo systemctl daemon-reload 
sudo systemctl restart temperaturas
sudo systemctl status temperaturas
```

## Configurar Apache como proxy inverso

