# Proyecto Final Sistemas Operativos
**Nombres:**  
Yesid Leonardo López  
Víctor Andrés Calambas Hurtado  
**Código:**  
A00056408  
A00059956  
**URL Repositorio:** 
https://github.com/leonleo997/so-project.git

# Objetivo General  

Realizar el despliegue de dos servidores web y un balanceador de carga por medio de contenedores LXC/LXD. 
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/objetivos.png)  

# Aprovisionamiento de la máquina virtual  

Se usará como sistema operativo Ubunto en su versión de [16.04.1](http://releases.ubuntu.com/16.04/). Para ello en las especificaciones generales, en la pestaña de General le asignamos un nombre a la maquina virtual, en nuestro caso fue llamada ubuntu que es el nombre de Sistema Operativo que vamos a instalar, luego seleccionamos el tipo de sistema que es basado en una distribución de Linux, y por ultimo seleccionamos la version de Ubuntu de (64 bit)La vista inicial con la configuración mencionada se puede ver en la siguiente imagen:  

Especificaciones generales:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuGeneral.png)  
Luego para tener un gran desempeño en la maquina virtual que hemos instalado en uno de los computadores de la universidad, le asignamos una cantidad de RAM un poco superior a las 8 GB de memoria ya que estos computadores tienen alrededor de 21 GB el rendimiento no se va a ver afectado.
Placa base:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuPlaca.png)  
Luego le asignamos a la maquina virtual la cantidad de procesadores que van a funcionar con nuestra maquina, utilizamos la cantidad necesaria para realizar la ejecucion del proyecto, las cuales fueron asignadas a la maquina 4 procesadores y el limite de ejecucion quedo al 100% para que el rendimiento de los procesadores de la maquina sean exclusivamente para el proyecto 
Procesadores utilizados:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuProcesadores.png)  
Luego procedemos a asignar un tamaño de disco duro virtual de 30GB que es una configuración por defecto que nos recomienda

Imagen:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuImagen.png)  

La configuración de la pestaña de Red del adaptador 1 es la conexion mediane NAT
Configuración NAT:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuRed1.png)

Luego en la pestaña de Red seleccionamos la configuración de adaptador de tipo puente como se puede evidenciar en la siguiente imagen.
Adaptador de red:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuAdaptor.png)  


# Instalación de LXD  

## Instalación de LXD  

Para poder instalar lxd le damos permisos al usuario operativos que creamos anteriormente . Para realizar esto escribimos en consola ``$ vim /etc/sudoers``. Aquí con el comando le daremos a `operativos` permisos de instalacion de tipo sudo.  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/sudoers.png)

Ahora, procedemor a la instalación de LXD. Para esto escribimos en consola el siguiente comando:  
```console
operativos@Ubuntu:~$ sudo apt-get install LXD 
```

Debido a que LXD ya se encuentra instalado en Ubuntu por defecto, necesitamos asegurarnos de que se encuentre instalado con las versiones más recientes, ya que necesitamos configurar el usuario operativos para administrar contenedores, y después configuramos el tipo de almacenamiento para guardar los contenedores y configurar la red.  

Procedemos a Instalar lxd con el siguiente comando: 

```console
operativos@Ubuntu:~$ sudo apt-install lxd
```

Luego adicionamos el usuario al grupo lxd, esta adición es necesaria para que podamos usar el usuario operativos para realizar todas las tareas de administración de los contenedores. Para hacer esta acción utilizamos el siguiente comando:  


```console
operativos@Ubuntu:~$ sudo adduser operativos lxd
```

Debido a que el nuevo grupo será efectiva en la siguiente sesión, por lo tanto, para aplicar los cambios en la shell actual ejecutamos el comando: 

```console
operativos@Ubuntu:~$ newgrp lxd
```  
## Storage pool  

Existen varias definiciones para este concepto en varias implementaciones. Sin embargo, el más genérico se refiere a la capacidad de almacenamiento combinada que se obtiene combinando uno o más dispositivos de almacenamiento. AL referirnos con dispositivos de almacenamiento se puede referir desde algo tan pequeño como un Hard Disk Drive a los arreglos de Hard Disk Drives.

