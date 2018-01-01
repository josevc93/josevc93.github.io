---
tags:
- Symfony
- Angular
- Producción
categories:
- Producción
title: Subir aplicación con Symfony3 y Angular a un hosting compartido.
---

Subir a producción nuestro proyecto con Symfony3 y Angular (utilizando [Angular CLI](https://cli.angular.io/)) es muy sencillo utilizando un hosting compartido.

## Symfony3

Únicamente es necesario eliminar los directorios *var/cache* y *var/logs*.

## Angular

Hay que ejecutar el comando *ng build - -prod*. Esto generará un directorio */dist* con el contenido que hay que subir al hosting.

## Subir archivos y .htaccess

Hay que subir todos los ficheros de Symfony, y el directorio */dist* de Angular al hosting. Para ello una buena opción es utilizar [Filezilla](https://filezilla-project.org/).

Este es el contenido del directorio /public_html en el hosting que he contratado:

![ArchivosAngular]({{ site.baseurl }}/images/hostingComp01.png "Archivos Angular")

Estos archivos son los generados por Angular, y además se incluye un directorio /api con los archivos de Symfony:

![ArchivosSymfony]({{ site.baseurl }}/images/hostingComp02.png "Archivos Symfony")

Una vez que está todo subido es importante añadir un archivo **.htaccess** para evitar errores en la aplicación. En caso de no añadirlo la aplicación no cargaría al recargar la página.

```yml
 RewriteEngine On 
 RewriteCond %{SERVER_PORT} 80 
 RewriteRule ^(.*)$ RUTA/$1 [R,L] 

 Options -MultiViews
 #RewriteEngine On
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteRule ^ index.html [QSA,L]
```

**¡Importante!** Cambiar *RUTA* por la dirección de tu web.

Con esto ya tenemos la aplicación funcionando correctamente. 
