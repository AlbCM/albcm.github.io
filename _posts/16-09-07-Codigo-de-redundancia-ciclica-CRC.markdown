---
layout: post
title:  "Comprobación de redundancia cíclica. (CRC)"
date:   2016-09-07 22:19:13
categories: Programacion
---

La Comprobación de redundancia cíclica(CRC), es una técnica de detección de errores usada frecuentemente en redes digitales y en dispositivos de almacenamiento para detectar cambios accidentales en los datos. Los bloques de datos ingresados en estos sistemas contiene un valor de verificación adjunto, basado en el residuo de una división de polinomios, por eso también se conoce con el nombre de **códigos polinómicos**.

# ¿Como funciona?

Consideremos una secuencia de datos de **n** bits que llamaremos **D**, que el nodo *Emisor* desea enviar al nodo *Receptor*. El Emisor y el receptor tienen que acordar primero un patrón de *r + 1 bits* conocido como *Generador* que denominaremos con la letra **G**. 

Para un determinada secuencia de datos, **D**, el emisor seleccionará r bits adicionales, **R**, y se los añadirá a **D**, de modo que el patrón de **d + rbits resultante (interpretado como un número binario)** sea exactamente divisible por **G** (es decir, no tenga ningún resto) utilizando aritmética módulo 2. El proceso de comprobación de errores con los códigos CRC es, por tanto, muy simple: el receptor divide los **d + rbits** recibidos entre **G**. Si el resto es distinto de cero, el receptor sabrá que se ha producido error; en caso contrario, se aceptarán los datos como correctos.

Tal vez parezca un poco confuso pero trataremos de ilustrar mejor el metodo con un ejemplo, Asi:

# Ejemplo 

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

**1-** Tomamos la secuencia de datos **D** y le agregamos tantos 'ceros' nos diga el grado del polinomio, esto equivale al tamaño de nuestro **CRC** y que reemplazaremos posteriormente para que sea enviado al receptor. Para nuestro ejemplo el grado del polinomio generador es 5 por lo tanto tendremos lo siguiente que denotaremos como **D'** =  D + 00000, entonces:

{% highlight ruby  %}

D' = 111100101   00000

{% endhighlight %}<br>

**2-** Hacemos los cálculos de los códigos CRC, que se realizan en aritmética módulo 2, es decir sin ningun tipo de acarreo ni en las sumas ni en las restas. Esto quiere decir que la suma y la resta son idénticas, y que ambas son equivalentes a la operación OR-exclusiva (XOR) bit a bit de los operandos, por ejemplo:

{% highlight ruby  %}
      1011           1001  
      0101           1101
XOR   ----     XOR   ----
      1110           0100

{% endhighlight %}<br>

Regresando a nuestro ejemplo lo que haremos es una division usando la operacion *OR-Exclusiva* de nuestro **D'** con **G** para obtener el **CRC** asi: 

{% highlight ruby  %}
 EMISOR

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
       
{% endhighlight %}<br>

El resto o residuo de nuestra operación es *00**01010*** y este es el dato que nos interesa tomaremos los 5 ultimos bits de la operación ya que **5** es el grado de nuestro polinomio generador y este sera nuestro **CRC** asi para nuestro ejercicio:

{% highlight ruby  %}
CRC = 01010    
{% endhighlight %}.

**3-** Con nuestro CRC calculado entonces enviaremos el mensaje **M** resultante a nuestro *Receptor* de esta forma **D**+**CRC** asi:
{% highlight ruby  %}
M = D + CRC = 111100101 01010
{% endhighlight %}<br>

**4-** Como nuestro *Receptor* había negociado el patron conocido como *Generador* y lo tiene a disposición  debe realizar la división OR-Exclusiva de **M** y **G** y si es exactamente divisible por **G** ( Es decir no tiene ningún resto) entonces el *Receptor* aceptará los datos como correctos; en caso contrario ( el resto es ditinto de cero) el *Receptor* sabrá que se ha producido un error. Para nuestro ejemplo entonces:

{% highlight ruby  %}
RECEPTOR

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
 
{% endhighlight %}<br>

**5-** Finalmente el resto o residuo de nuestra operación es **0000000** esto quiere decir que **M** Es exactamente divisible por **G** Entonces el *Receptor* puede concluir que los datos no contienen ningún error y concluye la comprobación de redundancia cíclica.

