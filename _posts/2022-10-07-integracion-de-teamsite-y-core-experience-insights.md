---
title: "Integración de TeamSite y Core Experience Insights"
header:
  image: /images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-dashboard-drill-down.png
  og_image: /images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-dashboard-drill-down.png
tags:
  - OpenText
  - TeamSite 
  - Core Experience Insights
last_modified_at: 2022-10-07T18:25:28-09:00
---

Hoy explicaremos como realizar la integración de **TeamSite** y **Core Experience Insights (CXI)** para poder
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

Podemos [inyectar archivos de configuración](https://webapp.opentext.com/piroot/wcts/v220200/wcts-cdg/en/html/jsframe.htm?inject-config-files) 
almacenados en la carpeta de personalización del paquete de implementación de TeamSite y usar un script para transferirlos y hacerlos visibles 
para los contenedores de TeamSite en Kubernetes. Este método utiliza la función *ConfigMap* de *Kubernetes*, que está diseñada para archivos de 
configuración pequeños basados ​​en texto.

Copiamos el archivo de configuración que deseamos inyectar en los contenedores de TeamSite, en el siguiente directorio de nuestra máquina:

```
 <bundle>/configfiles/<configmap>/<target path>/
```

Donde:

 - `<configmap>` es uno de los siguientes nombres de los objetos del mapa de configuración que se usan para transferir los archivos 
    de configuración a los pods: **authoring**, **fluentd**, **indexer**, **logrotate**, **lsdsrt**, **lscsrt**, **odauthoring**, **odreceiver**, 
	**postgres**, **prometheus**, **solr**, **webd**, y **zookeeper**
 - `<target path>` es la ruta completa al directorio del archivo en el sistema de archivos del contenedor. Podemos crear uno o más de estos directorios según sea necesario.

Debemos seguir los siguientes pasos:

 1. Editar el fichero:  
 
    ```
	/home/otadmin/helm_charts/teamsite/bundle/configfiles/lsdsrt/Interwoven/LiveSiteDisplayServices/runtime/web/WEB-INF/conf/livesite_customer/covisint.properties 
	```
	
	| Significado | Path                                                                                               |
	|-------------|----------------------------------------------------------------------------------------------------|	
	| Bundle      | /home/otadmin/helm_charts/teamsite/bundle                                                          |
	|             | /configfiles                                                                                       |
	| Configmap   | /lsdsrt                                                                                            | 
	| Target path | /Interwoven/LiveSiteDisplayServices/runtime/web/WEB-INF/conf/livesite_customer/covisint.properties |
	
 2. Cambiar el directorio a /home/otadmin/Desktop/customization: 
    
	```
	cd /home/otadmin/Desktop/customization/
	```
	
 3. Ejecutar el script `./customizehostname.sh <vmname>`
 
    En nuestro ejemplo: `./customizehostname.sh vm-joaquin`
	
	Este script, actualizará el nombre de host e implementará los archivos de configuración
	
	**NOTA:** Paso opcional. Este script puede no estar disponible en tu entorno
	
 4. Se crea una nueva carpeta  en **/home/otadmin/Desktop/customization/updates/** con la fecha de hoy
 
 5. Reiniciar el servidor y los pods comenzarán: `sudo reboot now`
 
 6. Verificar que los cambios se hayan aplicado:

    ```
	kubectl exec -it -n runtime lsds-0 -- cat /Interwoven/LiveSiteDisplayServices/runtime/web/WEB-INF/conf/livesite_customer/covisint.properties
    ```

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

### Habilitar el rastreo del  sitio web

Sigamos estos pasos para habilitar el **seguimiento del sitio web**:

 1. Navegar a nuestro **sitio web**
 2. Hacer clic en el botón **"Detalles"**

   ![Habilitar rastreo de sitios en TeamSite](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/teamsite-enable-site-tracking.png)

 3. Hacer clic en la pestaña **"Basic"**
 4. Expandir la sección **"Core Experience Insights Integration"**
 5. Seleccionar **"Enable Experience Insights Integration"**
 6. Hacer clic en el botón **"Save"**

   ![Habilitar la integración con Core Experience Insights](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/teamsite-enable-experience-insights-integration.png)

### Habilitar el rastreo de la página

Completemos estos pasos para habilitar el **seguimiento de la página**:

 1. Navegar a nuestro **sitio web**
 2. Hacer clic en **"Edit"** en nuestra **página**
 
   ![TeamSite - editar página](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/teamsite-edit-page.png)

 3. Hacer clic en el 'icono de la hamburguesa' en la parte superior izquierda
 4. Hacer clic en la pestaña **"Advanced"**
 5. Ampliar la sección **"Analytics"**
 6. Seleccione la verificación **"Track Page View"**
 7. Hacer clic en el botón **"Save"**
 
   ![TeamSite - habilitar el seguimiento de la página](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/teamsite-track-page-view.jpg)
 
## Core Experience Insights (CXI)

Vayamos ahora a `Core Experience Insight` para completar la configuración del viaje del cliente.

### Añadir un 'data point'

Para crear un nuevo **data set** debemos seguir los siguientes pasos:

 1. Navegar a nuestr servidor de **Core Experience Insights**
 2. Hacer clic en **"Create new data set"**

   ![Core Experience Insights - crear un nuevo data set](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-create-new-data-set.png)
 
En el popup "Create new data set" debemos:

 1. Proporcionar un **"name"** para el *data set*
 2. Clic en **"Create"**

   ![Core Experience Insights - crear un nuevo data set](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-save-new-data-set.png)

 3. Clic en **"Get Started"** en la sección **Data Points**

   ![Core Experience Insights - Get Started](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-get-started.png)

 4. Arrastrar un *data point* del tipo *TeamSite Page View* al lado derecho
 5. Hacer clic en el *data point*
 6. Hacer clic en el icono del "Lápiz" para editar las propiedades del *data point*

   ![Core Experience Insights - Data points](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-data-point.png)
 
Vamos a configurar nuestro *data point*. Para ello tenemo que: 

 1. Definir el **"Data point name"**
 2. Seleccionar el origen del dato, en nuestro ejemplo **"PersonalLoan"**
 3. Seleccionar el tipo de evento, en nuestro ejemplo **"page.view"**
 4. Clic en el **icono (+)** en la sección **Extra coditions**
 5. Definir la regla con los siguientes valores:
   - **file**
   - **=**
   - **home.page**
   
   > **NOTE**: home.page is full name of the page that we want to track
   
 6.  Clic en **"Apply"**

   ![Core Experience Insights - Salvar la configuración del data point](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-edit-data-point.png) 
 
 
### Añadir un cuadro de mandos
 
A continuación, añadiremos un *cuadro de mandos* para poder visualizar el viaje del cliente en nuestro sitio web 
 
 1. Hacer clic en el botón **"Test"** de la esquina superior derecha
 2. Hacer clic en el botón **"Activate"** de la esquina superior derecha
 
   ![Core Experience Insights - añadir data point](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-add-dashboard.png) 
 
 3. Hacer clic en el botón **"Add dashboard"** de la sección *Dashboards*

   ![Core Experience Insights - añadir dashboard](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-add-data-set-dashboard.png) 

 4. Seleccionar el tipo de gráfico
 5. Hacer clic en el botón **"Next"**

    ![Core Experience Insights - seleccionar el tipo de gráfico](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-drill-down.png) 
 
Felicidades, hemos creado nuestro primer cuado de mandos en Core Experience Insights.

![Core Experience Insights - Drill down dashboard](/images/2022-10-07-integracion-de-teamsite-y-core-experience-insights/core-experience-insights-dashboard-drill-down.png) 

> **NOTA:** Debemos navegar por el sitio publicado en TeamSite para ver los datos reflejados en nuestro cuadro de mandos.
 
 