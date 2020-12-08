---
title: "Metadato pregenerado por Nuxeo en la creación del documento"
header:
  image: /images/metadato-pregenerado-nuxeo-570x255.png
  og_image: /images/metadato-pregenerado-nuxeo-570x255.png
tags:
  - Nuxeo
  - Nuxeo Studio
last_modified_at: 2018-06-19-T20:59:57-04:00  
---

En ocasiones queremos que un documento tenga un título, u otro metadato, inicializado a la hora de mostrar el formulario de creación de un tipo de documento en Nuxeo. Para ello podemos utilizar el objeto Funtions, denominado **Fn**, que proporciona varias funciones muy útiles, entre ellas **Fn.getNextId(String)**

> **Fn.getNextId(String)**: obtiene un valor único para la clave dada. Cada vez que se llama a esta función utilizando la misma clave, se devolverá una cadena diferente.

Para inicializar un metadado debemos:

   - Definir un Automation Chain que asigne un valor al metadato que nos interese
   - Definir un Event Handler que ejecute nuestra cadena de automatización cuando el documento se ha creado vacio.

En nuestro ejemplo trabajaremos con un **tipo de documento** llamado **Tender**, que tiene un **esquema asociado** llamado **tend**, con un único **metadato** asociado, **ref**, que es un String.

Queremos inicializar la refencia en el momento de mostrar al usuario el formulario de creacion. La refenrecia debe seguir el siguiente patrón:

> TEND-XXXXXX

donde:

   - TEND: literal
   - -: literal
   - XXXXXX: Número único de referencia

## Definición de un Automation Chain
En primer lugar accederemos al menú **Automation > Automation Chains** y crearemos una nueva cadena de automatización llamada acInitTenderAttributes, que incluirá las siguiente operaciones:

   - Fetch > Context.FetchDocument
   - Document > Document.SetProperty
      - XPath: tend:ref
      - save: false
      - value: TEND-@{ Fn.getNextId(«TEND») }

![automation chain init metatdata nuxeo](/images/automation-chain-init-metatdata-nuxeo.png "automation chain init metatdata nuxeo")


## Definición de un Event Handler
A continuación crearemos un Event Handler desde el menu Automation > Event Handlers que se encargue de ejecutar la cadena de automatización cuando el documento se crea como un  «Documento vacio». Para ellos debemos establecer los siguientes valores en la pantalla de configuración:

   - **Event handler definition**
      - **Events**: Empty document created
   - **Event handler activation**:
      - **Current document has one of the types**: Tender
      - **Current document has facet**: Any
      - **Current document is**: Regular Document
   - **Event handler execution**:
      - **Select an existing automation chain**: acInitTenderAttributes

![Event handler Nuxeo](/images/event-handler-nuxeo.png "Event handler Nuxeo")


 

## Vista de creación
La vista de nuestro formulario de creación para el tipo de documento «Tender» tendrá este aspecto en View Designer:

```html
<!--
`nuxeo-tender-create-layout`
@group Nuxeo UI
@element nuxeo-tender-create-layout
-->
<link rel="import" href="../../elements/nuxeo-tender-edit-widget.html">

<dom-module id="nuxeo-tender-create-layout">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>

    <nuxeo-input role="widget" 
                 value="{{document.properties.dc:title}}" 
                 label="[[i18n('title')]]" 
                 type="text">
    </nuxeo-input>
    
    <nuxeo-input role="widget" 
                 value="{{document.properties.dc:description}}" 
                 label="[[i18n('label.description')]]" 
                 type="text">
    </nuxeo-input>

    <nuxeo-input role="widget" 
                 value="{{document.properties.tend:ref}}" 
                 label="[[i18n('label.tender.ref')]]" 
                 type="text">
    </nuxeo-input> 

  </template>

  <script>
  Polymer({
    is: 'nuxeo-tender-create-layout',
    behaviors: [Nuxeo.LayoutBehavior],
    properties: {

      /**
         * @doctype Tender
         */
      document: {
        type: Object,
      },

    }
  });
  </script>
</dom-module>
```
 

### Desplegando los cambios
Una vez completados los pasos anteriores sólo debemos desplegar los cambios que hemos realizado en nuestro entorno. Cuando creemos un nuevo documento veremos como el campo referencia tiene un valor por defecto que varía cada vez.

![metadato pregenerado  nuxeo](/images/metadato-pregenerado-nuxeo-570x255.png "metadato pregenerado  nuxeo")

## Una vuelta de tuerca más
Nuestra referencia utiliza un número correlativo: 1, 2, 3,… Por lo que las referencias generadas son: TEND-1,TEND-2,TEND-3… Pero, ¿Que ocurre si queremos que nuestra referencia tenga siempre la misma longitud?

Tan sólo debemos añadir un par de llamadas a objetos Java para formatear con ceros a la izquierda nuestro número correlativo. Así nuestras referencias pasarán a tener este aspecto: TEND-0000004,TEND-0000005,TEND-0000006…

   - **Fetch > Context.FetchDocument**
   - **Document > Document.SetProperty**
   - **XPath: tend:ref**
      - **save**: false
      - **value**: TEND-@{java.lang.String.format(«%07d», java.lang.Integer.parseInt( Fn.getNextId(«TEND») ) )}

**Integer.parseInt** convierte el identificador a entero, ya que **Fn.getNextId** devuelve una cadena. Por otro lado la llamada a **java.lang.String.format** rellena con ceros a la izquiera hasta un máximo de 7 posiciones.


![Automation chain Nuxeo](/images/automation-chain-nuxeo.png "Automation chain Nuxeo")


> Cuando utilizamos objeto java en expresiones MVEL debemos utilizar el nombre del objeto incluyendo el nombre del paquete en el que esta contenido para evitar posibles conflictos

Ahora nuestra pantalla de creación de un documento tipo Tender tendrá este aspecto:


![metadato pregenerado ceros izquierda nuxeo](/images/metadato-pregenerado-ceros-izquierda-nuxeo.png "metadato pregenerado ceros izquierda nuxeo")


 

