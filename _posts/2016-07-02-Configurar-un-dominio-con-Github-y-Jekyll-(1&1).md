---
tags:
- Github
- Jekyll
categories:
- Github
title: Configurar un dominio con Github y Jekyll (1&1)
---

Es muy sencillo configurar nuestro propio dominio en Github, para obtener *www.miDominio.com* en lugar de *username.github.io*.

## Comprar un dominio

En primer lugar, es necesario comprar un dominio. Yo lo he comprado en [1&1](https://www.1and1.es/ "www.1and1.es"), ya que el precio no es muy elevado (0,99€ el primer año, y 9,99€ los siguientes).

Si te decides por esta web, únicamente debes de comprobar si el dominio que buscas está disponible e introducir tus datos.

## Configurar el dominio

Una vez comprado se te enviará un correo con tu ID de cliente (puede tardar unos minutos). Con esto puedes acceder al área de cliente para configurar tu dominio, aunque también puedes acceder introduciendo tu dominio.

![Acceso 1&1]({{ site.baseurl }}/images/configurar-dominio-01.jpg "Acceso 1&1")

Ya es posible editar nuestro dominio, para ello accede a *Mis Productos -> Dominios -> Gestionar Dominios*. Es posible que no te permita configurarlo, en ese caso únicamente debes esperar hasta que sea posible (tarda unos minutos). Seguidamente creamos un subdominio (recomiendo *www* o *blog*).

![Crear subdominio]({{ site.baseurl }}/images/configurar-dominio-02.jpg "Crear subdominio")

Una vez creado seleccionamos *Administrar Subdominios -> Modificar configuración DNS*. Y añadimos la dirección de GitHub en el apartado *CNAME*.

![DNS Subdominio]({{ site.baseurl }}/images/configurar-dominio-03.jpg "Crear subdominio")

Esto es todo lo que debemos de configurar en 1&1.

##Archivo CNAME

Dentro de GitHub debemos crear un archivo CNAME en el cuál únicamente hay que incluir el dominio que acabamos de crear.

![CNAME]({{ site.baseurl }}/images/configurar-dominio-04.jpg "CNAME")

##Propagación de DNS

Aunque ya está todo configurado aún no es posible acceder al nuevo dominio.



