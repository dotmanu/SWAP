# Práctica 3
## Balanceo de carga

Creamos una tercera máquina con la característica de que ningún software se apropie del puerto 80, por lo que no podemos tener instalado el Apache. Para conseguir dicho cometido, visitamos *Stack Overflow* y descubrimos que es tan simple como ejecutar ```sudo update-rc.d apache2 remove``` en nuestra nueva máquina. Sin embargo, aún así, parece que había otro proceso utilizando dicho puerto. Para evitar tal cosa, ejecutamos ```sudo fuser -k 80/tcp```.

Lo siguiente será instalar el servidor Nginx.

```
sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get autoremove
sudo apt-get install nginx 
sudo systemctl start nginx
```

Vemos que accede correctamente.

![alt text](http://i.imgur.com/8iBKXcv.png)

Ahora pasamos a modificar su configuración, situada en ```/etc/nginx/conf.d/default.conf```. La guardamos con la siguiente información:

![alt text](http://i.imgur.com/0Irx6cb.png)

Tras esto, ejecutamos ```service nginx restart``` y acto seguido probamos a realizar peticiones desde otra máquina a esta (el balanceador) con ```curl```.

![alt text](http://i.imgur.com/J30ALHR.jpg)
