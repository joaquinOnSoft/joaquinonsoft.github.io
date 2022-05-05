---
title: "Mi primer componente en TeamSite"
header:
  image: /images/2022-05-05-Mi-primer-componente-en-TeamSite/24-nte-transcribe-2nd-time.png
  og_image: /images/2022-05-05-Mi-primer-componente-en-TeamSite/24-nte-transcribe-2nd-time.png
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

![Estudia la página de tu cliente](study-your-client-web-page.png)

## Crea o elige una plantilla gratuita de Bootstrap o Foundation

Podemos crear nuestra propia plantilla (HTML + css), o bien utilizar alguna plantilla disponible en alguna
de las webs que las proporcionan de forma gratuita:

| Web de plantillas  | Logo                                                                        |
|--------------------|-----------------------------------------------------------------------------|
| themefisher.com    | ![themefisher.com](/images/2022-05-05-Mi-primer-componente-en-TeamSite/themefisher-logo.png   )     |
| colorlib.com       | ![colorlib.com](/images/2022-05-05-Mi-primer-componente-en-TeamSite/colorlig-logo.png)              |
| bootstrapmade.com  | ![bootstrapmade.com](/images/2022-05-05-Mi-primer-componente-en-TeamSite/bootstrapmade-logo.png)    |
| startbootstrap.com | ![startbootstrap.com](/images/2022-05-05-Mi-primer-componente-en-TeamSite/strartbootstrap-logo.png) |

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
