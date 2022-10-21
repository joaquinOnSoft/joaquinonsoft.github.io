---
title: "Como instalar las licencias en Qfiniti y Explore"
header:
  image: /images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/05-qfiniti-licenses-installed.png
  og_image: /images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/05-qfiniti-licenses-installed.png
tags:
  - OpenText
  - Qfiniti
  - Explore
last_modified_at: 2022-10-21T15:27:15-04:00
---

En este artículo veremos como instalar las licencias en Qfiniti y Explore. Esta acción es 
necesaria cuando instalamos el servidor por primera vez, o bien porque las licencias han expirado.

> **OpenText™ Qfiniti** es un conjunto unificado y de soluciones de optimización de la fuerza laboral, 
> administrado de forma centralizada, para el análisis de interacciones multicanal. Disponible 
> on-premise o en la nube, Qfiniti se integra con los principales sistemas de administración de 
> centros de atención telefónica, para brindar automáticamente inteligencia de clientes accesible y procesable 
> que ayuda a las organizaciones a comprender mejor las interacciones con los clientes y brindar 
> un servicio excepcional.

> **OpenText(TM) Explore** es una solución de "descubrimiento" que permite a los profesionales de negocio y 
> Centros de Atención al Cliente ver las interacciones a través de diversos canales colectivamente para 
> obtener una imagen completa de los comportamientos y las relaciones de los clientes.


Los ficheros de licencias pueden tener la extensión **.lic** o **.xml**. El nombre de los archivos 
y el número de ellos dependerá de los módulos contratados.

Habitualmente los ficheros de licencia tienen nombres similares a estos:

```
Qfiniti 20 Demo-Advise-250-exp-07.31.23.lic
Qfiniti 20 Demo-API-1-exp-07.31.23.lic
Qfiniti 20 Demo-Expert-250-exp-07.31.22.lic
Qfiniti 20 Demo-ICE-250-exp-07.31.23.lic
Qfiniti 20 Demo-Live Assist-250-exp-07.31.23.lic
Qfiniti 20 Demo-Obs Screen-250-exp-07.31.23.lic
Qfiniti 20 Demo-Obs Voice-250-exp-07.31.23.lic
Qfiniti 20 Demo-Observe ARI Avaya Switch-250-exp-07.31.23.lic
Qfiniti 20 Demo-Optimize Guide-250-exp-07.31.23.lic
Qfiniti 20 Demo-Optimize Measure-250-exp-07.31.23.lic
Qfiniti 20 Demo-OT Explore AutoScore-250-exp-07.31.23.lic
Qfiniti 20 Demo-OT Explore Ultimate-exp-7-31-23.lic
Qfiniti 20 Demo-OT Qfiniti AutoScore-250-exp-07.31.23.lic
Qfiniti 20 Demo-OT Qfiniti Explore Analytics Connector-exp-07.31.23.lic
Qfiniti 20 Demo-OT Qfiniti Explore Ingestion-exp-07.31.23.lic
Qfiniti 20 Demo-OT Qfiniti Explore Users-25-exp-07.31.23.lic
Qfiniti 20 Demo-OT Qfiniti Speech Analytics-1500-exp-07.31.23.lic
Qfiniti 20 Demo-Survey-250-exp-07.31.23.lic
Qfiniti 20 Demo-Switch Conn-Avaya-1-exp-07.31.23.lic
Qfiniti 20 Demo-Workforce Avaya Switch-250-exp-07.31.23.lic
Qfiniti 20 Demo-Workforce Connector for Email-250-exp-07.31.23.lic
Qfiniti 20 Demo-Workforce Connector for SMS Text-250-exp-07.31.23.lic
Qfiniti 20 Demo-Workforce Connector for Staging Database-250-exp-07.31.23.lic
Qfiniti 20 Demo-Workforce-250-exp-07.31.23.lic
```

## Instalar las licencias de Qfiniti

Para instalar las licencias de OpenText Qfiniti debemos seguir los siguientes pasos:

 - Acceder a nuestro servidor de Qfiniti con un usuario administrador
 - Navegar al menú `Administer > Settings >  License Settings` 
 
   ![Qfiniti license settings](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/01-qfiniti-license-settings.png)

 - Pulsar en el botón **Import License Key**. 
 - Se muestra el pop-up **Import License Key(s)**. 
 
   ![Qfiniti a license key is required for each product](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/02-qfiniti-a-license-key-is-required-for-each-product.png)

 - Pulsamos el botón **Browse...*
 - Se muestra un pop-up de selección de archivos del `Explorador de Windows`
 
   ![Qfiniti import license keys](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/03-qfinit-import-license-keys.png)

 - Seleccionamos los ficheros **.lic** o **.xml** que se nos hayan proporcionado (excepto los que contienen `explore` en su nombre)
 
   ![qfinit license keys explorer pop-up](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/04-qfinti-license-keys-explorer-pop-up.png)
   
 - Pulsamos el botón **Open**
 - Al volver al pop-up **Import License Key(s)** pulsamos el botón **OK**
 - Qfiniti realiza la comprobación de las licencias. Una vez finalizada, se muestra la información actualizada
 
   ![qfinit licenses installed](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/05-qfiniti-licenses-installed.png)

> Conviene destacar que junto a los módulos `OpenText Explore #Agents` y `Qfiniti Explore` 
> se indica **N/A** (no aplica), ya que las licencias de relativas a Explore se gestionan desde
> la pantalla de administración de Explore

## Instalar las licencias de Explore

Para instalar las licencias de OpenText Explorer debemos seguir los siguientes pasos:

 - Abrir un *explorador de archivos* en el directorio `<EXPLORE_HOME>Explore\ExploreWeb\Licenses`. 
   En nuestro ejemplo, `C:\Program Files (x86)\OpenText\Explore\ExploreWeb\Licenses`
   
   > NOTA: En caso de que el directorio `Licenses` no exista tendremos que crearlo.
   
 - Eliminar los ficheros de licencia expirados
 - Copiar los archivos de licencia, que pueden tener extensión .lic o .xml, en el direcorio `Licenses`.
   En este ejemplo copiaremos estos 3 ficheros:
 
```
Qfiniti 20 Demo-OT Qfiniti Explore Analytics Connector-exp-07.31.23.lic
Qfiniti 20 Demo-OT Qfiniti Explore Ingestion-exp-07.31.23.lic
Qfiniti 20 Demo-OT Qfiniti Explore Users-25-exp-07.31.23.lic
```
 
 - Abrir la aplicación `Command Prompt`
 - Reiniciar `IIS`. Para ello ejecutaremos el comando `iisreset`

Listo ya podemos acceder a Qfiniti y Explore. Las licencias ya están instaladas 
 

