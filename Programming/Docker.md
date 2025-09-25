links: [[001 - 030 Programming|Programming]]


# Docker
Docker ha revolucionado el desarrollo y despliegue de aplicaciones, ofreciendo una solución flexible y eficiente para la virtualización a nivel de sistema operativo. Su capacidad para crear entornos consistentes y portables lo convierte en una herramienta esencial para desarrolladores y equipos de operaciones en la actualidad[^1].

## Componentes principales de Docker

### Motor Docker

El corazón de Docker es su motor, una aplicación cliente-servidor que consta de tres componentes principales[^1]:

1. **Docker daemon**: Funciona como servidor en segundo plano, gestionando imágenes, contenedores y redes.
2. **API REST**: Permite la interacción entre programas y el daemon.
3. **Terminal del sistema operativo (CLI)**: Interfaz de usuario para controlar el daemon mediante comandos.

### Imágenes Docker

Las imágenes Docker son plantillas de solo lectura que contienen las instrucciones necesarias para crear un contenedor. Son portátiles y se presentan como archivos de texto llamados Dockerfiles[^1].

### Docker Hub

Docker Hub es un registro en la nube que actúa como biblioteca para imágenes Docker. Ofrece repositorios públicos y privados, permitiendo a los usuarios compartir y acceder a imágenes[^1].

## Ventajas de Docker

1. **Portabilidad**: Las aplicaciones en contenedores pueden ejecutarse en diversas plataformas e infraestructuras sin necesidad de adaptación[^1].
2. **Eficiencia**: Permite ejecutar aplicaciones con diferentes requisitos de forma aislada sin la sobrecarga de sistemas huésped separados[^1].
3. **Consistencia**: Facilita la transición de desarrollo a producción, asegurando que la aplicación funcione de manera idéntica en diferentes entornos[^2].

## Uso básico de Docker

### Comandos fundamentales

- `docker build`: Crea una imagen a partir de un Dockerfile[^1].
- `docker pull`: Descarga una imagen de un repositorio[^1].
- `docker run`: Inicia un contenedor basado en una imagen[^1].


### Creación de un contenedor

Para crear un contenedor Docker, sigue estos pasos:

1. Escribe un Dockerfile con las instrucciones necesarias.
2. Construye la imagen usando `docker build`.
3. Ejecuta el contenedor con `docker run`[^9].

## Buenas prácticas

### Optimización del Dockerfile

Para mejorar el rendimiento y la eficiencia:

1. Utiliza una imagen base ligera.
2. Minimiza el número de capas.
3. Aprovecha la caché de Docker copiando primero los archivos de dependencias[^3].

### Gestión de equipos y organizaciones

Docker Hub permite crear y gestionar grupos de trabajo (organizaciones) para compartir imágenes y colaborar en proyectos[^1].


---
tags:
	#Concept #Programming

[^1]: https://www.ionos.mx/digitalguide/servidores/configuracion/tutorial-docker-instalacion-y-primeros-pasos/

[^2]: https://github.com/brunocascio/docker-espanol

[^3]: https://fastapi.tiangolo.com/es/deployment/docker/

[^4]: https://docs.github.com/es/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/introduction-to-dev-containers

[^5]: https://learn.microsoft.com/es-es/dotnet/core/docker/build-container

[^6]: https://cloud.google.com/artifact-registry/docs/docker/authentication?hl=es-419

[^7]: https://profile.es/blog/como-documentar-tus-bases-de-datos-con-schemaspy/

[^8]: https://docs.aws.amazon.com/es_es/serverless-application-model/latest/developerguide/install-docker.html

[^9]: https://www.hostinger.es/tutoriales/como-crear-contenedor-docker

[^10]: https://docs.github.com/es/actions/sharing-automations/creating-actions/creating-a-docker-container-action

[^11]: http://recetas-docker.readthedocs.io/es/latest/capitulo_1.html

[^12]: https://www.freecodecamp.org/espanol/news/guia-de-docker-para-principiantes-como-crear-tu-primera-aplicacion-docker/

[^13]: https://pilasguru.gitbooks.io/docker-guia-para-el-usuario/chapter03/04crear-dockerfile.html

[^14]: https://aulasoftwarelibre.github.io/taller-de-docker/dockerfile/

[^15]: https://learn.microsoft.com/es-es/azure/ai-services/document-intelligence/containers/install-run?view=doc-intel-4.0.0

[^16]: https://docs.rockylinux.org/es/guides/contribute/localdocs/rockydocs_web_dev/

