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

### Montaje RAID automático y simulación de fallo
