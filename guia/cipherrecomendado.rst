Cipher recomendado para cumplir con PCI
=======================================

Si es necesario deshabilitar SSL versión 2 de compatibilidad a fin de cumplir Requisitos de cumplimiento PCI, Tendrá que añadir la siguiente directiva en el archivo de configuración de Nginx::

        ssl_protocols       TLSv1.2 TLSv1.3;
        ssl_ciphers         HIGH:!aNULL:!MD5;

Si la directiva ya existe, probablemente tendrá que modificar para desactivar 2 versión SSL.


	ssl_ciphers EECDH+AESGCM:EDH+AESGCM;



