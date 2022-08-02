---
title: "Exportar e importar proyectos en teamsite"
header:
  image: /images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/project-in-estudio-teamsite.png
  og_image: /images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/project-in-estudio-teamsite.png
tags:
  - OpenText
  - OpenText Media Management 
  - Dockers
  - Kubernetes
last_modified_at: 2022-08-01T18:06:36-09:00
---

En este artículo explicaremos como exportar un proyecto de TeamSite desde una máquina virtual (VM)
e importar ese mismo proyecto en un entorno basado en dockers y kubernetes.

## Exportar un proyecto desde una máquina virtual (VM)

En primer lugar, debemos copiar el nombre del proyecto que deseamos exportar:

![Listado de proyectos en TeamSite](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/20220801-exportar-e-importar-proyectos-en-teamsite/lista-proyectos-teamsite.png)

Nos conectaremos a nuestra máquina virtual usando `ssh`:

```shell
 ssh <USER>@<SERVER> -p <PORT>
```

Por ejemplo, usando el puerto por defecto (22):

```shell
 ssh root@vm-joaquinonsoft.azure.com
```

Utilizaremos el comando `archiveexport` para exportar nuestro proyecto:

```shell
archiveexport -p <PROJECT_NAME> -s  <SPAR_FILE_NAME>
```

En nuestro ejemplo:

```shell
cd /usr/Interwoven/TeamSite/bin
./archiveexport -p StandardBank -s  StandardBank

Project being exported is "StandardBank"
Workarea being exported is "/default/main/StandardBank/WORKAREA/default"
Include Workflows set to "false"
Include Campaigns set to "false"
Include TeamSite customizations set to "false"
Include LiveSite customizations set to "none"
Export process details log set to "true
Export process started at 2022-08-01 05:36:02 EDT
Export Project Content started...
Export Project Content finished
Export Associations and Extended Attributes started...
Export Associations and Extended Attributes finished
Export Manifest File started...
Export Manifest File finished
Export Customizations started...
Export Customizations finished
Export Global taxonomies started...
Export Global taxonomies finished
Export process completed at 2022-08-01 05:36:21 EDT
Project export file location:exported_spars/StandardBank.spar
Project export log file location:/usr/Interwoven/TeamSite/local/logs/content_exporter.log
```

> **NOTA**: La ruta donde se encuentra el comando `archiveexport` puede variar en tu entorno

El archivo de exportado de nuestro ejemplo se encuentra en esta ruta:

```shell
/usr/Interwoven/TeamSite/install/exported_spars/StandardBank.spar
```

Copiaremos el archivo `.spar` a nuestro ordenador. Por ejemplo, desde una consola usando el comando `scp`:

```shell
scp <USER>@<VM_ADDR>:<PATH_TO_FILE_IN_THE_VM>  <LOCAL_PATH>
```

> El programa `scp` es un cliente que implementa el protocolo SCP, es decir, es un programa que realiza copia segura.


En nuestro ejemplo, sería así:

```shell
scp root@vm-joaquinonsoft.azure.com:/usr/Interwoven/TeamSite/install/exported_spars/StandardBank.spar  .
root@vm-joaquinonsoft.azure.com's password:
StandardBank.spar                                                100%   14MB  14.0MB/s   00:01
```

