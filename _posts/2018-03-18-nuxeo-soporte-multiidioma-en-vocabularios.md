---
title: "Nuxeo: Soporte multiidioma en vocabularios"
header:
  image: /images/vocabulario-multiidioma.jpeg
  og_image: /images/vocabulario-multiidioma.jpeg
tags:
  - Nuxeo
  - Nuxeo Dev Tools
last_modified_at: 2018-03-18-T20:59:57-04:00  
---

Web UI, la interface de usuario de Nuxeo, ofrece soporte para la internalización de los diferentes web elements que la componen. Sin embargo el soporte multiidioma de los vocabularios se gestiona de forma direferente. En este artículo vamos a explicar los pasos a seguir para poder dotar de soporte multiidioma a nuestros vocabularios.

## Definición de un vocabulario
Un vocabulario no es más que una lista de etiquetas que se usa en la aplicación. En primer lugar debemos definir el vocabulario desde la opción **CONFIGURATION > Vocabularies** de Nuxeo Studio. En nuestro ejemplo definiremos un vocabulario denominado contractNature con dos entradas

   - **id**: mnda – **label**: label.directories.ContractNature.mnda
   - **id**: partner – **label**: label.directories.ContractNature.msa

![nuxeo vocabulary definition](/images/nuxeo-vocabulary-definition-744x162.png "nuxeo vocabulary definition")

 
## Definición de un esquema
A continuación vamos a definir un esquema (una colección de metadatos) que asociaremos a un tipo de documento.  El esquema, denominado contract, consta de 4 campos:

   - **expiryDate**: (Date) fecha de expiración del contrato
   - **nature**: (Directoy) tipo de contrato
   - **owner**: (User/Group) Usuario responsable del contrato
   - **startDate**: Fecha de inicio de vigencia del contrato

El campo que nos interesa es nature, que hemos definido como tipo Directory, es decir, un vocabulario. En concreto un vocabulario de tipo contractNature.

![nuxeo schema with directory field](/images/nuxeo-schema-with-directory-field-744x357.png "nuxeo schema with directory field")
 

## Recursos i18n para la traducción de un vocabulario
El siguiente paso consiste en crear un **fichero properties** con los nombres de las etiquestas de nuestro vocabulario y sus traducciones. El fichero para el idioma predeterminado es **messages.properties**. En nuestro ejemplo el idioma predeterminado es el ingles y el fichero messages.properties tendría este aspecto:

```shell
label.directories.ContractNature.mnda=Mutual Non-Disclosure Agreement
label.directories.ContractNature.msa=Master Services Agreement
```

Podemos crear traducciones para tantos idiomas como sea necesario. Los ficheros para las traducciones a idiomas adicionales deben seguir este patrón **messages_[lang]_[country].properties**, donde *[lang]* es el código ISO del idioma y *[country]* es el código ISO del país. Por ejemplo si queremos añadir traducciones en español debemos crear el fichero **messages_es_ES.properties**

```shell
label.directories.ContractNature.mnda=Acuerdo mutuo de no divulgación
label.directories.ContractNature.msa=Acuerdo marco de servicios
```

Una vez creados estos ficheros debemos subirlos a Studio desde la opcion i18n Files accesible desde el menu **CONFIGURATION > Resources**

![nuxeo resources multilanguage](/images/nuxeo-resources-multilanguage-744x344.png "nuxeo resources multilanguage")



Una vez subidos los ficheros de propiedades podremos editarlos desde Nuxeo Studio.


![nuxeo edit resources](/images/nuxeo-edit-resources-744x357.png "nuxeo edit resources")


Y Voilà, ya tenemos todos los ingredientes que necesitamos para traducir a múltiples idiomas nuestro vocabulario


![nuxeo suggestion directory multiidioma](/images/nuxeo-suggestion-directory-744x221.png "nuxeo suggestion directory multiidioma")


