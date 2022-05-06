---
title: "Configurar el API de Google Computer Vision en OTMM"
header:
  image: /images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/componente-teamsite-editable-on-the-glass.png
  og_image: /images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/componente-teamsite-editable-on-the-glass.png
tags:
  - OpenText
  - OTMM
last_modified_at: 2022-05-06T19:01:56-04:00
---

## Crear un proyecto en Google Cloud Vision

En primer lugar hemos de acceder a [Google Cloud](https://cloud.google.com/) e iniciar sesión con nuestra cuenta de Google. 
A continuación hemos de seguir los siguientes pasos:

   - En la página de inicio, hacer clic en el botón **Go to Console**.

   - En la página de la consola, hacer clic en **Select Project**

![Google Cloud Platform dashboard](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-dashboard.png)

   - En la ventana *Projects*, elija **New Project**
   
![Google Cloud Platform - New Project](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-new-project.png)
   
   - Asignar un nombre al proyecto, por ejemplo, `GoogleVision` y haga clic en **Create**.
   
![Google Cloud Platform - Project name](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-project-name.png)   

## Habilitar el API de Google Cloud Vision

A continuación, debemos habilitar el API de Google Cloud Vision. Para ello tendremos que realizar las siguientes acciones:

   - Desde el panel, hacer clic en **Go to APIs overview** en la sección `APIs`:

![Google Cloud Platform - APIs](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-apis.png)   

   - En la página *APIs and Services*, hacer clic en **Library** y escribir `vision` en el cuadro de búsqueda.
   - Seleccionar **Cloud Vision API**
   - En la página de *Cloud Vision API*, hacer clic en **Enable**

![Enagle Cloud Vision API](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-enable-cloud-vision-api.png)   

## Crear credenciales

Una vez que el API de *Google Cloud Vision* está habilitada, crearemos las credenciales que usaremos mas tarde. Tan solo tendremos que seguir estos pasos:

   - En la página *Cloud Vision API*, seleccione **Credentials**

![Enagle Cloud Vision - Credentials](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-credentials.png)   

   - En la página *Credentials*, hacer clic en **+CREATE CREDENTIALS**
   
![Enagle Cloud Vision +CREATE CREDENTIALS](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-plus-create-credentials.png)      
   
   - Seleccionar **Service Account**
 
![Enagle Cloud Vision - Service Account](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-service-account.png)       
 
   - Proporcionar el nombre de la cuenta de servicio y una descripción opcional
   - Hacer clic en **Create and Continue**

![Enagle Cloud Vision - Create Service Account](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-create-service-account.png)          

   - Seleccionar un Rol, por ejemplo, **Owner**, hacer clic en **Continue**
   - Hacer clic en **Done**
   
## Agregar una clave a la cuenta de servicio

Una vez creada la cuenta de servicio, tenemos que 

   - Hacer clic en el nombre de la cuenta en la página Credenciales

![Enagle Cloud Vision - Service accounts](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-service-accounts.png)          

   - En la página de detalles de la cuenta de servicio, seleccione **Keys** y haga clic en **Add Keys > Create New**
   - Seleccionar `JSON` en la ventana emergente y hacer clic en **Create**

![Enagle Cloud Vision - Create private key for Vision API](/images/2022-05-06-configurar-el-api-de-google-computer-vision-en-otmm/google-cloud-platform-create-private-key-for-vision-api.png)          
