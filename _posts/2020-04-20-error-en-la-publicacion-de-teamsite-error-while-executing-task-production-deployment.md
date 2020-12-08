---
title: "Error en la publicación de Teamsite: 'Error while executing task: Production Deployment'"
header:
  image: /images/teamsite-deploy-error-570x255.jpg
  og_image: /images/teamsite-deploy-error-570x255.jpg
tags:
  - OpenText
  - TeamSite
last_modified_at: 2020-04-20-T20:59:57-04:00  
---

Has creado una página web con TeamSite, el gestor de contenido web (WCM) de OpenText, pero al tratar de publicar tu página  encuentras el mensaje «Error while executing task: Production Deployment» en la pantalla de tareas pendientes (To do). Algo similar a esto:

 
```
Job Description: A streamlined workflow for publishing content from Experience Studio to LiveSite.Experience Studio Publish Now

Last Comment: Error while executing task: Production Deployment [root: 4/1/20, 2:30 PM] [ +view 6 previous ]

Task Description: Provides an opportunity to resolve errors that caused the production failure and retry the production deployment.
```

Para solucionar este problema debemos realizar los pasos que detallo a continuación

> **NOTA**: Las rutas indicadas en este artículo pueden variar en función de tu instalación.

 

## Reiniciar LiveSite Content Services (LSCS)
Desde la consola del servidor de TeamSite debemos reiniciar el Tomcat que utilizamos con LiveSite Content Services (LSCS), un API ofrecida por TeamSite:

```
$ /usr/tomcat_lscs/bin/catalina.sh stop

$ /usr/tomcat_lscs/bin/catalina.sh start
```
 

## Gestión de Solr
En primer lugar debemos detener los nodos de Solr. Para ello desde un terminar ejecutaremos los siguiente:

Detener nodos de Solr

```
$ /etc/init.d/solr1 stop

$ /etc/init.d/solr2 stop
```

El número de nodos puede variar en función de nuestra instalación.

Tras un par de minutos debemos volver a iniciar los nodos de Solr:

Reiniciar los nodos de Solr

```
$ /etc/init.d/solr1 start

$ /etc/init.d/solr2 start
```

A continuación, desde un navegador, debemos acceder a la URL del panel de administración de Solr de cada uno de los nodos. Las URL tendran un aspecto similar a este (el número de puerto puede variar en cada instalación):

```
http://<YOUR_IP_ADDRESS>:8983/solr/#/~collections

http://<YOUR_IP_ADDRESS>:8985/solr/#/~collections
```

Debemos acceder a la opción de menu *Collections*:

![Solr admin panel](/images/solr-admin-panel-744x337.jpg "Solr admin panel")

Vamos a eliminar todas las colecciones  cuyo nombre sea un número. En este ejemplo «1367217309». Para ello debemos:

   - Seleccionar el nombre de la colección
   - Hacer click sobre el botón «Delete»
   - Introducir el nombre de la colección en el pop-up de confirmación
   - Hacer click en el botón «Delete» del pop-up


![Solr remove collection](/images/solr-remove-collection-744x336.jpg "Solr remove collection")


Repetiremos esta operación para todas las colecciones con nombre numerico presentes en cada instancia de Solr.

Eso es todo, ahora sólo debemos comprobar que la publicación de contenido funciona correctamente.

## Publicar contenido

Ahora que hemos solucionado el problema vamos a comprobar que la publicación desde e-studio funciona correctamente.  Tan sólo hemos de selecionar la opción «Publish now» para una página o cualquier recurso (imágenes, videos…) y esperar unos segundos.

![TeamSite publish now](/images/teamsite-publish-now-744x340.jpg "TeamSite publish now")
 

Nuestro contenido pasará del estado «NEVER» a «PUBLISHED»

![TeamSite published](/images/teamsite-published-744x341.jpg "TeamSite published")

