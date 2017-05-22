# Práctica 4
## Asegurar la granja web

En esta práctica vamos a poner en práctica el uso de certificados SSL que permitan el acceso por HTTPS.

### Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS

Empezamos generando nuestro certificado SSL con el comando ```a2enmod ssl``` para generar los módulos de ssl en nuestra máquina, tras lo cual reiniciaremos Apache, creamos la carpeta que contendrá el certificado SSL y finalmente, lo generamos con el comando:

```openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt```

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


### Configuración del cortafuegos

Primero comprobamos el estado del cortafuegos:

```iptables –L –n -v```

![alt text](http://i.imgur.com/FDxqM73.png)

De donde se deduce que no hay ningún tipo de configuración y permite todo el tráfico, así que de entrada vamos a denegarlo estableciendo las políticas por defecto. Lo hacemos con:

```
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```

Permitimos el tráfico de salida con ```iptables -P OUTPUT ACCEPT``` y ```iptables -A INPUT -m state --state NEW,ESTABLISHED -j ACCEPT```. Además, bloqueamos el tráfico ICMP (ping), para evitar ataques como el del ping de la muerte con ```iptables -A INPUT -p icmp --icmp-type echo-request -j DROP```. 

Abrimos los puertos HTTP y HTTPS, 80 (```iptables -A INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT```) y 443 (```iptables -A INPUT -m state --state NEW -p tcp --dport 443 -j ACCEPT```) respectivamente, para poder servir webs.

Permitimos el acceso DNS abriendo el puerto 53 con ```iptables -A INPUT -m state --state NEW -p udp --dport 53 -j ACCEPT``` y ```iptables -A INPUT -m state --state NEW -p tcp --dport 53 -j ACCEPT```.

Para poder administrar remotamente la máquina por SSH, abrimos el puerto 22 para la entrada con ```iptables -A INPUT -p tcp --dport 22 -j ACCEPT``` y la salida con ```iptables -A OUTPUT -p udp --sport 22 -j ACCEPT```.

Ejecutamos ```iptables –L –n -v``` y vemos el resultado:

![alt text](http://i.imgur.com/HxKLhn1.png)

Solo resta crear un script que se ejecute en el arranque del sistema con la configuración del IPTABLES para el cortafuegos. Ejecutamosn ```ano script_iptables.sh``` y ponemos lo siguiente:

![alt text](http://i.imgur.com/KoQ1REY.png)

Para que se ejecute cada que vez que se inicie la máquina, modificamos el archivo ```/etc/rc.local```:

![alt text](http://i.imgur.com/RVZX2jP.png)

Y ya lo tenemos. Cada vez que la máquina se inicie aparecerá con nuestra configuración de IPTABLES por defecto.

### Testeando nuestro Firewall con *nmap* y *tcpdump*

Para ver que el Firewall está funcionando correctamente, utilizamos ```nmap```y ```tcpdump```. Para ello seguimos estos pasos, previa creación del directorio ```scan_results/syn_scan/packets```:

```tcpdump host 10.0.2.4 -w ~/scan_results/syn_scan/packets```

Con esto empezamos a capturar el tráfico para nuestra IP; podemos hacer ```CTRL-Z``` y luego ```bg``` para que el proceso corra en segundo plano.

Tras esto ejecutamos nmap:

```nmap -sS -Pn -p- -T4 -vv --reason -oN ~/scan_results/syn_scan/nmap.results 10.0.2.4```

Y ya podemos devolver tcpdump a primer plano y pararlo con CTRL-C. Veamos los resultados.

*Tráfico enviado*

```tcpdump -nn -r ~/scan_results/syn_scan/packets 'dst 10.0.2.4' | less```

![alt text](http://i.imgur.com/en0930f.png)

*Tráfico de respuesta*

```tcpdump -nn -r ~/scan_results/syn_scan/packets 'src 10.0.2.4' | less```

![alt text](http://i.imgur.com/kXRGr5K.png)

Adicionalmente, los puertos TCP responderán a estas peticiones con un paquete SYN. Podemos filtrarlas así:

```tcpdump -nn -r ~/scan_results/syn_scan/packets 'src 10.0.2.4 and tcp[tcpflags] & tcp-syn != 0' | less```

![alt text](http://i.imgur.com/836JHkm.png)

Esto nos mostrará aquellas respuestas SYN que se han realizado con éxito.

Más información sobre el uso de ```nmap```y ```tcpdump``` para testear nuestro Firewall en este tutorial de [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-test-your-firewall-configuration-with-nmap-and-tcpdump).
