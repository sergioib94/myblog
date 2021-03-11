---
title: "Virtualhost Apache"
date: 2021-03-11T09:47:06+01:00
---

### **Configuración de VirtualHosting** ###

Lo primero sera instalar apache2 en nuestra maquina virtual vagrant.

~~~
sudo apt install apache2
~~~

Una vez descargado apache2, creamos los ficheros.conf que usaran tanto www.iesgn.org como www.departamentosgn.org para ello primero copiamos el 000-default para crearlos y luego los modificamos dejándolos de la siguiente forma:

~~~
vagrant@nodo1:/etc/apache2/sites-available$ sudo cp 000-default.conf iesgn.conf
vagrant@nodo1:/etc/apache2/sites-available$ sudo cp 000-default.conf departamentos.conf
~~~

### **Modificaciones de iesgn.conf y departamentos.conf** ###

* iesgn.conf:

~~~
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /srv/www/iesgn
        ServerName www.iesgn.org
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
~~~

* departamentos.conf:

~~~
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /srv/www/departamentos
        ServerName www.departamentosgn.org
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
~~~

Cuando estén configurados, creamos los directorios que alojaran los index de iesgn y departamentos en /srv/www.

~~~
vagrant@nodo1:/srv$ sudo mkdir -p www/iesgn
vagrant@nodo1:/srv$ sudo mkdir -p www/departamentos
~~~

Se configura el fichero apache2.conf para que srv sea accesible dejándolo de la siguiente forma:

~~~
<Directory /srv/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
~~~

Creamos los enlaces simbólicos de ficheros a apache2/sites-enabled usando para ello el comando a2ensite.

~~~
vagrant@nodo1:/srv/www/iesgn$ sudo a2ensite iesgn.conf
Enabling site iesgn.
To activate the new configuration, you need to run:
  systemctl reload apache2

vagrant@nodo1:/srv/www/departamentos$ sudo a2ensite departamentos.conf 
Enabling site departamento.
To activate the new configuration, you need to run:
  systemctl reload apache2
~~~

Creamos los index en los directorios creados anteriormente.

~~~
vagrant@nodo1:/srv$ sudo touch www/iesgn/index.html
vagrant@nodo1:/srv$ sudo touch www/departamentos/index.html
~~~

Una vez hecho todo reiniciamos el servicio y comprobamos que ambas paginas son accesibles desde nuestro navegador.

~~~
vagrant@nodo1:/srv/www/iesgn$ sudo systemctl restart apache2
~~~

Esto es todo por parte del servidor web, ahora solo quedara modificar el fichero /etc/hosts de nuestra maquina para que se haga una conversion entre los nombres de los dominios y la ip de nuestro servidor de forma que seamos capaces de acceder a las paginas.

### **Prueba de funcionamiento** ###
