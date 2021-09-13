---
title: "Active Directory"
date: 2021-05-23T15:28:17+02:00
categories: [Sistemas, Apuntes]
---

### ¿Que es? ###

Active Directory es un servicio de directorio que almacena objetos de datos en su entorno de red local. El servicio registra datos en los usuarios, dispositivos, aplicaciones, grupos, y dispositivos en una estructura jerárquica.

La estructura de los datos permite encontrar los detalles de los recursos conectados a la red desde una ubicación. En esencia, Active Directory actúa como un directorio telefónico para su red, por lo que puede buscar y administrar dispositivos fácilmente.

**Bosque**

Es el nivel mas alto de organizacion en active directory. Un bosque es un grupo de dominios, cuando múltiples árboles se agrupan juntos se convierten en un bosque.

**Arbol**

El árbol es una entidad con un solo dominio o grupo de objetos seguido por dominios secundarios. Los distintos arboles en un bosque se conectan entre si a traves de una relación de confianza (la confianza entre los distintos dominios se hara de forma automatica) que permite que los distintos dominios compartan la información.

**Dominios**

Un dominio de Active Directory es un contenedor lógico utilizado para administrar usuarios, grupos y computadoras entre otros objetos.

Todos estos objetos son contenidos en una partición específica dentro de la base de datos de Active Directory (ADDS).

**Unidad Organizativa**

Las unidaddes organizativas (OU) son contenedores de Active Diretory dentro de un dominio con los que podemos agrupar para organizar multitud de objetos de active directory como pueden ser: usuarios de dominios, grupos de dominios, equipos e incluso otras unidades organizativas.

![jerarquia active directory](/Active-Directory/jerarquia.png)

### Instalacion (En windows server 2016) ###

1. Nos dirigimos al Administrador de Servidor o Server Manager y hacemos clic en Agregar roles o características.

![](/Active-Directory/ws-1.png)

2. En la ventana inicial hacemos clic en siguiente y elegimos la opción de la Instalación basada en roles.

![](/Active-Directory/ws-2.png)

3. Hacemos clic en siguiente y elegimos el servidor donde queremos instalar nuestro rol de AD DS.

![](/Active-Directory/ws-3.png)

4. Hacemos clic en siguiente y elegimos el rol solicitado (Servicios de dominio de Active Directory), agregamos las características.

![](/Active-Directory/ws-4.png)

5. Hacemos clic en siguiente seleccionamos las características y clic en siguiente y se desplegarán las caracteristicas que se van a instalar.
       
6. Hacemos clic en siguiente y comenzamos con la instalacion.

![](/Active-Directory/ws-6.png)
       
7. Una vez instalado nuestro rol de Active Directory reiniciamos el servidor para aplicar los cambios. Antes de iniciar nuestro asistente de AD DS debemos promover nuestro servidor a controlador de dominio.
       
8. Para promover el servidor a DC nos dirigimos a nuestro Administrador de Servidor y desplegamos el flag de información (la bandera) e indicamos la opcion "promover este servidor a controlador de dominio".
       
Hacemos clic en esta opción y se desplegará el asistente para configuración de servicios de dominio de Directorio Activo.
       
Veremos que contamos con 3 opciones de configuración:

* Agregar un controlador de dominio a un dominio existente:
* Agregar un nuevo dominio a un bosque existente:
* Agregar un nuevo bosque:

En nuestro caso elegiremos la opción Agregar un nuevo bosque, elegiremos el nombre para nuestro domino y damos clic en Siguiente.

![](/Active-Directory/ws-8.png)

9. Después que el sistema valide si el nombre de dominio es válido, se desplegarán las opciones del controlador de dominio, donde contamos con algunas opciones muy importantes que debemos tener en cuenta a nivel de funcionalidad de nuestro servidor. 

El nivel funcional que se indique debe ser siempre el mas bajo de la red, es decir, en mi caso el servidor con Active Directory cuenta con windows server 2012 por lo que el nivel funcional tanto de dominio como de bosque, sera de windows 2012, sin embargo si en nuestra red contamos con algun otro servidor con windows server 2008 por ejemplo, el nivel funcional se debe de establecer en windows server 2008.

![](/Active-Directory/ws-10.png)

10. En la siguiente ventana tenemos las opciones de DNS.

![](/Active-Directory/ws-11.png)
       
11. Hacemos clic en siguiente, y veremos el nombre de NETBIOS asignado por el sistema, nosotros decidimos si lo cambiamos o dejamos ése por defecto.

![](/Active-Directory/ws-12.png)
       
12. Clic en Siguiente, se desplegará la ventana informativa de ubicación de la base de datos de AD DS.

![](/Active-Directory/ws-13.png)
       
13. Clic en Siguiente, a continuación se desplegará una ventana informativa con el proceso a realizar, si notamos en la parte inferior podemos realizar este mismo procedimiento usando Powershell, Windows Server nos da la opción de ver y copiar el script.

![](/Active-Directory/ws-14.png)
       
14. Hacemos clic en siguiente, y Windows Server realizará una validación para que todos los requisitos de instalación estén correctos y así poder comenzar la instalación.

![](/Active-Directory/ws-15.png)
       
15. Hacemos clic en Instalar para iniciar el proceso de promover nuestro servidor a controlador de dominio.
       
16. El sistema se reiniciará para guardar la configuración y veremos en el panel del Administrador del Servidor que ya contamos con las opciones relacionadas al Directorio Activo.
       
