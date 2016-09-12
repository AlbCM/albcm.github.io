---
layout: post
title:  "Proxy inverso con Apache"
date:   2016-09-12 22:19:13
categories: Programacion
---

Un proxy es un servicio que actúa como intermediario entre una comunicacón Cliente-Servidor, mientras que un proxy normal o **Forward Proxy** mantiene a un cliente en "anonimato" debido a que es el proxy quien se conecta con el servidor, un Proxy inverso **(Reverse Proxy)** mantiene al servidor "oculto" para con sus clientes.

Los usos de un Proxy inverso son muchos por ejemplo, este blog aparte de estar en GitHub Pages esta alojado tambien en un VPS y como esta hecho sobre *Jekyll* este debe correr en una dirección y un puerto, por ejemplo **http://albcm.ml:8090** pero queremos acceder a la pagina mediante [http://albcm.ml](http://albcm.ml)  sin especificar el puerto, generalmente nosotros no tenemos que escribir el puerto de una dirección porque el navegador nos facilita este trabajo ya que al acceder a esta por defecto lo hace al puerto 80, entonces algunos preguntarán *"¿ Por qué no correr Jekyll en el puerto 80 ?"* y la respuesta es *"No se puede porque tenemos un servidor Web Apache corriento en este puerto"* entonces aqui es donde viene la magia del Proxy inverso y lo que hace es Enviar todas las peticiones que lleguen **http://albcm.ml** las reenvia a **http://albcm.ml:8090** sin que el usuario vea el cambio.
# Configurar Reverse Proxy en Apache:

**1-** Instalamos algunos paquetes:
{% highlight AppleScript %}
$ sudo apt-get-install libapache2-mod-proxy-html
{% endhighlight %}<br>

**2-** Activamos los modulos de apache
{% highlight AppleScript %}
$ sudo a2enmod proxy
$ sudo a2enmod proxy_html 
$ sudo a2enmod proxy_http
{% endhighlight %}<br>

**3-** Reiniciamos apache
{% highlight AppleScript %}
$ sudo service apache2 restart
{% endhighlight %}<br>

**4-** Ya estaremos listos para crear el Proxy, desde aqui asumo que tienen conceptos básicos en la configuración y terminología de Apache2. Añadiremos las siguientes lineas dentro de un virtual Host de Apache en **/etc/apache2/sites-avalaible/tupagina.com.conf**.
{% highlight AppleScript %}
NameVirtualHost *:80
<VirtualHost *:80>
	ServerName tupagina.com
	ProxyRequests Off	
	ProxyPass / http://www.<tupaginaodireccionippublica>.com:8080/
	ProxyPassReverse / http://www.<tupaginaodireccionippublica.com>.com>:8080/
</VirtualHost>
{% endhighlight %}<br>

**ServerName** hacemos referencia a la URL a la que se dirige la petición. 

**ProxyPass** tenemos que poner dentro el nombre de dominio, si no tenemos dominio la dirección IP publica de nuestro servidor junto con el puerto de nuestro servicio, si por ejemplo mi Servicio Jekyll corre en el *8080* debemos especificarlo en esa parte.

**ProxyRequests Off** Deshabilitamos para evitar problemas de seguridad

**ProxyPreserveHost On** Permite que el salto sea transparente para el usuario, ya que si no lo habilitamos el usuario veria la transición del cambio de pagina.

**ProxyPass y ProxyPassReverse** Gestionan el salto y vuelda del servidor

**5-** Habilitamos el Virtual host y reiniciamos el servicio
{% highlight AppleScript %}
$ sudo a2ensite tupagina.com.conf
$ sudo service apache2 restart
{% endhighlight %}<br>

Con esto deberiamos poder acceder a **http://tupagina.com** como si la tuvieramos por defecto en el puerto 80.