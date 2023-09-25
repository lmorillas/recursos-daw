---
title: "Despliegue de una web hecha con Hugo en S3"
linkTitle: "Web estática con Hugo"
weight: 20
description: >
  Despligue de una web estática con Hugo en S3 + CloudFront

doc: >
  https://medium.com/@stefonsimmons1/guide-to-deploy-hugo-to-aws-cloudfront-260b780d8f79
  https://mocki.io/hosting-a-hugo-site-on-aws-s3-and-cloudfront
  https://blog.cavelab.dev/2021/08/deploying-hugo-blog-to-s3/

---

{{% pageinfo %}}
## Learner Lab
* https://gohugo.io/hosting-and-deployment/hugo-deploy/

{{% /pageinfo %}}

## Pasos

### Previo
#### Configurar AWS CLI
* Conecta con el `Learner Lab`
* Copia las `credentials` de tu usuario de IAM en `~/.aws/credentials`

### Configurar `hugo.yml`

Ejemplo
```yaml
[deployment]
# By default, files are uploaded in an arbitrary order.
# Files that match the regular expressions in the "Order" list
# will be uploaded first, in the listed order.
order = [".jpg$", ".gif$"]


[[deployment.targets]]
# An arbitrary name for this target.
name = "mydeployment"

# S3; see https://gocloud.dev/howto/blob/#s3
# For S3-compatible endpoints, see https://gocloud.dev/howto/blob/#s3-compatible
URL = "s3://<Bucket Name>?region=<AWS region>" # Configura aquí tu bucket

# If you are using a CloudFront CDN, deploy will invalidate the cache as needed.
cloudFrontDistributionID = <ID>  # Sólo si quieres usar cloudfront

# Optionally, you can include or exclude specific files.
# See https://godoc.org/github.com/gobwas/glob#Glob for the glob pattern syntax.
# If non-empty, the pattern is matched against the local path.
# All paths are matched against in their filepath.ToSlash form.
# If exclude is non-empty, and a local or remote file's path matches it, that file is not synced.
# If include is non-empty, and a local or remote file's path does not match it, that file is not synced.
# As a result, local files that don't pass the include/exclude filters are not uploaded to remote,
# and remote files that don't pass the include/exclude filters are not deleted.
# include = "**.html" # would only include files with ".html" suffix
# exclude = "**.{jpg, png}" # would exclude files with ".jpg" or ".png" suffix


# [[deployment.matchers]] configure behavior for files that match the Pattern.
# See https://golang.org/pkg/regexp/syntax/ for pattern syntax.
# Pattern searching is stopped on first match.

# Samples:

[[deployment.matchers]]
# Cache static assets for 1 year.
pattern = "^.+\\.(js|css|svg|ttf)$"
cacheControl = "max-age=31536000, no-transform, public"
gzip = true

[[deployment.matchers]]
pattern = "^.+\\.(png|jpg)$"
cacheControl = "max-age=31536000, no-transform, public"
gzip = false

[[deployment.matchers]]
# Set custom content type for /sitemap.xml
pattern = "^sitemap\\.xml$"
contentType = "application/xml"
gzip = true

[[deployment.matchers]]
pattern = "^.+\\.(html|xml|json)$"
gzip = true
```

### Crear web
> Genera la web en el directorio `public`
```bash
$ hugo
```

### Desplegar web

```bash
$ hugo deploy
```
