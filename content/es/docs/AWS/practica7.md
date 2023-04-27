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
