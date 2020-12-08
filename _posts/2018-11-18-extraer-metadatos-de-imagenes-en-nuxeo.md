---
title: "Extraer metadatos de imágenes en Nuxeo"
header:
  image: /images/imagen-geolocalizada-por-coordenadas-gps-nuxeo-570x255.png
  og_image: /images/imagen-geolocalizada-por-coordenadas-gps-nuxeo-570x255.png
tags:
  - Nuxeo
  - Nuxeo Studio
  - Metadatos
last_modified_at: 2018-11-18-T20:59:57-04:00  
---

El plugin DAM de Nuxeo proporciona nuevos tipos de documentos como **Picture, Video o Audio**. Por defecto, al añadir una nueva imagen se crea un documento de tipo Picture y se extraen una serie de metadatos de la imagen (orientación, apertura, exposición, distancia focal…). La información que se extrae no es más que una pequeña parte de la que puede llegar a contener (EXIFT, IPTC, GPS…). En determinados casos puede ser interesance extraer esta información adicional.  

## Definir un esquema para almacenar la información
Para extraer nuevos medatados  de una imagen, en primer lugar debemos definir el esquema o esquemas que usaremos para guardar la información. En este ejemplo crearemos un esquema llamado **gps** para almacenar las coordenadas de latitud y longitud en las se tomo la fotografía.

![Definir un esquema para almacenar la información](/images/nuxeo-gps-image-metadata-1200x625.png "Definir un esquema para almacenar la información")
 
## Asociar el esquema a un tipo de documento
A continuación debemos asociar el esquema con un tipo de documento, por ejemplo con el tipo Picture.  
Filtrado de documentos y mapeo a emplear
Nuestro siguiente paso conssitirá en crear una extensión XML desde el menú **CONFIGURATION >Advanced settings > XML Extensions** de Nuxeo Studio. La podemos llamar **MetadataRules**. Contendrá un filtro para identificar el tipo de documento sobre el que aplicar la extracción de metadatos (Picture en nuestro ejemplo).

```xml
  <filter id="isPicture">
    <rule grant="true">
      <type>Picture</type>
    </rule>
  </filter>
```

También debemos indicar que mapeo de metatados (EXIFT/IPTC/GPS… – esquema de Nuxeo) que queremos aplicar. En nuestro ejemplo el mapeo se llamará PICTURE (que definiremos más tarde):

```xml
  <rule id="apply-on-picture" order="0" enabled="true" async="false">
    <metadataMappings>
      <metadataMapping-id>PICTURE</metadataMapping-id>
    </metadataMappings>
    <filters>
      <filter-id>isPicture</filter-id>
    </filters>
  </rule>
```

La extensión XML **MetadataRules** completa tendrá este aspecto:

```xml 
<extension target="org.nuxeo.binary.metadata" point="metadataRules">

  <rule id="apply-on-picture" order="0" enabled="true" async="false">
    <metadataMappings>
      <metadataMapping-id>PICTURE</metadataMapping-id>
    </metadataMappings>
    <filters>
      <filter-id>isPicture</filter-id>
    </filters>
  </rule>

</extension>

<extension target="org.nuxeo.ecm.platform.actions.ActionService" point="filters">

  <filter id="isPicture">
    <rule grant="true">
      <type>Picture</type>
    </rule>
  </filter>

</extension>
```

## Conocer el nombre de los metadatos con ExifTool

¿Como posemos saber que nombre de los metadatos contenidos en la imagen? Nuxeo utiliza ExifTool para extraer los metadatos de las imágenes.

> ExifTool es un software gratuito de código abierto que permite leer, escribir y manipular metadatos de imágenes, audio, video y PDF. Es independiente de la plataforma. Esta disponible como biblioteca de Perl así como aplicación de línea de comandos.

Podemos ejecutar la siguiente instrucción por línea de comandos para conocer los nombres de los metatos:

```shell
$ exiftool -n -json image-example.jpg
```

La lista de metadatos es enorme, así que la podemos filtrar para encontrar los que nos interesan:

```shell
$ exiftool -n -json image-example.jpg | grep GPS
```

En la imagen de nuestro ejemplo obtendremos esta salida:

```shell
  "GPSLatitudeRef": "N",
  "GPSLongitudeRef": "E",
  "GPSLatitude": 43.4670816666667,
  "GPSLongitude": 11.8845383333333,
  "GPSPosition": "43.4670816666667 11.8845383333333",
```

Esos son los nombre de los metadatos que debemos utilizar en nuestra extensión XML de mapeos.  

## Mapeo de metadatos (EXIFT/IPTC/GPS… – Esquema de Nuxeo)
Debemos crear una segunda extensión XML, que llamaremos **MetadataMapping**, en la que definiremos la correspondencia entre los metadatos contenidos en la imagen y los campos de nuestros esquema.

```xml
<extension target="org.nuxeo.binary.metadata" point="metadataMappings">

  <metadataMapping id="PICTURE" processor="exifTool" blobXPath="file:content" ignorePrefix="true">

    <!-- GPS metadata -->
    <metadata name="GPSLatitude" xpath="gps:exifGPSLatitude"/>
    <metadata name="GPSLongitude" xpath="gps:exifGPSLongitude"/>

  </metadataMapping>

</extension>
```

## La etiqueta metadata incluye dos atributos:
   - **name**: El nombre del metadato en la imagen.
   - **xpath**: El XPath al campo del esquema que queremos utilizar para guardar la información.

Hay que fijarse en que el **id** de la etiqueta **metadataMapping** es **PICTURE**, que habíamos definido en un apartado anterios.  
  
![nuxeo image metadata mapping xml extension](/images/nuxeo-image-metadata-mapping-xml-extension.png "nuxeo image metadata mapping xml extension")


Y ya está. Una vez que despleguemos los cambios en nuestra instancia los metadatos de latitud y longitud estaran asociados a los documentos de tipo Picture y se extraenran de la imagen (siempre que esta contenga dicha información).  