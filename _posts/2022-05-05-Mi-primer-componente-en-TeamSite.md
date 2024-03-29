---
title: "Mi primer componente en TeamSite"
header:
  image: /images/2022-05-05-Mi-primer-component-en-TeamSite/componente-teamsite-editable-on-the-glass.png
  og_image: /images/2022-05-05-Mi-primer-component-en-TeamSite/componente-teamsite-editable-on-the-glass.png
tags:
  - OpenText
  - TeamSite
last_modified_at: 2022-05-05T18:39:28-04:00
---

Hoy explicaré como crear mi primer componente en **TeamSite**.

> [OpenText™ TeamSite™](https://www.opentext.com/products-and-solutions/products/customer-experience-management/web-content-management/opentext-teamsite) 
> es un sistema de administración de contenido de sitios web moderno y fácil de usar 
> que ayuda a las organizaciones a crear experiencias de clientes digitales personalizadas y visualmente 
> ricas optimizadas para cualquier dispositivo, canal digital o contexto. Simplifica todo el proceso de 
> gestión de contenido en todos los canales, incluidos sitios web, plataformas móviles, correo electrónico, 
> redes sociales, comercio, aplicaciones compuestas, sitios de colaboración y portales, lo que facilita la 
> entrega de experiencias digitales excepcionales. Desde una sola interfaz, los usuarios pueden crear, probar, 
> orientar y publicar su contenido, así como administrar medios enriquecidos, diseñar sitios web y crear 
> aplicaciones móviles.

## Estudia la página de tu cliente
En primer lugar debemos estudiar el diseño objetivo de la página que queremos crear. En este ejemplo 
tomaremos como referencia la página de la UNESCO.

![Estudia la página de tu cliente](/images/2022-05-05-Mi-primer-component-en-TeamSite/study-your-client-web-page.png)

## Crea o elige una plantilla gratuita de Bootstrap o Foundation

Podemos crear nuestra propia plantilla (HTML + css), o bien utilizar alguna plantilla disponible en alguna
de las webs que las proporcionan de forma gratuita:

| Web de plantillas  | Logo                                                                        |
|--------------------|-----------------------------------------------------------------------------|
| themefisher.com    | ![themefisher.com](/images/2022-05-05-Mi-primer-component-en-TeamSite/themefisher-logo.png)        |
| colorlib.com       | ![colorlib.com](/images/2022-05-05-Mi-primer-component-en-TeamSite/colorlig-logo.png)              |
| bootstrapmade.com  | ![bootstrapmade.com](/images/2022-05-05-Mi-primer-component-en-TeamSite/bootstrapmade-logo.png)    |
| startbootstrap.com | ![startbootstrap.com](/images/2022-05-05-Mi-primer-component-en-TeamSite/strartbootstrap-logo.png) |

Para este ejemplo he seleccionado el [template Small Business de la web startbootstrap.com](https://startbootstrap.com/templates/small-business/)

![Small Business template de  startbootstrap.com](/images/2022-05-05-Mi-primer-component-en-TeamSite/small-business-template.png)

Es conveniente revisar el código HTML de la plantilla y familiarizarse con él. Las diferentes secciones de la página están bien identificadas.

![Small Business template HTML](/images/2022-05-05-Mi-primer-component-en-TeamSite/small-business-template-html-source-code.png)

## Preparación del projecto

### Crear un nuevo proyecto: custom-demo-tutorial

Vamos a crear un nuevo proyecto al que llamaremos `custom-demo-tutorial`

![Small Business template HTML](/images/2022-05-05-Mi-primer-component-en-TeamSite/custom-demo-tutorial-project.png)

Dentro del proyecto debemos crear un nuevo `site`, llamado `custom-demo-site`

### Importar recursos de la plantilla en TeamSite

A continuación, importaremos los activos (css, javascript, imágenes...) en nuestro proyecto de TeamSite. 
Para ello debemos seguir los siguientes pasos:

   - Ir a **CC Profesional**
   - Navegar a: `//tsbase/default/main/custom-demo-tutorial/WORKAREA/default`
   
     > NOTA: `custom-demo-tutorial` es el nombre de nuestro proyecto
	 
   - Hacer clic en la opción **Importar** en el menú superior derecho
   
     ![Importar CC Professional](/images/2022-05-05-Mi-primer-component-en-TeamSite/import-cc-professional.png)
	 
   - Arrastrar y soltar los recursos (js, css, imágenes...) en el área de importación
   - Hacer clic en el botón **Importar** en la esquina inferior derecha
   
Una vez importados los activos, volveremos a **Experience Studio** donde podremos comprobar que todos los activos se han importado correctamente.

![Recursos importados en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/resources-imported-in-teamsite.png)

### Añadir los activos  como Recursos Compartidos

Debemos incluir los recursos CSS y JavaScript para que se incluyan en las páginas de nuestro sitio web:

> **NOTA:** Los recursos CSS y JavaScript se enumeran en el fichero **index.html** de la plantilla

   - Navega a nuestro *site*
   - Hacer clic en el botón **Site Details**
   - Desplegar la sección **Shared Resources**
   - Añadir ficheros css a mediante el botón **Add CSS file**
   - Añadir ficheros JavaScript mediante el botón **Add script file**
   - Hacer clic en **Save**
   
   ![Recursos importados en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/shared-resources-teamsite.png)

### Crear una plantilla personalizada


   - Ir a **Sites** en el menú del lado izquierdo
   - Abrir nuestro sitio
   - Hacer clic en **Templates** en el menú superior
   - Hacer clic en **New Template > Create new template**
   - Hacer clic en **Site template**
   - Seleccionar **Blank**
   - Dar un nombre a la plantilla
   - Hacer clic en **Confirm**

   ![Crear plantilla en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/create-custom-template-teamsite.png)

   - Añadir varios componentes de **Multi placeholder** (Se encuentran en **Custom Components**)
   
   ![Añadir componentes Multi placeholder en una plantilla de TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/add-multi-placeholderin-template-teamsite.png)

   - Hacer clic en **Submit** para publicar la plantilla

   ![Publicar plantilla de TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/submit-template.png)
   
## Componentes personalizados

Para crear nuestro componente personalizado debemos **identificar el HTML en la plantilla**.

![Identificar el HTML de nuestro componente en la plantilla](/images/2022-05-05-Mi-primer-component-en-TeamSite/identificar-el-html-de-nuestro-componente-en-la-plantilla.png)

### Crear una carpeta para nuestros componentes compartidos

Vamos a crear una carpeta para nuestros componentes compartidos. Para ellos seguiremos los siguientes pasos:

   - Ir a **CC Profesional**
   - Navegar a `//tsbase/iwadmin/main/livesite/component/WORKAREA/shared`
   - Hacer clic en el botón **New folder** en el menú superior derecho
   - Dar un nombre, por ejemplo, `CustomDemoTutorial`.
   - Esta carpeta contendrá todos los componentes de nuestro proyecto.
   - Hacer clic en **OK**
   - Hacer clic en la carpeta `CustomDemoTutorial`
   
![Crear una carpeta para nuestros componentes compartidos](/images/2022-05-05-Mi-primer-component-en-TeamSite/crear-carpeta-para-componentes-compartidos.png)
   
### Crear un nuevo componente

Para crear un nuevo componente desde *CC Professional* hay que hacer clic en **File > New Component** en el menú superior izquierdo.

![Nuevo componente en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/new-component.png)   

   - Seleccionar **Create From** `HTML`
   
![Crear componente HTML](/images/2022-05-05-Mi-primer-component-en-TeamSite/create-from-html.png)      
   
   - Proporcionar la información requerida
      - **Nombre:** Custom_demo_heading_row
	  - **Configuración de vista previa:** 
	     - predeterminado: /main/custom-demo-tutorial 
		 - predeterminado: custom-demo-site
      - **Apariencia:** pegar el código de la plantilla entre las etiquetas `<div>` que se muestran de forma predeterminada.
   - Hacer clic en **Finalizar**

En nuestro ejemplo seleccionamos el código correspondiente a la cabecera de la plantilla *Small Business*:
   
```html
<!-- Heading Row-->
<div class="row gx-4 gx-lg-5 align-items-center my-5">
	<div class="col-lg-7"><img class="img-fluid rounded mb-4 mb-lg-0" src="https://dummyimage.com/900x400/dee2e6/6c757d.jpg" alt="..." /></div>
	<div class="col-lg-5">
		<h1 class="font-weight-light">Business Name or Tagline</h1>
		<p>This is a template that is great for small businesses. It doesn't have too much fancy flare to it, but it makes a great use of the standard Bootstrap core components. Feel free to use this template for any project you want!</p>
		<a class="btn btn-primary" href="#!">Call to Action!</a>
	</div>
</div>
```

Y lo pegamos en el cuadro de texto *Appearance*:

![Crear componente HTML](/images/2022-05-05-Mi-primer-component-en-TeamSite/new-component-popup-teamsite.png)      

### Publicar el componente

Para publicar el nuevo componete tan solo hay que seleccionar el componente y hacer clic en el botón **Submit** 
en el menú de la esquina superior derecha:

![Publicar componente en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/publicar-componente-teamsite.png)      

Por último, debemos hace click en el botón **Run Job** del pop-up **Submit workflow**   

![Ejecutar trabajo en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/run-job-teamsite.png)      


### Añadir el componente a una página

Ahora que hemos publicado nuestro componente podemos añadirlo en una página. Para ellos seguiremos estos pasos:

   - Ir a **Experiencia Estudio**
   - Navegar a **Sites**
   - Abrir el sitio web `custom-demo-site`
   - Hacer clic en **New page** en el menú superior izquierdo
   - Seleccione una plantilla de sitio: `custom-demo-main-template`
   - Hacer clic en **Next**
   - Darle un nombre, por ejemplo, `custom-demo-home`
   - Hacer clic en **Next**
   - Hacer clic en **Confirm**

![Crear una nueva página](/images/2022-05-05-Mi-primer-component-en-TeamSite/create-a-new-page.png)

   - Mover el mouse sobre un **MULTI-COMPONENT PLACEHOLDER**
   - Hacer clic en **Add component**
   - Hacer clic en **Components > Custom Components > CustomDemoTutorial**

![Añadir componente a la página](/images/2022-05-05-Mi-primer-component-en-TeamSite/agregar-componente-a-pagina.png)
   
¡Felicidades! Hemos creado nuestro primer componente:
   
![Componente personalizado añadido a una página](/images/2022-05-05-Mi-primer-component-en-TeamSite/componente-personalizado-agregado-a-pagina-teamsite.png)   

A continuación, vamos a mejorar nuestro componente.

## Hacer nuestro componente editable

Para hacer nuestro componente editable debemos:

   - Ir a **CC Professional**
   - Navegar a: //tsbase/iwadmin/main/livesite/component/WORKAREA/shared/CustomDemoTutorial
   - Seleccionar nuestro componente
   - Hacer clic en **Edit**
   - Hacer clic en **Edit with CC Professional**

![Editar componente en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/edit-component-teamsite.png)   

Vamos a añadir 3 atributos que serán modificables en nuestro componente. El *título*, la 
*descripción* y la *imagen*. Para ello debemos añadir 3 campos de tipo`String` (título), 
`TextArea`(descripción) e `image` (imagen) en la sección **Content XML** de nuestro componente.

> **NOTA:** La *ayuda* (Help) de la sección **Content XML** facilita ejemplos de la sintaxis de cada 
> tipo de atributo `Datum Type` que podemos añadir a nuestro componente

```xml
<Data>
  <Datum ID="TITLE" Type="String" Name="Title">Title placeholder</Datum>
  <Datum ID="DESCRIPTION" Type="String" Name="Subtitle">Subitle placeholder</Datum>
  <Datum ID="IMGSRC" Type="Image" Name="Background image">
    <Image>
      <Path>$URL_PREFIX/images/app/banner-bo-app.jpg</Path>
      <Description>Background image</Description>
    </Image>
  </Datum>
</Data>
```

![Añadir atributos en la sección content XML del componente](/images/2022-05-05-Mi-primer-component-en-TeamSite/content-xml.png)   

A continuación, añadimos referencias a los atributos que hemos creado en la sección `Appearance`:

![Editar componente - Appearance](/images/2022-05-05-Mi-primer-component-en-TeamSite/edit-component-appearance-teamsite.png)   

   - Atributo `src` de la etiqueta `<img>`: 

```
$MODEL{/Properties/Data/Datum[@ID='IMGSRC']/Image/Path}
```

> **NOTA:** `IMGSRC` es el nombre que hemos dado al *Datum Type*  de tipo `Image`

   - Título del contenido (etiqueta `<h1>`): 

```
$MODEL{/Properties/Data/Datum[@ID='TITLE’]}
```

> **NOTA:** `TITLE` es el nombre que hemos dado al *Datum Type*  de tipo `String`

   - Descripción (etiqueta `<p>`): 

```
$MODEL{/Properties/Data/Datum[@ID='TEXT']}
```

> **NOTA:** `TEXT` es el nombre que hemos dado al *Datum Type*  de tipo `TextArea`

Tras aplicar los cambios la sección `Appearance` tendrá este aspecto:

```html
<!-- Heading Row-->
<div class="row gx-4 gx-lg-5 align-items-center my-5">
	<div class="col-lg-7"><img class="img-fluid rounded mb-4 mb-lg-0" src="$MODEL{/Properties/Data/Datum[@ID='IMGSRC']/Image/Path}" alt="$MODEL{/Properties/Data/Datum[@ID='IMGSRC']/Image/Description}" /></div>
	<div class="col-lg-5">
		<h1 class="font-weight-light">$MODEL{/Properties/Data/Datum[@ID='TITLE’]}</h1>
		<p>$MODEL{/Properties/Data/Datum[@ID='TEXT']}</p>
		<a class="btn btn-primary" href="#!">Call to Action!</a>
	</div>
</div>
```

Finalmente, hemos de guardar los cambios y publica el componente.

Felicidades, hemos creado nuestro primer componente editable.

### Aplicar los cambios de nuestro componente en la página

Una vez que nuestro componente es editable vamos a aplicar los cambios en la página 
en la que insertamos previamente el componente. Para ello debemos:

   - Ir a `Experience Estudio`
   - Hacer clic en **Sites**
   - Abrir nuestro sitio: `custom-demo-site`
   - Hacer clic en **Edit** en su página `custom-demo-home`
   - Hacer clic en **MERGE**

![Aplicar los cambios de nuestro componente](/images/2022-05-05-Mi-primer-component-en-TeamSite/merge-component-changes-in-page-teamsite.png)   

La página con nuestro componente tendrá este aspecto:

![Componente editable en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/componente-teamsite-editable.png)   

## Hacer el componente `editable on the glass`

Los componentes de TeamSite pueden crearse de modo que el usuario pueda modificar directamente el contenido, 
una vez includido en una página. Es lo que se denomina, en inglés, *edit on the glass*.

Para ello tendremos que modificar de nuevo nuestro componente. Tenemos que:

   - Ir a **CC Profesional**
   - Navegar a : `//tsbase/iwadmin/main/livesite/component/WORKAREA/shared/CustomDemoTutorial`
   - Seleccionar nuestro componente
   - Hacer clic en **Editar**
   - Hacer clic en **Edit wit CC Professional**

![Editar componente con CC Professional](/images/2022-05-05-Mi-primer-component-en-TeamSite/edit-component-with-cc-professional.png)

   - Añadir la etiqueta `<div ddGroup…>` alrededor de las referencias a los atributos editables (referenciadas mediante `$MODEL`)
   
```html
<div ddGroup="imageDropZoneGroup" ddGroupPropertyId="$MODEL{/Properties/Data/Datum[@ID='TEXT']/@ID}" in-context-edit="$MODEL{/Properties/Data/Datum[@ID='TEXT']/@ID}">        
   <p>$MODEL{/Properties/Data/Datum[@ID='TEXT']}</p>
</div>

```

![Etiqueta ddGroup](/images/2022-05-05-Mi-primer-component-en-TeamSite/etiqueta-ddGroup.png)

Finalmente, hemos de guardar los cambios y publica el componente.

### Aplicar los cambios de nuestro componente en la página

Una vez que nuestro componente es `editable on the glass` vamos a aplicar los cambios en la página 
en la que insertamos previamente el componente. Para ello debemos:

   - Ir a `Experience Estudio`
   - Hacer clic en **Sites**
   - Abrir nuestro sitio: `custom-demo-site`
   - Hacer clic en **Edit** en su página `custom-demo-home`
   - Hacer clic en **MERGE**

![Aplicar los cambios de nuestro componente](/images/2022-05-05-Mi-primer-component-en-TeamSite/merge-component-changes-in-page-teamsite.png)   

La página con nuestro componente tendrá este aspecto:

![Componente editable en TeamSite](/images/2022-05-05-Mi-primer-component-en-TeamSite/componente-teamsite-editable-on-the-glass.png)   

El icono del lapiz sobre el texto de la descripción indica que el usuario puede editar directamente el contenido de nuestro componente.