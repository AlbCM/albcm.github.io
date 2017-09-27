---
layout: post
title:  "Procesos e hilos en los sistemas operativos "
date:   2017-09-27 22:19:13
categories: Ensayos
---

### Título: Procesos e hilos en los sistemas operativos.
### Autor: Albeiro José Cuadrado Machado

En el mundo en que vivimos, iniciamos actividades por costumbres o previamente planeadas, finalizamos actividades, las suspendemos, retomamos y en general  esto lo hacemos para satisfacer nuestras necesidades o lograr nuestros objetivos es decir, somos seres dinámicos.
Por ejemplo al ir de viaje existen varias tareas por debajo para cumplir nuestro objetivo general entre ellas pueden estar, llenar de combustible nuestro auto, comprar víveres, revisar nuestro equipaje etc.

En el mundo de los sistemas operativos a estos conceptos de actividades se les conoce como procesos y puede que sea el concepto más importante en cualquier sistema operativo y está definido como una abstracción de un programa en ejecución, estos procesos son tratados por los ordenadores a petición del usuario o por causa de otro proceso.  Tienen como propósito básico completar las tareas que nosotros les indiquemos y arrojarnos los resultados casi siempre esperados, sin este concepto simplemente la computación que hoy día conocemos no sería posible.

Siguiendo con el ejemplo de ir de viajes el proceso padre sería ir de viajes y los subprocesos o hilos del proceso padre serían las actividades relacionadas con abastecer el vehículo, comprar víveres y alistar el equipaje. Es decir son derivaciones del proceso padre. Estas derivaciones se pueden realizar en paralelo a las otras por ejemplo podemos dejar nuestro vehículo abasteciendo mientras realizamos otra actividad como ir fijando nuestra ruta en nuestro teléfono móvil. Estos conceptos existen en el mundo de los Sistemas operativos como hilos de ejecución y multihilos el primero permite en los sistemas compartir los recursos de un proceso y realizar varias actividades en paralelo y el último es la capacidad que tiene el sistema operativo para mantener varios hilos de ejecución dentro de un mismo proceso. 

Los hilos pueden ser de dos tipos Hilos de kernel o Hilos de usuario en los hilos a nivel de usuario se usan bibliotecas para la creación, destrucción y planificación de hilos. En los hilos a nivel de kernel el sistema operativo conoce la existencia de los hilos a diferencia de los hilos de usuarios. Recordemos que el kernel es la parte principal del código del sistema operativo y se encarga de controlar y administrar los servicios y peticiones de recursos de unos o varios procesos. Los hilos pueden estar básicamente en tres estados Listo, Ejecutándose y Bloqueado.

Por otra parte las actividades principales de nuestra viaje pueden pasar por diversos estados por ejemplo pausar nuestra revisión de equipaje para responder una llamada o simplemente aplazar nuestro abastecimiento de combustible para cuando vayamos a mitad de camino. En los sistemas operativos sucede lo mismo los procesos pueden pasar por distintos estados y existen distintos modelos para ellos como por ejemplo:

El modelo de dos estados que es el más simple, un proceso puede estar ejecutándose o no, cuando se crea  un nuevo proceso, se pone en estado no ejecución y en algún momento el que está ejecutándose, se pone en no ejecución y otro proceso se elegirá de la lista de procesos listos y se pondrá en ejecución .

Existe también el modelo de más de 2 estados en este se agregan los estados Nuevo, listo, Bloqueado y Terminado al anterior modelo. Los procesos que están en el estado Nuevo son procesos que recién fueron creados,no han sido admitidos por el sistema operativo y  todavía no se encuentran en la memoria principal. Los procesos en estado Listo estan esperando que el planificador lo disponga para moverlos de estado. En el estado Bloqueado están aquellos procesos que no se pueden ejecutar hasta que no se produzca cierto suceso, como una operación de Entrada/Salida. Los procesos en estado Ejecución como su nombre lo indica se encuentran en el momento en ejecución y por último el estado Terminado indica que el proceso fue expulsado por el grupo de procesos ya sea por conclusión del mismo o por errores.

Como anteriormente mencione los procesos son tratados por los ordenadores a petición del usuario o por causa de otro proceso, para este último los procesos necesitan mecanismos de comunicación que pueden ser Síncrona en este caso el proceso que envía permanece bloqueado esperando la respuesta del receptor, Asíncrona el proceso envía el mensaje y continúa con sus actividades sin bloquearse esperando respuesta del receptor , Persistente en este tipo el receptor no tiene que estar operativo en el momento en el que se envía el mensaje este se almacena por tanto tiempo sea necesario para su entrega, Momentánea el mensaje se descarta si el receptor no está activo al momento que se envía el mensaje, directas en esta las primitivas enviar y recibir explicitan el nombre del proceso con el que se comunica, es decir se especifica cuál va a ser el proceso emisor y cuál va a ser el proceso receptor, indirectas la comunicación está basada en una herramienta o instrumento ya que el emisor y el receptor están a distancia , Simétrica todos los procesos pueden enviar o recibir llamada también bidireccional y Asimetrica un proceso puede enviar y los otros procesos sólo reciben, también llamada comunicación unidireccional.

En conclusión podemos observar que cada proceso que realiza nuestro sistema operativo es semejante a los procesos que se lleva a cabo en la vida cotidiana de un ser humano, cada proceso necesita una serie de recursos que es brindado por el kernel el cual es secuencial para ser atendido, ocupa un espacio de memorias que contiene el programa ejecutable, sus datos, su pila y un conjunto de registros asociados al proceso.