Un storage pool es generalmente creado para tener una capacidad grande de GBs/TBS/PBs disponible desde el cual los usuarios proveen pequeñas capacidades de almacenamiento.

Por ejemplo, podemos combinar 10 Hard Disk Drives de 3TB cada uno, teniendo un total de 30TBs.


## ZSF  

Es un sistema de archivo combinado y un administrador de volumen lógico diseñado por Sun Microsystem. Este es descrito por los analistas como "el único sistema de archivo empresarial de validación de datos Open Source". El ZSF es escalable e incluye protección extensiva contra la corrupción de datos, soporta altas capacidades de almacenamiento, compresión de daros eficientes, integración de conceptos de filesystem y administración de volumen, snapshots y clones copy-on-write.
Este sistema de archivos cuenta con un gran número de medidas de protección de datos con sistemas de integridad contra la pérdida y corrupción, lo que le hace ideal para funcionar en grandes centros de datos y dispositivos NAS y, aunque está optimizado para sistemas de discos RAID, en los últimos meses está ganando una gran importancia también a nivel de usuario en escritorios convencionales.

## Ventajas ZSF  

* Ofrece integridad de los datos comprobable, de forma que todos los datos en disco sean correctos al usar un modelo transaccional para realizar las escrituras.   
* El modelo transaccional viene dado por el uso de copy-on-write, una técnica con la que no se sobreescriben los datos en el disco cuando se modifican, sino que se crean nuevos bloques donde se graban y posteriormente se modifican las estructuras para que apunten a estos bloques nuevos.  
* Esto permite, además, la existencia de snapshots y clones. Los snapshots son copias del sistema de ficheros en un determinado momento. Son solo de lectura, pero su creación es muy rápida, lo que permite hacer copias de seguridad de forma casi inmmediata. Los clones son snapshots en los que se puede escribir, de forma que se crean dos sistemas de ficheros que comparten bloques en el disco, ahorrando espacio.  
* la creación de un sistema de ficheros es una operación muy ligera. Esto, junto al hecho de que no existen cuotas de espacio por usuario sino por sistema de ficheros propicia que se creen sistemas de ficheros para cada usuario en lugar de directorios dentro de un mismo sistema de ficheros. También ayuda a esto el que la gestión de los sistemas de ficheros sea mucho más sencilla que con otros sistemas existentes.  
* ofrece compresión transparente al usuario, lo que maximiza el espacio disponible en disco y, muchas veces, la velocidad de lectura y que, por ahora, no ofrece cifrado transparente de datos, aunque desde Sun están trabajando en ello.
* El sistema de archivos cuenta con gran cantidad de medidas de protección de datos con sistemas de integridad contra la pérdida y corrupción de los archivos.
* Está optimizado para el uso de almacenamiento de disco que utiliza la tecnología RAID, lo cual es importante porque se proyecta que con muchas unidades de almacenamiento se construyan grandes volúmenes de información.
* Actualmente hay dificultad de hacer “borrón y cuenta nueva” en los servidores, por lo que con los ficheros a una velocidad mayor no es necesario hacer ese cambio cuando se sature el rendimiento del sistema.
* Todo el espacio de almacenamiento se comparte entre los diferentes sistemas de ficheros.
* El sistema es autorreparable, ya que si se detecta que una parte de un fichero ha sido corrompida se busca en otro disco y se sobreescribe la que está incorrecta con el objetivo de corregir el archivo corrupto.



Instalamos la ZFS con el siguiente comando: 

```console
operativos@Ubuntu:~$ sudo apt-get install zfsutils-linux
```

## Default LXD bridge configuration  

Para realizar la configuración inicial de lxd usamos el siguiente comando:  
```console
operativos@Ubuntu:~$ sudo lxd init
```

Luego nos aparece un mensaje que nos dice si buscamos configurar un nuevo pool de conexiones, la imagen es la siguiente:


![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_005.png)

Escribimos en consola respondiendo yes o No segun los parametos por defecto, para no modificar los valores asignados por defecto simplemente damos Enter y estos resultados se evidencian en la siguiente imagen:  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/default.png)

