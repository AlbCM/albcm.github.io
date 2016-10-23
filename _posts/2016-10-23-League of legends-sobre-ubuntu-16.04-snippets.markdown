---
layout: post
title:  "League of Legends sobre Ubuntu 16.04"
date:   2016-10-23 19:40:13
categories: games
---

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

