# Traducción de la interface de usuario de Nuxeo

![Traducción de la interface de usuario de Nuxeo](/images/atlas-continent-country-570x255.jpg "Traducción de la interface de usuario de Nuxeo")

Nuxeo ofrece la opción de traducir la interface de usuario. En este artículo vamos a explicar como internacionalizr y localizar  la interface de usuario de Nuxeo.

## Definición del tipo de documento y esquema
Comenzaremos con un ejemplo sencillo de un tipo de documento personalizado llamado **Client**. Este tipo de documento lleva asociado un esquema (una colección de metadatos) con la información básica de del cliente: gender (género), name (nombre) y surname (apellido).

![Nuxeo Studio document schema](/images/nuxeo-studio-document-schema-744x567.png "Nuxeo Studio document schema")


## Traducción con i18n
Cuando generamos el layout para las vistas de un tipo de documento **se utilizan los nombres de los campos del esquema como títulos de los campos del formulario**. Si nuestra aplicación sólo se va a utlizar en un país abríamos acabado.

![Nuxeo view designer create layout](/images/nuxeo-view-designer-create-layout-744x567.png "Nuxeo view designer create layout")
 

Para traducir la interface a otro idioma debemos sustituir los literales por una llamada al **método i18n()** del *behavior* **Nuxeo.LayoutBehavior**.

```xml
<nuxeo-directory-suggestion role="widget" 
      value="{{document.properties.client:gender}}" 
      label="Gender" 
      directory-name="genders">
</nuxeo-directory-suggestion>
```

**i18n()** recibe una etiqueta como entrada. Esta etiqueta debe estar definida en el fichero **messages.json** correspondiente a cada idioma.

```xml
<nuxeo-directory-suggestion role="widget" 
      value="{{document.properties.client:gender}}" 
      label="[[i18n('label.doctype.client.gender')]]" 
      directory-name="genders">
</nuxeo-directory-suggestion>
```

## Ficheros de traducciones
La **opción UI > Translations** de View Designer permite crear ficheros de traducciones para los diferentes idiomas que queramos traducir. El fichero de traducciones del idioma predeterminado se denomina **messages.json**. Es un fichero JSON en el que las **claves** son las etiquetas definidas para cada literal utilizado en la interface y el **valor** la traducción al idioma correspondiente.

```json
{
	"label.document.type.client": "Client",
  
	"label.doctype.client.name": "Name",  
	"label.doctype.client.surname": "Surname",
	"label.doctype.client.gender": "Gender",
  
	"label.personalinformation": "Personal Information"
}
```

Este sería el fichero de traducciones correspondiente al frances: **messages-fr.json**

```json
{
	"label.document.type.client": "Client",
  
	"label.doctype.client.name": "Nom",  
	"label.doctype.client.surname": "Nom de famille",
	"label.doctype.client.gender": "Genre",
  
	"label.personalinformation": "Informations personnelles"
}
```

Podemos crear tantos ficheros de traducción como deseemos. Los códigos de idioma soportados estan definidos en Crowdin.

![Nuxeo: crowdin language codes](/images/crowdin-language-codes-744x567.png "Nuxeo: crowdin language codes")
 

Sólo los códigos de idioma incluidos en Crowdin son  códigos de idioma validos. Por ejemplo **messages-es-ES.json es un fichero de traducciones valido**, mientras que** messages-es.json no lo es**, ya que el código de idioma es no esta incluido en Crowdin.

## Resultado final
En nuestro ejemplo el layout de creación del tipo de documento cliente tendría este aspecto:

![Nuxeo View Designer create layout source code](/images/nuxeo-view-designer-create-layout-source-code-744x567.png "Nuxeo View Designer create layout source code")

```html
<!--
`nuxeo-client-create-layout`
@group Nuxeo UI
@element nuxeo-client-create-layout
-->
<dom-module id="nuxeo-client-create-layout">
  <template>
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>
    <nuxeo-input role="widget" value="{{document.properties.dc:title}}" label="[[i18n('title')]]" type="text"></nuxeo-input>

    <nuxeo-input role="widget" value="{{document.properties.dc:description}}" label="[[i18n('label.description')]]" type="text"></nuxeo-input>

    <h3>
      [[i18n('label.personalinformation')]]
    </h3>
    <nuxeo-directory-suggestion role="widget" value="{{document.properties.client:gender}}" label="[[i18n('label.doctype.client.gender')]]" directory-name="genders"></nuxeo-directory-suggestion>

    <nuxeo-input role="widget" value="{{document.properties.client:name}}" label="[[i18n('label.doctype.client.name')]]" type="text"></nuxeo-input>

    <nuxeo-input role="widget" value="{{document.properties.client:surname}}" label="[[i18n('label.doctype.client.surname')]]" type="text"></nuxeo-input>

  </template>

  <script>
  Polymer({
    is: 'nuxeo-client-create-layout',
    behaviors: [Nuxeo.LayoutBehavior],
    properties: {

      /**
         * @doctype Client
         */
      document: {
        type: Object,
      },

    }
  });
  </script>
</dom-module>
```
 

## Traducciones en github
Podemos reutilizar las etiquetas definidas por Nuxeo en nuestros web elements, como hemo hecho con ‘title’ y ‘label.description’ en el ejemplo. Las traducciones estan accesibles en github.

![Nuxeo: traducciones github](/images/nuxeo-traducciones-github-744x567.png "Nuxeo: traducciones github")


## Otras traducciones
Cualquier literal que se muestre en la interface de usuario es susceptible de traducirse utilizando el método i18n.

## Artículos relacionados
[Nuxeo: Soporte multiidioma en vocabularios](./nuxeo-soporte-multiidioma-en-vocabularios)
