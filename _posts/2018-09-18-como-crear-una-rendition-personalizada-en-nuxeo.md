---
title: "Como crear una rendition personalizada en Nuxeo"
header:
  image: /images/nuxeo-custom-view-rendition-570x255.png
  og_image: /images/nuxeo-custom-view-rendition-570x255.png
tags:
  - Nuxeo
  - Rendition
  - Nuxeo Studio
last_modified_at: 2019-04-20-T22:59:57-04:00  
---

En Nuxeo las **rendition** son representaciones alternativas de un documento o su contenido, como:

   - Una representación en PDF de Office
   - Una imagen con marca de agua
   - Un video redimensionado
   - Una exportación XML del documento
   - ...

En este artículo, explicaremos cómo crear una rendition personalizada y aplicarla en función de una regla de negocio, por ejemplo: aplicar una marca de agua en la vista y descargar las representaciones de un documento dado cuando el usuario pertenece a un grupo llamado «externals».

## Guía paso por paso
   - Crea un nuevo tipo de documento personalizado llamado **ExtDocument**
   - Crea un **nuevo grupo** llamado **externals**
   - Crea un nuevo **usuario** llamado **Ernesto**
   - **Añadir al usuario Ernesto al grupo "externals"**

## Cadena de automatización: añadir una marca de agua

Desde el menu **Automation > Automation chains** crearemos  una **cadena de automatización** llamada **watermarkPdfChain** que nos permitirá añadir una marca de agua a un documento:

![Nuxeo automation chain watermark pdf](/images/nuxeo-automation-chain-watermark-pdf-1.png "Nuxeo automation chain watermark pdf")

> Primera operación de la cadena de automatización
> Las cadenas de automatización utilizadas en event handlers, workflows … generalmente comienzan con la operación Context.FetchDocument, pero las cadenas de automatización utilizadas en las **renditions** deben comenzar con la operación **Context.PopDocument**

```
- Context.PopDocument
- Document.GetBlob: {}
- PDF.WatermarkWithText:
    text: Confidential
    properties:
      rotation: "45"
      xPosition: "0.5"
      yPosition: "0.5"
      relativeCoordinates: "true"
      alphaColor: "0.5"
      hex255Color: "#FF0000"
``` 

## XML extension: rendition
Desde el menu **Advanced Settings > XML extensions** crearemos  una **extensión XML** llamada **WatermarkedPdfRendition** que configura una nueva rendition

![XML extension: rendition](/images/nuxeo-xml-extension-watermarked-pdf-rendition-1-1200x588.png "XML extension: rendition")

```xml 
<extension target="org.nuxeo.ecm.platform.rendition.service.RenditionService" point="renditionDefinitions">
  <renditionDefinition name="watermarkedpdf">
    <label>label.rendition.pdf</label>
    <icon>/icons/pdf.png</icon>
    <contentType>application/pdf</contentType>
    <operationChain>watermarkPdfChain</operationChain>
    <storeByDefault>true</storeByDefault>
    <filters>
      <filter-id>allowPDFRendition</filter-id>
    </filters>
  </renditionDefinition>
</extension>
``` 

> La etiqueta **operationChain** hace referencia a la cadena de automatización **watermarkPdfChain** que hemos creado en el paso anterior.

 

## XML extension: sobrescribir la rendition predeterminada
Por último vamos a crea una extensión XML llamada **OverwriteDefaultRendition** que sobrescribe la representación predeterminada:

```xml
<extension point="defaultRendition" target="org.nuxeo.ecm.platform.rendition.service.RenditionService">
    <defaultRendition>
      <script language="JavaScript">
        function run() {
         
          if (CurrentUser.getGroups().contains("externals")) {
            if(Reason == 'download' || Reason == 'preview'){
              if (Document.getType() == "ExtDocument") {
                return 'watermarkedpdf';
              }
            }
          }
 
         
         if (Reason == 'download') {
            if (Document.getType() == "File") {
              return 'mainBlob';
            } else if (Document.getType() == 'Picture') {
              return 'mainBlob';
            } else if (Document.getType() == 'Video') {
              return 'mainBlob';
            } else if (Document.getType() == 'Audio') {
              return 'mainBlob';
            } else if (Document.getType() == 'Note') {
              return 'pdf';
            } else if (Document.getType() == 'Collection') {
              return 'containerDefaultRendition';
            } else if (Document.getType() == 'Folder') {
              return 'containerDefaultRendition';
            } else {
              return 'mainBlob';
            }
          } else {
            return 'mainBlob';
          }       
           
        }
      </script>
    </defaultRendition>
  </extension>
```

> El nombre de la rendition que devolvemos en esta estensión coincide con el valor del atributo **name** de la etiqueta **renditionDefinition** que hemos definico en el paso anterior. En nuestro ejemplo «watermarkedpdf»

Bien hecho, hemos creado una nueva rendition y sobrescrito la predeterminada para el tipo de documento «ExtDocument». 

##Personalizando la vista

Veamos que aspecto tiene. Para ello debemos crear los **layouts** para el tipo de documento personalizado «ExtDocument» en **View Designer**.

La vista **nuxeo-extdocument-view-layout** que genera View Designer por defecto tiene este aspecto:

```html
<!--
`nuxeo-extdocument-view-layout`
@group Nuxeo UI
@element nuxeo-extdocument-view-layout
-->
<dom-module id="nuxeo-extdocument-view-layout">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>
 
    <nuxeo-connection id="nxcon"></nuxeo-connection>
 
 
    <nuxeo-document-viewer role="widget" document="[[document]]"></nuxeo-document-viewer>
 
    <nuxeo-document-attachments role="widget" document="[[document]]"></nuxeo-document-attachments>
 
  </template>
 
  <script>
  Polymer({
    is: 'nuxeo-extdocument-view-layout',
    behaviors: [Nuxeo.LayoutBehavior],
    properties: {
 
      /**
         * @doctype ExtDocument
         */
      document: {
        type: Object,
      },
 
    }
  });
  </script>
</dom-module>
```
 
Si ojeas el web element nuxeo-document-preview en GitHub, puedes verificar que siempre se muestre la URL de datos del blob asociado con el documento

```xml
<template mime-pattern="application/pdf">
  <nuxeo-pdf-viewer src="[[_blob.data]]"></nuxeo-pdf-viewer>
</template>
```

Debemos hacer unos pequeños cambios para utilizar la rendition correcta:

```html
<dom-module id="nuxeo-extdocument-view-layout">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>
 
    <nuxeo-connection id="nxcon"></nuxeo-connection>
 
        <nuxeo-pdf-viewer src="[[_url(document)]]"/>
 
    <nuxeo-document-attachments role="widget" document="[[document]]"></nuxeo-document-attachments>
 
  </template>
 
  <script>
  Polymer({
    is: 'nuxeo-extdocument-view-layout',
    behaviors: [Nuxeo.LayoutBehavior],
    properties: {
 
      /**
         * @doctype ExtDocument
         */
      document: {
        type: Object,
      },
 
    },
 
    _url: function(){
      var baseUrl = this.$.nxcon.url;
      var id = this.get("document.uid");
 
      return baseUrl + '/nxfile/default/' + id;
    }
  });
  </script>
</dom-module>
```
![Nuxeo custom view rendition](/images/nuxeo-custom-view-rendition-1200x597.png "Nuxeo custom view rendition")

Este es el resultado final cuando accedemos a un documento de tipo ExtDocument con un usuario que pertenece al grupo «externals»:

