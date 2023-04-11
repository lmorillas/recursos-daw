---
title: "Integración continua"
linkTitle: "CI/CD"
weight: 10
description: >
  
---

{{% pageinfo %}}
## Documentación
* https://en.wikipedia.org/wiki/CI/CD
* https://docs.github.com/en/actions
* https://www.redhat.com/en/topics/devops/what-is-ci-cd
{{% /pageinfo %}}

## ¿Qué es?
Lee el artículo de wikipedia e investiga sobre el tema.

Aquí tienes un vídeo explicativo sencillo:

{{< youtube 6eRkCnFhHRg >}}

## ¿Cómo podemos implementarlo con GitHub Actions?
Podemos implentar la integración continua y el despliegue continuo con GitHub Actions. Para ello, debemos crear un fichero de configuración en la carpeta `.github/workflows` de nuestro repositorio. 

### ¿Qué son GH Actions?
La documentación de GH Actions la puedes encontrar en [este enlace](https://docs.github.com/en/actions). Comienza por el [inicio rápido](https://docs.github.com/en/actions/quickstart) y el [tutorial](https://docs.github.com/en/actions/learn-github-actions).

Seguramente encontrarás ya acciones hechas para lo que necesites en el [Marketplace](https://github.com/marketplace?category=&query=&type=actions&verification=).

### Un ejemplo
Aquí tienes un ejemplo para despliegues con [Kubernetes](https://kubernetes.io/es/). Kubernetes (K8s) es una plataforma de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones en contenedores. No lo hemos utilizado en el curso pero es una tecnología muy interesante.

{{< youtube MNBf-ylhtK0 >}}