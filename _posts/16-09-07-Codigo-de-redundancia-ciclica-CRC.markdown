---
layout: post
title:  "Comprobación de redundancia ciclica (CRC)"
date:   2016-09-07 22:19:13
categories: Programacion
---

La Comprobación de redundancia ciclica (CRC), es una técnica de detección de errores usada frecuentemente en redes digitales y en dispositivos de almacenamiento para detectar cambios accidentales en los datos. Los bloques de datos ingresados en estos sistemas contiene un valor de verificación adjunto, basado en el residuo de una división de polinomios, por eso también se conoce con el nombre de **códigos polinómicos**.

Los códigos CRC funcionan esta forma:
Consideremos una secuencia de datos de **n** bits que llamaremos **D** 

Asi por ejemplo:

{% highlight text %}
D= 111100101
{% endhighlight %}
