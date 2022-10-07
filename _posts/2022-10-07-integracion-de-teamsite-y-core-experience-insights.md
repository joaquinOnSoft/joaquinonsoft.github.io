---
title: "Integración de TeamSite y Core Experience Insights "
header:
  image: /images/2022-10-07-integracion-de-teamsite-y-core-experience-insights.md/core-experience-insights-dashboard-drill-down.png
  og_image: /images/2022-10-07-integracion-de-teamsite-y-core-experience-insights.md/core-experience-insights-dashboard-drill-down.png
tags:
  - OpenText
  - TeamSite 
  - Core Experience Insights
last_modified_at: 2022-10-07T18:25:28-09:00
---

Hoy explicaremos como realizar la integración de `TeamSite` y `Core Experience Insights` **(CXI)** para poder
tener una visión del **viaje del cliente** en nuestro sitio web y otros canales.

> OpenText™ Core Experience Insights es una aplicación SaaS basada en la nube diseñada
> para ayudar a crear experiencias de cliente más atractivas. Al agregar datos de 
> eventos de clientes de múltiples puntos de contacto, Core Experience Insights 
> produce análisis e información integrales que permiten la orquestación de 
> eventos de comunicación con el cliente desde el el primer contacto hasta la adquisición.

> OpenText™ TeamSite™ es una solución de administración de contenido que permite a 
> las organizaciones crear y administrar experiencias de clientes digitales ricas y 
> personalizadas optimizadas para cualquier dispositivo, canal o contexto. Desde una 
> sola interfaz, los usuarios pueden crear, probar, orientar y publicar contenido, así 
> como administrar medios enriquecidos, diseñar sitios web y crear aplicaciones móviles.


## covisint.properties

### covisint.properties (en una máquina virtual)

Primero, necesitamos configurar el **API de Covisint** para que funcione con TeamSite en una **máquina virtual**.

En entornos de demostración de OpenText, por lo general, el archivo **covisint.properties** se encuentra en:

> /Interwoven/LiveSiteDisplayServices/runtime/web/WEB-INF/conf/livesite_customer/covisint.properties

### covisint.properties (en un entorno contenedorizado)

Primero, necesitamos configurar el **API de Covisint** para que funcione con TeamSite en un entorno en contenedores

Podemos [https://webapp.opentext.com/piroot/wcts/v220200/wcts-cdg/en/html/jsframe.htm?inject-config-files](inyectar archivos de configuración) 
almacenados en la carpeta de personalización del paquete de implementación de TeamSite y usar un script para transferirlos y hacerlos visibles 
para los contenedores de TeamSite en Kubernetes. Este método utiliza la función ConfigMap de Kubernetes, que está diseñada para archivos de 
configuración más pequeños basados ​​en texto.

Copie el archivo de configuración que desea inyectar en los contenedores de TeamSite en el siguiente directorio de su máquina de implementación:

> <paquete>/configfiles/<configmap>/<ruta de destino>/

dónde:
 - `<configmap>` es uno de los siguientes nombres de los objetos del mapa de configuración que se usan para transferir los archivos 
    de configuración a los pods: **authoring**, **fluentd**, **indexer**, **logrotate**, **lsdsrt**, **lscsrt**, **odauthoring**, **odreceiver**, 
	**postgres**, **prometheus**, **solr**, **webd**, y **zookeeper**
 - `<ruta de destino>` es la ruta completa al directorio del archivo en el sistema de archivos del contenedor. Podemos crear uno o más de estos directorios según sea necesario.

Debemos seguir los siguientes pasos:

 1. Editar el fichero:  
    > /home/otadmin/helm_charts/teamsite/bundle/configfiles/lsdsrt/Interwoven/LiveSiteDisplayServices/runtime/web/WEB-INF/conf/livesite_customer/covisint.properties 
	
 2. Cambiar el directorio a /home/otadmin/Desktop/customization: 
    > cd /home/otadmin/Desktop/customization/
	
 3. Ejecutar el script `./customizehostname.sh <vmname>`
 
    En nuestro ejemplo: `./customizehostname.sh vm-joaquin`
	
	Este script, actualizará el nombre de host e implementará los archivos de configuración
	
	**NOTA:** Paso opcional. Este script puede no estar disponible en tu entorno
	
 4. Se crea una nueva carpeta  en **/home/otadmin/Desktop/customization/updates/** con la fecha de hoy
 
 5. Reiniciar el servidor y los pods comenzarán: `sudo reboot now`
 
 6. Verificar que los cambios se hayan aplicado:
    > kubectl exec -it -n runtime lsds-0 -- cat /Interwoven/LiveSiteDisplayServices/runtime/web/WEB-INF/conf/livesite_customer/covisint.properties

### covisint.properties (puntos comunes)

Estos son unos *valores de ejemplo*, del fichero `covisint.properties`, para un entorno de CXI:

```script
covisintforec.baseurl=https://basic.dms.covapp.io/delivery-management-service/receptor/message
covisintforec.clientid=bea10f73614324cc26e02127d41fbeeebb5b                      
covisintforec.secret=c599b447095036a1efac4337836a31d4c2b6
covisintforec.tenantid=a8b5dac0955f7e318a8d0117c9f1ad7f33d5
covisintforec.counter=3
covisintforec.sslEnable=true
``` 

## Habilitar la integración de Experience Insights en TeamSite 

Siga estos pasos para habilitar el seguimiento del sitio:
 1. Navegar a nuestro **sitio web**
 2. Hacer clic en el botón **"Detalles"**

![Habilitar rastreo de sitios en TeamSite](/images/teamsite-enable-site-tracking.png)
