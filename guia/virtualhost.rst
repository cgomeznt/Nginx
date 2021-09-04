Server Block
=============

Introducción
+++++++++++++++++
Cuando se usa Nginx como Web Server, el **Server Blocks** (Similar a los virtualhost de Apache) pueden ser usados para encapsular detalles de configuración y más de un dominio en un simple server.

Virtual host es para correr mas de un dominio en una IP address. Es especialmente usado para el publico que necesita correr varios sites en un virtual private server. El site mostrara diferente informacion a los visitadores, dependiendo de que site ingrese.
Creamos un directorio para almacenar el contenido de la pagina que servirá el **Server Blocks**.::

	# mkdir /var/www/public_html
	# mkdir /var/www/example_html


Recuerda configurar los DNS para este site corresponda con la IP, para probar lo puedes crear en el /etc/hosts.::

	# vi /etc/hosts
	192.168.1.20     public.com www.public.com
	192.168.1.20     example.com www.example.com

Creamos una pagina de prueba.::

	# vi /var/www/public_html/index.html

	<html>
	  <head>
		<title>www.public.com</title>
	  </head>
	  <body>
		<h1>Felicitaciones, se creo el Server Blocks de public en public_html</h1>
	  </body>
	</html>

Creamos una pagina de example.::

	# vi /var/www/example_html/index.html

	<html>
	  <head>
		<title>www.example.com</title>
	  </head>
	  <body>
		<h1>Felicitaciones, se creo el Server Blocks de example en example_html</h1>
	  </body>
	</html>


Se asigna el propietario::

	chown -R $USER:$USER /var/www/public_html
	chown -R $USER:$USER /var/www/example_html

Se asignan los permisos::

	chmod -R 755 /var/www


Crear los Server Block
++++++++++++++++++++++

Si no existe los siguientes directorios los creamos **/etc/nginx/sites-available** y el  **/etc/nginx/sites-enabled**, luego editamos el **/etc/nginx/nginx.conf** y agregamos esta linea, dentro del bloque del **http**::

	http {
		(...)
		include /etc/nginx/sites-enabled/*.conf;
		server_names_hash_bucket_size 64;
	}


Verificamos la configuración::

	nginx -t
	nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
	nginx: configuration file /etc/nginx/nginx.conf test is successf

Creando el primer **Server Block**, utilizando el default que trae nginx.conf.default pero solo le dejamos el bloque de **server**::

	vi /etc/nginx/sites-enabled/public_html.conf

	    server {
	       listen       80;
		listen       [::]:80;
		server_name  public.com www.public.com;
		root         /var/www/public_html;

		access_log /var/log/nginx/public.com.access.log;
		error_log /var/log/nginx/public.com.error.log;
		# Load configuration files for the default server block.
		include /etc/nginx/default.d/*.conf;

		error_page 404 /404.html;
		location = /404.html {
		}

		error_page 500 502 503 504 /50x.html;
		location = /50x.html {
		}
	    }


Creando el segundo **Server Block**, utilizando el default que trae nginx.conf.default pero solo le dejamos el bloque de **server**::

	vi /etc/nginx/sites-enabled/public_html.conf

	    server {
	       listen       80;
		listen       [::]:80;
		server_name  example.com www.example.com;
		root         /var/www/example_html;

		access_log /var/log/nginx/example.com.access.log;
		error_log /var/log/nginx/example.com.error.log;
		# Load configuration files for the default server block.
		include /etc/nginx/default.d/*.conf;

		error_page 404 /404.html;
		location = /404.html {
		}

		error_page 500 502 503 504 /50x.html;
		location = /50x.html {
		}
	    }

Terminamos por verificar la configuración::

	nginx -t
	nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
	nginx: configuration file /etc/nginx/nginx.conf test is successful



Iniciamos el Nginx.::

	# systemctl restart nginx

Probamos los dos (2) Server Block.::

	# curl www.public.com
		<html>
		  <head>
			<title>www.public.com</title>
		  </head>
		  <body>
			<h1>Felicitaciones, se creo el Server Block de public en public_html</h1>
		  </body>
		</html>


	# curl www.example.com
	<html>
	  <head>
		<title>www.ejemplo.com</title>
	  </head>
	  <body>
		<h1>Felicitaciones, se creo el Virtual Host de ejemplo.com</h1>
	  </body>
	</html>




