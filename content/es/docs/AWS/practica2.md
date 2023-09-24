---
title: "Despliegue de una web estática con S3 + CloudFront"
linkTitle: "P 2: Static Web S3"
weight: 10
description: >
  Despligue de una web estática en S3 + CloudFront

docs: >
  https://hackmd.io/@dme26/ryg8sv9Br#
  https://cosc349.cspages.otago.ac.nz/cache/s3-website-tute.pdf
  https://github.com/oucs-teaching/cosc349-labs/blob/master/lab08.md
  https://altitude.otago.ac.nz/cosc349/lab09-vagrant-aws/-/blob/master/Vagrantfile
  https://blog.knoldus.com/how-to-configure-aws-ec2-instance-using-vagrant/
  https://docs.aws.amazon.com/pdfs/AmazonS3/latest/userguide/s3-userguide.pdf#WebsiteHosting
  > https://github.com/aws-samples/amazon-cloudfront-secure-static-site#user-content-amazon-cloudfront-secure-static-website

draft: False
---

{{% pageinfo %}}
## Learner Lab
* Curso: https://awsacademy.instructure.com/courses/57608
* Learner Lab: https://awsacademy.instructure.com/courses/57608/modules/items/5062395

### Ayuda:
* https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html
* [Amazon S3](https://aws.amazon.com/es/s3/?trk=d0993ae4-4193-4d67-b2a9-e83cdb563369&sc_channel=ps&s_kwcid=AL!4422!3!588732081285!p!!g!!s3&ef_id=Cj0KCQiApKagBhC1ARIsAFc7Mc7CCaBhfqHJdFw2YSTyXlrvmr8VVrNBcUNMaO9uCX4QSVelI1ALPKsaAuxDEALw_wcB:G:s&s_kwcid=AL!4422!3!588732081285!p!!g!!s3)
* Ejemplo S3 + CloudFront: 
  * https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/examples-s3-origin.html
  * https://dev.to/aws-builders/deploy-static-website-on-s3-bucket-and-configure-cloudfront-distribution-12em

{{% /pageinfo %}}


![Static Web S3](https://d1.awsstatic.com/s3-pdp-redesign/product-page-diagram_Amazon-S3_HIW.cf4c2bd7aa02f1fe77be8aa120393993e08ac86d.png)

## Objetivo:
Desplegar una web estática en S3 y añadir CloudFront como CDN.

## Tarea1: Despliegue de una web estática en S3
> Sigue las instrucciones de https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html
* Crear un bucket S3
* Configurar el bucket para que sea accesible como web estática
* Añade los ficheros de una de las webs estáticas del ejercicio anterior. Puedes subirlos desde la misma interfaz web de la consola de AWS. Veremos también cómo puedes hacerlo desde AWS CLI.

## Tarea2: Añadir CloudFront como CDN 
Sigue el ejemplo de https://aws.amazon.com/es/blogs/aws-spanish/como-alojar-tu-sitio-web-estatico-en-amazon-s3-y-amazon-cloudfront/

> Nosotros no integraremos AWS CodePipeline, AWS CodeCommit y AWS CodeBuild.


Aquí tienes un ejemplo paso a paso:

{{< youtube DgQroj70CJ0 >}}




