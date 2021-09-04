SSL
====


Cuando un Private Key viene protegida con clave al hacer el test nos pedira la clave::

	# nginx -t
	Enter PEM pass phrase:
	nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
	nginx: configuration file /etc/nginx/nginx.conf test is successful

Para evitar que el nginx nos pida la clave cada vez que iniciemos, utilizaremos la directiva de **ssl_password_file** en donde le indicamos en un archivo cual es la clave::

	vi ssl_password

        ssl_certificate "/opt/certificados/srvutils-password.crt";
        ssl_certificate_key "/opt/certificados/srvutils-password.key";
        ssl_protocols       TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        ssl_password_file /etc/nginx/ssl_passwords;

Cuando hacemo nuevamente el test vemos que ya no pide la clave::

	# nginx -t
	nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
	nginx: configuration file /etc/nginx/nginx.conf test is successful

