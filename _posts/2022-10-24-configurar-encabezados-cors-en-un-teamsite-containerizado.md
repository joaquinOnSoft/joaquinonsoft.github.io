---
title: "Configurar encabezados CORS en un TeamSite containerizado"
header:
  image: /images/2022-10-24-configurar-encabezados-cors-en-un-teamsite-containerizado/no-access-control-allow-horigin-header-is-present.png
  og_image: /images/2022-10-24-configurar-encabezados-cors-en-un-teamsite-containerizado/no-access-control-allow-horigin-header-is-present.png
tags:
  - OpenText
  - Qfiniti
  - Explore
last_modified_at: 2022-10-24T19:27:15-04:00
---

En este artículo explicaré cómo configurar **LSCS (LiveSite Content Services)** para permitir 
solicitudes de servidores, que no sean él mismo, según la política de seguridad de `same-origin`. 
Las solicitudes entre sitios están prohibidas por defecto, ya que pueden generar 
muchos problemas de `cross-site scripting`.

La cabecera [Access-Control-Allow-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) 
indica si la respuesta se puede compartir con el código de solicitud del `origen` dado.

Cuando CORS está configurado correctamente en el servidor, el encabezado de respuesta del servidor 
incluirá `origenes` aceptados.  Por ejemplo, añadimos la cabecera
`Access-Control-Allow-Origin: https://teamsite-lsds.joaquinonsoft.com` para informar al 
navegador (cliente) que es correcto mostrar resultados de este sitio y no generar errores. 

Imaginemos que tenemos *LSDS* ejecutándose en https://teamsite-lsds.joaquinonsoft.com y *LSCS* 
en https://teamsite-lscs.joaquinonsoft.com. A partir de las URL, podemos ver que ambos se ejecutan 
en el mismo dominio pero con diferentes subdominios. Si no configuramos CORS para permitir el 
origen cruzado, tendremos errores como el que se muestra a continuación cuando *LSDS* 
envía una solicitud a *LSCS*.

![no access control allow horigin header is present](/images/2022-10-24-configurar-encabezados-cors-en-un-teamsite-containerizado/no-access-control-allow-horigin-header-is-present.png)

En un TeamSite containerizado debemos seguir los siguietes pasos para habilitar *CORS*:
 - Editar el fichero `/home/otadmin/helm_charts/teamsite/bundle/configfiles/lscsrt/Interwoven/LiveSiteCSRT/runtime/webapps/lscs/WEB-INF/web.xml`
 - Agregar el siguiente código a nuestro fichero `web.xml`

```xml
<!-- ================== CORS Filter Definitions ===================== -->
<filter>
	<filter-name>CorsFilter</filter-name>
	<filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
	<init-param>
		<param-name>cors.allowed.origins</param-name>
		<param-value>https://teamsite-lsds.joaquinonsoft.com</param-value>
	</init-param>
	<init-param>
		<param-name>cors.allowed.methods</param-name>
		<param-value>GET,POST,HEAD,OPTIONS,PUT</param-value>
	</init-param>
	<init-param>
		<param-name>cors.exposed.headers</param-name>
		<param-value>Access-Control-Allow-Origin,Access-Control-Allow-Methods</param-value>
	</init-param>
</filter>
<!-- ==================== CORS Filter Mappings ====================== -->
<filter-mapping>
	<filter-name>CorsFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
 
 - Guardar el fichero `web.xml`
 - Abrir una terminal y ejecutar los siguientes comandos
```shell
 cd /home/otadmin/Desktop/customization
 ./customizehostname.sh <vmname> 
```


Podemos validar que el fichero de configuración se ha propagado al `pod` adecuado de Kubernets 
ejecutando este comando en una terminal:

```
kubectl exec -n runtime lscs-0 – cat /Interwoven/LiveSiteCSRT/runtime/webapps/lscs/WEB-INF/web.xml
```