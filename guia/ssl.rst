SSL
====

Para configurar el HTTPS server, el parametro ssl debe estar habilitado sobre un listening sockets en el bloque de **server** y se debe indicar la localidad del certificado y de la private key::

	server {
	    ...
	    listen              443 ssl;
	    ...
	    ssl_certificate     /path/certificado.crt;
	    ssl_certificate_key /path/privatekey.key;
            ssl_protocols       TLSv1.2 TLSv1.3;
            ssl_ciphers         HIGH:!aNULL:!MD5;

	    ...
	}

Verificamos::

	nginx -t

Reiniciamos::

	systemctl restart nginx

Probamos::

	nginx -V



Para mayor detalle ver la pagina oficial

http://nginx.org/en/docs/http/configuring_https_servers.html
