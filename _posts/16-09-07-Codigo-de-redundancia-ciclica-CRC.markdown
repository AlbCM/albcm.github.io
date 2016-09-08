---
layout: post
title:  "Comprobación de redundancia ciclica (CRC)"
date:   2016-09-07 22:19:13
categories: Programacion
---

La Comprobación de redundancia ciclica (CRC), es una técnica de detección de errores usada frecuentemente en redes digitales y en dispositivos de almacenamiento para detectar cambios accidentales en los datos. Los bloques de datos ingresados en estos sistemas contiene un valor de verificación adjunto, basado en el residuo de una división de polinomios, por eso también se conoce con el nombre de **códigos polinómicos**.

# ¿Como funciona?

Consideremos una secuencia de datos de **n** bits que llamaremos **D**, que el nodo *Emisor* desea enviar al nodo *Receptor*. El Emisor y el receptor tienen que acordar primero un patrón de *r + 1 bits* conocido como *Generador* que denominaremos con la letra **G**. 

Para un determinada secuencia de datos, **D**, el emisor seleccionará r bits adicionales, **R**, y se los añadirá a **D**, de modo que el patrón de **d + rbits resultante (interpretado como un número binario)** sea exactamente divisible por **G** (es decir, no tenga ningún resto) utilizando aritmética módulo 2. El proceso de comprobación de errores con los códigos CRC es, por tanto, muy simple: el receptor divide los **d + rbits** recibidos entre **G**. Si el resto es distinto de cero, el receptor sabrá que se ha producido error; en caso contrario, se aceptarán los datos como correctos.

# Ejemplo 
Tal vez parezca un poco confuso pero trataremos de ilustrar mejor el metodo con un ejemplo, Asi:

*Nos piden que Consideremos una secuencia de datos  **D = 111100101**, que un nodo Emisor desea enviar al nodo Receptor y cuyo  Generador es **G = 10101**. Calcule el CRC y compruebe en el lado del receptor la secuencia de datos completa* 

{% highlight ruby  %}
D = 111100101
G =  1      0      1      1      0      1
    x^5           x^3    x^2           x^0    

// Entonces podemos escribir G como un polinomio asi: x^5 +x^3 + x^2 + x^0
// PD: Vease que no se tiene en cuenta los bits en 0 ya que al evaluar será 0

gr_polinomio = 5 // Este es el grado del polinomio del generador


{% endhighlight %}

## Procedimiento para calcular CRC

1- Tomamos la secuncia de datos **D** y le agregamos tantos 'ceros' nos diga el grado del polinomio, esto equivale al tamaño de nuestro **CRC** y que remplazaremos posteriormente para que sea enviado al receptor. Para nuestro ejemplo el grado del polinomio generador es 5 por lo tanto tendremos lo siguiente que denotaremos como **D'** =  D + 00000, entonces:

{% highlight ruby  %}

D' = 111100101   00000

{% endhighlight %}

2- Hacemos los cálculos de los códigos CRC, que se realizan en aritmética módulo 2, es decir sin ningun tipo de acarreo ni en las sumas ni en las restas. Esto quiere decir que la suma y la resta son idénticas, y que ambas son equivalentes a la operación OR-exclusiva (XOR) bit a bit de los operandos, por ejemplo:

{% highlight ruby  %}
      1011           1001  
      0101           1101
XOR   ----     XOR   ----
      1110           0100

{% endhighlight %}

Regresando a nuestro ejemplo lo que haremos es una division usando la operacion *OR-Exclusiva* de nuestro **D'** con **G** para obtener el **CRC** asi: 

{% highlight ruby  %}

11110010100000     | 101101 
101101||||||||      110110010    
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
       0001010       
       
{% endhighlight %}

El resto o residuo de nuestra operación es *00**01010*** y este es el dato que nos interesa tomaremos los 5 ultimos bits de la operación ya que **5** es el grado de nuestro polinomio generador y este sera nuestro **CRC** asi para nuestro ejercicio:

{% highlight ruby  %}
CRC = 01010    
{% endhighlight %}

3. Con nuestro CRC calculado entonces enviariamos el mensaje **M** resultante a nuestro *Receptor* de esta forma **D**+**CRC** asi:
{% highlight ruby  %}
M = D + CRC = 111100101 01010
{% endhighlight %}

4. Como nuetro *Receptor* habia negociado el patron conocido como *Generador* es decir tiene a disposición  debe realizar la división OR-Exclusiva de **M** y **G** y si es exactamente divisible por **G** ( Es decir no tiene ningún resto) entonces el *Receptor* aceptará los datos como correctos; en coso contrario( el resto es ditinto de cero) el *Receptor* sabrá que se ha producido un error y se concluye la comprobación. Para nuestro ejemplo entonces:

{% highlight ruby  %}
11110010101010     | 101101 
101101||||||||       110110010
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
    000101101|
       101101|
       ------|
       0000000
 
{% endhighlight %}


