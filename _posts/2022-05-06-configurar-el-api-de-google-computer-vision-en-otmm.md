---
title: "Configurar el API de Google Computer Vision en OTMM"
header:
  image: /images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/rma-google-vision-api.png
  og_image: /images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/rma-google-vision-api.png
tags:
  - OpenText
  - OpenText Media Management  
  - OTMM
last_modified_at: 2022-05-06T19:01:56-04:00
---

**Rich Media Analysis**, el módulo de OpenText Media Management (OTMM) para el análisis de imágenes y videos, puede 
funcionar con diversos proveedores de IA como Google, Amazon y Azure. En este artículo explicaré como configurar 
el **API de Google Computer Vision** en **OTMM**.

> **Rich Media Analysis** proporciona análisis de imágenes almacenadas en OpenText Media Management 
> para enriquecer la experiencia de búsqueda y descubrimiento. 
> Mediante los conectores con Azure Computer Vision, Amazon Recognition y Google Vision permite realizar 
> etiquetado automático de imágenes por número de personas, rostros, edad, género, descripciones, objetos, colores y OCR de subtítulos.

## Configurar el API de Google Computer Vision

### Crear un proyecto en Google Cloud Vision

En primer lugar hemos de acceder a [Google Cloud](https://cloud.google.com/) e iniciar sesión con nuestra cuenta de Google. 
A continuación hemos de seguir los siguientes pasos:

   - En la página de inicio, hacer clic en el botón **Go to Console**.

   - En la página de la consola, hacer clic en **Select Project**

![Google Cloud Platform dashboard](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-dashboard.png)

   - En la ventana *Projects*, elija **New Project**
   
![Google Cloud Platform - New Project](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-new-project.png)
   
   - Asignar un nombre al proyecto, por ejemplo, `GoogleVision` y haga clic en **Create**.
   
![Google Cloud Platform - Project name](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-project-name.png)   

### Habilitar el API de Google Cloud Vision

A continuación, debemos habilitar el API de Google Cloud Vision. Para ello tendremos que realizar las siguientes acciones:

   - Desde el panel, hacer clic en **Go to APIs overview** en la sección `APIs`:

![Google Cloud Platform - APIs](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-apis.png)   

   - En la página *APIs and Services*, hacer clic en **Library** y escribir `vision` en el cuadro de búsqueda.
   - Seleccionar **Cloud Vision API**
   - En la página de *Cloud Vision API*, hacer clic en **Enable**

![Enable Cloud Vision API](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-enable-cloud-vision-api.png)   

### Crear credenciales

Una vez que el API de *Google Cloud Vision* está habilitada, crearemos las credenciales que usaremos mas tarde. Tan solo tendremos que seguir estos pasos:

   - En la página *Cloud Vision API*, seleccione **Credentials**

![Enable Cloud Vision - Credentials](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-credentials.png)   

   - En la página *Credentials*, hacer clic en **+CREATE CREDENTIALS**
   
![Enable Cloud Vision +CREATE CREDENTIALS](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-plus-create-credentials.png)      
   
   - Seleccionar **Service Account**
 
![Enable Cloud Vision - Service Account](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-service-account.png)       
 
   - Proporcionar el nombre de la cuenta de servicio y una descripción opcional
   - Hacer clic en **Create and Continue**

![Enable Cloud Vision - Create Service Account](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-create-service-account.png)          

   - Seleccionar un Rol, por ejemplo, **Owner**, hacer clic en **Continue**
   - Hacer clic en **Done**
   
### Agregar una clave a la cuenta de servicio

Una vez creada la cuenta de servicio, tenemos que 

   - Hacer clic en el nombre de la cuenta en la página Credenciales

![Enable Cloud Vision - Service accounts](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-service-accounts.png)          

   - En la página de detalles de la cuenta de servicio, seleccione **Keys** y haga clic en **Add Keys > Create New**
   - Seleccionar `JSON` en la ventana emergente y hacer clic en **Create**

![Enable Cloud Vision - Create private key for Vision API](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-create-private-key-for-vision-api.png)          

   - La página mostrará la ventana **Save as** para descargar el archivo JSON. Debemos guardarlo en un lugar seguro, ya que no se puede volver a descargar desde Google Cloud

### Agregar una cuenta de facturación al proyecto

Por último vamos a añadir una cuenta de facturación al proyecto. Para ello hemos de:

   - Desde el panel, hacer clic en el enlace **Billing**
   
![Enable Cloud Vision - Agregar una cuenta de facturación](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-billing.png)             
   
   - La página nos avisará de que no hay una cuenta de facturación asociada: `This project has no billing account`.

![Enable Cloud Vision - This project has no billing account](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-this-project-has-no-billing-account.png)
 
   - Hacer clic en **Link a billing account**
   - Si se muestra el mensaje `No active billing accounts`, tendremos que hacer clic en **Manage Billing Accounts** y cree una nueva cuenta

![Enable Cloud Vision - Crear cuenta de facturación](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-create-billing-account.png)

   - Una vez que se crea una cuenta de facturación, volveremos al paso **Link account** del proceso.
   - Seleccionar la cuenta de facturación del menú desplegable y haga clic en **Set Account**

![Enable Cloud Vision - Asignar cuenta](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-set-account.png)                 


Nuestro API de Cloud Vision esta lista.

## Configurar las propiedades de Rich Media Analysis (RMA)

### Añadir la ubicación del archivo de clave del API al archivo `google.properties`

Una vez finalizada la creacción de nuestra cuenta en Google Cloud Vision y haber habilitado el API, solo tenemos que
añadir la ubicación del archivo de clave del API al archivo `google.properties`. Para ello debemos:

   - Guardar el archivo JSON con la clave del API del Google Cloud Vision  en el servidor RMA.
   - Abrir el archivo **google.properties** ubicado en la carpeta `%RMA_HOME%\conf` y añadir la propiedad con la ubicación del archivo JSON:

```sh
mediaanalysis.google.service-account-key-file=C:/apps/RichMediaAnalysis/conf/cloud-vision-237961-98ae24d938b4.json
```

> **NOTA:** Hay que usar **/** en el *path*

   - Opcionalmente, podemos añadir la propiedad `mediaanalysis.google.features` y seleccionar algunas o todas las funciones de la siguiente lista:

```sh
mediaanalysis.google.features=TYPE_UNSPECIFIED,FACE_DETECTION,LANDMARK_DETECTION,LOGO_DETECTION,LABEL_DETECTION,TEXT_DETECTION,DOCUMENT_TEXT_DETECTION,SAFE_SEARCH_DETECTION,IMAGE_PROPERTIES,CROP_HINTS,WEB_DETECTION,PRODUCT_SEARCH,OBJECT_LOCALIZATION
```

###	Establecer Google Cloud Vision como proveedor en las propiedades de la aplicación

   - Abrir el archivo **application.properties** ubicado en la carpeta `%RMA_HOME%\conf` y configurar la propiedad `mediaanalysis.image.provider`
   
```sh
   mediaanalysis.image.provider=Google
```
   
   - Reiniciar los servicios de RMA.

## Prueba en OpenText Media Management

Para finalizar, vamos a importar una imagen para ver la información de análisis que se muestra en etiquetas y otros metadatos.

![Analisis de la imagen en RMA](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/RMA-imagen.png)                 
