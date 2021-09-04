Instalar Nginx en CentOS
==========================

Agregar repositorio::

	yum install epel-release

Instalar Nginx::

	yum -y intall nginx

Iniciar Nginx::

	systemctl status nginx
	systemctl enable nginx
	systemctl start nginx

Verificamos los puertos::

	netstat -natp | grep nginx
	tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1361/nginx: master  
	tcp6       0      0 :::80                   :::*                    LISTEN      1361/nginx: master

Consultamos el portal::

	curl localhost:80

**Default Server Root**
El directorio raiz es /usr/share/nginx/html. 

**Default File Conf**
El archvivo de configuración es/etc/nginx/conf.d/default.conf.
