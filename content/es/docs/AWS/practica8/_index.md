---
title: "Infraestrucutra como código: CloudFormation"
linkTitle: "P 8: IAC - CloudFormation"
weight: 80
description: >
  Despliegue usando CloudFormation

---

{{% pageinfo %}}
## Documentación
* https://aws.amazon.com/es/cloudformation/
* https://josejuansanchez.org/iaw/practica-aws-cloudformation/index.html
* Ejemplos: https://github.com/josejuansanchez/aws-cloudformation-playground
{{% /pageinfo %}}

## Despliegue de una aplicación web con CloudFormation
Despliegue de una web estática en EC2 con NGINX usando CloudFormation

### Requisitos
* Instalar AWS CLI

### Creación de la plantilla
Podemos tenerla en S3 o en local. En este caso la tendremos en local.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: |
  Esta plantilla crea un grupo de seguridad con los puertos 22, 80 y 443 abiertos,
  crea una instancia EC2 con la AMI Amazon Linux 2023 y le asocia el grupo de seguridad.
  También crea una IP elástica y la asocia a la instancia EC2 mediante el recurso AWS::EC2::EIP.
  Configura nginx en la instancia EC2 y despliega una web estática en el directorio /usr/share/nginx/html. La web está en un recurso compartido en drive.google.com.
  En el output muestra la IP pública de la instancia EC2 (la IP elástica).
Resources:
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupName: sg_web-nginx
      GroupDescription: Grupo de seguridad del web-nginx
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 443
          ToPort: 443
          CidrIp: 0.0.0.0/0
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-04cb4ca688797756f
      InstanceType: t2.small
      SecurityGroups:
        - !Ref MySecurityGroup
      KeyName: vockey
      Tags:
        - Key: Name
          Value: nginx-user-data
      UserData:
        Fn::Base64: 
          !Sub |
            #!/bin/bash -xe
            dnf update -y
            dnf install nginx -y
            dnf install unzip -y
            systemctl start nginx
            systemctl enable nginx  
            cd /usr/share/nginx/html
            curl -L -o airspace.zip  "https://drive.google.com/uc?export=download&id=1CXihQ2U-EFzHaO3OnggQ6qkkxdC-ibsp"
            unzip airspace.zip
            # extracción de archivos: están en la carpeta airspace.com
            cd airspace.com
            mv * ..
            cd ..
            rm -r airspace.com/
            rm airspace.zip 
  MyEIP:
    Type: AWS::EC2::EIP
    Properties:
      InstanceId: !Ref MyEC2Instance

Outputs:
  Website:
    Description: The Public IP for the EC2 Instance (Elasic IP)
    Value: !Sub 'http://${MyEIP.PublicIp}'
```

### Despliegue de la plantilla
> Recuerda que tienes que tener instalado AWS CLI

> Conecta con el Learner Lab (`Start Lab`) y configura las credenciales de AWS CLI (`AWS Details` -> `AWS CLI` -> `Show`)

Descarga el archivo [nginx-user-data.yml](/recursos-daw/recursos/nginx-user-data.yml) con la plantilla anterior y ejecuta el siguiente comando:

```bash
$ aws cloudformation create-stack --stack-name nginx-user-data --template-body file://nginx-user-data.yml
```

### Comprobación del despliegue

```bash
$ aws cloudformation describe-stacks --stack-name nginx-user-data
```

Cuando se haya desplegado podremos comprobar que la web está desplegada en la IP pública de la instancia EC2 (la IP elástica que muestra el output del comando anterior).

### Eliminación de la pila

```bash
$ aws cloudformation delete-stack --stack-name nginx-user-data
```

## Propuestas
* Configurar https con certbot automatizado.
* Desplegar varias páginas en nginx para diferentes dominios.