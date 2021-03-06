---
tags:
- GitHub
- Jekyll
categories:
- GitHub
title: Crear un blog con GitHub y Jekyll
---
[Jekyll](http://jekyllrb.com/) permite crear un blog fácilmente, alojándose en GitHub de forma gratuita. 

## Crear una cuenta en GitHub

Si aún no la tienes, el primer paso es crear una cuenta en [GitHub](https://github.com/). Es suficiente con la cuenta gratuita, a no ser que quieras repositorios privados.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-01.jpg "Jekyll")

## Diseño del blog

Estos son algunos de los diseños entre los que podemos elegir:

* [Hyde](https://github.com/poole/hyde)
* [Lanyon](https://github.com/poole/lanyon)
* [Left](https://github.com/holman/left)
* [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes)
* [Skinny Bones](https://github.com/mmistakes/skinny-bones-jekyll)

Puedes encontrar muchos más aquí [JekyllThemes](http://jekyllthemes.org/).

## Creando el blog

Vamos a utilizar el diseño *Minimal Mistakes*, aunque el proceso es el mismo para cualquiera que escojamos. En primer lugar realizamos un *Fork*.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-02.jpg "Jekyll")

Tardará unos minutos y podemos ver que ya tenemos el proyecto en nuestro repositorio.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-03.jpg "Jekyll")

Entramos en el repositorio que se ha creado, y seleccionamos *Settings* en la parte superior.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-04.jpg "Jekyll")

Aquí se debe de cambiar el nombre del repositorio, y hay que llamarlo _**usuario**.github.io_, en mi caso _**josevc93**.github.io_.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-05.jpg "Jekyll")

Finalmente si ponemos _**usuario**.github.io_ en el navegador ya podemos visualizar nuestro blog.

## Primeras configuraciones

Lo primero que vamos a hacer es editar el archivo **_config.yml** modificando los siguientes apartados:

```yml
locale  : "es-ES" (Si eres de España)
title   : "Titulo de tu blog"
name    : "Tu nombre"
description : "Descripción del blog"
url     : "http://**usuario**.github.com"
```

Además se modificarán también los datos del autor, que aparecen en el apartado *# Site Author*. Ahí podemos definir nuestro nombre, biografía, localización, email, twitter, facebook, imagen de perfil, etc... Los campos que no queramos añadir simplemente se dejan vacíos. 

Para añadir la imagen de perfil se escribirá el nombre y extensión de la imagen, por ejemplo:

```yml
avatar  : "perfil.jpg"
```

Finalmente subiremos la imagen a la carpeta *images*.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-07.jpg "Jekyll")

¡Es importante que trás editar *_config.yml* ó subir una imagen, se haga un *Commit changes* para guardar los cambios, sino no se actualizará! Para ello en la parte inferior disponemos del siguiente botón.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-08.jpg "Jekyll")

## Escribir posts

Una vez que ya tienes el blog funcionando, ya puedes comenzar a escribir. En la parte superior seleccionamos *'Create new file'*.

![Jekyll]({{ site.baseurl }}/images/blogJekyll-09.jpg "Jekyll")

Los *posts* deben de añadirse a la carpeta ____posts__, aunque en este diseño no viene creada por defecto. Entonces la primera vez que creamos un archivo, crearemos además la carpeta añadiendo ____posts/archivo__ (El nombre del archivo debe ser **_año-mes-dia-nombre.md_**).

![Jekyll]({{ site.baseurl }}/images/blogJekyll-11.jpg "Jekyll")

Aunque ya tengas creado tu primer post, no es posible visualizarlo (todavía). Tal y como viene en la [documentación](https://mmistakes.github.io/minimal-mistakes/docs/posts/), editaremos el archivo **_config.yml** añadiendo al final:

```yml
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
```

Ya es posible visualizar los *posts*, y para añadirles categorías y tags se añadirá al principio del post lo siguiente:

```yml
---
tags:
- uno
- dos
categories:
- tres
---
```

Se pueden añadir tantos tags y categorías como queramos, aunque lo normal es asignar una categoría y varios tags a cada entrada. Finalmente [aquí](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) puedes ver como se escriben los *posts*.

Ejemplo:

![Jekyll]({{ site.baseurl }}/images/blogJekyll-12.jpg "Jekyll")

En el siguiente post veremos como listar categorías, tags y como añadir un sistema de comentarios. 

[Crear un blog con GitHub y Jekyll (parte 2)](http://www.josemanuelvazquez.es/github/Crear-Blog-Github-Jekyll-2/)
