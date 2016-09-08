---
layout: post
title:  "Comprobación de redundancia ciclica (CRC)"
date:   2016-09-07 22:19:13
categories: Programacion
---

La Comprobación de redundancia ciclica (CRC), es una técnica de detección de errores usada frecuentemente en redes digitales y en dispositivos de almacenamiento para detectar cambios accidentales en los datos. Los bloques de datos ingresados en estos sistemas contiene un valor de verificación adjunto, basado en el residuo de una división de polinomios, por eso también se conoce con el nombre de **códigos polinómicos**.

Los códigos CRC funcionan esta forma:

Consideremos una secuencia de datos de **n** bits que llamaremos **D**, que el nodo *Emisor* desea enviar al nodo *Receptor*. El Emisor y el receptor tienen que acordar primero un patrón de *r + 1 bits* conocido como *Generador* que denominaremos con la letra **G**. 

Para un determinada secuencia de datos, **D**, el emisor seleccionará r bits adicionales, **R**, y se los añadirá a **D**, de modo que el patrón de **d + rbits resultante (interpretado como un número binario)** sea exactamente divisible por **G** (es decir, no tenga ningún resto) utilizando aritmética módulo 2. El proceso de comprobación de errores con los códigos CRC es, por tanto, muy simple: el receptor divide los **d + rbits** recibidos entre **G**. Si el resto es distinto de cero, el receptor sabrá que se ha producido error; en caso contrario, se aceptarán los datos como correctos.

Tal ves parezca un poco confuso pero trataremos de ilustrar mejor el metodo con un ejemplo, Asi:

{% highlight ruby  %}
D = 111100101
G =  1      0      1      1      0      1
    x^5           x^3    x^2           x^0    

// Entonces podemos escribir G como un polinomio asi: x^5 +x^3 + x^2 + x^0
// PD: Vease que no se tiene en cuenta los bits en 0 ya que al evaluar será 0

gr_polinomio = 5 // Este es el grado del polinomio del generador


{% endhighlight %}

**EL PROCEDIMIENTO PARA CALCULAR EL CRC ES EL SIGUIENTE**

1- Tomamos la secuncia de datos **D** y le agregamos tantos 'ceros' nos diga el grado del polinomio, esto equivale al tamaño de nuestro **CRC** y que remplazaremos posteriormente para que sea enviado al receptor. Para nuestro ejemplo el grado del polinomio generador es 5 por lo tanto tendremos lo siguiente que denotaremos como **D'** =  D + 00000, entonces:

{% highlight ruby  %}

D' = 111100101   00000

{% endhighlight %}

2 - Hacemos los cálculos de los códigos CRC, que se realizan en aritmética módulo 2, es decir sin ningun tipo de acarreo ni en las sumas ni en las restas. Esto quiere decir que la suma y la resta son idénticas, y que ambas son equivalentes a la operación OR-exclusiva (XOR) bit a bit de los operandos, por ejemplo:

{% highlight ruby  %}
      1011           1001  
      0101           1101
XOR   ----     XOR   ----
      1110           0100

{% endhighlight %}

Regresando a nuestro ejemplo lo que haremos es una division usando la operacion *OR-Exclusiva* de nuestro **D'** con **G** para obtener el **CRC** asi: 

{% highlight ruby  %}

11110010100000     | 101101 
101101||||||||      11011001    // Cociente
------||||||||
0100011|||||||
 101101|||||||
 ------|||||||
 00111001|||||
   101101|||||
   ------|||||
   0101000||||
    101101||||
    ------||||
    000101000|
       101101|
       ------|
       0001010       // Residuo o resto
{% endhighlight %}
