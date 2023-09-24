---
title: "Uso de AWS CLI con AWS CloudShell"
linkTitle: "P 9: AWS CloudShell"
weight: 80
description: >
  Introducción al uso del CLI desde AWS CloudShell

---

{{% pageinfo %}}
## Documentación
* https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html
{{% /pageinfo %}}

## Acceso AWS CloudShell
Tienes un botón para abrir el CloudShell

CloudShell ya tiene instalado y configurado el AWS CLI.

## Uso del AWS CLI EC2

### Listar instancias EC2

> Todas las instancias. Toda la información. Salida como json
```bash
$ aws ec2 describe-instances
```
> Todas las instancias. Toda la información. Salida como tabla
```bash
$ aws ec2 describe-instances --output table
```

### Uso con filtros

> Ip, nombre y estado de las instancias que están en ejecución. Salida como tabla

```bash
$ aws ec2 describe-instances  \
--query "Reservations[*].Instances[*].{PublicIP:PublicIpAddress,Name:Tags[?Key=='Name']|[0].Value,Status:State.Name}" \
--filters Name=instance-state-name,Values=running \
--output table
```


> Id y nombre de las instancias 
```bash
$  aws ec2 describe-instances  --query "Reservations[*].Instances[*].{Name:Tags[?Key=='Name']|[0].Value,InstanceId:InstanceId}" --output table
```

> Parar instancia

```bash
$ aws ec2 stop-instances --instance-ids <id de instancia>
```
> Arrancar instancia

```bash
$ aws ec2 start-instances --instance-ids <id de instancia>
```

## Uso del AWS CLI S3

> listar buckets
```bash
$ aws s3 ls
```

> listar objetos de bucket
```bash
$ aws s3 ls s3://bucket-name
```
O recursivo

```bash
$ aws s3 ls s3://bucket-name --recursive
```

> subir un archivo a un bucket
```bash
$ aws s3 cp file.txt s3://bucket-name
```


> subir un directorio a un bucket
```bash
$ aws s3 cp --recursive dir s3://bucket-name
```
> descargar un archivo de un bucket
```bash
$ aws s3 cp s3://bucket-name/file.txt .
```
> eliminar bucket
```bash
$ aws s3 rb s3://bucket-name --force
```
> eliminar objeto de un bucket
```bash
$ aws s3 rm s3://bucket-name/file.txt
```
