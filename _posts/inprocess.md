---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Login de usuario en Symfony 3.
---

En este post se mostrará un ejemplo sobre como crear un [login](http://symfony.com/doc/current/security/form_login_setup.html) en Symfony 3.

## Rutas

En el *bundle* que queramos añadir el login se modifica el archivo *Resources/config/routing.yml* para añadir las rutas necesarias para la creación del login:

```yml
login:
    path: /login
    defaults: { _controller: FilmBundle:User:login }

login_check:
    path: /login_check

logout:
    path: /logout
```
