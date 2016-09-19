---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: El enrutamiento en Symfony 3.
---

En este post se indicará como crear, o eliminar un bundle, así como sus controladores.

## Rutas

Al generar un bundle, se añade automáticamente en 'app/config/routing.yml' una ruta hacía ese nuevo bundle. Aunque hay que configurar las rutas a los controladores. Supongamos lo siguiente:

**AppBundle / DefaultController / loginAction**

Para añadirle una ruta simplemente accedemos a *AppBundle/Resources/config/routing.yml* y se añade:

```yml
default_login:
  path: /login 
  defaults: { _controller: AppBundle:Default:login }
```
* default_login: Debe tener un nombre descriptivo, pero funcionará con cualquiera que le pongas.
* path: Es la URL desde la que se accede, en este caso: *localhost:8000/login*.
* defaults: Es la ruta para que la aplicación lo encuentre, es importante escribirlo en el orden adecuado *Bundle:Controlador:Método*.

### Rutas con variables

Por ejemplo vamos a pasarle a una ruta tres variables: idioma, nombre y página. 

```php
function loginAction($idioma, $nombre, $page){
  ...
}
```
Las variables tendrán un contenido por defecto, y el idioma solo podrá ser españo, inglés o francés, nombre tendrá que ser una cadena alfabética y página será un entero. Además únicamente se puede llamar por método GET.

```yml
default_index:
  path: /index/{lang}/{name}/{page}
  defaults: { _controller: AppBundle:Default:login, lang:es, name:jose, page:0}
  methods: [GET]
  requirements: 
    name: "[a-zA-Z]*"
    page: \d+
    lang: es|en|fr
```
* El contenido por defecto se añade en 'defaults', sino todas las variables serían obligatorias.
* En methos también es posible añadir [POST].
