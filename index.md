# Práctica 1. Configuración de máquina virtual en el IaaS
### Andrea Hernández Martín - alu0101119137
[Enlace a la Github Page](https://ull-esit-inf-dsi-2021.github.io/ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137/)

## Introducción
En esta práctica llevaremos a cabo la configuración de la máquina virtual que tenemos disponible a través del servicio IaaS de la ULL, además de la instalación y configuración de todas las herramientas necesarias para comenzar a trabajar en la asignatura Desarrollo de Sistemas Informáticos.

## Configuración de la máquina virtual en el IaaS
En primer lugar, debemos configurar el servicio VPN de la ULL para poder acceder al Iaas en caso de que estemos intentando utilizar el servicio fuera de la red universitaria.  
    
Una vez conectados a la VPN, tenemos que acceder a nuestro Servicio Iaas de la ULL mediante nuestas credenciales, una vez dentro del Servicio tenemos que encender la máquina virtual de la asignatura llamada DSI, a la que se le asignará un número una vez encendida (en mi caso, DSI-35).  
  
A continuación, dentro de nuestra máquina, en el apartado de *interfaces de red*, obtenemos nuestra dirección IP para poder conectarnos a nuestra máquina mediante SSH. Para ello, abrimos una terminal y escribimos lo siguiente, donde el nombre de usuario de la máquina virtual es 'usuario' y la contraseña es 'usuario'. Una vez hecho esto, nos pedirá que cambiemos la contraseña de la máquina virtual, en primer lugar pide la contraseña actual 'usuario' y a continuación, repetir dos veces la nueva contraseña.
```shh usuario@10.6.129.216 ```



## Instalación de git y Node.js en la máquina virtual del IaaS
## Conclusiones
## Bibliografía
