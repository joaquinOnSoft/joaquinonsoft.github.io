---
title: "Integración de OpenText Media Management y ShutterStock"
header:
  image: /images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/opentext-media-management-stock-libraries-shutterstock.png
  og_image: /images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/opentext-media-management-stock-libraries-shutterstock.png
tags:
  - Shutterstock
  - OpenText Media Management
  - OTMM
last_modified_at: 2023-01-17T20:32:22-04:00
---

En este artículo se explica la Integración de OpenText Media Management y ShutterStock, el proveedor líder de imágenes de stock.

A partir de la **versión 21.3 de OpenText Media Management** se amplía las búsquedas a **Shutterstock**. Esta integración permite 
utilizar nuestra cuenta de Shutterstock para obtener licencias o solicitar imágenes y vídeos.

Los usuarios buscan primero en DAM los activos adecuados, evitando compras innecesarias.

## Configuración de Shutterstock

Debemos seguir los siguientes pasos para configurar la integración entre **OpenText Media Management** y **ShutterStock**:

### Crear una nueva aplicación API

Debemos acceder al portal de Desarrollador de ShutterStock:

   -	Abrir un navegador web a la [página de desarrollador de Shutterstock para configurar aplicaciones API](https://www.shutterstock.com/account/developers/apps) 
   -	Hacer clic en el botón `Create new app`.
   
   ![ShutterStock - Create new app](/images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/shutterstock-create-new-app.png)
   
   -	Introduzca los siguientes valores sustituyendo su VM
   
      a.	**AppName**: Nombre de nuestra aplicación, i.e. OTMM Joaquín On Software
      b.	**Callback URL**: URL de nuestro servidor de OTMM, i.e. www.joaquinonsoft.com
      c.	**Referrer**: URL de nuestro servidor de OTMM, i.e. www.joaquinonsoft.com
      d.	**Included products**: Lista de productos a incluir. Dejar todas las casillas marcadas
      e.	**Company name**: Nombre de la empresa, i.e. Joaquín On Software
      f.	**Página web**: https://www.joaquinonsoft.com
      g.	**Intended use**: Seleccionar el valor `API integration for my company` en la lista desplegable
      h.	**Description**: Descripción de la aplicación, i.e. "Se utiliza para conectar Shutterstock con el servidor de OTMM de Joaquín On Soft"
      i.	**Terms of service**: Marque la casilla para aceptar las condiciones
	  
	  
	  ![ShutterStock - Save new app](/images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/shutterstock-save-new-app.png)
	  
   - Hacer clic en **Save**
   - Buscar nuestra aplicación en la lista de aplicaciones
   
   ![ShutterStock - Developers apps](/images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/shutterstock-developers-app.png)
   
   - Para Clave de usuario y Secreto de usuario, haga clic en el globo ocular de la derecha con una línea que lo atraviesa para mostrar los valores. Copie estos valores para utilizarlos más tarde.

### Actualización de las credenciales en OTMM

A continuación debemos proporcionar las credenciales de la aplicación de Shutterstock en el panel de adminsitración de OTMM. 
Para ello debemos seguir los siguientes pasos:

   - Iniciar sesión en `Teams` en nuestro servidor de OTMM: http://<HOST_NAME>/teams
   - Hacer clic en `Security > Credentials`
   
   ![OpenText Media Management - Security > Credentials](/images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/opentext-media-management-security-credentials.png)
   
   - Hacer clic en Editar en la línea en la que el Nombre de credencial es `Shutterstock`
   - Introducir los valores de nuestra `App Shutterstock` como se indica a continuación
      - **Client ID**: Valor del campo `Consumer Key` en nuestra App en Shutterstock
      - **Client Secret**: Valor del campo `Consumer Secret` en su App en Shutterstock
   - Hacer clic en `Save`


![OpenText Media Management - Stock libraries - ShutterStock](/images/2023-01-17-integracion-de-opentext-media-management-y-shutterstock/opentext-media-management-stock-libraries-shutterstock.png)
