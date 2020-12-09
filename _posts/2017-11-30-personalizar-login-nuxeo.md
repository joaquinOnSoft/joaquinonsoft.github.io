---
title: "Personalizar la pantalla de login en Nuxeo"
header:
  image: /images/nuxeo-dev-tools-570x255.png
  og_image: /images/nuxeo-dev-tools-570x255.png
tags:
  - Nuxeo
  - Nuxeo Studio
last_modified_at: 2017-11-30-T20:59:57-04:00  
---

Algo tan sencillo como personalizar la pantalla de login en Nuxeo nos ayudará a dar un aspecto corporativo a la web. En este artículos vamos a explicar los pasos a seguir en Nuxeo Studio para modificar la configuración y obtener el aspecto deseado en la pantalla de login.


## Crear un nuevo Branding
En primer lugar debemos acceder a Nuxeo Studio desde el menu *"ADMINISTRADOR > Centro de Actualizaciones > Ir a Nuxeo Studio"*.  Una vez allí tenemos que:

   1. Seleccionar la opción *Branding* del menu izquierdo
   2. Pulsar el botón *New*
   3. Introducir el nombre de nuestra marca, por ejemplo *«ACME»*.

![Nuxeo Studio: Branding](/images/nuxeo-studio-branding.png "Nuxeo Studio: Branding")



## Personalizar la interface JSF
Una vez creado nuestro Branding podemos comenzar la personalización. La opción «Main Pages» nos permite definir el logotipo y la paleta de color a usar en la interfaz JSF de Nuxeo. Aunque lo habitual es utilizar la interfaz Web UI, podemos asignar el logo para JSF ya que también se puede usar en la pantalla de login. Tan sólo debemos seguir estos pasos:

   1. Hacer click en el botón *Select Resource* que aparece junto a la opción *Logo image*
   2. Pulsar *Seleccionar archivo* en el pop-up *Select Resource*
   3. Seleccionar la imagen a utilizar como logo
   4. Pulsar en el botón *Upload*
   5. Pulsar en el botón *OK*
   6. Añadir el nombre de nuestra marca en el campo *Logo Alt*. Es el texto que se mostrará al pasar sobre el logo con el puntero del ratón

![Nuxeo Studio: Main pages branding with logo](/images/nuxeo-studio-branding-main-pages-with-logo.png "Nuxeo Studio: Main pages branding with logo")



## Personalizar la pantalla de login en Nuxeo
La pestaña *Login Page* permite definir la apariencia de la pantalla de login. Para seleccionar la imagen de fondo debemos seguir los siguientes pasos:

   1. Hacer click en el botón *Select Resource* que aparece junto a la opción *Background Image*
   2. Pulsar *Seleccionar archivo*en el pop-up *Select Resource*
   3. Seleccionar la imagen a utilizar como imagen de fondo
   4. Pulsar en el botón *Upload*
   5. Pulsar en el botón *OK*

Si no queremos utilizar la misma imagen en la cabecera de la interface JSF y en la pantalla de login, debemos seleccionar NO en la opción *Apply Application Logo*. En cuyo caso se habilitará la opción *Login Logo*, donde podremos definir una imagen diferente.


![Nuxeo Studio: login page config](/images/nuxeo-studio-branding-login.png "Nuxeo Studio: login page config")


Podemos utilizar un video en lugar de una imagen como fondo de nuestra pantalla de login. Tan sólo debemos añadir el link al video que queremos usar, así como el tipo de contenido. Por ejemplo:

   * **URL**: MEDIA.NUXEO.COM/VIDEOS/LOGIN_7_10.WEBM 
   * **MIME Type**: VIDEO/WEBM

No es aconsejable utilizar video muy pesados, ya que pueden incrementar sensiblemente el tiempo de carga de la página.


![Nuxeo Studio: login page with background video](/images/nuxeo-studio-branding-login-page-with-video.png "Nuxeo Studio: login page with background video")


Por último sólo debemos guardar los cambios realizados pulsando en el botón *Save* de la esquina superior derecha.