En caso de que necesitemos borrar un controlador de dominio de nuestra estructura de red, debemos seguir los siguientes pasos:

1. Entramos al menú Administrar y elegimos "Quitar roles y funciones".

2. Se desplegará la ventana informativa, Hacemos clic en Siguiente

3. Seleccionamos el servidor donde tenemos instalado el rol de AD DS

4. En la ventana de Roles de servidor seleccionamos la opción Servicios de dominio de directorio activo (debe quedar desactivada), quitamos las características de AD DS.

5. Hacemos clic en siguiente, seleccionamos las características que deseemos eliminar y hacemos clic en siguiente, se mostrará un resumen de la tarea que estamos por ejecutar y hacemos clic en Quitar.

## Creación y administración de usuarios y equipos ##

Podemos crear un usuario a través de diferentes herramientas como lo son: Los asistentes de la consola de AD DS, el comando DSADD.EXE, PowerShell entre otros, en nuestro ejemplo usaremos el asistente de AD DS. Para ello vamos a realizar los siguientes pasos:

Ingresamos a Administre el servidor, elegimos en el menú Herramientas la opción Centro de administración de Active Directory:

Se desplegará la consola del centro de administración de AD.

Al seleccionar nuestro servidor en el panel lateral izquierdo podremos ver que se despliegan una serie de opciones, como por ejemplo, equipos, usuarios, sistema, dispositivos TPM entre otros.

En el panel lateral derecho, en la parte inferior veremos que se abren las opciones referentes al objeto Users:

Allí podremos elegir lo que deseemos crear, usuarios, equipos, grupos, etc. Elegimos Usuario y podremos ver cómo cambia la estructura de creación comparada con versiones anteriores (a nivel de apariencia).

![](/Active-Directory/ad-3.png)

Una vez tengamos los cambios necesarios en el perfil del usuario damos clic en Aceptar para guardar dicha configuración.

![](/Active-Directory/ad-5.png)

Si deseamos crear nuestros usuarios o equipos a través de cmd, podemos ejecutar el comando DCA.MSC o dirigirnos al menú Herramientas y elegir la opción Usuarios y equipos de Active Directory.

El mismo procedimiento realizamos para la creación de cualquier otro objeto en nuestro Directorio Activo, equipos, OUs, etc. Por ejemplo para la creación de un equipo nos dirigimos al menú inferior derecho del Centro de administración de Active Directory y elegimos Nuevo / Equipo.

En la ventana desplegada ingresamos información del equipo, así mismo indicamos a que dominio se va a incluir dicho equipo y para guardar los cambios damos clic en Aceptar.

![](/Active-Directory/ad-4.png)

En el centro de administración de Active Directory podemos gestionar otras funciones de nuestro rol, en el panel lateral derecho podemos encontrar lo siguiente:

* Cambiar el controlador de dominio: nos da la posibilidad de modificar nuestro DC.

* Elevar el nivel funcional del bosque: nos permite elevar la funcionalidad del bosque de nuestro servidor.

* Elevar el nivel funcional del dominio: da la posibilidad de elevar la funcionalidad de nuestro dominio.

* Habilitar la papelera de reciclaje: permite recuperar objetos eliminados en AD

### **Creación y administración de OUs (unidades organizacioneales)** ###

Dentro de nuestro dominios podemos crear Unidades Organizacionales para tener un control más específico de los usuarios u objetos, así mismo podemos crear diferentes perfiles para cada OU. 

Para ello abrimos el Centro de administración de Active Directory desde el menú Herramientas y elegiremos del panel lateral derecho la opción Nuevo / Unidad Organizativa.

Allí debemos especificar el nombre de la OU (en este caso usemos Pruebas) y podemos agregar comentarios y en la opción superior derecha Crear en: podemos definir en qué parte de nuestro Árbol vamos a crear dicha OU.

Podríamos incluir Pruebas dentro de la OU:

Ingresamos los datos en la ventana del Centro de administración:

Podremos ver que nuestra OU Pruebas ha sido creada correctamente dentro de nuestro árbol:

### **Creación y administración de grupos en AD** ###

Cuando un usuario inicia sesión en el dominio, Active Directory crea un token o una especie de identidad que indica automáticamente en que grupos está incluido este usuario; Un grupo puede crearse para fines distintos, por ejemplo, para acceder a ciertas rutas, para poder imprimir, para que le lleguen determinados correos etc.

Existen básicamente dos tipos de grupos:
    
* Distribución: Son creados básicamente para el envío de información a una o más personas
* Seguridad: Son creados para garantizar el acceso a los recursos de la organización.

Para crear nuestro grupo realizaremos el siguiente proceso, primero nos dirigimos al Centro de administración de Active Directory, en el panel derecho elegimos Nuevo y luego Grupo. A continuación se desplegará la siguiente ventana donde elegiremos el tipo de grupo, ingresar nombre, entre otros parámetros (En nuestro caso elegiremos un grupo de Seguridad con ámbito global):

En este menú también podremos elegir la ubicación de nuestro grupo y damos clic en Aceptar. 

Las principales diferencias entre los diferentes ámbitos de grupos: 

* **Dominio Local**

Pueden incluir miembros de los siguientes tipos: Usuarios, equipos, grupos globales, grupos universales y grupos de dominio local que estén dentro del mismo dominio.
 
* **Universal**

Pueden albergar: Usuarios, equipos, grupos globales y otros grupos universales.
 
* **Global**

Pueden albergar Usuarios, equipos y otros grupos globales dentro del mismo dominio.


