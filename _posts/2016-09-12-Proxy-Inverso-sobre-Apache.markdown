---
layout: post
title:  "Proxy inverso sobre Apache"
date:   2016-09-12 22:19:13
categories: Programacion
---

Un proxy es un servicio que actúa como intermediario entre una comunicacón Cliente-Servidor. Mientras que un proxy normal o **Forward Proxy** mantiene a un cliente en "anonimato" ya que es el proxy quien se conecta con el servidor, un Proxy inverso **(Reverse Proxy)** mantiene al servidor "oculto" para con sus clientes.

# Configurar Reverse Proxy (caso de uso Jekyll):

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

# Nota 11/10/2016:
Para lo descrito arriba, si tenemos un blog alojado por GithubPages y tenemos un dominio propio, unicamente lo que tendremos que hacer es contactar a nuestro provedoor DNS y configurar un "A records" apuntando a la dirección que nos proporciona github pages. 

Luego simplemente tendremos que irnos a la configuración de nuestro repositorio y en la casilla "Custom domain" colocar nuestro dominio propio, este proceso puede que tarde hasta un día para actualizarse.

Aqui dejo la documentación necesaria:

*-[https://help.github.com/articles/quick-start-setting-up-a-custom-domain/](https://help.github.com/articles/quick-start-setting-up-a-custom-domain/)*

