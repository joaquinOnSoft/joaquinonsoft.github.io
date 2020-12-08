---
title: "Crear un tipo de documento en función del mime-type en Nuxeo"
header:
  image: /images/nuxeo-contract-custom-doc-type-570x255.png
  og_image: /images/nuxeo-contract-custom-doc-type-570x255.png
tags:
  - Nuxeo
  - Web UI
last_modified_at: 2018-08-01-T20:59:57-04:00  
---

Cuando arrastramos y soltamos un fichero sobre una carpeta o espacio de trabajo en Nuxeo automáticamente se crea un nuevo documento de tipo *File*, *Image*… en función del tipo de fichero que hemos utilizado. Por ejemplo, al arrastrar un fichero *.png* se crea un documento de tipo *Image*. ¿Pero que ocurre si queremos crear un tipo de documento determinado en función del tipo de fichero (mime-type)?

Nuxeo proporciona un conjunto de extensiones que nos permiten modificar el comportamiento de la plataforma sin necesidad de programar. Podemos consultar todos los puntos de extensión disponibles en Nuxeo Platform Explorer.

El sistema de plugins para **FileManager** ofrece la posibilidad de registrar la extensión que será responsable de crear un documento a partir de un mime-type determinado. Esta es la extensión por defecto:

```xml
<extension point="plugins" target="org.nuxeo.ecm.platform.filemanager.service.FileManagerService">
    <documentation>
      Default plugins for the file manager.

      NoteImporter creates a Note document from any text-based content.

      DefaultFileImporter creates a File document from any content.
    </documentation>

    <plugin class="org.nuxeo.ecm.platform.filemanager.service.extension.NoteImporter" name="NoteImporter" order="10">
      <filter>text/plain</filter>
      <filter>text/html</filter>
      <filter>application/xhtml+xml</filter>
      <filter>text/x-web-markdown</filter>
    </plugin>

    <plugin class="org.nuxeo.ecm.platform.filemanager.service.extension.DefaultFileImporter" name="DefaultFileImporter" order="100">
      <filter>.*</filter>
    </plugin>

    <plugin class="org.nuxeo.ecm.platform.filemanager.service.extension.ExportedZipImporter" name="ExportedArchivePlugin" order="10">
      <filter>application/zip</filter>
    </plugin>

    <plugin class="org.nuxeo.ecm.platform.filemanager.service.extension.CSVZipImporter" name="CSVArchivePlugin" order="11">
      <filter>application/zip</filter>
    </plugin>

    <documentation>
      Use a query model to find the list of all Workspaces the user has the
      right to create new document into.
    </documentation>
    <creationContainerListProvider class="org.nuxeo.ecm.platform.filemanager.service.extension.DefaultCreationContainerListProvider" name="defaultCreationContainerListProvider"/>
  </extension>
```

Supongamos que hemos creado un nuevo tipo de documento llamado Contrat. Ademas queremos que cada vez que arrastremos y soltemos un documento MS Word o PDF (.doc, .docx y .pdf) se cree un nuevo documento de tipo Contrat.

Queremos tratar tres tipos de documentos (MIME-types). Estos son sus MIME-types asociados.:

   - **doc**: application/msword
   - **docx**: application/vnd.openxmlformats-officedocument.wordprocessingml.document
   - **pdf**: application/pdf

Vamos a sobrescribir la extensión por defecto

```xml 
<require>org.nuxeo.ecm.platform.picture.filemanager.contrib</require>

<extension target="org.nuxeo.ecm.platform.filemanager.service.FileManagerService" point="plugins">
  <plugin class="org.nuxeo.ecm.platform.filemanager.service.extension.DefaultFileImporter" name="ContractImporter" order="1" docType="Contract">        
      <filter>application/msword</filter>    
      <filter>application/vnd.openxmlformats-officedocument.wordprocessingml.document</filter>        
      <filter>application/pdf</filter>            
  </plugin>  
  
  <plugin class="org.nuxeo.ecm.platform.filemanager.service.extension.DefaultFileImporter" name="DefaultFileImporter" order="100">
    <filter>.*</filter>
  </plugin>    
</extension>
```
Hay que prestar atención  a varios detalles:

   - Atributo **order**: Indica el orden de cargar del plugin. Se empieza a cargar por el número más alto. Los más bajos al final, por lo que su configuración prevalece en caso de coincidencia.
   - Atributo **docType**: Indica el tipo de documento a generar. En este caso un documento de tipo Contrat
   - Etiquetas **filter**: Definen los tipos
   
Por último debemos acceder a la opción **CONFIGURATION > Advanced Setting > XML Extension** de Nuxeo Studio y pegar el XML en una nueva extensión que crearemos a tal efecto.

![Nuxeo: Advanced setting: XML Extension](/images/nuxeo-advanced-setting-xml-extension-1200x555.png "Nuxeo: Advanced setting: XML Extension")

Tras desplegar los cambios en nuestra instancia la próxima vez que arrastremos  y soltemos un fichero .docx, .doc o .pdf se creará un documento de tipo Contract



