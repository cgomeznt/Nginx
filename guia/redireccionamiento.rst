Nginx redirect http to https
================================

En el archivo de configuración agregue un bloque de **server** que solo escuche por el 80 y haga el redirect hacia el 443::

	server {
		listen 80 default_server;
		server_name mywebsite.me www.mywebsite.me;

		return 301 https://$server_name$request_uri;
	}

Debe tener otro bloque de **server** en donde este en escucha por el 443 y pueda recibir el redirect.::

	    server {
		listen  443 ssl;
		listen  [::]:443 ssl;
		server_name  _;
		root         /usr/share/nginx/html;

		# Load configuration files for the default server block.
		include /etc/nginx/default.d/*.conf;

		error_page 404 /404.html;
		location = /404.html {
		}

		error_page 500 502 503 504 /50x.html;
		location = /50x.html {
		}
	    }



Tipos de Redirect
++++++++++++++++++

Hay 4 tipos de redireccionamiento que podemos utilizar según nuestro criterio.

301
Indica que el recurso se ha movido de forma permanente.

302
Indica que el recurso se ha movido de forma temporal.

303
Indica que el recurso ha sido reemplazado por otro.

402
Indica que el recurso se ha eliminado de forma permanente. Cuando se utiliza este estado el argumento URL debe omitirse.



Redirect Specific Sites
++++++++++++++++++++++++++

::

	server {
	    listen 80;

	    server_name foo.com;
	    return 301 https://foo.com$request_uri;
	}

Redirect segun Server Block
+++++++++++++++++++++++++++++

::


	server {
		listen 80 default_server;
		server_name _;

		return 301 https://$server_name$request_uri;
	}

	server {
	    listen 443 ssl default_server;
	    server_name foo.com;
	}

	server {
	    listen 443 ssl;
	    server_name bar.com;
	}

	# and so on...

