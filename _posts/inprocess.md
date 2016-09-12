---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar entidades a partir de la base de datos en Symfony 3.
---
En este artículo se aprenderá como generar entidades a partir de la base de datos utilizando [Symfony](http://symfony.com/) y se aprenderá como configurar las relaciones **ManyToOne**, **OneToMany** y **ManyToMany**. Para las dos primeras relaciones se utilizará como ejemplo el siguiente diagrama:

![Symfony]({{ site.baseurl }}/images/entidadesbd01.png "Symfony")

*(El diagrama se ha generado con [DIA](http://dia-installer.de/index.html.es))*

## Generando entidades

``` shell
$ php bin/console doctrine:mapping:convert xml ./src/NombreBundle/Resources/config/doctrine/metadata/orm --from-database --force
$ php bin/console doctrine:mapping:import NombreBundle yml
$ php bin/console doctrine:generate:entities NombreBundle
```
*(Hay que sustituir NombreBundle por el nombre de nuestro Bundle)*

## Relaciones ManyToOne

En este ejemplo tenemos una relación ManyToOne entre Empleado y Departamento, dado que muchos empleados pertenecen a un departamento. Este tipo de relaciones se genera automáticamente y permite por ejemplo mostrar el nombre y apellido de cada empleado junto con el nombre del departamento al que pertenece de manera sencilla. Veamos como se hace:

``` php
$em = $this->getDoctrine()->getEntityManager();
$empleado_repo = $em->getRepository("EmpresaBundle:Empleado");
$empleados = $empleado_repo->findAll();

foreach($empleados as $empleado)
  echo $empleado->getName()." ".$empleado->getSurname()." ".$empleado->getDepartamento()->getName()."<br/>";
```

## Relaciones OneToMany

En el ejemplo en el que nos encontramos, existe una relación OneToMany entre Departamento y Empleado, dado que un departamento tiene asignados a muchos empleados. Esto permite mostrar de forma sencilla, todos los empleados asignados a cada departamento. Sin enmbargo, para que este tipo de relación funcione hay que realizar algunas configuraciones.

Dentro de *Departamento.orm.yml* hay que añadir:

``` yml
OneToMany:
  empleado:
    targetEntity: Empleado
    mappedBy: departamento
    cascade: ["persist"]
```

Dentro de *empleado.orm.yml* hay que modificar:

``` yml
ManyToOne:
 departamento:
    ...
    inversedBy: empleado
```

*(Se modifica inversedBy, que se encontraba a null)*

Por último se modifica *Departamento.php* añadiendo:

``` php
  protected $empleado;
  
  public function _construct(){
    $this->entry = new \Doctrine\Common\Collections\ArrayCollection();
  }
  
  public function getEmpleados(){
    return $this->entry;
  }
```

Con estas modificaciones realizadas, ya podemos probar si todo funciona correctamente. Para ello, vamos a mostrar el nombre de cada departamento y la lista de empleados asignados a el.

``` php
$em = $this->getDoctrine()->getEntityManager();
$departamento_repo = $em->getRepository("EmpresaBundle:Departamento");
$departamentos = $departamento_repo->findAll();

foreach($departamentos as $departamento){
  echo "<b>.$departamento->getName()."</b><br/>";
  
  $empleados = $departamento->getEmpleados();
  foreach($empleados as $empleado)
    echo $empleado->getName()." ".$empleado->getSurname()."<br/>";
}
```

## ManyToMany

Dado que las relaciones ManyToMany no son recomendables, debemos tratar de evitarlas utilizando tablas intermedias en las que se utilizan relaciones OneToMany. Ejemplo:

![Symfony]({{ site.baseurl }}/images/entidadesbd02.png "Symfony")

En este ejemplo tenemos una relación OneToMany entre Class y TeacherClass, y otra relación OneToMany entre Teacher y TeacherClass. Se configura, por lo tanto, igual que el apartado OneToMany anterior. Veamos un ejemplo de como mostraríamos todos los profesores asignados a cada clase.

``` php
$em = $this->getDoctrine()->getEntityManager();
$clase_repo = $em->getRepository("FilmBundle:Class");
$clases = $clase_repo->findAll(); 

foreach ($clases as $clase){
  echo "<b>".$clase->getName()."</b><br/>";
  $teacherclass = $clase->getTeacherClass();  
  foreach($teacherclass as $tc)
    echo $tc->getTeacher()->getName()."<br/>";
}
```
