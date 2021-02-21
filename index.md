# Práctica 1. Configuración de máquina virtual en el IaaS
### Desarrollo de Sistemas Informáticos
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
Además de modificar el fichero */etc/hostname* tenemos que modificar también el fichero */etc/hosts*, en el que cambiaremos también el nombre del host al establecido anteriormente (iaas-dsi35):  
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
En caso de no tenerla generada, lo anterior indicaría que el fichero no existe, por lo que tendríamos que crearla. Por lo general, se toman los valores por defecto del script, para ello pulsamos *enter* cada vez que nos pida introducir algún dato. Es importante no introducir ningún *passphrase*. La clave se crea con el comando siguiente:
```
andrea@andrea-laptop:~$ ssh-keygen
```  
**7. Copia de la clave pública-privada de la máquina local en la máquina virtual e inicio de sesión.**  
Con el siguiente comando copiaremos la clave generada de la máquina local en la máquina virtual, con esto conseguiremos conectarnos mediante SSH a la máquina virtual sin tener que recordar la dirección IP y sin tener que introducir la contraseña, solo nos haría falta el nombre de usuario y el nombre de la máquina virtual. Cuando ejecutamos ese comando nos sale un mensaje de si estamos seguros de que queremos conectarnos, tenemos que introducir 'yes' y a continuación, pide introducir la contraseña de nuestra máquina virtual. Una vez hecho esto, nos dice que se ha añadido la clave. 
```
andrea@andrea-laptop:~$ ssh-copy-id usuario@iaas-dsi35
```  
Ahora probamos a iniciar sesión con 'usuario@iaas-dsi35' y efectivamente, comprobamos que podemos conectarnos sin tener que introducir la dirección IP ni la contraseña.  
```
andrea@andrea-laptop:~$ ssh usuario@iaas-dsi35
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Feb 20 20:23:56 WET 2021
  ...
```  
**8. Edición del fichero de configuración SSH de la máquina local.**  
Para no tener que introducir el usuario a la hora de conectarnos mediante SSH, editamos el fichero *~/.ssh/config*:
```
andrea@andrea-laptop:~$ vim ~/.ssh/config 
andrea@andrea-laptop:~$ cat ~/.ssh/config 
Host iaas-dsi35
HostName iaas-dsi35
User usuario
```  
Con lo anterior conseguimos iniciar sesión a la máquina virtual de la siguiente forma:  
```
andrea@andrea-laptop:~$ ssh iaas-dsi35
```  
**9. Generación de la clave pública-privada en la máquina virtual.**  
Ahora tenemos que generar la clave pública-privada como hicimos en el paso anterior 6, pero en este caso lo hacemos en la máquina virtual.  
```
usuario@iaas-dsi35:~$ cat .ssh/id_rsa.pub
Generating public/private rsa key pair.
...
usuario@iaas-dsi35:~$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKrOe8u+BaaAUqDjfiqvoehIcl4nd2xSXxnBT7OaBwpZXisgdBn6CLZnfPYDGNhTbxBnbaWj+ljLPr6Znzw7hguH6f8ci36x5+Z+5h869ujaABAlr6MdpMxy6NMYxOJ22Dt9OBucremwLGYHAsMd+wju0DxEUUJcRM3JxuAjIfd0s4W44oCuzZQxsunx6K4PMLF4huTa2zIlKespXpj+Ho1jwDgtshHg/grdY+FtKTMA5GHtjtR8Ig17qVwGonoe7EjAh5duDAKQuD5TpyhIg7pnXGj69our4cftMElWNwyY9sLktM9HXKO8OJSj/mHDOBu+519zvB1CJ7NS9TW+CR usuario@iaas-dsi35
```  
## Instalación de git y Node.js en la máquina virtual del IaaS  
**1. Instalación y configuración de Git en la máquina virtual**  
Ejecutamos el siguiente comando para instalar Git:
```
usuario@iaas-dsi35:~$ sudo apt install git
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho
git ya está en su versión más reciente (1:2.17.1-1ubuntu0.7).
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```
Y a continuación, ejecutamos los siguientes comandos para su configuración:
```
usuario@iaas-dsi35:~$ git config --global user.name "Andrea Hernandez"
usuario@iaas-dsi35:~$ git config --global user.email alu0101119137@ull.edu.es
usuario@iaas-dsi35:~$ git config --list
user.name=Andrea Hernandez
user.email=alu0101119137@ull.edu.es
```  
**2. Configuración del prompt de la terminal de la máquina virtual**  
Configuraremos el prompt de la terminal para que aparezca la rama actual en la que nos encontramos cuando accedemos a algún directorio que resulta estar asociado a un repositorio git. Para ello, descargamos el script [git prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) o copiamos su contenido en un fichero que crearemos en nuestra máquina virtual llamado *git-prompt.sh*. Los pasos a realizar son descargarnos el script y, a continuación, modificar el fichero *~/.bashrc*, incluyendo al final del mismo las dos líneas que aparecen en el código siguiente. El último comando nos permite reiniciar la terminal. 
```
usuario@iaas-dsi35:~$ mv git-prompt.sh .git-prompt.sh
usuario@iaas-dsi35:~$ vim .bashrc
usuario@iaas-dsi35:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi35:~$ exec bash -l
[~()]$
```  
Como podemos observar en la última línea anterior, el prompt ha cambiado.
No obstante para comprobar realmente que el prompt muestra lo que deseamos, tendremos que añadir la clave pública de la máquina virtual en la configuración de las claves de nuestra cuenta de GitHub, así nos sera más facil trabajar con repositorios remotos y así también poder clonar el repositorio para hacer la prueba. Esto se realizará en el siguiente apartado.

