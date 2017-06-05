# Práctica 5
## Replicación de bases de datos MySQL

En esta práctica crearemos copias de seguridad de nuestras bases de datos (BD) MySQL a través de una réplica maestro-esclavo.

### Creando la BD y volcándola con *mysqldump*

Primero creamos la base de datos en nuestra máquina maestro. Para ello ejecutamos ```mysql -u root -p``` y ejecutamos los siguientes comandos para crear la base de datos y la tabla. 

![alt text](http://i.imgur.com/DljOUEa.png)

Insertamos algún dato en la tabla y vemos que está bien creada.

![alt text](http://i.imgur.com/jV0pVHJ.png)

Tras ello pasamos a utilizar ```mysqldump```, con lo que podremos hacer una copia de los datos. Antes, será necesario ejecutar ```FLUSH TABLES WITH READ LOCK;``` para bloquear la base de datos durante la copia y que esta se haga de forma correcta.

![alt text](http://i.imgur.com/8wpgzzK.png)

Tras hacer el volcado con ```mysqldump contactos -u root -p > ejemplodb.sql```, desbloqueamos las tablas con ```UNLOCK TABLES;```. Ahora solo resta, desde la máquina esclavo, copiar la base de datos, para lo cual utilizamos el comando ```scp```, creamos una base de datos y la copiamos.

![alt text](http://i.imgur.com/shrkFzc.png)

### Configuración maestro-esclavo

Lo que vamos a hacer ahora es automatizar el proceso descrito anteriormente. Para ello, primero modificamos el archivo de configuración de *mysql* y comentamos el *bind address* en la máquina maestro.

![alt text](http://i.imgur.com/yTDODby.png)

Habilitamos el *error log* y el *bin log*, y ponemos el *server-id* a 1.

![alt text](http://i.imgur.com/PD7eb0D.png)

Tras esto, reiniciamos *mysql*.

![alt text](http://i.imgur.com/ti13uGE.png)

Ahora hacemos lo mismo en la máquina esclavo, con la diferencia de que pondremos el *server-id* a 2.

![alt text](http://i.imgur.com/gReOdvS.png)

De vuelta a la máquina maestro, creamos un usuario en la base de datos que de acceso a la máquina esclavo. 

![alt text](http://i.imgur.com/mqvpef6.png)

En la máquina esclavo, configuramos los datos de la máquina maestro. Lo iniciamos.

![alt text](http://i.imgur.com/bBJ5Q8J.png)

En la máquina esclavo, desbloqueamos las tablas. 

![alt text](http://i.imgur.com/sUZLA0r.png)

Para comprobar que está bien configurado, ejecutamos ```SHOW SLAVE STATUS\G``` y nos aseguramos de que ```Seconds_Behind_Master``` no tiene un valor de ```NULL```.

![alt text](http://i.imgur.com/itPsSB6.png)

¡Ya está! Comprobamos que funciona:

![alt text](http://i.imgur.com/Z6ADEGq.png)
