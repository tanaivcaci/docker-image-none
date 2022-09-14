# docker-image-none
## Una interesante experiencia con las imagenes &lt;none>

> Aviso importante, estoy tratando de solucionar inconvenientes que tengo para levantar microservicios mientras estoy aprendiendo. Lo que comparto a continuación es mi experiencia, algunas búsquedas de tutoriales y nada más. Seguro esto es mejorable, tengo mucho que aprender y agradezco los aportes. 



Encontré muchas imagenes de docker con la etiqueta <none> que me estaban ocupando mucho espacio. Las ví con éste comando:

`docker image ls -a`

> ![imagen](https://user-images.githubusercontent.com/82984433/190110118-034aa56c-3a7d-4670-9d6b-bf082674ea93.png)



Muestra ordenadas todas las imagenes con su respectivo peso y etiquetas. 


El tema es que no funciona el comando `docker rmi [image id]` para eliminar las imágenes que tienen etiqueta <none> 

Aparentemente las imágenes <none> por algún motivo no se terminaron de construir y por eso quedan así. 

El problema es que no sólo son imágenes fallidas, además, algunas son de tipo capa (o base) y no se pueden borrar porque suelen tener dependencias descendentes. Efectivamente el error que tenía al intentar eliminarlas era: 

`Error response from daemon: conflict: unable to delete 7e0868d9abfa (cannot be forced) - image has dependent child images`

Con `docker images -f dangling=true` podemos ver todas las imágenes que se quedaron a mitad de camino en su construcción pero no muestra las que son de tipo capa. 

En el video que sigue trato de eliminar las imágenes <none> y lo logro sólo con aquellas que no son de tipo capa: 
[un intento](https://youtu.be/OW6w9Dn7Sz0)

En éste otro video logro eliminar las imágenes <none> de tipo capa pero no se si hacer ésto es recomendable o no. Yo lo hice porque básicamente no tenía nada que perder borrándolas. 
[solución que encontré](https://youtu.be/224JdSTrd-M)


## Para poder eliminar imagenes que tienen hijos dependientes

Este es el error que obtenía: 
Error response from daemon: conflict: unable to delete 7e0868d9abfa (cannot be forced) - image has dependent child images

Este es el comando que usé: 
`docker [ rmi | images rm ] [image ID]`


En image ID referencie la imagen que tenía TAG latest, en éste caso es el repo api-gateway. 
Cuando se elimina también se borran las imágenes <none> de las que dependía.

En la imagen se ve el resultado de `docker image ls -a` y luego el resultado de `docker rmi <imageId>` de api-gateway. 


![imagen](https://user-images.githubusercontent.com/82984433/190112851-645a8037-e945-4889-a6b3-c792406549b6.png)

En ésta imagen muestro la salida de `docker image ls -a` donde ya no se ve api-gateway ni las imágenes <none> con las que aparentemente había una dependencia. 

![imagen](https://user-images.githubusercontent.com/82984433/190113590-b21deeea-b650-4e95-880a-3e5592938eb0.png)



## Links consultados: 

[stackoverflow - obvio ](https://stackoverflow.com/questions/38118791/can-t-delete-docker-image-with-dependent-child-images)

[eduardoaf.com](https://eduardoaf.com/blog/docker/docker-eliminar-imagenes-del-tipo-none-106#:~:text=%C2%BFQu%C3%A9%20es%20una%20imagen%20%22None,se%20ha%20abortado%20la%20operaci%C3%B3n.)


### Final feliz, logré levantar los microservicios
![imagen](https://user-images.githubusercontent.com/82984433/190114841-ce5b96a6-0725-4e63-95cd-7b24f897cba6.png)
