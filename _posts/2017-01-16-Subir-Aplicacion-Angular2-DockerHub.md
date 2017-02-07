---
tags:
- Docker
- Docker Hub
- Angular2
categories:
- Docker
title: Subir aplicación Angular 2 a Docker Hub.
---

La idea detrás de [Docker](https://www.docker.com/) es crear contenedores ligeros y portables para las aplicaciones software que puedan ejecutarse en cualquier máquina con Docker instalado, independientemente del sistema operativo que la máquina tenga por debajo, facilitando así también los despliegues.

## Instalación

En primer lugar es necesario [instalar Docker](https://docs.docker.com/engine/installation/linux/ubuntu/#/install-docker).

Algunos comandos útiles:

```
# Comprobamos que se está ejecutando
sudo service docker status

# Vemos que versión estamos trabajando
docker --version

# Lista de comandos
docker

# Información del sistema
docker info

# Descarga y ejecución de un contenedor básico
docker run hello-world
```

## Docker Hub

Vamos a crear una cuenta en Docker Hub para subir ahí el contenedor que vamos a crear. Por lo tanto el primer paso es entrar [aquí](https://hub.docker.com/).

Una vez que nos hemos creado una cuenta el siguiente paso es crear un repositorio:

![Docker]({{ site.baseurl }}/images/DockerHub.png "Docker")

En la carpeta de nuestro proyecto en Angular vamos a crear un archivo Dockerfile, que puede ser el siguiente: 

```
# Create image based on the official Node 6 image from dockerhub
FROM node:6

# Create a directory where our app will be placed
RUN mkdir -p /usr/src/app

# Change directory so that our commands run inside this new directory
WORKDIR /usr/src/app

# Copy dependency definitions
COPY package.json /usr/src/app

# Install dependecies
RUN npm install

# Get all the code needed to run the app
COPY . /usr/src/app

# Expose the port the app runs in
EXPOSE 3000

# Serve the app
CMD npm start
```
No es necesario subir a Docker Hub la carpeta dist, ni la carpeta node_modules. Una vez eliminadas nos situamos en la carpeta del proyecto y ejecutamos:

```
docker build -t usuarioDocker/nombreRepositorio .
```

Para ver que se ha creado la imagen se puede ejecutar lo siguiente:

```
Docker images
```

Si se ha creado bien, se ejecuta el siguiente comando para subir la imagen al repositorio:

```
docker push usuarioDocker/nombreRepositorio
```

Y finalmente podemos cargar nuestra aplicación en cualquier lugar, en el puerto 3000 ejecutando únicamente lo siguiente:

```
docker run --name container -t -i  -d -p 3000:3000 usuarioDocker/nombreRepositorio
```

Para ver como eliminar todas las imagenes y contenedores de Docker en vuestro pc, accede [aquí](https://gist.github.com/erknrio/e470a14f76377883980fa1d5fd531087).

Ejemplo de aplicación subida con Docker: [Ejemplo](https://github.com/josevc93/Angular2Docker).
