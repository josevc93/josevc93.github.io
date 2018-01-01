---
tags:
- GitHub
- Jekyll
categories:
- GitHub
title: Configurar un dominio con GitHub y Jekyll 
---

Subir a producción nuestro proyecto con Symfony3 y Angular (utilizando [Angular CLI](https://cli.angular.io/)) es muy sencillo utilizando un hosting compartido.

Es muy sencillo configurar nuestro propio dominio en GitHub, para obtener *www.miDominio.com* en lugar de *username.github.io*.

## Symfony

Únicamente es necesario eliminar los directorios *var/cache* y *var/logs*.

## Angular

Hay que ejecutar el comando *ng build --prod*. Esto generará un directorio */dist* con el contenido que hay que subir al hosting.

## Subir archivos y .htaccess

Hay que subir todos los ficheros de Symfony, y el directorio */dist* de Angular al hosting. Para ello una buena opción es utilizar [Filezilla](https://filezilla-project.org/).

Este es el contenido del directorio /public_html en el hosting que he contratado:



Una vez comprado se te enviará un correo con tu ID de cliente (puede tardar unos minutos). Con esto puedes acceder al área de clientes, aunque también puedes acceder introduciendo tu dominio.

![Acceso 1&1]({{ site.baseurl }}/images/configurar-dominio-01.jpg "Acceso 1&1")

Ya es posible editar nuestro dominio, para ello accede a *Mis Productos -> Dominios -> Gestionar Dominios*. Es posible que no te permita configurarlo, en ese caso únicamente debes esperar hasta que sea posible (tarda unos minutos). Seguidamente creamos un subdominio (recomiendo *www* o *blog*).

![Crear subdominio]({{ site.baseurl }}/images/configurar-dominio-02.jpg "Crear subdominio")

Una vez creado seleccionamos *Administrar Subdominios -> Modificar configuración DNS*. Y añadimos la dirección de GitHub en el apartado *CNAME*.

![DNS Subdominio]({{ site.baseurl }}/images/configurar-dominio-03.jpg "Crear subdominio")

Esto es todo lo que debemos de configurar en 1&1.

## Archivo CNAME

Dentro de GitHub debemos crear un archivo CNAME en el cuál únicamente hay que incluir el dominio que acabamos de crear.

![CNAME]({{ site.baseurl }}/images/configurar-dominio-04.jpg "CNAME")

## Propagación de DNS

Aunque ya está todo configurado aún no es posible acceder al nuevo dominio. Esto se debe a que cuando se produce un cambio en un dominio, esta nueva información debe propagarse a todos los servidores DNS del planeta. Suele tardar entre **24-48 horas**. ¿Por qué? Hay que esperar que expire la entrada en caché (el TTL generalmente tiene un valor de 48 horas) para que, cuando esos servidores reciban una consulta de un cliente, se vea obligados a contactar con el servidor DNS autoritario. El servidor DNS autoritario responderá con los datos actualizados.

Podemos comprobar como va la propagación de DNS en la siguiente página:

<https://www.whatsmydns.net/>



