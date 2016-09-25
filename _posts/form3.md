---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar formularios en Symfony 3 (parte 3).
---

Continuando con el [post anterior] sobre formularios vamos a ver como procesar los datos que nos llegan (...)

## Validar formulario

Para [validar el formulario](http://symfony.com/doc/current/validation.html) se crea un archivo **validation.yml** en *FilmBundle/Config/*. Por ejemplo podríamos indicar que el título no puede estar vacío, que la longitud debe ser mínimo de 3 caracteres y que el tipo sea numérico.

```yml
FilmBundle\Entity\Film:
  properties:
    title:
      - NotBlank: {message: "El nombre no puede estar vacío."}
      - Length:
          min: 2
          minMessage: "El nombre tiene que tener mas de 2 caracteres"
      - Type: 
          type: numeric
          message: "El tipo debe ser numérico"
```

Entra [aquí](http://symfony.com/doc/current/reference/constraints/Type.html#reference-constraint-type-type) para comprobar los distintos tipos que se pueden aplicar. Además estas son todas las [restricciones](http://symfony.com/doc/current/validation.html#basic-constraints) que pueden aplicarse. 

