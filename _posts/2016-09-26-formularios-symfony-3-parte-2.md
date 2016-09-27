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

Continuando con el [post anterior](http://www.josemanuelvazquez.es/symfony/formularios-symfony-3/) sobre formularios vamos a ver como procesar los datos que nos llegan.


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

## Imagen opcional

Hay algunos atributos que no deberían ser obligatorios, por ejemplo la imagen. Para hacer la imagen opcional lo primero es editar */Form/FilmType.php* añadiendo en *->add('image',...)*:

```javascript
 "required" => false
```

Además hay que modificar el controlador para que no se produzca ningún fallo si el campo imagen se envía vacío. De forma que sustituimos el código del apartado anterior por el siguiente:

```javascript
if(!empty($file) && $file!=null){
    $ext=$file->guessExtension();
    $file_name=time().".".$ext;
    $file->move("uploads",$file_name);

    $film->setImage($file_name);      
}else{
    $film->setImage(null);
}    
```

### Control de películas duplicadas

Se controlará que no existan dos películas repetidas (con el mismo nombre). Por lo que hay que realizar una consulta a la base de datos con el nombre de la película que se recibe, y en el caso de que no devuelva ningún registro se almacenará la nueva película.

```javascript
$em = $this->getDoctrine()->getEntityManager();
$film_repo=$em->getRepository("FilmBundle:Film");
$film = $film_repo->findOneBy(array("title"=>$form->get("title")->getData()));

if(count($film)==0){
   ...
}else{
   $status = "La película ya existe";
}
```

### Mostrar mensaje Flash

Es posible mostrar el contenido de la variable status previamente creada, para indicar al usuario si se ha producido un fallo. Para que funcione hay que añadir:

```javascript
use Symfony\Component\HttpFoundation\Session\Session;
```

Además se añade dentro de la clase:

```javascript
private $session; 

public function __construct(){
    $this->session = new session();
}  

public function addAction(Request $request){
  ...
  if($form->isSubmitted()){ //Si el formulario se ha enviado
    ...
    $this->session->getFlashBag()->add("status", $status);
  }
  ...
}
```

Finalmente editamos la vista para poder ver el resultado:

```javascript
{% raw  %}
{% for message in app.session.flashbag().get('status') %}
		<div class="alert alert-success">{{ message }}</div>
{% endfor %}
{% endraw %}
```

![Jekyll]({{ site.baseurl }}/images/form04.png "Jekyll")

### FilmController.php

Finalmente, en el controlador tenemos:

```javascript
<?php

namespace FilmBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Session\Session;
use FilmBundle\Entity\Film;
use FilmBundle\Form\FilmType;

class FilmController extends Controller
{
    private $session; 

    public function __construct(){
        $this->session = new session();
    }

    public function addAction(Request $request){
        $film = new Film();
        $form = $this->createForm(FilmType::class, $film);

        $form->handleRequest($request);

        if($form->isSubmitted()){ //Si el formulario se ha enviado
            if($form->isValid()){ //Si el formulario es válido
                //Se comprueba que la película no este repetida
                $em = $this->getDoctrine()->getEntityManager();
                $film_repo=$em->getRepository("FilmBundle:Film");
                $film = $film_repo->findOneBy(array("title"=>$form->get("title")->getData()));

                if(count($film)==0){
                    //Objeto Film, que se va a añadir
                    $film = new Film(); 

                    //Inicializamos los atributos del objeto film con los datos recibidos del formulario
                    $film->setTitle($form->get("title")->getData()); 
                    $film->setDescription($form->get("description")->getData());
               
                    //La imagen no es obligatoria, si no se envía se asigna a null.
                    if(!empty($file) && $file!=null){
                        $ext = $file->guessExtension();
                        $file_name = time().".".$ext;
                        $file->move("uploads",$file_name);

                        $film->setImage($file_name);      
                    }else{
                        $film->setImage(null);
                    }          

                   //Se recibe el nombre de la categoría, pero es necesario almacenar su id.
                    $category_repo=$em->getRepository("FilmBundle:Category");  
                    $category = $category_repo->find($form->get("category")->getData());
                    $film->setCategory($category);

                    //Se inserta en la base de datos
                    $em->persist($film); 
                    $flush=$em->flush();

                    if($flush==null){
                        $status = "La película se ha añadido correctamente.";
                    }else{
                        $status = "Error al añadir la película.";    
                    }
                }else{
                    $status = "La película ya existe";
                }
            }else{
                $status = "La película no se ha creado";
            }
            $this->session->getFlashBag()->add("status", $status);
        }

        return $this->render("FilmBundle:Film:add.html.twig",array(
            'form' => $form->createView()
        )); 
    }

}
```

Aunque el formulario se envía y se almacena en la base de datos, es posible añadir reglas de validación a los distintos campos. Por ejemplo poniendo un tamaño mínimo al título, o no permitir ciertos caracteres. Esto, junto con eliminar y editar la información de la base de datos se verá en el [siguiente artículo](http://www.josemanuelvazquez.es/symfony/formularios-symfony-3-parte-3.md/).
