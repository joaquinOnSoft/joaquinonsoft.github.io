---
title: "Mostrar la información del usuario en Web UI"
header:
  image: /images/nuxeo-user-info-570x255.png
  og_image: /images/nuxeo-user-info-570x255.png
tags:
  - Nuxeo
  - Web UI
last_modified_at: 2018-07-17-T20:59:57-04:00  
---

En este artículo vamos a explicar como crear un web element con Polymer para mostrar la información del usuario en Web UI, la interface de Nuxeo.

En primer lugar seleccionaremos la pestaña **RESOURCES** de View Designer. A continuación vamos a crear el directorio **UI\element**; para ello debemos hacer click sobre el directorio UI y luego sobre el botón **Create**, que mostrará el pop-up de creación. Debemos elegir el tipo **Folder** e introducir **elements** como nombre de la nueva carpeta. Tras pulsar el botón OK la carpeta se creará.

![Nuxeo Web UI: create elements folder](/images/nuxeo-web-ui-create-elements-folder-1200x643.png "Nuxeo Web UI: create elements folder")

Debemos repetir la operación para crear el nuevo web  element bajo la carpeta elements. Esta vez debemos facilitar estos datos:

   - **Type**: Element
   - **Name**: nuxeo-user-info (la extensión .html se añadirá automáticamente)
   - **Template**: Sample layout template

![Nuxeo Web UI: create nuxeo-user-info web element](/images/nuxeo-web-ui-create-nuxeouser-info-web-element-1200x643.png "Nuxeo Web UI: create nuxeo-user-info web element") 

View Designer generará la estructura por defecto para nuestro web element. Hay parte del código que podemos eliminar ya que no lo vamos a necesitar. A continuación aparece comentado el código a borrar:

```html
<!--
`nuxeo-user-info`
@group Nuxeo UI
@element nuxeo-user-info
-->
<dom-module id="nuxeo-user-info">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>
<!--
    <paper-input id='title' role="widget" value="{{document.properties.dc:title}}" label="Title"></paper-input>

    <paper-textarea id='description' role="widget" value="{{document.properties.dc:description}}" label="Description"></paper-textarea>
-->
  </template>

  <script>
    Polymer({
      is: 'nuxeo-user-info',
      behaviors: [Nuxeo.LayoutBehavior],
      properties: {
        // title: {
        //  type: String,
        //  value: 'Title'
        //},

        //You can use either @schema or @doctype annotation in your model
        /**
         * @schema dublincore
         */
        document: {
          type: Object
        }
      },

      // _title: function() {
      //   return this.document.properties['dc:title'];
      // },

      // _setTitle: function() {
      //   this.$.title.label = this._title;
      //}

    });
  </script>
</dom-module>
```

Debemos recuperar la información del usuario, para ello vamos a utiliza el web element nuxeo-connection:

```xml
<nuxeo-connection role="widget" user="{{user}}"></nuxeo-connection>
```

El objeto User que recupera nuxeo-connection tiene las siguientes propiedades:

   - **id**: Alias del usuario
   - **isAdministrator**: Flag para indicar si es un usuario administrador
   - **isAnonymous**: Flag para indicar si es un usuario anónimo
   - **properties**
      - **firstName**: Nombre del usuario
      - **lastName**: Apellido del usuario
      - **email**: Correo electrónico del usuario
      - **groups**: Lista de los grupos a los que pertenece el usuario

Vamos a acceder a las diferentes propiedades y las mostraremos en pantalla, por ejemplo:

```xml
      <div role="widget">
        <label>[[i18n('label.user.id')]]</label>
        <div name="id">[[user.id]]</div>
      </div>

      <div role="widget">
        <label>[[i18n('label.user.firstName')]]</label>
        <div name="firstName">[[user.properties.firstName]]</div>
      </div>
```

El código completo de nuestro nuevo web element trendrá este aspecto

```html 
<!--
`nuxeo-user-info`
@group Nuxeo UI
@element nuxeo-user-info
-->
<dom-module id="nuxeo-user-info">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>

    <nuxeo-connection role="widget" user="{{user}}"></nuxeo-connection>
    
    <nuxeo-card role="widget">
      <div role="widget">
        <label>[[i18n('label.user')]]</label>
        <nuxeo-user-tag user="[[user]]"></nuxeo-user-tag>
      </div>
            
      <div role="widget">
        <label>[[i18n('label.user.id')]]</label>
        <div name="id">[[user.id]]</div>
      </div>

      <div role="widget">
        <label>[[i18n('label.user.firstName')]]</label>
        <div name="firstName">[[user.properties.firstName]]</div>
      </div>   
      
      <div role="widget">
        <label>[[i18n('label.user.lastName')]]</label>
        <div name="lastName">[[user.properties.lastName]]</div>
      </div>         
      
      <div role="widget">
        <label>[[i18n('label.user.email')]]</label>
        <div name="email">[[user.properties.email]]</div>
      </div>              
      
      <div role="widget">
        <label>[[i18n('label.user.isAdministrator')]]</label>
        <nuxeo-tag>[[user.isAdministrator]]</nuxeo-tag>
      </div>            
      
      <div role="widget">
        <label>[[i18n('label.user.isAnonymous')]]</label>
        <nuxeo-tag>[[user.isAnonymous]]</nuxeo-tag>
      </div>    
      
      <div role="widget">
        <label>[[i18n('label.user.groups')]]</label>
        <nuxeo-tags type="tag" items="[[user.properties.groups]]"></nuxeo-tags>
      </div>    
      
      
		</nuxeo-card>


  </template>

  <script>
    Polymer({
      is: 'nuxeo-user-info',
      behaviors: [Nuxeo.LayoutBehavior],
      properties: {
        /**
         * @schema dublincore
         */
        document: {
          type: Object
        }        
      }      
    });
  </script>
</dom-module>
```

Ahora que ya tenemos nuestro web element sólo tenemos que utilizarlos en las vista de nuestra elección. En este ejemplo lo vamos a añadir en la vista view de los documentos de tipo File:

```html
<!--
`nuxeo-file-view-layout`
@group Nuxeo UI
@element nuxeo-file-view-layout
-->
<link rel="import"  href="../../elements/nuxeo-user-info.html">

<dom-module id="nuxeo-file-view-layout">

  <template>
    <nuxeo-document-viewer role="widget" document="[[document]]"></nuxeo-document-viewer>

    <nuxeo-user-info role="widget"></nuxeo-user-info>

    <nuxeo-document-attachments role="widget" document="[[document]]"></nuxeo-document-attachments>
  </template>

  <script>
  Polymer({
    is: 'nuxeo-file-view-layout',
    behaviors: [Nuxeo.LayoutBehavior],
    properties: {
      /**
      * @doctype File
      */
      document: {
        type: Object
      }    
    },
    

    
    
  });
  </script>

</dom-module>
```

Esta es la apariencia de nuestro nuevo web element:

![nuxeo-user-info web element](/images/nuxeo-user-info-web-element-1200x643.png "nuxeo-user-info web element")

 