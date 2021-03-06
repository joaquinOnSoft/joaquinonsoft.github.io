---
title: "Como cambiar el logo en Web UI (Nuxeo)"
header:
  image: /images/web-ui-custom-logo-1200x886.png
  og_image: /images/web-ui-custom-logo-1200x886.png
tags:
  - Nuxeo
  - Web UI
last_modified_at: 2017-11-16T15:59:57-04:00
---

Una de las primeras tareas a abordar a la hora de personalizar la interfaz **Web UI** de Nuxeo utilizando **Studio Designer**,  es la de adaptar la apariencia de las páginas a la imagen corporativa de la empresa. Esto implica la inclusión del logo de la empresa y el uso de una paleta de colores determinada. Para ello debemos crear un nuevo tema en Studio Designer.

## Cambiar el logo en Web UI
En primer lugar debemos acceder a la pantalla **UI > Themes**  de Studio Designer y hacer clic en el **botón añadir (+)** que aparece en la esquina inferior derecha para crear un nuevo tema.

![Studio Designer: UI](/images/studio-deigner-ui-theme-1200x970.png "Studio Designer: UI")

A continuación se nos muestra el pop-up de creación del nuevo tema. Debemos introducir el nombre que queremos darle a nuestro tema y la plantilla que deseamos usar como punto de partida. Nuxeo proporciona 4 temas por defecto: *Nuxeo, Dark, Kawaii y Light*. En este ejemplo utilizaremos el tema *Kawaii* como base para nuestro tema, al que llamaremos *velvetTheme*.

![Studio Designer: New Theme](/images/studio-designer-new-theme-pop-up-1200x887.png "Studio Designer: New Theme")

Posteriormente veremos la pantalla de edición de nuestro tema, en la que se muestran cuatro elementos: la hoja de estilo, el nombre del tema, la imagen utilizada para la previsualización en la pantalla de selección de tema y el botón para añadir nuevos recursos (imágenes, videos o fuentes que utilizaremos en el tema).

![Studio Designer: New theme editor](/images/studio-designer-new-theme-editor-1200x886.png "Studio Designer: New theme editor")

Para cambiar el logo predeterminado debemos añadir un nuevo recurso pulsando sobre el botón *«ADD»* y adjuntando un fichero que debe llamarse *«logo.png»*.

Adicionalmente podemos añadir una imagen de previsualización, que se utilizará en la pantalla de selección de temas de la interfaz de usuario  (**User Settings > Themes**). Para ello debemos hacer click sobre el icono que aparece junto al título Screenshot y subir nuestra imagen (tamaño recomendado 1280 x 850).

Una vez completados estos pasos debemos pulsar el botón «Save» para guardar nuestros cambios.


![Studio Designer: New theme with logo](/images/studio-designer-new-theme-editor-with-logo-1200x885.png "Studio Designer: New theme with logo")


## Traducciones
Studio Designer ha creado internamente una etiqueta llamada themes. **THEME_NAME**, donde **THEME_NAME** es el nombre que le dimos a nuestro tema (en nuestro ejemplo themes.velvetTheme).  Debemos añadir una traducción en el fichero de traducciones, por defecto messages.json, (accesible desde el menú **UI &gt; Translations**) con la etiqueta correspondiente  a nuestro tema.

```json
{
	"themes.velvetTheme": "Velvet"
}
``` 

![Studio Designer: UI translations](/images/studio-designer-ui-translations-1200x607.png "Studio Designer: UI translations")


Viendo los resultados
Una vez creado el tema y añadida la traducción tan  sólo tenemos que aplicar los cambios realizados desde Studio Designer en nuestra instancia y seleccionar el nuevo tema desde la opción de menu User Settings > Themes.

![Nuxeo Web UI: User settings > themes](/images/nuxeo-web-ui-user-settings-themes-1200x883.png "Nuxeo Web UI: User settings > themes")

¡Voilà! ya tenemos nuestra instancia de Nuxeo con un nuevo tema que incluye un logo y colores personalizados.

![Web UI: Custom logo](/images/web-ui-custom-logo-1200x886.png "Web UI: Custom logo")
 