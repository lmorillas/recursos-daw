---
title: "Crear web estática en S3 con CLI"
linkTitle: "S3 + CLI"
weight: 30
description: >
  Crear Web estática en S3 con AWS CLI


---

{{% pageinfo %}}
## Learner Lab
* https://docs.aws.amazon.com/cli/latest/reference/s3/website.html
{{% /pageinfo %}}

## Pasos

### Previo
#### Configurar AWS CLI
* Conecta con el `Learner Lab`
* Copia las `credentials` de tu usuario de IAM en `~/.aws/credentials`

### Crear Bucket S3

 ```bash

bucket_name="miwebs3-nombre-bucket-unico"

aws s3api create-bucket --bucket "${bucket_name}"
```

### Configuramos el bucket para que sea publico
```bash
aws s3api put-public-access-block --bucket "${bucket_name}" --public-access-block-configuration "BlockPublicPolicy=false"

aws s3api put-bucket-policy --bucket "${bucket_name}" --policy '{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::'"${bucket_name}"'/*"
 
            ]
        }
    ]
}'
```

### Subimos un fichero html
> Mostramos un ejemplo, puedes subir tu web estática
```bash
echo "<html><center><h1>My Static Website on S3</h1></center></html>" > index.html

aws s3 cp index.html s3://"${bucket_name}"
```

### Configuramos el bucket para que sea un sitio web
```bash
aws s3 website s3://"${bucket_name}" --index-document index.html
```

### Comprobamos que funciona
```bash
curl http://"${bucket_name}".s3-website.us-east-1.amazonaws.com
```