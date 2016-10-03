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

## Security

Es necesario modificar el archivo *app/config/security.yml* de la siguiente manera:

```yml
# To get started with security, check out the documentation:
# http://symfony.com/doc/current/book/security.html
security:
    encoders:
        FilmBundle\Entity\User:
            algorithm: bcrypt #Algoritmo de cifrado de la contraseña
            cost: 4

    # http://symfony.com/doc/current/book/security.html#where-do-users-come-from-user-providers
    providers:
        our_db_provider: #parece que aquí puede ir cualquier nombre
            entity:
                class: FilmBundle:User #Ya que te autentificas con un usuario
                property: email #Campo con el que te identificas

    firewalls:
        # disables authentication for assets and the profiler, adapt it according to your needs
        dev:
            pattern: ^/(_(profiler|wdt)|css|images|js)/
            security: false

        main:
            anonymous: ~
            provider: our_db_provider #ESte debe coincidir con el nombre de arriba, es su única utilidad
            form_login:
                login_path: /login
                check_path: /login_check
            logout:
                path: /logout
                target: /login #Lugar al que te destina tras cerrar sesión
            # activate different ways to authenticate

            # http_basic: ~
            # http://symfony.com/doc/current/book/security.html#a-configuring-how-your-users-will-authenticate

            # form_login: ~
            # http://symfony.com/doc/current/cookbook/security/form_login_setup.html

```
