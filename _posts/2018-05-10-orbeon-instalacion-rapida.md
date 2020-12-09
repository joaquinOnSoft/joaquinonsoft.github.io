---
title: "Orbeon: Instalación rápida"
header:
  image: /images/orbeon-form-builder-example-460x255.png
  og_image: /images/orbeon-form-builder-example-460x255.png
tags:
  - Orbeon
last_modified_at: 2018-05-10-T20:59:57-04:00  
---

**Orbeon** es un sofware para la gestión de formularios grandes que incluyen validaciones complejas así como amplias colecciones de formularios. Implementa el estándar **W3C XForms** y está disponible en una Edición Community de código abierto, así como en una Edición Profesional con soporte comercial.

En este artículo se explica como instalar Orbeon para poder comenzar a trabajar con el.

Orbeon se distribuye en un zip que contiene varios ficheros .war, por lo que necesitaremos un servidor de aplicaciones, como Tomcat, para su ejecución.

## Instalación de Tomcat
   - Descarga la última versión de Tomcat (8.0.52 en el momento de escribir este artículo)
   - Descomprime el fichero que acabas de descargar. En mi caso *apache-tomcat-8.5.31.zip*

> Asumiremos que TOMCAT_HOME representa la ubicación de nuestra instalación de Tomcat.

   - Asegurate de que el script **TOMCAT_HOME/bin/catalina.sh** tiene permisos de ejecución
```sh 
TOMCAT_HOME/bin$ chmod 744 catalina.sh
```

## Orbeon: Instalación rápida
   - Descarga de la última versión de Orbeon (2017.2.1 en el momento de escribir este artículo) y descomprime el fichero que acabas de descargar. En mi caso *orbeon-2017.2.1.201803152243-PE.zip*
   - Crea un nuevo directorio *TOMCAT_HOME/webapps/orbeon*
   - Descomprime el fichero **orbeon.war** en el directorio orbeon que acabas de crear. Tras descomprimir el fichero debemos tener  un directorio **TOMCAT_HOME/webapps/orbeon/WEB-INF**.
   - Si estamos utilizando la **versión Professional Edition (PE)** de Orbeon debemos copiar el fichero de licencia **license.xml** en el directorio **TOMCAT_HOME/webapps/orbeon/WEB-INF/resources/config/**.
   - Ahora podemos iniciar Tomcat. En sistemas tipo UNIX sería así:

```sh
TOMCAT_HOME/bin$ ./catalina.sh start
```

   - Acceder a http://localhost:8080/orbeon/ para probar nuestra instalación (reemplazando localhost y 8080 con el nombre de host y el número de puerto de nuestra instalación Tomcat si es diferente de la predeterminada).

![Orbeon home page](/images/orbeon-home-page-744x567.png "Orbeon home page")


Para comenzar a trabajar tan sólo debemos pulsar sobre **Form Builder** y podremos comenzar a crear nuestro primer formulario

![Orbeon form builder](/images/orbeon-form-builder-744x567.png "Orbeon form builder")