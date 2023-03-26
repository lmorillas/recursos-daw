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
## Apapche / Nginx proxy inverso
* https://akira3030.github.io/formacion/articulos/python-flask-gunicorn-docker.html
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
#Group=apache
WorkingDirectory=/home/ec2-user/Weather-App
ExecStart=/home/ec2-user/Weather-App/env/bin/gunicorn -w 2 -b 127.0.0.1:8080 run:app
ExecReload=/bin/kill -s HUP $MAINPID

[Install]
WantedBy=multi-user.target

```

```bash
sudo systemctl daemon-reload 
sudo systemctl restart temperaturas
sudo systemctl status temperaturas
```

## Configurar Apache como proxy inverso

> Versión sencilla

```apacheconf
# /etc/httpd/conf.d/temperaturas.conf

DocumentRoot "/home/ec2-user/Weather-App"

ProxyPass / http://127.0.0.1:5000/
ProxyPassReverse / http://127.0.0.1:5000/

<Directory /home/ec2-user/Weather-App>
 Require all granted
</Directory>

```


## User Data
Si configuras la instacia (tipo, tamaño, claves, puertos, etc) desde la consola de AWS, puedes añadir un script de arranque que se ejecutará al arrancar la máquina. 

```bash
#!/bin/bash
yum update -y
yum install -y httpd git
#Apache
systemctl start httpd
systemctl enable httpd
# Tempertaturas
cd /home/ec2-user
git clone https://github.com/DAW-distancia/Weather-App.git
cd Weather-App
python3 -m venv env
source env/bin/activate
pip install -r requirements.txt
# inicializa db
python -c "from weather_app import db; db.create_all()"
# Gunicorn
pip install gunicorn
# Permisos (si no, serán los archivos de root)
chown -R ec2-user:apache /home/ec2-user/Weather-App
# Servicio
cat <<EOF > /etc/systemd/system/temperaturas.service
[Unit]
Description=Gunicorn daemon to serve my flaskapp    
After=network.target

[Service]
User=ec2-user
#Group=apache
WorkingDirectory=/home/ec2-user/Weather-App
ExecStart=/home/ec2-user/Weather-App/env/bin/gunicorn -w 2 -b 127.0.0.1:8080 run:app
ExecReload=/bin/kill -s HUP $MAINPID

[Install]
WantedBy=multi-user.target
EOF
systemctl daemon-reload
systemctl start temperaturas
systemctl enable temperaturas
# Apache
cat <<EOF > /etc/httpd/conf.d/temperaturas.conf
DocumentRoot "/home/ec2-user/Weather-App"

ProxyPass / http://127.0.0.1:8080/
ProxyPassReverse / http://127.0.0.1:8080/

<Directory /home/ec2-user/Weather-App>
 Require all granted
</Directory>
EOF
systemctl restart httpd
```
