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

![img01]({{ site.baseurl }}/images/pythonDropbox01.jpg "img01")

Una vez creado ya podemos generar el token, que necesitaremos más adelante.

![img02]({{ site.baseurl }}/images/pythonDropbox02.jpg "img02")

## Python

```python
import dropbox
import tempfile

#Autenticación
token = "INTRODUCIR TOKEN GENERADO"
dbx = dropbox.Dropbox(token)

#Obtiene y muestra la información del usuario
user = dbx.users_get_current_account()
print(user)

#Sube archivo
with open("ruta/nombre.txt", "rb") as f:
   dbx.files_upload(f.read(), '/nombre.txt', mute = True)

#Descarga archivo
dbx.files_download_to_file("ruta/nombre.txt", '/nombre.txt')
```

