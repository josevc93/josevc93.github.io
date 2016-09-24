---
tags:
- Desarrollo web
- Programación web
- Framework
- Linux
categories:
- Symfony
title: Generar formularios en Symfony 3 (parte 2).
---

Continuando con el [post anterior] sobre formularios vamos a ver como procesar los datos que nos llegan (...)


## Controlador

### Almacenar los datos

    public function addAction(Request $request){
        $film = new Film();
        $form = $this->createForm(FilmType::class, $film);

        $form->handleRequest($request);

        if($form->isSubmitted()){ //Si el formulario se ha enviado
            if($form->isValid()){ //Comprobar esto
                
                $film = new Film(); //Objeto Film, que se va a añadir

                $film->setTitle($form->get("title")->getData()); //Almacenamos los datos que se reciben
                $film->setDescription($form->get("description")->getData());
                
                //Subir y almacenar la imagen
               // $film->setImage(null); En principio
               /* $file=$form["image"]->getData();
                $ext=$file->guessExtension();
                $file_name=time().".".$ext;
                $file->move("uploads",$file_name);

                $film->setImage($file_name);*/

                //IMAGEN NO REQUERIDA
                if(!empty($file) && $file!=null){
                    $ext=$file->guessExtension();
                    $file_name=time().".".$ext;
                    $file->move("uploads",$file_name);

                    $film->setImage($file_name);      
                }else{
                    $film->setImage(null);
                }          

                
                //Necesario, ya que el formulario obtiene el nombre de la categoría pero queremos sacar su ID para almacenarlo
                $em = $this->getDoctrine()->getEntityManager(); 
                $category_repo=$em->getRepository("FilmBundle:Category");  
                $category = $category_repo->find($form->get("category")->getData());
                $film->setCategory($category);

                /*Si tuviera un campo usuario, se podría sacar y almacenar así. Obteniendo el usuario logeado en la sesión.
                $user=$this->getUser();
                $film->setUser($user);*/

                $em->persist($film); //Se añade a la base de datos
                $flush=$em->flush();

                if($flush==null){
                    $status = "La película se ha añadido correctamente.";
                }else{
                    $status = "Error al añadir la película.";    
                }
            }else{
                $status = "La película no se ha creado";
            }
            $this->session->getFlashBag()->add("status", $status);
         //  return $this->redirectToRoute("blog_homepage");
        }

        return $this->render("FilmBundle:Film:add.html.twig",array(
            'form' => $form->createView()
        )); 
    }

}
