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
security:
    encoders:
        FilmBundle\Entity\User:
            algorithm: bcrypt #Algoritmo de cifrado para la contraseña
            cost: 4
            
    providers:
        our_db_provider: 
            entity:
                class: FilmBundle:User #Clase con la que se realiza el login (Usuario)
                property: email #Campo con el que se identifica (email)

    firewalls:
        dev:
            pattern: ^/(_(profiler|wdt)|css|images|js)/
            security: false

        main:
            anonymous: ~
            provider: our_db_provider 
            form_login:
                login_path: /login
                check_path: /login_check
            logout:
                path: /logout
                target: /login #Ruta a la que se dirige al cerrar sesión (vuelve al login)
```

## User.php

El login se realizará con la clase user.php por lo que para que funcione la clase debe de implementar *UserIntarface*:

```javascript
use Symfony\Component\Security\Core\User\UserInterface;

class User implements UserInterface
{
...
}
```
