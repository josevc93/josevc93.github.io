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

```javascript
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

Si tuviera un campo usuario, se podría obtener el usuario logeado en la sesión y almacenarlo fácilmente:

```javascript
$user=$this->getUser(); //Obtiene el usuario de la sesión actual
$film->setUser($user);
```

### Subir y asignar imagen

Por ahora, ya se almacenan los datos que enviamos a excepción de la imagen que se almacena como 'null'. Para almacenar la imagen se sustituye *$film->setImage(null);* por lo siguiente:

```javascript
//Se obtiene la imagen
$file=$form["image"]->getData();
//Se obtiene la extensión
$ext=$file->guessExtension();
//Se asigna un nombre que será la fecha (para que sea único) seguido de la extensión
$file_name=time().".".$ext;
//Se mueve la imagen al directorio web/uploads
$file->move("uploads",$file_name);
//Se asigna al objeto Film
$film->setImage($file_name);
```

### Control de películas duplicadas

Se controlará que no existan dos películas repetidas (con el mismo nombre).

-> Controlar que la película no este repetida (otra con el mismo titulo)
-> Imagen Opcional
-> Validar Formulario
-> Mensaje Flash (quizás en 3º parte)
--------
3º parte
--------
- Eliminar película
- Editar película
