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

En este ejemplo tenemos una relación ManyToOne entre Empleado y Departamento, dado que muchos empleados pertenecen a un departamento. Este tipo de relaciones se genera automáticamente y permite por ejemplo mostrar el nombre y apellido de cada empleado junto con el nombre del departamento al que pertenece de manera sencilla. Veamos como se hace:

```php
$em = $this->getDoctrine()->getEntityManager();
$empleado_repo = $em->getRepository("EmpresaBundle:Empleado");
$empleados = $empleado_repo->findAll();

foreach($empleados as $empleado)
  echo $empleado->getName()." ".$empleado->getSurname()." ".$empleado->getDepartamento()->getName()."<br/>";
```

