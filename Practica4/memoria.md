# Práctica 4
## Asegurar la granja web

En esta práctica vamos a poner en práctica el uso de certificados SSL que permitan el acceso por HTTPS.

### Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS

Empezamos generando nuestro certificado SSL con el comando ```a2enmod ssl``` para generar los módulos de ssl en nuestra máquina, tras lo cual reiniciaremos Apache, creamos la carpeta que contendrá el certificado SSL y finalmente, lo generamos con el comando:

```openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
/etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt```

Se muestra a continuación el proceso al completo realizado en la máquina:

![alt text](http://i.imgur.com/DmKXozk.png)

Ahora modificamos el archivo de configuración con la ubicación del certificado SSL. Para ello utilizamos el comando ```nano /etc/apache2/sites-avilable/default-ssl``` y añadimos las siguientes líneas:

![alt text](http://i.imgur.com/UUIFsyp.png)

Solo resta activar el modo ```default-ssl``` con ```a2ensite default-ssl``` y reiniciar Apache.

![alt text](http://i.imgur.com/F68wJul.jpg)

¡Ya está! Ahora podemos comprobar que podemos acceder con HTTPS, si bien al tratarse de un certificado autofirmado nuestro navegador nos "pondrá pegas" diciendo que la conexión no es segura. Desde el propio terminar, accedemos:

```links2 https://10.0.2.4```

Nos aparecerá el mensaje de certificado no válido:

![alt text](http://i.imgur.com/hYsry5S.png)
