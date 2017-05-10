---
tags:
- Seguridad
- Web
categories:
- Seguridad
title: Seguridad en el desarrollo web (WebGoat)
---

En este artículo se va a hablar sobre [WebGoat](https://www.owasp.org/index.php/Category:OWASP_WebGoat_Project), que se trata de una aplicación deliberadamente insegura, para el aprendizaje de la seguridad web.

## WebGoat

El primer paso es descargar WebGoat desde este [enlace](https://github.com/WebGoat/WebGoat/releases). Una vez descargado se ejecutará mediante "java -jar nombre" y ya podemos acceder a la aplicacin desde nuestro navegador en la ruta http://localhost:8080/WebGoat/.

![WebGoat01]({{ site.baseurl }}/images/webgoat01.jpg "WebGoat")

Accedemos con el usuario 'webgoat' y la password 'webgoat' y podemos ver a la izquierda los distintos ataques que podemos realizar. Por ejemplo tenemos Inyección SQL, XSS, concurrencia, denegación de servicios, etc...

![WebGoat02]({{ site.baseurl }}/images/webgoat02.jpg "WebGoat")

Al seleccionar cualquiera de los ataques aparecerá un ejercicio a resolver, junto con su explicación. Como ejemplo vamos a realizar una inyección SQL:

![WebGoat03]({{ site.baseurl }}/images/webgoat03.jpg "WebGoat")

En este ejercicio se muestra un formulario que permite a un usuario ver el número de su tarjeta de crétido, pero mediante una inyección SQL se mostrarán los números de tarjetas de crédito de todos los usuarios.

![WebGoat04]({{ site.baseurl }}/images/webgoat04.jpg "WebGoat")

Para solucionarlo, hay que fijarse en la sentencia SQL que se va a ejecutar, por lo que si introducimos " Jose' OR '1'='1 "la consulta que finalmente se ejecuta es:

SELECT * FROM user_data WHERE last_name = 'Jose' OR '1'='1'

Por lo tanto, como 1 siempre es igual a 1, mostrará todos los usuarios.

![WebGoat05]({{ site.baseurl }}/images/webgoat05.jpg "WebGoat")

Este es solo un ejemplo de todos los que esta herramienta ofrece, en otros ejemplo se puede modificar nuestro propio salario, enviar una imagen con código malicioso, comprar a un precio menor del que deberíamos pagar, subir y ejecutar código malicioso en un campo de imagen, etc...

Espero que esta información te haya sido de utilidad, y que practiques con todos los ataques que esta herramienta te ofrece.
