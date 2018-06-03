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

# Instalación de LXC/LXD  

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
* 