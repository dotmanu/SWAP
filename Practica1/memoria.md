# Práctica 1
## Server version: Apache/2.4.18 (Ubuntu)
Server built:   2016-07-14T12:32:26
Presentación de las prácticas

Máquina I con Ubuntu Server 

```
apache2 -v
Server version: Apache/2.4.18 (Ubuntu)
Server built:   2016-07-14T12:32:26
```

```
ps aux | grep apache
root      1273  0.0  1.2 258244 24912 ?        Ss   18:34   0:00 /usr/sbin/apache2 -k start
www-data  1288  0.0  0.3 258268  7900 ?        S    18:34   0:00 /usr/sbin/apache2 -k start
www-data  1289  0.0  0.3 258268  7900 ?        S    18:34   0:00 /usr/sbin/apache2 -k start
www-data  1290  0.0  0.3 258268  7900 ?        S    18:34   0:00 /usr/sbin/apache2 -k start
www-data  1291  0.0  0.3 258268  7900 ?        S    18:34   0:00 /usr/sbin/apache2 -k start
www-data  1292  0.0  0.3 258268  7900 ?        S    18:34   0:00 /usr/sbin/apache2 -k start
manu      1805  0.0  0.0  16756  1012 tty1     S+   19:19   0:00 grep --color=auto apache
```

Máquina II con Ubuntu Server 

```
apache2 -v
Server version: Apache/2.4.18 (Ubuntu)
Server built:   2016-07-14T12:32:26
```

```
root      1275  0.0  1.2 258244 25016 ?        Ss   18:58   0:00 /usr/sbin/apache2 -k start
www-data  1282  0.0  0.3 258268  7988 ?        S    18:58   0:00 /usr/sbin/apache2 -k start
www-data  1283  0.0  0.3 258268  7988 ?        S    18:58   0:00 /usr/sbin/apache2 -k start
www-data  1284  0.0  0.3 258268  7988 ?        S    18:58   0:00 /usr/sbin/apache2 -k start
www-data  1285  0.0  0.3 258268  7988 ?        S    18:58   0:00 /usr/sbin/apache2 -k start
www-data  1286  0.0  0.3 258268  7988 ?        S    18:58   0:00 /usr/sbin/apache2 -k start
manu      1743  0.0  0.0  16756  1032 tty1     S+   19:19   0:00 grep --color=auto apache
```

Se ha configurado ambas máquinas según el tutorial provisto en la práctica y se ha utilizado *tomcat* para establecer conexión entre mi máquina y sendas virtuales. Por último, se ha probado que todo funcionaba correctamente al modificar el index.html situado en /var/www. A continuación se muestra una captura tras el uso de *curl*.

![alt text](http://i.imgur.com/VzTNHc5.jpg)
