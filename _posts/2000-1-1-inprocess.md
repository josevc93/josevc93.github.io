---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Bundle y Controlador en Symfony 3.
---

En este post se indicará como crear, o eliminar un bundle, así como sus controladores.

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

Los controladores se añaden en cada bundle dentro del directorio **/Controller** y se nombran con el nombre que queramos seguido de "Controller". Ejemplo: UserController, DefaultController... Dentro de los controladores, tenemos distintas funciones denominadas por su nombre seguidas de "Action. Ejemplo: indexAction, loginAction...

### Redirecciones

Como ejemplo, para redirigir a la ruta "login" desde indexAction se realizaría del siguiente modo:

```php
public function indexAction(){
  return $this->redirect($this->generateUrl("login");
}
```

### Objeto Request

Este objeto permite obtener variables mediante POST y GET. Ejemplo:

```php
public function indexAction(Request $request)
{
  //'page' puede ser un valor obtenido mediante POST o GET.
  echo $request->query->get('page');
}
```

*(Para que funcione hay que añadir 'use Symfony\Component\HttpFoundation\Request;')*

### LLamando a la vista

Para finalizar el controlador, se llama a la vista pasandole o no algún parámetro. Ejemplos:

``` php
//Se llama a login.html.twig sin parámetros
return $this->render("AppBundle:login.html.twig");

//Se le pasa a login.html.twig un parámetro 'valor'
$valor = 3;
return $this->render("AppBundle:login.html.twig", array(
  "valor" = $valor;
));
```


