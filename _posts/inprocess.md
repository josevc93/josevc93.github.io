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

El login se realizará con la clase *Entity/User.php* por lo que para que funcione la clase debe de implementar *UserInterface*: 

```javascript
use Symfony\Component\Security\Core\User\UserInterface;

class User implements UserInterface
{
...
}
```

Además hay que añadir los siguientes métodos:

```javascript
public function getUserName(){
    return $this->email; //Ya que se logea con el email
}

public function getSalt(){
    return null;
}

public function getRoles(){
    return array($this->getRole());
}

public function eraseCredentials(){}
```

## Controlador

En el controlador, como viene en la [documentación](http://symfony.com/doc/current/security/form_login_setup.html) se llama a la vista pasandole el error en caso de que se produzca y el último usuario introducido.

```javascript
<?php

namespace FilmBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Request;

class UserController extends Controller
{
    public function loginAction(Request $request){
        $authenticationUtils = $this->get("security.authentication_utils");
        $error = $authenticationUtils->getLastAuthenticationError();
        $lastUsername = $authenticationUtils->getLastUsername();

        return $this->render("FilmBundle:User:login.html.twig", array(
            "error" => $error,
            "last_username" => $lastUsername
        ));
    }
}
```



- Método login en el controlador user
- Añadir la vista login.html.twig
- Cifrar contraseñas
- Rutas y control de acceso
- Mirar formulario de registro
