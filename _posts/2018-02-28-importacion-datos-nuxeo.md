---
title: "Importación de datos con Nuxeo"
header:
  image: /images/nuxeo-data-import-570x255.png
  og_image: /images/nuxeo-data-import-570x255.png
tags:
  - Nuxeo
  - Importación datos
last_modified_at: 2018-02-28-T20:59:57-04:00  
---

En este artículo descubrirás las diferentes **estrategias de importación** que ofrece Nuxeo. El objetivo es integrar un conjunto de datos en un repositorio Nuxeo, proporcionando la forma más segura y eficiente de importar  datos. Detallaremos las formas más populares de importar contenido: **Nuxeo Web UI Mass Importer, Nuxeo CSV Importer, Nuxeo Bulk Importer y REST API Import**.


##UI Mass Importer
Proporciona una sencilla forma de añadir documentos en la plataforma Nuxeo. **El usuario tan sólo debe arrastrar y soltar sus archivos en la carpeta de destino**. Esto permite al usuario hacer pequeñas importaciones por lotes (cientos de documentos).

![UI Mass Importer](/images/ui-mass-mporter-744x464.png "UI Mass Importer")

Las características clave de este importador son:

   - Número de documentos: cientos
   - Permite establecer los metadatos de un conjunto de archivos: Sí
   - Personalizado con: Nuxeo Studio Designer
   - Ubicación del archivo de origen: en el escritorio
   - Multi-hilo: No
   - Rendimiento (documentos / seg): 5
   - Lógica de personalización: Studio View Designer

Otros importadores más avanzados y de alto rendimiento proporcionados por la plataforma Nuxeo son:


## Nuxeo Drive
**Nuxeo Drive** es un plugin que permite la sincronización bidireccional de contenido entre el escritorio local y el repositorio de contenido de Nuxeo. Gestiona la modificación offline de los archivos y se ocupa de las actualizaciones concurrentes.


![Nuxeo Drive](/images/nuxeo-drive-744x448.png "Nuxeo Drive")

Sus principales características son:

   - **Número de documentos**: miles
   - **Permite establecer los metadatos de un conjunto de archivos**: no
   - **Multi-hilo**: Si
   - **Rendimiento (documentos/seg)**: 10
   - El proceso de sincronización se puede pausar
   - Tamaño medio del lote
   - Cliente de escritorio


## Nuxeo CSV Import
Nuxeo CSV Import es un plugin que propociona la capacidad de importar documentos en un repositorio Nuxeo en grandes cantidades, cientos de miles, con sus metadatos asociados utilizando archivos CSV .

![Nuxeo CSV Importer](/images/nuxeo-drive-744x448.png "Nuxeo CSV Importer")

   - **Número de documentos**: hasta cientos de miles
   - **Rellena los valores de un conjunto de archivos**: Sí
   - **Rendimiento (documentos / seg)**: 100
   - Basado en un archivo CSV
   - Con importación de metadatos

A continuación se muestra un ejemplo de ficher csv para realizar una importación:

```csv
"name","type","dc:title","dc:description"
"mi-archivo-1", "Archivo", "Mi archivo 1", "Esta es la descripción de mi archivo 1"
"mi-archivo-2", "Archivo", "Mi archivo 2", "Esta es la descripción de mi archivo 2"
```
 

## Nuxeo REST API
Nuxeo proporciona una API completa accesible a través de HTTP / HTTPS. Esta API es la mejor forma de integrar de forma remota portales, motores de flujo de trabajo, ESB y aplicaciones personalizadas escritas en JavaScript, Ruby, etc., con un repositorio Nuxeo.

Todos los métodos  accesibles siguen este patrón de URL:

```
http://NUXEO_SERVER/nuxeo/api/v1/*
```

   - **Número de documentos**: hasta millones
   - **Permite establecer los metadatos de un conjunto de archivos**: Sí
   - **Rendimiento (documentos / seg)**: 1.000-2.000
   - Tamaño medio del lote
   - Implementación específica de lógica de negocios
   - Adaptado a cada cliente Nuxeo
```
POST     http://NUXEO_SERVER//api/v1/upload
```


## Nuxeo Bulk Importer
El plugin Nuxeo Bulk Document Importer permite la importación masiva de documentos en un repositorio Nuxeo. Puede alcanzar tasas de importación de hasta miles de documentos por segundo. Se puede configurar para utilizar ficheros XML para asignar valores a los metadatos de los documento y elegir los tipos de documentos que se utilizan para el proceso de creación.

![Nuxeo Bulk Import](/images/nuxeo-bulk-importer-744x486.png "Nuxeo Bulk Import")


   - **Número de documentos**: hasta miles de millones
   - **Permite establecer los metadatos de un conjunto de archivos**: Sí
   - **Rendimiento (documentos / seg)**: hasta 10.000
   - Con importación de propiedades
   - Fácilmente personalizable con Nuxeo Studio y en Java
 