# Implementación CRC en Python
La comprobación de redundancia cíclica se realiza en el hardware de las tarjetas de red entre dos nodos.   

Con el fin de entender el funcionamiento de dicha técnica realizaremos una analogía de esta pero en la capa de aplicación. De tal forma que cada nodo estará representado por un proceso, y el enlace estará representado por un socket de la capa de transporte.

Utilizaremos el lenguaje Python por su sintaxis sencilla y cómoda para esta demostración, además es fácil el manejo de sockets.

**Cliente (client.py):**

{% highlight python  %}
import socket
from utilities import crc # Importamos la función crc 

sock = socket.socket() # Creamos el socket
sock.connect(('127.0.0.1',9999)) # Conectamos el socket con el servidor

generator = input('Type a generator: ') # Pedimos un generador
sock.send(generator.encode()) # Enviamos el generador.

data = input('Type data to transmit: ') # Pedimos data a transmitir
crc_code = crc(data,generator) # Calculamos el CRC

request = data + ' ' + crc_code   # Se entrama data con el CRC
sock.send(request.encode()) # Enviamos la trama 

response = sock.recv(1024) # Se espera por la respuesta
print(response.decode()) # Mostramos la respuesta
sock.close() # Cerramos la conexión.

 
{% endhighlight %}<br>

**Servidor (server.py):**

{% highlight python  %}
import socket
from utilities import crc #Importamos la función crc

server = socket.socket() # Creamos el socket 
server.bind(('127.0.0.1',9999)) # Asignamos el puerto al socket
server.listen(1) # Empezamos a escuchar por el puerto indicado

client, client_addr = server.accept() # Cuando se conecte un cliente
print('Device connected: ', client_addr) # Mostrar dirección

# Negociación del generador
generator = client.recv(1024).decode()
print("Generator:" ,generator)

# Esperar por una trama
data = client.recv(1024).decode()
print('Frame received: ' , data) # Mostrar trama

#Separar la trama en data y código CRC
data , crc_code = data.split(' ') 

# Realizar la comprobación
result = crc(data, generator, crc_code)
print ('Checksum result: ' ,result)

# Parsear la comprobación a un entero y validar resultado
result = int(result)
if result != 0:  
	response = 'err'
else:
	response = 'ok'

#Enviar respuesta al cliente.
client.send(response.encode())
#Cerrar la conexión con el cliente.
client.close()

#Cerrar el socket.
server.close()
 
{% endhighlight %}<br>


{% highlight python  %}
import socket
from utilities import crc # Importamos la función crc 

sock = socket.socket() # Creamos el socket
sock.connect(('127.0.0.1',9999)) # Conectamos el socket con el servidor

generator = input('Type a generator: ') # Pedimos un generador
sock.send(generator.encode()) # Enviamos el generador.

data = input('Type data to transmit: ') # Pedimos data a transmitir
crc_code = crc(data,generator) # Calculamos el CRC

request = data + ' ' + crc_code   # Se entrama data con el CRC
sock.send(request.encode()) # Enviamos la trama 

response = sock.recv(1024) # Se espera por la respuesta
print(response.decode()) # Mostramos la respuesta
sock.close() # Cerramos la conexión.

 
{% endhighlight %}<br>

**Utilities.py**

{% highlight python  %}

def crc(message, generator, crc_code='0000'): #Función crc

	if int(crc_code) == 0 : # Si se quiere calcular por 1era vez
      # Se agregan tantos ceros (0) indique el grado del polinomio
		crc_code = ''
		for i in range(len(generator) -1):
			crc_code = crc_code + '0'

      # Se agrega el crc al mensaje(data).
	message = message + crc_code

      # Se convierte tanto el mensaje(data) como el generador a listas 
	message = list(message)
	generator = list(generator)

	# Para i divisiones
	for i in range(len(message) - len(crc_code)):
		if message[i] == '1': # si el bit es un uno (1)
                 # Para cada bit del generador
			for j in range(len(generator)):
      #Dividir cada bit del generador con el del msg y traer el sgte. 
				message[i+j] = str((int(message[i+j])+int(generator[j]))%2)       
      # devolver n bits como se hayan agregado
	return ''.join(message[-len(crc_code):]) 
{% endhighlight %}<br>


## Ejecución

**Cliente (client.py)**


**Servidor (server.py)**
