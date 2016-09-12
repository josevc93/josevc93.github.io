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

## Generando entidades

```shell
$ php bin/console doctrine:mapping:convert xml ./src/NombreBundle/Resources/config/doctrine/metadata/orm --from-database --force
$ php bin/console doctrine:mapping:import NombreBundle yml
$ php bin/console doctrine:generate:entities NombreBundle
```
*(Hay que sustitur NombreBundle por el nombre de nuestro Bundle)*

## Relaciones ManyToOne