**3. Configuración de las claves de nuestra cuenta de GitHub.**  
En primer lugar, tenemos que copiar la clave pública-privada de la máquina virtual, mediante el comando siguiente podemos ver la clave para copiarla.
``` 
[~()]$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKrOe8u+BaaAUqDjfiqvoehIcl4nd2xSXxnBT7OaBwpZXisgdBn6CLZnfPYDGNhTbxBnbaWj+ljLPr6Znzw7hguH6f8ci36x5+Z+5h869ujaABAlr6MdpMxy6NMYxOJ22Dt9OBucremwLGYHAsMd+wju0DxEUUJcRM3JxuAjIfd0s4W44oCuzZQxsunx6K4PMLF4huTa2zIlKespXpj+Ho1jwDgtshHg/grdY+FtKTMA5GHtjtR8Ig17qVwGonoe7EjAh5duDAKQuD5TpyhIg7pnXGj69our4cftMElWNwyY9sLktM9HXKO8OJSj/mHDOBu+519zvB1CJ7NS9TW+CR usuario@iaas-dsi35
```  
Una vez copiada la clave, accedemos a la configuración de nuesta cuenta de Github *(Account settings)*, una vez ahí, vamos a la sección de *SSH and GPG keys* y pulsamos en *New SSH key*. Nos pide un 'Title' para la SSH key (en mi caso puse usuario@iaas-dsi35) y en el campo 'Key' pegamos la clave de nuestra máquina virtual, pulsamos sobre Add SSH key y ya tendríamos la clave en nuestra cuenta de Github.  
Para comprobar que funciona, clonamos un repositorio desde la terminal mediante el siguiente comando:
```
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2021/ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137
Clonando en 'ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137'...
remote: Enumerating objects: 106, done.
remote: Counting objects: 100% (106/106), done.
remote: Compressing objects: 100% (96/96), done.
remote: Total 106 (delta 25), reused 0 (delta 0), pack-reused 0
Recibiendo objetos: 100% (106/106), 28.93 KiB | 352.00 KiB/s, listo.
Resolviendo deltas: 100% (25/25), listo.

[~()]$ls
ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137

[~()]$cd ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137/
[~/ull-esit-inf-dsi-20-21-prct01-iaas-alu0101119137(main)]$
```  
Como podemos observar, se ha clonado un directorio sin tener que introducir ningún credencial y además, el prompt del sistema indica la rama actual de trabajo.  
**4. Instalación de Node Version Manager (nvm).**  
Ahora vamos  a instalar Node Version Manager (nvm), el gestor de versiones de Node.js que es un entorno que permite la ejecución de código desarrollado en JavaScript y variantes.
```
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
[~()]$nvm --version
0.37.2
```
Para instalar la versión más reciente tenemos que hacer lo siguiente:
```
[~()]$nvm install node
...
Now using node v15.9.0 (npm v7.5.3)
[~()]$node --version
v15.9.0
[~()]$npm --version
7.5.3
```
**5. Instalación concreta de Node Version Manager.**  
Si queremos utilizar versión diferente de la que tenemos instalada, podemos instalar la otra de la siguiente forma:
```
[~()]$nvm install 12.0.0
...
Now using node v12.0.0 (npm v6.9.0)
[~()]$node --version
v12.0.0
[~()]$npm --version
6.9.0
```
Y para cambiar de versiones, utilizamos estos comandos:
```
[~()]$nvm list
->      v12.0.0
        v15.9.0
default -> node (-> v15.9.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v15.9.0) (default)
stable -> 15.9 (-> v15.9.0) (default)
lts/* -> lts/fermium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.23.3 (-> N/A)
lts/erbium -> v12.20.2 (-> N/A)
lts/fermium -> v14.15.5 (-> N/A)

[~()]$nvm use v15.9.0
Now using node v15.9.0 (npm v7.5.3)
[~()]$node --version
v15.9.0
[~()]$npm --version
7.5.3
```  
## Conclusiones  
La realización de esta práctica me ha resultado bastente interesante, sobre todo la parte de configuración de Git ya que no tengo ninguna experencia en ese ambiente y me ha parecido bastante útil el poder clonar los repositorios en mi máquina sin tener que utilizar ningún tipo de credencial. Además, me parece que Git es una herramienta que debemos aprender a utilizar debido a que nos puede facilitar bastante el control de versiones de nuestros proyectos. Por un lado, con la parte de configuración del IaaS no tuve ninguna dificultad, ya que en otra ocasión tuve que hacerlo por lo que fue un repaso. Por el otro lado, nunca había utilizado Github Pages ni markdown para hacer informes y después de leer información sobre ellos, me parecieron basatante sencillos de usar. Por lo que puedo concluir que en general, no tuve ningún problema a la hora de realizar la práctica.  
## Bibliografía  

- Servicio de VPN de la ULL | https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/ 
- Configurar Git por primera vez | https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez 
- Libro Pro Git de Scott Chacon y Ben Straub | https://git-scm.com/book/es/v2 
- Información Github Pages | https://docs.github.com/es/github/working-with-github-pages/about-github-pages 
- Web Jekyll | https://jekyllrb.com/ 
- Información Node.js | https://nodejs.org/es/about/ 
- Node Version Manager | https://github.com/nvm-sh/nvm 
- Node Package Manager | https://www.npmjs.com/ 
- Introducción a Markdown | https://guides.github.com/features/mastering-markdown/ 
- Sintaxis Markdown | https://markdown.es/sintaxis-markdown/ 

