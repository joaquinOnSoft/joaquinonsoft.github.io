---
title: "Añadir una marca de agua en Web UI"
layout: posts
header:
  image: /images/marca-de-agua-web-ui-570x255.png
  og_image: /images/marca-de-agua-web-ui-570x255.png
author_profile: true
categories:
  - Nuxeo
tags:
  - Nuxeo
  - Web UI
last_modified_at: 2018-07-18T15:59:57-04:00
---


Cuando trabajamos con sistemas de gestión documental es habitual querer añadir una marca de agua  a un documento en determinados casos, por ejemplo, cuando lo descargamos. Hoy vamos a explicar como configurar Nuxeo Studio para añdir una marca de agua a una documento en Web UI.

En primer lugar crearemos una cadena de automatización en Nuxeo Studio, desde la opción CONFIGURATION > Automation Chain. Nuestra cadena contará con las siguientes operaciones:

``` 
- Context.FetchDocument
- Document.GetBlob:
    xpath: "file:content"
- PDF.WatermarkWithText:
    text: Top Secrect
    properties:
      rotation: "45"
      xPosition: "0.5"
      yPosition: "0.5"
      relativeCoordinates: "true"
      alphaColor: "0.5"
      hex255Color: "#FF0000"
``` 

![Nuxeo Studio automation chain: add watermark](/images/nuxeo-studio-automation-chain-add-watermark-1.png "Nuxeo Studio automation chain: add watermark")

A continuación en View Designer crearemos una operación desde el menú **UI > Actions**. Debemos definir los siguientes parámetros:

   - **Name**: addTopSecreteWatermark
   - **Available**: Si
   - **Slot**: Document actions (Se mostrará en la esquina superior derecha, junto a los otros botones de acción)
   - **Order**: 200 (para que aparezca a la derecha de los demás iconos)
   - **Download**: yes
   - **Icon**: watermark
   - **Input**: [[document]]
   - **label**: Download with watermark
   - **operation**: acAddTopSecretWatermark (El nombre de la cadena de automatización que hemos creado en el paso anterior)
   - **Document has > one of the types**: File, Document
 

![View Designer operation add watermark](/images/view-designer-operation-add-watermark.png "View Designer operation add watermark")


Tras desplegar nuestros cambios en nuestra instancia sólo tenemos que acceder a algún documento de tipo File o Document (o crear uno nuevo). En la esquina superior derecha se muestra un nuevo icono «Download with watermark».

![Download with watermark Action button](/images/download-with-watermark-action-button.png "Download with watermark Action button")

Al pulsar sobre el nuevo icono descargaremos el documento con una nueva marca de agua añadida en cada página.

 
![Marca de agua Web UI todas las paginas](/images/marca-de-agua-web-ui-todas-las-paginas.png "Marca de agua Web UI todas las paginas")

¿A que ha sido fácil? Además sin escribir una línea de código.