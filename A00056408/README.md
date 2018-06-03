# Proyecto   
**Nombre:** Yesid Leonardo López  
**Código:** A00056408  
**URL Repositorio:** 
# Objetivo General  

Realizar el despliegue de dos servidores web y un balanceador de carga por medio de contenedores LXC/LXD. 
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/objetivos.png)  

# Aprovisionamiento de la máquina virtual  

Se usará como sistema operativo Ubunto en su versión de [16.04.1](http://releases.ubuntu.com/16.04/). El aprovisionamiento se ve en las siguientes imagenes:  

Especificaciones generales:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuGeneral.png)  

Placa base:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuPlaca.png)  

Imagen:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuImagen.png)  

Adaptador de red:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuAdaptor.png)  

Procesadores utilizados:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuProcesadores.png)  

Configuración NAT:  
![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/ubuntuRed1.png)

# Instalación de LXD  

## Instalación de LXD  

Para instalarlo le damos permisos al usuario operativos. Para esto escribimos en consola ``$ vim /etc/sudoers``. Aquí se le daremos a `operativos` permisos sudo.  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/sudoers.png)

Ahora, instalaremos LXD. Para esto escribimos en consola el siguiente comando:  
```console
operativos@Ubuntu:~$ sudo apt-get install LXD 
```

LXD ya está instalado en Ubuntu por defecto, sin embargo, necesitamos configurar el usuario operativos para administrar contenedores, después configuramos el tipo de almacenamiento para guardar los contenedores y configurar la red.  

Instalamos lxd con el siguiente comando: 

```console
operativos@Ubuntu:~$ sudo apt-install lxd
```

Adicionamos el usuario al grupo lxd, de esta manera podremos usar el usuario operativos para realizar todas las tareas de administración de contenedor.  


```console
operativos@Ubuntu:~$ sudo adduser operativos lxd
```

El nuevo grupo será efectiva en la siguiente sesión, por lo tanto, para aplicar los cambios en la shell actual ejecutamos: 

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
* El sistema es autorreparable: de forma que si se detecta que una parte de un fichero se ha corrompido se busca en otro disco y si esta es buena se sobreescribe la que era incorrecta de forma que ahora ambas serán buenas.  
* El modelo transaccional viene dado por el uso de copy-on-write, una técnica con la que no se sobreescriben los datos en el disco cuando se modifican, sino que se crean nuevos bloques donde se graban y posteriormente se modifican las estructuras para que apunten a estos bloques nuevos.  
* Esto permite, además, la existencia de snapshots y clones. Los snapshots son copias del sistema de ficheros en un determinado momento. Son solo de lectura, pero su creación es muy rápida, lo que permite hacer copias de seguridad de forma casi inmmediata. Los clones son snapshots en los que se puede escribir, de forma que se crean dos sistemas de ficheros que comparten bloques en el disco, ahorrando espacio.  
* la creación de un sistema de ficheros es una operación muy ligera. Esto, junto al hecho de que no existen cuotas de espacio por usuario sino por sistema de ficheros propicia que se creen sistemas de ficheros para cada usuario en lugar de directorios dentro de un mismo sistema de ficheros. También ayuda a esto el que la gestión de los sistemas de ficheros sea mucho más sencilla que con otros sistemas existentes.  
* ofrece compresión transparente al usuario, lo que maximiza el espacio disponible en disco y, muchas veces, la velocidad de lectura y que, por ahora, no ofrece cifrado transparente de datos, aunque desde Sun están trabajando en ello.  

Instalamos la ZFS con el siguiente comando: 

```console
operativos@Ubuntu:~$ sudo apt-get install zfsutils-linux
```

## Default LXD bridge configuration  

Para realizar la configuración inicial de lxd usamos el siguiente comando:  
```console
operativos@Ubuntu:~$ sudo lxd init
```
Damos enter para dejar los datos por defecto como se ve en la siguiente:  

![](https://github.com/leonleo997/so-project/blob/yesid/A00056408/Images/default.png)

En la imagen anterior, se configura primero un nuevo storage pool. 

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
* 