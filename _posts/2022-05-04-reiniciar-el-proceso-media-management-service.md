---
title: "Reiniciar el proceso Media Management service"
header:
  image: /images/2022-05-04-reiniciar-el-proceso-media-management-service/reiniciar-opentext-media-management-desde-la-app-services.png
  og_image: /images/2022-05-04-reiniciar-el-proceso-media-management-service/reiniciar-opentext-media-management-desde-la-app-services.png
tags:
  - OpenText
  - OpenText Media Management
  - OTMM
last_modified_at: 2022-05-04T18:39:28-04:00
---

Cuando estamos trabajando con **OpenText Media Management**, OTMM, podemos tener que
reiniciar el proceso `Media Management service`. Existen diversas maneras de hacerlo en Windows. 
Veamos cuales son.

## Reiniciar OpenText Media Management desde la app Services

Para reiniciar OpenText Media Management desde la app Services de Windows debemos seguir los siguientes pasos:

   - Hacer clic con el botón derecho en el `botón Inicio` para abrir el menú
   - Seleccionar `Ejecutar`
   - Escribir `services.msc` en el pop-up `Ejecutar` que se muestra
   - Se abrirá el `Administrador de servicios` de Windows.
   - Seleccionar el servicio llamado `OpenText Media Manager`
   - Hacer clic con el botón derecho
   - Seleccionar la opción `Reiniciar`

![Reiniciar OpenText Media Management desde la app Services](/images/2022-05-04-reiniciar-el-proceso-media-management-service/reiniciar-opentext-media-management-desde-la-app-services.png)

## Reiniciar OpenText Media Management desde línea de comandos

También podemos reiniciar OpenText Media Management desde la consola de Windows. Para ello debemos seguir los siguientes pasos:

   - En Windows 8.1 y Windows 10 podemos utilizar el menú de usuario accesible con la combinación de teclas `Windows + X` 
     o pulsando con el botón derecho del ratón sobre el botón de inicio y seleccionando `Símbolo del sistema`.
   - A continuación debemos ejecutar los siguientes comandos

    ```shell
    C:\>sc queryex OpenTextMediaManagementService

    C:\>taskkill /f /pid <PID>
    ```

Este es un ejemplo real de ejecución:

![Reiniciar OpenText Media Management desde línea de comandos](/images/2022-05-04-reiniciar-el-proceso-media-management-service/reiniciar-opentext-media-management-desde-la-linea-de-comandos.png)

