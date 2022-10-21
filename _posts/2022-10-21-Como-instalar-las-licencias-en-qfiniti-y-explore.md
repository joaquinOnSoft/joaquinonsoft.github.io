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

## Instalar las licencias de Qfiniti

 - Acceder a nuestro servidor de Qfiniti con un usuario administrador
 - Navegar al menú `Administer > Settings >  License Settings` 
 
   ![Qfiniti license settings](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/01-qfiniti-license-settings.png)

 - Pulsar en el botón **Import License Key**. 
 - Se muestra el pop-up **Import License Key(s)**. 
 
   ![Qfiniti a license key is required for each product](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/02-qfiniti-a-license-key-is-required-for-each-product.png)

 - Pulsamos el botón **Browse...*
 - Se muestra un pop-up de selección de archivos del `Explorador de Windows`
 
   ![Qfiniti import license keys](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/03-qfinit-import-license-keys.png)

 - Seleccionamos los ficheros **.lic** o **.xml** que se nos hayan proporcionado (excepto los que cotienen `explore`en su nombre)
 
   ![qfinit license keys explorer pop-up](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/04-qfinti-license-keys-explorer-pop-up.png)
   
 - Pulsamos el botón **Open**
 - Al volver al pop-up **Import License Key(s)** pulsamos el botón **OK**
 - Qfiniti realiza la comprobación de las licencias. Una vez finalizada, se muestra la información actualizada
 
   ![qfinit licenses installed](/images/2022-10-21-Como-instalar-las-licencias-en-qfiniti-y-explore/05-qfiniti-licenses-installed.png)

> Conviene destacar que junto a los módulos `OpenText Explore #Agents` y `Qfiniti Explore` 
> se indica **N/A** (no aplica), ya que las licencias de relativas a Explore se gestionan desde
> la pantalla de administración de Explore