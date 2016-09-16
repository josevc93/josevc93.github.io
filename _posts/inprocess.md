
---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Bundle, Controlador y rutas en Symfony 3.
---

sadfkjds kddsjf kdsjfk dsjfkj eqfj jf sdjf kwfejk.

## Bundle

Un bundle simplemente es un conjunto estructurado de archivos que se encuentran en un directorio y que implementan una sola característica. Puedes crear por ejemplo un BlogBundle, un ForoBundle o un bundle para gestionar usuarios (muchos de ellos ya existen como bundles de software libre). Cada directorio contiene todo lo relacionado con esa característica, incluyendo archivos PHP, plantillas, hojas de estilo, archivos Javascript, tests y cualquier otra cosa necesaria.

### Crear un bundle

Podemos [generar un bundle](https://symfony.com/doc/current/bundles.html#creating-a-bundle) muy fácilmente utilizando el siguiente comando:

``` shell
$ php bin/console generate:bundle
```
* El nombre del bundle deberá ser el nombre que queramos seguido de "Bundle". Ejemplo: ForoBundle, BlogBundle...

### Eliminar un bundle

Para [eliminar un bundle](http://symfony.com/doc/current/bundles/remove.html) no existe un comando específico, por lo que debemos de hacer lo siguiente:

1. Acceder a **app/AppKernel.php** y eliminar el Bundle del array $bundle, en el método "registerBundles()".
2. Acceder a **app/config/routing.yml** y eliminar la ruta del bundle que queremos eliminar.
3. Eliminar las referencias al bundle en **app/config/config.yml**.
4. Eliminar el directorio del bundle.

## Controlador
