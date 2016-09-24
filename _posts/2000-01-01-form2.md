---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar formularios en Symfony 3 (parte 2).
---

Continuando con el [post anterior] sobre formularios vamos a ver como procesar los datos que nos llegan (...)


## Controlador

### Almacenar los datos

Se modifica el controlador, para almacenar los datos recibidos en la base de datos.

```php
public function addAction(Request $request){
  $film = new Film();
  $form = $this->createForm(FilmType::class, $film);
  $form->handleRequest($request);

  if($form->isSubmitted()){ //Si el formulario se ha enviado
      if($form->isValid()){ //Si el formulario es válido
          //Objeto film, para insertarlo posteriormente en la base de datos
          $film = new Film(); 

          //Se inicializan los valores del objeto film que se va a insertar con los datos recibidos
          $film->setTitle($form->get("title")->getData()); 
          $film->setDescription($form->get("description")->getData());
          $film->setImage(null);

          //Se recibe el nombre de la categoría, pero es necesario almacenar su id.
          $em = $this->getDoctrine()->getEntityManager(); 
          $category_repo=$em->getRepository("FilmBundle:Category");  
          $category = $category_repo->find($form->get("category")->getData());
          $film->setCategory($category);

          //Se inserta en la base de datos
          $em->persist($film); 
          $flush=$em->flush();

          if($flush==null){ //Si se inserta correctamente
              $status = "La película se ha añadido correctamente.";
          }else{ //Si se produce un fallo al insertar
              $status = "Error al añadir la película.";    
          }
      }else{ //Si el formulario envíado contiene datos no válidos
          $status = "La película no se ha creado";
      }
  }

  return $this->render("FilmBundle:Film:add.html.twig",array(
      'form' => $form->createView()
  )); 
}
```

   /*Si tuviera un campo usuario, se podría sacar y almacenar así. Obteniendo el usuario logeado en la sesión.
          $user=$this->getUser();
          $film->setUser($user);*/
