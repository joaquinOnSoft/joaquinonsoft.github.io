---
title: "Como añadir un facet a un tipo de documento en Nuxeo"
header:
  image: /images/add-facet-to-document-type-xml-extension-570x255.png
  og_image: /images/add-facet-to-document-type-xml-extension-570x255.png
tags:
  - Nuexeo
  - Facet
  - Nuxeo Studio
last_modified_at: 2018-09-18-T22:59:57-04:00  
---

De acuerdo a la documentación de Nuxeo los **facets** son marcadores asociados a los tipos de documentos que indican a Nuxeo Platform (o a cualquier otra parte del sistema que se preocupe por ellos) que debe comportarse de manera diferente, o que se configuran automáticamente en algunas instancias de documentos.

> Facets are markers on document types that instruct the Nuxeo platform (and any part of the system that cares about them) to behave differently, or that are automatically set on some document instances.

¿Como podemos crear un nuevo facet y habilitarlo en un tipo de documento?

## Añadir un facet en un extensión XML
En este ejemplo vamos a añadir un facet llamado **Embargable** a los documentos de tipo *Picture*.

Para añadir un facet a un tipo de documento tan sólo hemos de crear una nueva extensión XML. Accederemos a  la opción **CONFIGURATION > Advanced settings > XML extensions** y pegaremos este XML:

```HTML
<require>org.nuxeo.ecm.platform.picture.coreTypes</require>
<extension target="org.nuxeo.ecm.core.schema.TypeService" point="doctype">

  <doctype append="true" name="Picture">    
    <facet name="Embargable"/>
  </doctype>

</extension>
```

![XML Extension: add facet to document type](/images/add-facet-to-document-type-xml-extension.png "XML Extension: add facet to document type")


Una vez añadido el facet al tipo de documento podremos utilizarlo en otras partes del sistema. Por ejemplo en un automation script:

```JavaScript
function run(input, params){ 
    [...]

    if(parent.hasFacet("Embargable")){
      [...]
    }

    [...]
}
```
 

 