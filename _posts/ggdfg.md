---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: El enrutamiento en Symfony 3.
---

En este post se indicará como crear, o eliminar un bundle, así como sus controladores.

## Rutas

Al generar un bundle, se añade automáticamente en 'app/config/routing.yml' una ruta hacía ese nuevo bundle. Aunque hay que configurar las rutas a los controladores. Supongamos lo siguiente:

**AppBundle / DefaultController / loginAction**

Para añadirle una ruta simplemente accedemos a *AppBundle/Resources/config/routing.yml* y se añade:

