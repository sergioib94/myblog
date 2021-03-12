---
title: "Instalacion Y Configuracion Basica Vmware ESXI"
date: 2021-03-12T14:24:08+01:00
---

La instalación inicialmente es bastante sencilla, haciendo uso de un usb con la iso de vmware 5.5 lo conectamos en el servidor y nos aseguramos de que este configurado en bios para que arranque por usb y no por otro medio.

Una vez que comience la instalación, lo primero sera confirmar dicha instalación y mas adelante tendremos que elegir el disco donde se instalara vmware. En este caso al haber una instalacion vmware anterior, tendremos que reinstalar vmware.

![confirmamos instalacion](/static/vmware/1.jpg)

![instalamos](/static/vmware/2.jpg)

![seleccionamos almacenamiento](/static/vmware/3.jpg)

Ya seleccionado el disco, el siguiente paso sera ponerle contraseña al usuario root, en este caso una tan simple como “usuario”.

![contraseña de root](/static/vmware/4.jpg)

Después de esto, terminara la instalación y comenzara a configurarse de forma automática, de forma que al acabar vmware obtendrá una dirección por dhcp de forma que desde un navegador se podrá administrar, en este caso la ip 172.22.8.130.

![instalacion completa](/static/vmware/5.jpg)

![carga de vmware](/static/vmware/6.jpg)

![vmware](/static/vmware/7.jpg)

Al acceder a la ip de vmware en el navegador, entramos en la pagina de inicio, donde podremos administrar las maquinas virtuales que se vayan a crear accediendo al cliente host de vmware.

![inicio vmware](/static/vmware/vmware1.png)

Creación de la maquina virtuales

Para ello hacemos click en “nueva maquina virtual” y después a crear nueva maquina virtual.

![creacion de la maquina](/static/vmware/vmware2.png)

Luego le damos un nombre y seleccionamos el tipo de sistema operativo que tendrá.

![nombre y sistema de la maquina](/static/vmware/vmware3.png)

Seleccionamos donde se almacenara, en este caso en nuestro disco.

![almacenamiento](/static/vmware/vmware4.png)

Personalizamos la configuracion:

![configuracion](/static/vmware/vmware6.png)

Una vez creada la maquina virtual, necesitaremos instalar la consola remota vmware, para ello en la pagina de vmware nos descargamos el fichero VMWare Remote Console 12.0.0 for linux. Después de haberlo descargado, lo instalamos en nuestra maquina mediante el comando sh y comenzara la instalación de la herramienta:

<pre>
sh VMWare-Remote-Console-12.0.0-17287072.x86_64.bundle
</pre>

Y una vez ya está instalado, accedemos mediante dicha herramienta a la máquina para su instalación. Una vez instalado el sistema, comprobamos que funciona y que tiene salida a la red:

![funcionamiento de la maquina](/static/vmware/vmware_final.png)