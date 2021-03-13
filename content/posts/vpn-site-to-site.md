---
title: "Vpn Site to Site"
date: 2021-03-12T19:00:38+01:00
categories: [Seguridad]
---

### Introduccion ###

Configura una conexión VPN sitio a sitio entre dos equipos del cloud openstack:

Cada equipo estará conectado a dos redes, una de ellas en común. Para la autenticación de los extremos se usarán obligatoriamente certificados digitales, que se generarán utilizando openssl y se almacenarán en el directorio /etc/openvpn, junto con con los parámetros Diffie-Helman y el certificado de la propia Autoridad de Certificación.

Se utilizarán direcciones de la red 10.99.99.0/24 para las direcciones virtuales de la VPN.
Tras el establecimiento de la VPN, una máquina de cada red detrás de cada servidor VPN debe ser capaz de acceder a una máquina del otro extremo.

### **VPN sitio a sitio con OpenVPN y certificados x509** ###

Escenario Propio:

Servidor -> 192.168.100.2
Cliente -> 192.168.100.10

Escenario compañera Celia:

Servidor -> 192.168.200.8
Cliente -> 192.168.200.4

En este caso mi servidor hace de cliente del servidor de celia.

Fichero de configuracion sergio:

~~~
#### Fichero de sit-to-site vpn ####
#Dispositivo de túnel
dev tun

#IP del servidor
remote 172.22.201.33

#Encaminamiento
ifconfig 10.99.99.2 10.99.99.1

# Subred remota
route 192.168.200.0 255.255.255.0

#Rol de cliente
tls-client

#Certificado de la CA
ca /etc/openvpn/ca_celia.crt

#Certificado cliente
cert /etc/openvpn/cliente_celia.crt

#Clave privada cliente
key /etc/openvpn/cliente_celia.key

#Activar la compresión LZO
comp-lzo

#Detectar caídas de la conexión
keepalive 10 60

#Nivel de la información
verb 3

log /var/log/vpn.log
~~~

Fichero Servidor celia:

~~~
# Use a dynamic TUN device
dev tun

# Virtual ip
ifconfig 10.99.99.1 10.99.99.2

# Local subnet
route 192.168.100.0 255.255.255.0

# Enable TLS and assume server role
tls-server

# Diffie-Hellman
dh /etc/openvpn/keys/dh.pem

# Certificado de la CA
ca /etc/openvpn/keys/ca.crt

# Certificado local
cert /etc/openvpn/keys/servidor.crt

# Clave privada 
key /etc/openvpn/keys/servidor.key

# Use fast LZO compression
comp-lzo

# Ping remote every 10sg and restart after 60sg passed without sign of file from remote.
keepalive 10 60

# Set output verbosity to normal usage range
verb 3

log /var/log/office1.log
~~~

![Pueba de ping](/site-to-site/Pueba_ping.jpeg)