A continuación, subiremos el archivo `.spar` en un *space* de  [Intercambio de archivos simple y seguro con Hightail](https://www.hightail.com/) 
o cualquier otra herramienta para compartir archivos.

![Archivo .spar compartido en Hightail](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/hightail-shared-space.png)

## Importar Proyecto en nuestro contenedor

Ahora vamos a importar el fichero `.spar` con nuestro proyecto. Para ello, nos conectaremos 
a nuestro entorno en contenedores mediante una conexión `RDP`

![Conexión RDP](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/rdp-connection.png)

Una vez conectamos debemos descargar archivo `.spar` de *Hightail* o la opción para compartir que seleccionamos anteriormente

![Descargar activo desde space de Hightail](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/hightail-donwload-asset-from-space.png)

Vamos a descargar nuestro archivo `.spar` en esta ruta: `/home/otadmin/Downloads`

Pudemos consultar la lista disponible de *pods* usando **k9s**

![Listado de pods en k9s](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/listado-pods-k9s.png)

O usando `kubectl` en una **Konsole**:

```shell
kubectl get pods --all-namespaces     
```

![kubectl get pods --all-namespaces](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/kubectl-get-pods--all-namespaces.png)

> Consulta la hoja de referencia de `kubectl` para obtener más información sobre 
> cómo trabajar con nuestro clúster de Kubernetes desde la línea de comandos: [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

### Importar el proyecto de TeamSite usando kubectl

Ahora, copiaremos el archivo `.spar` en el *pod* llamado `storage-server-0`, desde 
el espacio de nombres `authoring`, que es un *pod* de almacenamiento permanente que 
usa `kubectl cp`:

```shell
kubectl cp /tmp/foo my-namespace/my-pod:/tmp/bar       
# Copy /tmp/foo local file to /tmp/bar in a remote pod in 
# namespace my-namespace
```

En nuestro ejemplo, desde una *konsole* de la máquina virtual Linux (guess):

```shell
kubectl cp StandardBank.spar authoring/store-server-0:/tmp
```

![kubectl cp StandardBank.spar authoring/store-server-0:/tmp](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/kubectl-cp.png)

> **NOTA**: El archivo .spar debe subirse y desplegarse en un *pod* de almacenamiento permanente; 
> de lo contrario, se perderán los cambios una vez que reiniciemos el *pod*.

A continuación, crearemos un nuevo proyecto en TeamSite.

![Crear proyecto en TeamSite](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/create-project-teamsite.png)

Pudemos consultar la rama de nuestro nuevo proyecto en `CC Professional`:

![Rama de nuestro nuevo proyecto en CC Professional](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/project-branch-cc-professional.png)

Depues, abriremos una shell en el *pod* `store-server-0`. Desde una terminal ejecuta:

```shell
kubectl exec -it -n authoring store-server-0 – bash
```

En la shell, tenemos que ir al directorio donde copiamos nuestro archivo `.spar` y 
luego importaremos el archivo `.spar` con *sudo*:

```shell
sudo /Interwoven/TeamSite/install/install_archive.ipl <SPAR file name> <TeamSite branch to import to>
```

Entonces, en nuestro ejemplo sería así:

```shell 
sudo /Interwoven/TeamSite/install/install_archive.ipl StandardBank.spar  /default/main/StandardBank/WORKAREA/default
```

Felicidades, el proyecto se ha importado y esta listo para usar

![Projecto en estudio](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/project-in-estudio-teamsite.png)

### Importar el proyecto de TeamSite usando k9s

Si lo prefieres, podemos hacer la importación desde `k9s`, simplemente seleccionaremos
el *pod* **storage-server-0** y ejecutaremos la *shell* en él:

![Shell k9s](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/k9s-shell.png)

Elegimos el contenedor de *authoring-server*

![Elegimos el contenedor de authoring-server](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/pick-authoring-server.png)

En la *shell*, vamos al directorio en el que copiamos nuestro archivo `.spar` y luego importamos 
el archivo `.spar` con *sudo*:

![Importar el proyecto .spar desde k9s](/images/2022-08-01-exportar-e-importar-proyectos-en-teamsite/k9s-shell-import-teamsite-project.png)



> Consulte el *Capítulo 16: Apéndice D: Restricciones para las herramientas de línea de comandos de TeamSite en contenedores* 
> de la documentación de TeamSite para obtener más información sobre cómo importar contenido desde un archivo SPAR.
> [Chapter 16 – Appendix D – Restrictions for TeamSite Command-Line Tools in Containers](https://webapp.opentext.com/piroot/wcts/v220200/wcts-cdg/en/html/jsframe.htm?appendix-d)