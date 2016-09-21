---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar formularios en Symfony 3.
---

En este post se indicará como crear [formularios](http://symfony.com/doc/current/forms.html) en Symfony 3. Se va a crear un formulario que permita insertar películas, siguiendo el siguiente diagrama:

![Jekyll]({{ site.baseurl }}/images/form01.png "Jekyll")

*(El diagrama se ha generado con [DIA](http://dia-installer.de/index.html.es))*

## Creando un formulario

### FilmType.php

En primer lugar se ejecuta:

```shell
$ php bin/console doctrine:generate:form FilmBundle:Film
```
* FilmBundle: Nombre del bundle donde vamos a generar el formulario.
* Film: Nombre de la entidad sobre la que vamos a crear el formulario.

Al ejecutar este comando se ha creado un directorio */form* en *FilmBundle*, y dentro un archivo *.php* que contiene el formulario. Tenemos los siguientes atributos: title (**TextType**), description (**TextareaType**), image (**FileType**), categoría (**EntityType**) y será necesario un botón de envío (**SubmitType**). Por lo tanto habría que añadir:

``` php
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\Extension\Core\Type\FileType;
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
```
Para ver todos los campos que se puede aplicar entra [aquí](http://symfony.com/doc/current/forms.html#text-fields).


