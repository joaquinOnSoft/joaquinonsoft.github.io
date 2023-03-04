---
title: "Como incluir la etiqueta link en un componente de teamsite"
header:
  image: /images/2023-03-03-como-incluir-la-etiqueta-link-en-un-componente-de-teamsite/como-incluir-la-etiqueta-link-en-un-componente-de-teamsite.png
  og_image: /images/2023-03-03-como-incluir-la-etiqueta-link-en-un-componente-de-teamsite/como-incluir-la-etiqueta-link-en-un-componente-de-teamsite.png
tags:
  - TeamSite
last_modified_at: 2023-03-03T14:35:17-04:00
---

En determinadas circunstancias podemos quere añadir una etiqutea `<link>` en un componente de TeamSite. Por defecto no es posible. 
En este artículos vamos a explicar como incluir la etiqueta `<link>` en un componente de TeamSite.

Modificar ficheros en un entorno basado en contenedores no es tan simple como editar un fichero en el sistema de archivos y reiniciar los servicios.  

Tenemos que copiar el fichero

```console
/home/otadmin/helm_charts/teamsite/bundle/configfiles.default/authoring/Interwoven/TeamSite/iw-webd/conf/modsecurity.conf.template 
```

a 

```console
/home/otadmin/helm_charts/teamsite/bundle/configfiles/authoring/Interwoven/TeamSite/iw-webd/conf/modsecurity.conf.template 
```

A continuación, editamos el fichero 

```console
/home/otadmin/helm_charts/teamsite/bundle/configfiles/authoring/Interwoven/TeamSite/iw-webd/conf/modsecurity.conf.template.   
```

Debemos añadir lo siguiente al final de `modsecurity.conf.template` 

```console
SecRuleRemoveById 973321
```

Tenemos que repetir los pasos anteriormente descritos para el fichero `modsecurity.conf.template` alojado en `/home/otadmin/helm_charts/teamsite/bundle/configfiles.default/webd`. 

**Origen**

```console
/home/otadmin/helm_charts/teamsite/bundle/configfiles.default/webd/Interwoven/TeamSite/iw-webd/conf/modsecurity.conf.template 
```

**Destino**

```console
/home/otadmin/helm_charts/teamsite/bundle/configfiles/webd/Interwoven/TeamSite/iw-webd/conf/modsecurity.conf.template 
```

> **NOTA**: En esta ruta se cambia `authoring` del path inicial por `webd`

A continuación debemos aplicar los cambios descritos en la siguiente sección.


## Aplicación de los cambios en los archivos de configuración al sistema contenedor de Teamsite

Si necesitamos modificar los archivos de configuración predeterminados de la carpeta `/home/otadmin/helm_charts/teamsite/bundle/configfiles/`, 
estas instrucciones le ayudarán a aplicar los cambios después de realizarlos. Tenga en cuenta que a partir de **Teamsite 22.4**, los archivos de 
configuración por defecto se proporcionan en `/home/otadmin/helm_charts/teamsite/bundle/configfiles.default/` y cualquier cambio que deba realizarse 
requiere copiar primero el archivo y su jerarquía desde la carpeta configfiles.default a la carpeta `/home/otadmin/helm_charts/teamsite/bundle/configfiles`.  
Habrá algunos ejemplos en la VM como `/home/otadmin/helm_charts/teamsite/bundle/configfiles/webd/etc/iw.cfg`.

Después de haber actualizado los archivos de configuración, tendrás que aplicarlos haciendo una de las siguientes cosas dependiendo de la versión de la VM que tengas. 

Para **OTMM 22.4+**
   1. Abra un `terminal/konsole`
   2. Ejecute uno o ambos de los siguientes comandos dependiendo de si necesita aplicar los cambios al entorno de creación, al entorno de ejecución o a ambos:
      -	`/home/otadmin/helm_charts/teamsite/bundle/applyAuthoringConfig.sh -n authoring`
      -	`/home/otadmin/helm_charts/teamsite/bundle/applyRuntimeConfig.sh -n runtime`
   3. Esto forzará el reinicio de los pods. Espere a que los pods se inicien y pruebe su personalización.

Para **OTMM 22.2**
   1. Abra una `terminal/konsole`
   2. Vaya a `/home/otadmin/Desktop/customization`
   3. Ejecute `./customizehostname.sh <vm-name>`.
      -	Para `<vm-name>` no añada el **.eimdemo.com** o **-ui.eimdemo.com** o **-lsds.eimdemo.com** o 
         **-lscs.eimdemo.com**.  Indique sólo el nombre de la máquina virtual.  
         Por ejemplo, si el nombre de su máquina virtual es *teamsite.eimdemo.com* (URL OTDS), 
         indique sólo *teamsite* para `<vm-name>.
      -	Introduzca Y para aplicar las personalizaciones
   4. Esto forzará el reinicio de los pods. Espere a que se inicien los pods y pruebe su personalización.