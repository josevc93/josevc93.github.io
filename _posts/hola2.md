---
tags:
- GitHub
- Jekyll
categories:
- GitHub
title: Crear un blog con GitHub y Jekyll (parte 2)
---

Hemos visto en el post (anterior) <--ENLACE como crear un blog con Jekyll y Github y como configurar algunos aspectos. En este post se continuará configurando el blog.

## Tags y categorías

Hasta el momento, ya hemos visto como crear entradas en el blog, aunque al pulsar en alguna tag o categoría veremos que no dirige a ninguna parte. Para solucionarlo, se crearan dos archivos dentro de la carpeta **_pages**.

* [category-archive.html](https://github.com/mmistakes/minimal-mistakes/blob/gh-pages/_pages/category-archive.html)
* [tag-archive.html](https://github.com/mmistakes/minimal-mistakes/blob/gh-pages/_pages/tag-archive.html)

Con esto ya funcionará, y además podemos listar los tags y categorías en el menu editando el archivo **/_data/navigation.yml** de la siguiente manera:

```yml
main:
  - title: "Categorías"
    url: /categories/
  - title: "Tags"
    url: /tags/


```

## Añadir DISQUS a los posts

Podemos añadir un sistema de comentarios a nuestros posts de manera sencillo. Lo primero es crear una cuenta en [Disqus](https://disqus.com/). 
