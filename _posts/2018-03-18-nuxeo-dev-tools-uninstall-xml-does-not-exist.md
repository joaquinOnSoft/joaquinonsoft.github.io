---
title: "Nuxeo: Documentos relacionados"
header:
  image: /images/nuxeo-dev-tools-570x255.png
  og_image: /images/nuxeo-dev-tools-570x255.png
tags:
  - Nuxeo
  - Nuxeo Dev Tools
last_modified_at: 2018-03-18-T20:59:57-04:00  
---

**Nuxeo Dev Tools** es un add-on, disponible para Google Chrome o Mozilla Firefox. Mediante una ventana emergente pone a un click de ratón del administrador algunas de las acciones más comúnmente realizadas en la Plataforma Nuxeo. Es sin lugar a dudas una herramienta imprescindible para cualquiera que desarrolle con Nuxeo. La versión 2.1.1 ha introducido un bug que en ciertas ocasiones impide realizar un hot reload, dejando el siguiente mensaje en el fichero de log:

```sh
Caused by: java.io.FileNotFoundException: File '/var/lib/nuxeo/server/packages/store/nuxocial-0.0.0-SNAPSHOT/uninstall.xml' does not exist
```

Mientras se publica el parche que soluciona el problema, podemos seguir los siguiente pasos para poder recuperar nuestro sistema y ser capacer de volver a trabajar con normalidad.

En primer lugar debemos acceder al directorio **<NUXEO-HOME>/packages** y visualizar el contenido del fichero **.packages**. Encontraremos un linea indicando que uno de los paquetes esta en estado installing. Debemos borrar esa linea del fichero

```sh
$ more .packages 
nuxeo-template-rendering-6.7.3=started
nuxeo-9.10-HF01-1.0.0=started
nuxeo-spreadsheet-1.3.4=started
nuxeo-spreadsheet-1.3.3=downloaded
nuxeo-liveconnect-1.2.4=started
nuxeo-liveconnect-1.2.3=downloaded
nuxeo-9.10-HF03-1.0.0=started
nuxeo-web-ui-2.2.0=started
nuxocial-0.0.0-SNAPSHOT=installing
nuxeo-drive-1.7.3=downloaded
nuxeo-vision-1.2.3=started
nuxeo-drive-1.7.4=started
nuxeo-jsf-ui-9.10.0-HF01=started
nuxeo-dam-6.3.3=started
nuxeo-template-rendering-samples-6.7.3=started
nuxeo-9.10-HF02-1.0.0=started
nuxeo-showcase-content-1.2.3=downloaded
nuxeo-showcase-content-1.2.4=started
```

```sh
$ more .packages 
nuxeo-template-rendering-6.7.3=started
nuxeo-9.10-HF01-1.0.0=started
nuxeo-spreadsheet-1.3.4=started
nuxeo-spreadsheet-1.3.3=downloaded
nuxeo-liveconnect-1.2.4=started
nuxeo-liveconnect-1.2.3=downloaded
nuxeo-9.10-HF03-1.0.0=started
nuxeo-web-ui-2.2.0=started
nuxeo-drive-1.7.3=downloaded
nuxeo-vision-1.2.3=started
nuxeo-drive-1.7.4=started
nuxeo-jsf-ui-9.10.0-HF01=started
nuxeo-dam-6.3.3=started
nuxeo-template-rendering-samples-6.7.3=started
nuxeo-9.10-HF02-1.0.0=started
nuxeo-showcase-content-1.2.3=downloaded
nuxeo-showcase-content-1.2.4=started
```

A continuación debemos **borrar el fichero .jar** ubicado en el directorio **<NUXEO-HOME>/nxserver/bundles** correspondiente al paquete que estaba en estado *installing*. Ej.

```sh
$ pwd
/Users/joaquin/nuxeos/nuxeo-server-9.10-tomcat-nuxocial/nxserver/bundles
$ rm nuxocial-master-SNAPSHOT.jar
```

```sh
$ pwd
/Users/joaquin/nuxeos/nuxeo-server-9.10-tomcat-nuxocial/nxserver/bundles
$ rm nuxocial-master-SNAPSHOT.jar
```

Por último volveremos a instalar nuestro paquete y reiniciar el servidor de nuxeo

```sh
$ ./nuxeoctl mp-install nuxocial
$ ./nuxeoctl start
```

Voilà, nuestro sistema ya esta listo para volver a trabajar con normalidad.