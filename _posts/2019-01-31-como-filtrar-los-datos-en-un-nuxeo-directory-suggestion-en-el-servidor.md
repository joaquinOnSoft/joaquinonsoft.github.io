---
title: "¿Como filtrar los datos en un nuxeo-directory-suggestion en el servidor?"
header:
  image: /images/nuxeo-drectory-suggestion-filtered-570x255.png
  og_image: /images/nuxeo-drectory-suggestion-filtered-570x255.png
tags:
  - Nuxeo
  - Nuxeo Studio
  - Automation script
last_modified_at: 2019-01-31-T20:59:57-04:00  
---


En el artículo ¿Como filtrar los datos en un nuxeo-directory-suggestion?  veíamos como filtrar las entradas de un vocabulario en el cliente. En ocasiones el número de entradas devuelta por el servidor puede ser muy alta, por lo que es conveniente aplicar el filtro en el servidor antes de devolver la información.

Por defecto el web elements nuxeo-directory-suggestion utiliza la operación Directory.SuggestEntries. Esta operación proporciona un parámetro llamado filters que permite filtrar los entradas a devolver.

Si queremos filtrar los los países europeos incluidos en el vocabulario country debems aplicar el filtro:

> filters: parent=europe

En la operación **Directory.SuggestEntries**.

![nuxeo platform explorer Directory.SuggestEntries](/images/nuxeo-platform-explorer-Directory-SuggestEntries.png "nuxeo platform explorer Directory.SuggestEntries")

Por lo tanto para aplicar el filtro tan sólo debemos asignar el siguiente valor al atríbuto params de nuxeo-directory-suggestion:

> {«filters» : «parent=europe»}

 
```xml
   <nuxeo-directory-suggestion role="widget" 
                                value="{{document.properties.geo:country}}" 
                                label="Country" 
                                directory-name="country" 
                                params="{"filters" : "parent=europe"}" 
                                min-chars="0">
    </nuxeo-directory-suggestion>
``` 

![nuxeo-directory-suggestion params](/images/nuxeo-directory-suggestion-params.png "nuxeo-directory-suggestion params")

Si ahora desplegamos nuestros cambios veremos que el web element **nuxeo-directory-suggestion** solo muestra paises europeos.

![nuxeo-directory-suggestion filtered](/images/nuxeo-drectory-suggestion-filtered.png "nuxeo-directory-suggestion filtered")


 

 