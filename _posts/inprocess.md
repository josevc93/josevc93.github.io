---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar entidades a partir de la base de datos en Symfony 3.
---
En este artículo se aprenderá como generar entidades a partir de la base de datos utilizando [Symfony](http://symfony.com/) y se aprenderá como configurar las relaciones **ManyToOne** y **OneToMany**. En este artículo utilizaremos como ejemplo el siguiente diagrama:

![Symfony]({{ site.baseurl }}/images/entidadesbd01.png "Symfony")

*(El diagrama se ha generado con [DIA](http://dia-installer.de/index.html.es))*

## Requisitos previos

En primer lugar es necesario tener instalado un servidor web como Apache. Puedes ver como instalarlo [aquí](http://selmanarriaga.link/blog/es/2016/04/instalar-lamp-en-ubuntu-16-04/).

## Instalador de Symfony

Como se indica en la [documentación](http://symfony.com/doc/current/book/installation.html) se recomienda utilizar el instalador de Symfony para crear nuevas aplicaciones en Symfony. Para ello hay que abrir la consola y ejecutar los siguientes comandos:

```shell
$ sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
$ sudo chmod a+x /usr/local/bin/symfony
```

## Creando una aplicación

Hay que desplazarse a la carpeta dónde se quiere crear (si has instalado apache */var/www/html*) y ejecutar el siguiente comando:

```shell
$ symfony new my_project_name
```

Para comprobar que todo funciona, hay que moverse a la carpeta del proyecto y ejecutar:

```shell
$ php bin/console server:run
```

Finalmente en el navegador accedemos a *http://localhost:8000* y debería de mostrar lo siguiente:

![Symfony]({{ site.baseurl }}/images/symfony01.png "Symfony")

## Configurando la aplicación

Una vez que todo funciona correctamente, accedemos a *app/config/parameters.yml* donde modificaremos los siguientes valores de la base de datos:

```shell
database_name:
database_user:
database_password:
```
