---
title: "Práctica - Instalación de Docker en EC2"
linkTitle: "P 7: Docker en EC2"
weight: 70
description: >
   
draft: False

docs: >
 
---

{{% pageinfo %}}
## Presentación:
AWS tiene un servicio de contenedores [ECS](https://docs.aws.amazon.com/es_es/AmazonECS/latest/developerguide/Welcome.html) que permite ejecutar contenedores en la nube. Pero en esta práctica vamos a instalar Docker en una instancia EC2 y ejecutar un contenedor de prueba.

Para desarrollo con contenedores, mejor usar ECS y [ECR](https://docs.aws.amazon.com/ecr/)
{{% /pageinfo %}}


### Instalación de Docker
> Probado en AMI 2023

Todos los comando se ejecutan como root (con sudo)


```bash
sudo yum update
# Puedes buscar el paquete
yum search docker
yum info docker

# Instalar docker
sudo yum install docker

# servicio y comprobación
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker

# Hay que añadir el usuario al grupo docker para que pueda ejecutar los comandos
sudo usermod -a -G docker ec2-user
id ec2-user

# recargar el user (o salir y entrar en el shell)
newgrp docker
```

Puedes comprobar que funciona correctamente con:
```bash
docker run hello-world
```


### Instalación de docker-compose

```bash
# 1. Instalar pip 
sudo yum install python3-pip
# 2. Elige uno de los 3 métodos siguientes
sudo pip3 install docker-compose # con  root access
 
# O
pip3 install --user docker-compose # sin root access for security reasons

# O también
wget https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) 
sudo mv docker-compose-$(uname -s)-$(uname -m) /usr/local/bin/docker-compose
sudo chmod -v +x /usr/local/bin/docker-compose
```
Comprobación:
```bash
docker-compose --version
```

## Un ejemplo complejo
Vamos a instalar una aplicación más compleja y a interactuar con los contenedores. La aplicación a instalar es [BakeryDemo](https://github.com/wagtail/bakerydemo), un CMS de prueba de Wagtail.

En el README tienes las instrucciones para instalar con docker-compose. 
Si analizas el docker-compose:
```yml
version: '2'

services:
  db:
    environment:
      POSTGRES_DB: app_db
      POSTGRES_USER: app_user
      POSTGRES_PASSWORD: changeme
    restart: unless-stopped
    image: postgres:14.1
    expose:
      - '5432'

  redis:
    restart: unless-stopped
    image: redis:6.2
    expose:
      - '6379'

  app:
    environment:
      DJANGO_SECRET_KEY: changeme
      DATABASE_URL: postgres://app_user:changeme@db/app_db
      REDIS_URL: redis://redis
      DJANGO_SETTINGS_MODULE: bakerydemo.settings.dev
    build:
      context: .
      dockerfile: ./Dockerfile
    volumes:
      - ./bakerydemo:/code/bakerydemo
    links:
      - db:db
      - redis:redis
    ports:
      - '8000:8000'
    depends_on:
      - db
      - redis
```
Ves 3 servicios: una base de datos postgres, un redis para caché y la aplicación del CMS (con un Dockerfile específico para python)

### Instalación:
```bash
# clonamos repositorio
git clone https://github.com/wagtail/bakerydemo.git
cd bakerydemo/

# construir y levantar el contenedor
docker-compose up --build -d

# Comprobar el estado
docker-compose ps

# Ejecutar carga inicial de datos. Son instrucciones que se ejecutan dentro del contenedor de la app
docker-compose run app /venv/bin/python manage.py migrate
docker-compose run app /venv/bin/python manage.py load_initial_data

# Comprobamos que funciona correctamente
curl localhost:8000
```

Para verlo desde el navegador tendrás que abrir el puerto 8000 en el grupo de seguridad y acceder con el navegador a la ip pública de la instancia EC2, puerto 8000
