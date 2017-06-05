# Práctica 6
## Discos en RAID

En esta práctica configuraremos dos discos en RAID 1 por software, usando una maquina virtual con Ubuntu Server.

### Añadiendo discos virtuales y configurando el RAID

Primero añadimos dos discos virtuales desde VirtualBox.

![alt text](http://i.imgur.com/hvyngn4.png)

Para hacer la configuración por software, instalamos ```mdadm```. Tras ello, hacemos ```sudo fdisk -l``` para ver el tipo de identificación de los discos que hemos creado.

![alt text](http://i.imgur.com/x7fADNV.png)

Vemos que son ```sdb``` y ```sdc```. Creamos el RAID utilizando el dispositivo ```/dev/md0``` y le damos formato *ext2* con ```mkfs```.

![alt text](http://i.imgur.com/0SqtK9q.png)

![alt text](http://i.imgur.com/pdG5LVb.png)

Solo resta crear un directorio y montar el RAID.

```
mkdir /raid
mount /dev/md0 /raid
```

![alt text](http://i.imgur.com/dBFqatK.png)

Comprobamos que funciona correctamente:

![alt text](http://i.imgur.com/hMjUIC4.png)

### Montaje RAID automático y simulación de fallos

Con ```ls -l /dev/disk/by-uuid/``` obtenemos el UUID del disco. Hacemos esto para no tener que montar el disco cada vez que se inicie el sistema. 

![alt text](http://i.imgur.com/x5vwkVn.png)

Para evitarnos esto, añadiremos su UUID al ```fstab```.

![alt text](http://i.imgur.com/uDsxZSA.png)

Reiniciamos la máquina y vemos que funciona correctamente con la orden ```mount```.

![alt text](http://i.imgur.com/qv0sq8v.jpg)

Ahora, la prueba de fuego. Si un RAID falla, deberíamos poder seguir accediendo al otro. Para comprobarlo, simulamos un fallo en uno de ellos y hacemos que nos muestre información al respecto.

![alt text](http://i.imgur.com/q1sTtbY.png)

Vemos como uno de los discos está caído y, por tanto, podemos retirarlo.

![alt text](http://i.imgur.com/OnWHDyH.png)

Aún hay acceso a ```/raid```:  

![alt text](http://i.imgur.com/WxLKoIT.png)

Para terminar, volvemos a añadir el disco y vemos que se está reconstruyendo en *rebuild status*.

![alt text](http://i.imgur.com/8wrlHBA.png)

Ya lo tenemos listo.
