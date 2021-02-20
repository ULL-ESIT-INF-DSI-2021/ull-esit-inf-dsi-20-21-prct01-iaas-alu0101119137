# Práctica 1. Configuración de máquina virtual en el IaaS
### Andrea Hernández Martín - alu0101119137
[Enlace a la Github Page](https://ull-esit-inf-dsi-2021.github.io/ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137/)

## Introducción
En esta práctica llevaremos a cabo la configuración de la máquina virtual que tenemos disponible a través del servicio IaaS de la ULL, además de la instalación y configuración de todas las herramientas necesarias para comenzar a trabajar en la asignatura Desarrollo de Sistemas Informáticos.

## Configuración de la máquina virtual en el IaaS
**1. Configuración del servicio VPN de la ULL y acceso al servicio IaaS.**  
En primer lugar, debemos configurar el servicio [VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/) para poder acceder al Iaas en caso de que estemos intentando utilizar el servicio fuera de la red universitaria.  
    
Una vez conectados a la VPN, tenemos que acceder a nuestro [Servicio Iaas de la ULL](https://www.iaas.ull.es) mediante nuestas credenciales. A continuación, dentro del Servicio tenemos que encender la máquina virtual de la asignatura llamada DSI, a la que se le asignará un número una vez encendida (en mi caso, DSI-35).  
  
**2. Obtención de la dirección IP de la máquina y acceso mediante SSH.**  
A continuación, dentro de nuestra máquina, en el apartado de *interfaces de red*, obtenemos nuestra dirección IP (de la forma 10.6.XXX.XXX) para poder conectarnos a nuestra máquina mediante SSH. Para ello, abrimos una terminal en nuestra máquina local y escribimos el nombre de usuario de la máquina virtual que es 'usuario' y la dirección IP de nuestra máquina virtual. A continuación, pedirá la contraseña que es 'usuario'.  
```
shh usuario@10.6.129.216 
```  
Una vez hecho esto, nos pedirá que cambiemos la contraseña de la máquina virtual, en primer lugar pide introducir la contraseña actual 'usuario' y a continuación, repetir dos veces la nueva contraseña que decidamos establecer. Al cambiar la contraseña, cierra la sesión de la máquina virtual, por lo que tendremos que volver a conectarnos a ella con la nueva contraseña establecida.

**3. Modificación del nombre del host de la máquina virtual.**  
Para cambiar el nombre del host tenemos que modificar el fichero */etc/hostname*, en el que el nombre actual del host es ubuntu, y poner el nombre que se quiera, en mi caso iaas-dsi35, de la siguiente forma:
```
usuario@ubuntu:~$ cat /etc/hostname
ubuntu
usuario@ubuntu:~$ sudo vim /etc/hostname

usuario@ubuntu:~$ cat /etc/hostname
iaas-dsi35
```  
Además de modificar el fichero /etc/hostname tenemos que modificar también el fichero */etc/hosts*, en el que cambiaremos también el nombre del host al establecido anteriormente (iaas-dsi35):  
```
usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...

usuario@ubuntu:~$ sudo vi /etc/hosts

usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	iaas-dsi35
...
```
**4. Actualización de los cambios y reinicio de la máquina virtual.**  
En primer lugar, para que los cambios tengan efecto, debemos actualizar el software de la máquina virtual de la siguiente forma:
```
usuario@ubuntu:~$ sudo apt update
...
usuario@ubuntu:~$ sudo apt upgrade
...
```  
Y en segundo lugar, reiniciamos la máquina:
```
usuario@ubuntu:~$ sudo reboot
Connection to 10.6.XXX.XXX closed by remote host.
Connection to 10.6.XXX.XXX closed.
```  
**5. Edición fichero hosts en la máquina local.** 
A continuación, editamos el fichero de hosts de la máquina local añadiendo la información de conexión a la máquina virtual, es decir, la dirección IP de la máquina virtual y su nombre:  
```
andrea@andrea-laptop:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	andrea-laptop
...

andrea@andrea-laptop:~$ sudo vim /etc/hosts

andrea@andrea-laptop:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	andrea-laptop

10.6.XXX.XXX	iaas-dsi35
...
```  
**6. Configuración de la clave pública-privada en la máquina local.**  
En primer lugar, comprobamos si ya tenemos generada la clave pública-privada de esta forma:  
```
andrea@andrea-laptop:~$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDFkn0P3gkkmuwS4T4yGoeScoNWEv9hSH0tRszmWSEfxKszhkgRFAtM91RAMI1AigvOb8IRbcq7jXWDONlJs+Bvuq8Q9btGpHFyG3H2Y2dXeZ+Bvker6KvfHuRqJLlde57YYghcs2juj23oFQ3RU++B7IF31DvazDvvLHcG9oVf9ajdTxqM69WYhy8GaaIBVy4ywL84sCV2iWEIgP2qqN2p2Uxg4GXZo5ODO4uKJCG11551MHwA81RTluiX+W+wEPG5mDx3dkbD5rWjWYiAAWIqybFB0iondnAcuH1pOpD3S9qiAe3CbibpzcWwVEqw0CWntFfBrJ6CB4WLLqG9DBSQsCRKfNEUAHZujrpZGlbrevNwvZ/vsD2jGIkQpEUXqJn4crViuj7MotYUpcQ3eltPhpsSTco6H80t2O2iudoefDl0VmIVb9sGjsd0YjyRxrX3xyieypNgPPu29n5pIyFRRNWOVD1ycr0T+9ZlMCzWWnfy8x/CxLzeeEVUchAve9c= andrea@andrea-laptop
```
Como podemos comprobar con lo anterior, observamos que ya había generado previamente mi clave pública-privada.  
En caso de no tenerla generada, lo anterior indica que el fichero no existe, por lo que tenemos que crearla. Por lo general, se toman los valores por defecto del script, para ello pulsamos *enter* cada vez que nos pida introducir algún dato. Es importante no introducir ningún *passphrase*. La clave se crea con lo siguiente:
```
andrea@andrea-laptop:~$ ssh-keygen
```  


## Instalación de git y Node.js en la máquina virtual del IaaS
## Conclusiones
## Bibliografía
