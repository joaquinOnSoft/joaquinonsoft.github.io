---
title: "Primeros pasos con una máquina virtual con Nuxeo"
header:
  image: /images/nuxeo-virtual-machine-virtualbox-570x255.png
  og_image: /images/nuxeo-virtual-machine-virtualbox-570x255.png
tags:
  - Nuxeo
  - Nuxeo Studio
last_modified_at: 2019-02-26-T20:59:57-04:00  
---

Acabas de descargar una máquina virtual de Nuxeo y estas dando tus primeros pasos.  Aquí te contamos como realizar algunos ajustes que haran tu vida más sencilla.

![Nuxeo downloads virtual machine](/images/nuxeo-downloads-virtual-machine.png "Nuxeo downloads virtual machine")

## Actualizar la máquina virtual
Desde que se creo la máquina virtual que estamos utlizando pueden haberse publicado actualizaciones y/o parches de seguridad de Ubuntu. Por eso es interesante actualizar la máquina virtual:

```Shell
$ sudo apt-get update
``` 

## Configurar el teclado
Por defecto, la máquina virtual utiliza una distribución de teclado ingles. Si no tienes un teclado de este tipo usar la máquina virtual puede resultar un poco incomodo. Así que vamos a configurar nuestro teclado español:

```Shell
$ sudo loadkeys es
``` 

## Instalar software addicional
A continuación instalaremos software adicional que nos será útil más adelante. nano, un editor de texto para línea de comandos más amigable que vi, mediainfo, una herramienta que muestra información y etiquetas para ficheros de audio y video (útil si pensamos utilizar el add-on nuxeo-dam) y figlet, un programa que genera banners de texto:

```Shell
# Install nano editor
$ sudo apt-get install nano 

# Install mediainfo
$ sudo apt-get install mediainfo

# Install figlet
$ sudo apt-get install figlet
``` 

## Actualizar el fichero .profile del usuario ‘nuxeo’
Vamos a modificar el fichero .profile del usuario nuxeo para no tener que modificar la configuración de la máquina virtual cada vez que la utilizamos:

```Shell
$ cd /home/nuxeo

$ nano .profile
```

Ahora debemos añadir estas líneas a nuestro fichero .profile:

```
# Load Spanish keyboard
sudo loadkeys es > /dev/null 2>&1

# NUXEO_CONF environment variable missed on VM WMWare/VirtualBox image
# Only needed with Nuxeo 10.3 or older versions
# https://jira.nuxeo.com/browse/NXP-26152
NUXEO_CONF=/etc/nuxeo/nuxeo.conf
export NUXEO_CONF

EDITOR=nano
export EDITOR

figlet MY_PROJECT_NAME
```

Salvamos nuestros cambios en el fichero **.profile**, cerramos la sesión del usuario nuxeo.

La próxima vez que accedamos veremos algo así:

![Nuxeo Virtual Machine. nuxeo user .profile](/images/nuxeo-virtual-machine-nuxeo-user-dot-profile.png "Nuxeo Virtual Machine. nuxeo user .profile")
 
 
El banner con el nombre del proyecto, que se muestra tras iniciar sesión, ayuda a saber en que máquina virtual estas trabajando. Esto es especialmente útil cuando trabajas en varios proyectos.

## ¿Donde esta instalado nuxeo?
Buena pregunta. Nuxeo esta instalado en el directorio **/var/lib/nuxeo**. Encontraremos 2 carpetas, **data** donde se almacenan los binarios y **server**, donde se encuentra el software de Nuxeo.

El programa nuxeoctl se encuentra en el directorio **/var/lib/nuxeo/server/bin**
	
## ¿Donde esta el fichero nuxeo.conf?
El fichero **nuxeo.conf** esta ubicado en este path: **/etc/nuxeo/nuxeo.conf**

##Intalar los hotfixes disponibles
Siempre es recomendable instalar cualquier actualización disponible, ya que incluyen mejoras, resolución de errores y parches de seguridad:

```shell
$ nuxeoctl stop

$ nuxeoctl mp-hotfix

$ nuxeoctl start
```

Para instalar los hotfixes es necesario que la instancia este registrada en **Nuxeo On-line Services**. Podemos registrar la ejecutando el asistente de instalación o desde línea de comandos: **nuxeoctl register**

## ¿Cual es la IP de mi máquina virtual?

Podemos averiguar la IP de nuestra máquina virtual ejecutando la siguiente instrucción:

```
$ip addr show
```

![nuxeo virtual machine: IP address](/images/nuxeo-virtual-machine-ip-address.png "nuxeo virtual machine: IP address")


## ¿Como accedo a mi instancia de Nuxeo?
Acceder a nuestra instancia de Nuxeo es tan sencillo como introducir esta URL en nuestro navegador:

```
http://<VM_IP_ADDRESS>/nuxeo
```

En el caso de mi máquina virtual:

```
http://192.168.1.111/nuxeo
```

Y listo:

![Nuxeo Virtual Machine: Access by IP](/images/nuxeo-virtual-machie-access-by-ip.png "Nuxeo Virtual Machine: Access by IP")
