---
tags:
- PHP
- Scraping
categories:
- PHP
title: Scraping con PHP
---

Web scraping es el proceso de recopilar información de forma automática de la Web. Aunque esxisten varias formas de realizarlo, en este artículo haremos uso de [Simple HTML DOM Parser](http://simplehtmldom.sourceforge.net/).

## Simple HTML DOM Parser

Lo primero es descargar el archivo desde [aquí](http://simplehtmldom.sourceforge.net/). Como ejemplo vamos a crear un archivo que muestre los titulos y la URL de los posts de este blog. Para obtener esto únicamente hay que mirar el código fuente del sitio (click derecho -> Inspect).

![Scraping]({{ site.baseurl }}/images/scraping-01.jpg "Scraping")

Como puede verse todos los artículos se encuentran dentro de un *<div class="list__item"></div>* por lo que habrá que recorrer los div (con un foreach) imprimiento el título y la URL. 

```php
<?php

//Archivo descargado
require	'simple_html_dom.php';

//Web sobre la que se va a aplicar
$html = file_get_html('http://www.josemanuelvazquez.es');

//Recorremos todos los div class="list__item"
foreach( $html->find('div[class=list__item]') as $post )
{
	//Obtiene el 1º <a ..></a> dentro del div
	$link = $post->find('a',0);
	//Obtiene el href 
	$url = $link->attr['href'];
	//Obtiene el 1º h2, dentro de la etiqueta <a>, y obtiene su contenido
	$title = $link->find('h2',0)->innertext;
	//Se muestra el título y su enlace.
	echo $title.'</br>'.'<a href="'.$url.'">'.$url.'</a></br></br>';
}

```


