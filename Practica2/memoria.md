# Práctica 2
## Clonar la información de un sitio web

Probamos la copia de archivos con ssh.

![alt text](http://i.imgur.com/P1d7arw.png)

Vemos que se ha copiado en la máquina II.

![alt text](http://i.imgur.com/oMJNgGE.png)

Ahora vamos a copiar un directorio de la máquina I en la máquina II.

![alt text](http://i.imgur.com/ExyhOv6.png)

Copia del directorio en la máquina II utilizando rsync.

![alt text](http://i.imgur.com/TUtchcp.png)

Ahora vamos a configurar el acceso sin contraseña por ssh. En la máquina II, ejecutamos ```ssh-keygen -b 4096 -t rsa``` y generamos la clave ssh. Tras esto, para copiarla a la máquina I, hacemos uso de ```ssh-copy-id```. La orden, al completo y para este caso, sería ```ssh-copy-id -i .ssh/id_rsa.pub manu@192.168.1.43```. A continuación se muestra la generación de la clave.

![alt text](http://i.imgur.com/j5Hdoqn.png)

Podemos ver como ya no nos pide contraseña al acceder a la máquina I desde la máquina II.

![alt text](http://i.imgur.com/F6w9Ude.png)

Vamos ahora a por la última parte de la práctica, establecer una tarea en cron que se ejecute cada hora para mantener
actualizado el contenido del directorio /var/www entre las dos máquinas. Para ello, hacemos un ```nano /etc/crontab``` y escribimos lo siguiente.

```
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user command
01 * * * * root rsync -avz -e ssh root@192.168.1.43:/var/www/ /var/www
```