En la imagen anterior, se empieza la configuración de un nuevo storage pool. 

Las siguientes imagenes hacen referencia a las configuraciones del nombre del puente, la IPv4, la mascara de red, direccion DHCP, el trafico de NAT. Obviamos la configuracion de red IPv6 dando clic en No como se evidencian en las siguientes imagenes:

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_007.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_008.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_009.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_010.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_001.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_002.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_003.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_013.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_014.png)  


Luego de que el pool ha sido configurado satisfactoriamente saldrá la siguiente imagen  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_015.png) 



# Creación de contenedores con servicio web

## Instalación y configuración del servicio web  

 

Luego procedemos a crear un contenedor de ubuntu con el nombre webserver y lo iniciamos tambien con el siguiente comando  
```console
lxc launch ubuntu:16.04 webserver
```  
La salida resultante de ese comando se evidencia acontinuación con el inicio y la creación del webserver:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_017.png)  

Utilizamos el comando ``lxc list `` para mirar que el webserver esta corriendo  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_005.png)  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_019.png)  

Luego procedemos a crear otro contenedor de ubuntu con el nombre webserver2 y lo iniciamos tambien con el siguiente comando  
```console
lxc launch ubuntu:16.04 webserver2
```
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_020.png)  
Utilizamos de nuevo el comando ``lxc list `` para mirar que el webserver2  esta corriendo perfectamente y ejecutamos el comando para mirarlo en funcionamiento  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_006.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_022.png)  

Luego procedemos a entrar a la carpeta bash donde estan los webservers  
```console
lxc exec webserver2 --/bin/bash
```
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_024.png)  

## Página html que despliegue un mensaje que identifique el servicio web correspondiente  

Para realizar el despliegue que pueda identificar el servicio web correspondiente primero nos aseguramos de que se hayan actualizado todos los archivos de nuestra maquina virtual con el siguiente comando:   
```console
sudo apt-get update
```  

Luego procedemos a realizar la instalación de nginx, el cual es el que nos permite realizar el despliegue en una pagina web  
```console
sudo apt-get install nginx
```  
Luego hacemos la configuración del nginx que acabamos de instalar, con el comando a continuación :  
```console
sudo vim /var/www/html/index.nginx-debian.html
```  

-----Pantallazo pendiente
...-.-.-.-.-.-.-.-.-.

## Asignar un procesador único para cada servicio web  

Los siguientes comandos son utilizados para asignar un procesador a cada uno de los servicios web. El comando esta asignanadole al servicio web que nombramos webserver y webserver2 un procesador para su ejecución. Los comando utilizados son los siguientes:    
```console
lxc config set webserver limits.cpu 1
lxc config set webserver2 limits.cpu 1
```  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_025.png)  

El siguiente comando es utilizado para realizar la validación de que el servicio web se encuentra activo

```console
systemctl | grep lxd
```  
# Creación de contenedor con servicio de balanceo de carga

Procedemos a crear y e iniciar el balanceador de carga que fue llamado `loadbalancer` con el siguiente comando  
```console
lxc launch ubuntu:16.04 loadbalancer
```
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_007.png)

Procedemos a configurar el servicio del nginx del balanceador de carga como se ve en la siguiente imagen:  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_041.png)  


La siguiente imagen muestra el funcionamiento de el balanceador de carga
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_028.png)  

Luego editamos la pagina que viene con la configuración por defecto de los webservers como se ven en las siguientes dos imagenes:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_011.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_012.png)  

Luego procedemos a realizar la configuración como se había realizado anteriormente asignandole un procesador a cada ejecución con el comando anterior :  
```console
lxc config set webserver limits.cpu 1
lxc config set webserver2 limits.cpu 1
``` 
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_031.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_033.png)  

Y procedemos a reinicirar el servicio con el comando restart que vemos acontinuación:  
```console
sudo service nginx restart
``` 
Luego de que se haya creado el balanceador de carga y de haber realizado las configuraciones mostradas previamente en imagenes realizamos la configuración `Ngix`, previamente habiamos configurado los webservers para los cuales va a funcionar el balanceador. Procedimos a realizar la configuración para recibir conexiones remotas y con el siguiente comando aseguramos de que el servicio del grupo que creamos al inicio quede activo de la forma correcta, el comando utilizado es:  

