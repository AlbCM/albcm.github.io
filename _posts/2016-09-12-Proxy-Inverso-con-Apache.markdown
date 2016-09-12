---
layout: post
title:  "Proxy inverso con Apache"
date:   2016-09-07 22:19:13
categories: Programacion
---

# Reverse Proxy

Un proxy es un servicio que actúa como intermediario entre una comunicacón Cliente-Servidor, mientras que un proxy normal o **Forward Proxy** mantiene a un cliente en "anonimato" debido a que es el proxy quien se conecta con el servidor, un Proxy inverso **(Reverse Proxy)** mantiene al servidor "oculto" para con sus clientes.

Los usos de un Proxy inverso son muchos por ejemplo, este blog esta alojado tambien en un VPS en esta dirección [http://albcm.ml](http://albcm.ml) y esta hecho gracias a *Jekyll*, este debe correr en una dirección y un puerto, por ejemplo **http://albcm.ml:8080** pero queremos acceder a la pagina mediante **http://albcm.ml**  sin especificar el puerto, generalmente nosotros no tenemos que escribir el puerto de una dirección porque el navegador nos facilita este trabajo ya que al acceder a esta por defecto lo hace a travez del puerto 80, entonces algunos preguntarán *"¿ Por qué no correr Jekyll en el puerto 80 ?"* y la respuesta es *"No podemos esta vez, porque tenemos un servidor Web Apache corriento en este puerto"* entonces aqui es donde viene la magia del Proxy inverso.

# Configurar Reverse Proxy en Apache:

1. Instalamos algunos paquetes:

{% highlight CoffeeScript  %}
apt-get-install libapache2-mod-proxy-html
{% endhighlight %}<br>

2.