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

Continuando con el [post anterior](http://www.josemanuelvazquez.es/symfony/formularios-symfony-3-parte-2/), vamos a ver como validar un formulario. Y como eliminar o editar los registros de la base de datos.

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

## Eliminar y actualizar un registro

Para [eliminar](https://symfony.com/doc/current/doctrine.html#deleting-an-object) un registro se utiliza *remove()*.

```javascript
public function deleteAction($id){
  $em = $this->getDoctrine()->getEntityManager();
  $film_repo = $em->getRepository("FilmBundle:Film");
  
  $film = $film_repo->find($id);
  $em->remove($curso);
  
  $flush=$em->flush();
  
  if($flush!=null)
    echo "No se ha borrado.";
  else
    echo "La película se ha borrado correctamente.";
  
  die();
}
```
Y para [actualizar](https://symfony.com/doc/current/doctrine.html#persisting-objects-to-the-database) un registro se modifican los valores de los atributos que queramos y se utiliza *persist()*. Por ejemplo vamos a modificar el título:

```javascript
public function updateAction($id, $title, $description, $image){
  $em = $this->getDoctrine()->getEntityManager();
  $film_repo = $em->getRepository("FilmBundle:Film");
  
  $film = $film_repo->find($id);
  $film->setTitle($title);
  $film->setDescription($description);
  $film->setImage($image);
  
  $em->persist($film);
  $flush=$em->flush();
  
  if($flush!=null)
    echo "No se ha actualizado.";
  else
    echo "La película se ha actualizado correctamente.";
  
  die();
}
```
