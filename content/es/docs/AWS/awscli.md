---
title: "AWS Client"
linkTitle: "AWS Cli"
weight: 45
description: >
   
draft: True

docs: >
 * https://tutorialsdojo.com/working-with-aws-command-line-interface-cli/
 * https://github.com/josejuansanchez/aws-python-boto3
---

{{% pageinfo %}}
## Documentación
* https://josejuansanchez.org/iaw/practica-aws-cli/index.html
* https://dev.to/aws-builders/aws-academy-tarea-en-learning-lab-con-aws-cli-14hj

{{% /pageinfo %}}

## Instalación

https://josejuansanchez.org/iaw/practica-aws-cli/index.html#instalaci%C3%B3n-de-aws-cli

## Configuración
Arrancado el **Learner Lab**, las credenciales de acceso a AWS se encuentran en la pestaña **AWS Details -> Cloud Access -> AWS CLI** y tienen un aspecto así:
```
[default]
aws_access_key_id=ASIAYD2BNODVX757R
aws_secret_access_key=Xdm/GhxBBJV9kX+3+/b7iqJcmC9tIONVGFyY
aws_session_token=FwoGZXIvYXdzEJD//////////wEaDKO2mfuV...
```
Copiar nuestros datos de acceso en:
* En Linux/macOs `~/.aws/credentials`
* En Windows estará `C:\Users\usuario\.aws\credentials`

Una vez copiado:
```bash
aws configure
```
* Región: us-east-1
* Formato de salida: json

Cambian en cada sesión, por lo que hay que actualizarlos cada vez que se inicie el **Learner Lab**.

## Comandos básicos
