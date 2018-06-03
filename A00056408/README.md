# Proyecto   
**Nombres:**  
Yesid Leonardo López  
Víctor Andrés Calambas Hurtado  
**Código:**  
A00056408  
A00059956  
**URL Repositorio:** 

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

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_005.png)

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_007.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_008.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_009.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_010.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_012.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_013.png)
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/Selección_014.png)
------------------------------------------------------------------------

```console
operativos@Ubuntu:~$ newgrp lxd
```

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/)

```console
operativos@Ubuntu:~$ 
```


# Bibliografía  
* Instalación y configuración de lxd: https://tutorials.ubuntu.com/tutorial/tutorial-setting-up-lxd-1604#1  
* Definición de Storage pool: https://www.quora.com/What-is-a-storage-pool  
* ZFS: https://en.wikipedia.org/wiki/ZFS
* ZFS: https://www.redeszone.net/2016/10/01/zfs-las-caracteristicas-este-sistema-archivos-avanzado/
* Ventajas ZFS: https://www.genbeta.com/mac/zfs-un-repaso-al-sistema-de-ficheros
* ZFS en Ubuntu:https://bayton.org/docs/linux/lxd/lxd-zfs-and-bridged-networking-on-ubuntu-16-04-lts/
* Caracteristicas de ZFS: https://www.genbeta.com/mac/zfs-un-repaso-al-sistema-de-ficheros