```console
systemctl list-unit-files --state=enabled | grep lxd
```
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_008.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_036.png)  

La siguiente imagne muestra la configuración de la IPs de los servidores con los que va a funcionar el balanceador de cargas
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_013.png)  


La siguiente imagen muestra el resultado del comando que veremos a continuación:  
```console
systemctl | grep lxd
```
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_016.png)  

# Salida del comando lxc list con los contenedores creados y sus direcciones Ip  

La siguiente imagen muestra el correcto funcionamiento del balanceador de carga y los 2 webservers que habíamos creado antes.  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_009.png)  

# Pruebas del funcionamiento del balanceador
 ### Pruebas haciendo uso del comando curl con la respuesta de cada uno de los servicios web a través del balanceador

Las siguientes imagenes muestra a los contenedores y el balanceador de cargas funcionando en perfecto estado, y el registro del balanceador de cargas en funcionamiento
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_037.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_038.png)  

Las siguientes imagenes muestran el uso del comando ``curl`` a las IPs que hacen referencia a la configuración realizada de los dos webservers y a la IP que hace referencia a el balanceador de carga
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_017.png)  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selection_018.png)  


### Pruebas por medio de una herramienta de test de stress (siege)

Debido a unos problemas en la exportación  de la maquina virtual las siguientes pruebas fueron realizadas con una IP diferente debido a que los pasos que habíamos realizado anteriormente, nos toco volverlos a realizar en una nueva maquina virtual. 


La siguiente imagen muestra el desempeño del sistema con la configuración del servidor web con 64 Mb  
imagen 64Mb  

La siguiente imagen muestra el desempeño del sistema con la configuración del servidor web con 64 Mb  
imagen 128Mb  

La siguiente imagen muestra el desempeño del sistema con la configuración del servidor web con 64 Mb  
imagen 50% CPU  

La siguiente imagen muestra el desempeño del sistema con la configuración del servidor web con 64 Mb  
imagen 100% CPU  



El siguiente comando realiza una prueba de estres en la cual simula el comportamiento de 200 usuarios en un lapso de tiempo de 20 segundos, la dirección IP a la cual se somete a prueba hace referencia a la nueva IP que fue asignada al nuevo balanceador de carga  

```console
siege -c 200 -t 20 http://10.45.80.209
```  


## Pregunta Random

# Al reiniciar la máquina virtual en que estado quedan los contenedores?

Cuando se reinicia una maquina virtual, el estado de los contenedores es el mismo, debido a que para cambiar el estado de los contenedores es necesario utilizar comandos como 'stop' para pararlos o 'start' para que sigan corriendo normalmente. Los comandos de los contenedores en su mayoria es 'lxc comando nombreDelContenedor' los comandos para cambiar de estado a los contenedores se encuentran en la referencia con titulo commands to containers in linux.


# Bibliografía  
* Instalación y configuración de lxd: https://tutorials.ubuntu.com/tutorial/tutorial-setting-up-lxd-1604#1  
* Definición de Storage pool: https://www.quora.com/What-is-a-storage-pool    
* ZFS: https://en.wikipedia.org/wiki/ZFS  
* ZFS: https://www.redeszone.net/2016/10/01/zfs-las-caracteristicas-este-sistema-archivos-avanzado/  
* Ventajas ZFS: https://www.genbeta.com/mac/zfs-un-repaso-al-sistema-de-ficheros  
* ZFS en Ubuntu:https://bayton.org/docs/linux/lxd/lxd-zfs-and-bridged-networking-on-ubuntu-16-04-lts/  
* Caracteristicas de ZFS: https://www.genbeta.com/mac/zfs-un-repaso-al-sistema-de-ficheros  
* How to use siege in linux: https://www.linode.com/docs/tools-reference/tools/load-testing-with-siege/
* Commands to containers in linux : https://linuxcontainers.org/lxd/getting-started-cli/ 
