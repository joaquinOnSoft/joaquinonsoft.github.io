---
title: "¿Como filtrar los datos en un nuxeo-directory-suggestion?"
header:
  image: /images/como-filtrar-los-datos-en-un-nuxeo-directory-suggestion-570x255.png
  og_image: /images/como-filtrar-los-datos-en-un-nuxeo-directory-suggestion-570x255.png
tags:
  - Nuxeo
  - WebUI
last_modified_at: 2018-07-18T15:59:57-04:00
---

El web element nuxeo-directory-suggestion muestra una **lista desplegable con los valores de un vocabulario** definidio en Nuxeo Studio. En ocasiones no queremos mostrar todos los valores, sino solo unos pocos teniendo en cuenta algún criterio. ¿Como podemos filtrar los datos en un nuxeo-directory-suggestion? nuxeo-directory-suggestion incluye un atributo llamado **queryResultsFilter** que permite filtrar los valores del vocabulario a mostrar.

Nuxeo incluye algunos **vocabularios definidos por defecto**, entre ellos **continent** (lista de continentes) y **country** (lista de paises).

![Nuxeo vocabulary country](/images/nuxeo-vocabylary-country-1200x589.png "Nuxeo vocabulary country")

Vamos a crear un pequeño web element que permita filtrar los paises a mostrar en función del continente elegido por el usuario.

En primer lugar vamos a almacenar el valor del continente seleccionado en una variable. Para ello debemos definir una función que se llamará cuando se produzca evento **value-changed** del nuxeo-directory-suggestion que muestra el continente:

```html 
<dom-module id="nuxeo-se-geo-schema-edit-layout">
  <template>
    <style>
       ...
    </style>

    <nuxeo-directory-suggestion value="{{document.properties.geo:continent}}" 
                                label="[[i18n('label.schema.geo.continent')]]" 
                                directory-name="continent" 
                                role="widget"
                                min-chars="0"                                
                                on-value-changed="_continentUpdated">
    </nuxeo-directory-suggestion>

    ...
  </template>

  <script>
    var gContinent = null;
  
    Polymer({
       ...
       _continentUpdated: function(event){
        if(event && event.detail && event.detail.value){
          gContinent = event.detail.value;          
        }
      },
    });
  </script>
</dom-module>
```

A continuación vamos a escribir la función que se encargará de filtrar los paises en función del pais. Podemos darle a la función el nombre que queramos, pero debe recibir tres parametros:

   - **element**: La entrada del vocabulario que se esta evaluando
   - **index**: El índice del elemento dentro del vocabulario
   - **array**: La lista de entradas del vocabulario

```javascript
      _filterCountries: function(element, index, array){
        var isInContinent = false;

        if(element && element.parent && gContinent){
          isInContinent = (gContinent == element.parent);
        }

        return isInContinent;
      },
```
	  
Ahora sólo tenemos que hacer que asignar la nueva función a la propiedad queryResultsFilter del web element:

```xml
    <nuxeo-directory-suggestion value="{{document.properties.geo:country}}" 
                                label="[[i18n('label.schema.geo.country')]]" 
                                directory-name="country" 
                                role="widget"
                                min-chars="0"                                
                                query-results-filter="[[_filterCountries]]">
    </nuxeo-directory-suggestion>
```

Este sería el código completo del web element:

```html
<!--
`nuxeo-se-geo-schema-edit-layout`
@group Nuxeo UI
@element nuxeo-se-geo-schema-edit-layout
-->
<dom-module id="nuxeo-se-geo-schema-edit-layout">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>

    <nuxeo-directory-suggestion value="{{document.properties.geo:continent}}" 
                                label="[[i18n('label.schema.geo.continent')]]" 
                                directory-name="continent" 
                                role="widget"
                                min-chars="0"                                
                                on-value-changed="_continentUpdated">
    </nuxeo-directory-suggestion>

    <nuxeo-directory-suggestion value="{{document.properties.geo:country}}" 
                                label="[[i18n('label.schema.geo.country')]]" 
                                directory-name="country" 
                                role="widget"
                                min-chars="0"                                
                                query-results-filter="[[_filterCountries]]">
    </nuxeo-directory-suggestion>   
  </template>

  <script>
  	var gContinent = null;
  
    Polymer({
      is: 'nuxeo-se-geo-schema-edit-layout',
      behaviors: [Nuxeo.LayoutBehavior],
      properties: {
        /**
         * @schema dublincore
         * @schema geo         
         */
        document: {
          type: Object
        }
      },

       _continentUpdated: function(event){
        if(event && event.detail &&event.detail.value){
          gContinent = event.detail.value;          
        }
      },
   
      _filterCountries: function(element, index, array){
        var isInContinent = false;

        if(element && element.parent && gContinent){
          isInContinent = (gContinent == element.parent);
        }

        return isInContinent;
      },                
    });
  </script>
</dom-module>
```

Eh Voilà! nuestro nuxeo-directory-suggestion esta filtrando la lista de entradas del vocabulario.

