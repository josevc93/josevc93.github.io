---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar formularios en Symfony 3.
---

En este post se indicará como crear [formularios](http://symfony.com/doc/current/forms.html) en Symfony 3. Se va a crear un formulario que permita insertar películas, siguiendo el siguiente diagrama:

![Jekyll]({{ site.baseurl }}/images/form01.png "Jekyll")

*(El diagrama se ha generado con [DIA](http://dia-installer.de/index.html.es))*

## Creando un formulario

### FilmType.php

En primer lugar se ejecuta:

```shell
$ php bin/console doctrine:generate:form FilmBundle:Film
```
* FilmBundle: Nombre del bundle donde se va a generar el formulario. 
* Film: Nombre de la entidad sobre la que se va a crear el formulario.

Al ejecutar este comando se ha crea un directorio **/form** en *FilmBundle*, y dentro un archivo *.php* que contiene el formulario. Tenemos los siguientes atributos: category (*EntityType*), title (*TextType*), description (*TextareaType*), image (*FileType*) y será necesario un botón de envío (*SubmitType*). Por lo tanto habría que añadir:

``` php
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\Extension\Core\Type\FileType;
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
```
Para ver todos los campos que se pueden aplicar entra [aquí](http://symfony.com/doc/current/forms.html#text-fields). Además hay que modificar *$builder*:

```javascript
public function buildForm(FormBuilderInterface $builder, array $options)
{
    $builder
        ->add('title', TextType::class, array(
            "label" => "Titulo",
            "attr" => array("class" => "form-control")
        ))
        ->add('description', TextareaType::class, array(
            "label" => "Descripcion",
            "attr" => array("class" => "form-control")
        ))
        ->add('image', FileType::class, array(
            "label" => "Imagen",
            "attr" => array("class" => "form-control")
        ))
        ->add('category', EntityType::class, array(
            "class" => 'FilmBundle:Category',
            "attr" => array("class" => "form-control")
        ))            
        ->add ('Enviar', SubmitType::class, array("attr"=>array(
            "class" => "form-submit btn btn-success")
        ))
    ;
}
```
* Category es un campo *EntityType*, ya que proporcionará un listado de categorías (Acción, Drama, Comedia...), todas las que tengamos en la tabla *Category*.
* Se ha asignado *form-control* y *form-submit* para aplicar estilos de [Bootstrap](http://getbootstrap.com/).

### Controlador y Vista

En el controlador hay que añadir:

``` php
use FilmBundle\Entity\Film;
use FilmBundle\Form\FilmType;
```
Finalmente, le pasamos el formulario a la vista para mostrarlo:

```javascript 
public function addAction(Request $request){
    $film = new Film();
    $form = $this->createForm(FilmType::class, $film);
    $form->handleRequest($request);
    
    return $this->render("FilmBundle:Film:add.html.twig",array(
        'form' => $form->createView()
    )); 
}
```
Y en la vista se muestra el formulario que le pasamos:

```
<!DOCTYPE html>
<html lang="es">
<head>
	<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet" />
</head>
<body>
	<div class="col-lg-6">
		<h2>Nueva película</h2>
		{{ "{{ form_start(form) " }}}}
		{{ "{{ form_end(form) " }}}}
	</div>
	<div class="clearfix"></div>
</body>
</html>
```

Aunque cuando tratemos de acceder a la URL por ejemplo */film/add* dará un error.

![Jekyll]({{ site.baseurl }}/images/form02.png "Jekyll")

Esto se debe a que trata de convertir el objeto category a un string, sin estar esta función definida. Hay que editar 
*Category.php* añadiendo:

```javascript
 public function __toString(){
    return $this->name;
  }
```

Ahora, cuando se vaya a insertar una película aparece una lista de nombres de las distintas categorías para seleccionar una.

![Jekyll]({{ site.baseurl }}/images/form03.png "Jekyll")

Ya aparece el formulario, y aunque envía los datosm, todavía no se almacenan en la base de datos. Esto se verá en el [siguiente artículo](http://www.josemanuelvazquez.es/symfony/formularios-symfony-3-parte-2/).
