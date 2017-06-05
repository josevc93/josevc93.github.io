---
tags:
- Dropbox
- Python
categories:
- Python
title: Subir y descargar archivos de Dropbox con Python
---

En este artículo se enseñara a subir y descargar archivos utilizando [Dropbox](https://www.dropbox.com/). 

## Crear una aplicación de Dropbox y generar token de autenticación

En primer lugar accedemos a este [enlace](https://www.dropbox.com/developers/apps/create) y rellenamos los datos como se muestra en la siguiente imagen:

FOTO1

Una vez creado ya podemos generar el token, que necesitaremos más adelante.

FOTO2

## Python

```python
import dropbox
import tempfile

#Autenticación
token = "Lj_p_9vyaP8AAAAAAAAN4hgZ0OkBTXBucFGfpyV3S-VjtwR42GnM2OzXbs086yDt"
dbx = dropbox.Dropbox(token)

#Obtiene y muestra la información del usuario
user = dbx.users_get_current_account()
print(user)

#Sube archivo
with open("/home/jose/Escritorio/practicaSD/muestra.txt", "rb") as f:
   dbx.files_upload(f.read(), '/andres.txt', mute = True)

#Descarga archivo
dbx.files_download_to_file("/home/jose/Escritorio/practicaSD/jose.txt", '/andres.txt')
```

![CNAME]({{ site.baseurl }}/images/configurar-dominio-04.jpg "CNAME")

## Propagación de DNS

Aunque ya está todo configurado aún no es posible acceder al nuevo dominio. Esto se debe a que cuando se produce un cambio en un dominio, esta nueva información debe propagarse a todos los servidores DNS del planeta. Suele tardar entre **24-48 horas**. ¿Por qué? Hay que esperar que expire la entrada en caché (el TTL generalmente tiene un valor de 48 horas) para que, cuando esos servidores reciban una consulta de un cliente, se vea obligados a contactar con el servidor DNS autoritario. El servidor DNS autoritario responderá con los datos actualizados.

Podemos comprobar como va la propagación de DNS en la siguiente página:

<https://www.whatsmydns.net/>



