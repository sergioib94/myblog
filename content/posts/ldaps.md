---
title: "Ldaps"
date: 2021-03-12T14:13:08+01:00
categories: [Sistemas]
---

### **Introducción** ###

Configuraremos el servidor LDAP de frestón instalado y configurado en la practica anterior para que utilice el protocolo ldaps:// a la vez que el ldap:// utilizando el certificado x509 de la práctica de https o solicitando el correspondiente a través de gestiona. Realiza las modificaciones adecuadas en el cliente ldap de frestón para que todas las consultas se realicen por defecto utilizando ldaps://

### **LDAPs** ###

Lo primero que tendremos que hacer para usar ldaps, sera tener tanto un certificado .crt de la unidad certificadora, en este caso el IES Gonzalo Nazareno, un .crt firmado por la unidad certificadora y un fichero .key usado para crear un fichero csr que posteriormente se firmará.

Después de crear los certificados y ubicarlos en /etc/ssl/certs/ (los ficheros .crt) y el fichero .key en /etc/ssl/private/, hay que cambiar las acl con el comando setfacl para agregar los permisos:

Instalamos el paquete acl:

~~~
sudo apt install acl
~~~

Una vez tengamos los certificados en la maquina de freston, pasamos a ubicarlos en sus respectivos directorios:

~~~
debian@freston:~$ sudo mv gonzalonazareno.crt /etc/ssl/certs/
debian@freston:~$ sudo mv sergio.ibanez.crt /etc/ssl/certs/
debian@freston:~$ sudo mv sergio.ibanez.key /etc/ssl/private/
~~~

Agregamos los permisos necesarios para que el usuario openldap pueda hacer uso de los certificados usando el comando setfacl, en este caso usando la opcion -m ya que vamos a modificar las ACL de un determinado fichero o directorio:

~~~
debian@freston:~$ sudo setfacl -m u:openldap:r-x /etc/ssl/private
debian@freston:~$ sudo setfacl -m u:openldap:r-x /etc/ssl/private/sergio.ibanez.key
debian@freston:~$ sudo setfacl -m u:openldap:r-x /etc/ssl/certs
debian@freston:~$ sudo setfacl -m u:openldap:r-x /etc/ssl/certs/gonzalonazareno.crt 
debian@freston:~$ sudo setfacl -m u:openldap:r-x /etc/ssl/certs/sergio.ibanez.crt
~~~

Se crea un fichero .ldif con el siguiente contenido indicando que en el objeto config se van a modificar las ubicaciones de los certificados:

~~~
dn: cn=config
changetype: modify
replace: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/ssl/certs/gonzalonazareno.crt
-
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/ssl/private/sergio.ibanez.key
-
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/ssl/certs/sergio.ibanez.crt
~~~

Se añaden los cambios del fichero ldif creado anteriormente:

~~~
debian@freston:~$ sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f ldaps.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "cn=config"
~~~

Configuramos /etc/ldap/ldap.conf para indicar la ubicación del certificado de AC:

~~~
# TLS certificates (needed for GnuTLS)
TLS_CACERT      /etc/ssl/certs/gonzalonazareno.crt

~~~

Por defecto la ubicación es /etc/ssl/certs/ca-certificates.crt por lo que otra forma de hacerlo seria mover nuestro gonzalonazareno.crt a /etc/ssl/certs/ca-certificates.

Configuramos también en /etc/default/ldap la siguiente linea:

~~~
SLAPD_SERVICES="ldap:/// ldapi:/// ldaps:///"
~~~

Reiniciamos el servicio ldap:

~~~
sudo systemctl restart slapd
~~~

Comprobación usando ldaps:

~~~
debian@freston:~$ sudo ldapsearch -x -H ldaps://freston.sergio.gonzalonazareno.org:636 -b "cn=admin,dc=sergio,dc=gonzalonazareno,dc=org"
# extended LDIF
#
# LDAPv3
# base <cn=admin,dc=sergio,dc=gonzalonazareno,dc=org> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# admin, sergio.gonzalonazareno.org
dn: cn=admin,dc=sergio,dc=gonzalonazareno,dc=org
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
~~~

Comprobamos también si haciéndolo con ldap funciona:

~~~
debian@freston:~$ sudo ldapsearch -x -H ldap://freston.sergio.gonzalonazareno.org:636 -b "cn=admin,dc=sergio,dc=gonzalonazareno,dc=org"
ldap_result: Can't contact LDAP server (-1)
~~~