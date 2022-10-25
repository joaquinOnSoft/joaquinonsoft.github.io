---
title: "Pantalla en negro tras login con xRDP en SUSE"
header:
  image: /images/2022-10-25-pantalla-en-negro-tras-login-con-xrdp-en-suse/pantalla-en-negro-tras-login-con-xrdp-en-suse.png
  og_image: /images/2022-10-25-pantalla-en-negro-tras-login-con-xrdp-en-suse/pantalla-en-negro-tras-login-con-xrdp-en-suse.png
tags:
  - xRDP
  - SUSE
last_modified_at: 2022-10-25T20:32:22-04:00
---

Hoy veremos como solucionar el problema de encontrarnos con una pantalla en negro tras hacer login en xRDP en SUSE.

Si utlizas `xRDP` para conectarte a un servidor SUSE, en ocasiones puedes encontrar una pantalla en negro tras la pantalla de *login*.

![xRDP login popup](/images/2022-10-25-pantalla-en-negro-tras-login-con-xrdp-en-suse/xRDP-login-popup.png)

¿Como podemos eliminar esta pantalla en negro? ¿Por qué se produce?

Esta situación ocurre (o puede ocurrir) cuando la misma cuenta de usuario se utilice simultáneamente de forma local y remota.

Para solucionarlo debemos seguir los siguientes pasos:


 - Nos conectaremos al servidor mediante una conexión `ssh`. Desde un terminal debemos ejecutar este comando:
   ```shell
   ssh [username]@[server IP]
   ```

 - Parar el servidor `xRDP`. Para ello debemos ejecutar el siguiente comando en un terminal:
 
   ```shell
   systemctl stop xrdp
   ```
 - Comprobar las conexiones abiertas
   ```shell
   who -u
   ```
   Este comando nos proporcionará una salida similar a esta, en la que se muestra el alias del usuario, 
   el identificador del proceso y la IP de cada conexión abierta:
   
   ```
   ts-base:~ # who -u
   joaquin  pts/0        Oct 25 10:34  old       236876 (:200.0)
   joaquin  pts/1        Oct 25 11:39   .        240888 (64.XXX.XXX.XX)
   root     pts/1        Oct 25 11:37   .        240868 (62.YYY.YYY.YY)
   ```

   > **NOTA**: Si no estas seguro de cual es tu IP puede saberlo accediendo a [What Is My IP ADrress? - ifconfig.me](https://ifconfig.me/)
   
 - Vamos a finalizar las conexiones que no se corresponden con nuestra IP. 
 
   ```shell
   kill -9 <session number> 
   ```
   En nuestro ejemplo:

   ```shell
   kill -9 240888 
   ```
 - Ahora que ya no hay conexiones abiertas, tan sólo tenemos que reiniciar el servicio de xRDP
 
   ```shell
   systemctl start xrdp
   ```
Una vez hecho esto podemos lanzar nuestra conexión RDP. Esta vez funcionará sin problemas.

![SUSE xRDP connection](/images/2022-10-25-pantalla-en-negro-tras-login-con-xrdp-en-suse/suse-xrdp-connection.png)

