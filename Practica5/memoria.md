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
