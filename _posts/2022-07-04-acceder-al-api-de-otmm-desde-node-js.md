---
title: "Acceder al API de OTMM desde node.js"
header:
  image: /images/2022-07-04-acceder-al-api-de-otmm-desde-node-js/acceder-al-api-de-otmm-desde-node-js.png.png
  og_image: /images/2022-07-04-acceder-al-api-de-otmm-desde-node-js/acceder-al-api-de-otmm-desde-node-js.png.png
tags:
  - OpenText
  - OpenText Media Management 
  - TeamSite
last_modified_at: 2022-07-07T16:06:36-09:00
---

En este artículo veremos como acceder al **API de OTMM** desde **node.js**

> OpenText Media Management (OTMM) es una solución de gestión de activos digitales (DAM), 
> innovadora y altamente escalable que gestiona activos multimedia a través de la creación, 
> distribución y retirada. Media Management ofrece una biblioteca digital integrada y consolidada 
> para contenido de marketing, branding, comercio, difusión, patrimonio, seguridad y formación.

## API REST de OpenText Media Management

El **API REST de OpenText Media Management(OTMM)** esta documentada y actualizada en el 
[OpenText Developer Portal](https://developer.opentext.com/apis/14ba85a7-4693-48d3-8c93-9214c663edd2/06c4a79f-3f4a-4a5a-aab9-9519740b27c7).
La última versión publicada en el momento de escribir este artículo es la 
[https://developer.opentext.com/apis/14ba85a7-4693-48d3-8c93-9214c663edd2/06c4a79f-3f4a-4a5a-aab9-9519740b27c7/84d6a6c3-3d0b-4a70-aa47-3efca3c884ec]( V6 de OTMM 22.2).

### Fichero de propiedades .env

En primer lugar, vamos a crear un fichero de propiedades, llamado `.env` donde definiremos la URL, el usuario y
la contraseña de nuestra instancia de OTMM.

```shell
OTMM_API_URL=<OTMM_BASE_URL>/otmmapi
OTMM_USER=<USER>
OTMM_PASSWORD=<PASSWORD>
```

### Crear una sesión en OTMM

Para trabajar con OTMM debemos crear una sesión en OTMM. Para ello hemos de invocar el método `POST` `/v6/sessions`.

Al crear la sesión obtendremos la información necesaria para realizar cualquier otra llamada a un método del API.
Si la invocación finaliza con éxito obtendremos una respuesta *JSON* similar a esta:

```js
{
  session_resource: {
	session: {
	  domain_name: 'OTMM',
	  id: 471542185,
	  local_session: false,
	  login_name: 'tsuper',
	  message_digest: 'b8271108836bef44130e71ee91bd51d4e75e2733',
	  role_name: 'Administrator',
	  user_full_name: 'admin, otmm',
	  user_id: '1001',
	  user_role_id: 1,
	  validation_key: -2045393682
	}
  }
}
```

Este es el código fuente para crear la sesión:

```js
const axios = require('axios');
const querystring = require('node:querystring');
require('dotenv').config()

const url = process.env.OTMM_API_URL;
const user = process.env.OTMM_USER;
const pass = process.env.OTMM_PASSWORD;

/**
 * <strong>Create a Session</strong>
 * Create a security Session in OTMM. It returns a valid SecuritySession
 * object if the provided credentials are valid. This is equivalent to login to OTMM
 * <ul>
 * 	<li>Method: POST</li>
 * 	<li>API method: /v6/sessions</li>
 * 	<li>URL example: https://developer.opentext.com/otmmapi/v6/sessions</li>
 * </ul>
 * <strong>NOTE:</strong>
 * With the exception of methods related to 'sessions', any call to an OTMM REST API
 * method must include the session id in the 'X-Requested-By' header.
 * @param user - User alias
 * @param pass - User password
 * @return session information
 * <code>
 * {
 *	  session_resource: {
 *		session: {
 *		  domain_name: 'OTMM',
 *		  id: 471542185,
 *		  local_session: false,
 *		  login_name: 'tsuper',
 *		  message_digest: 'b8271108836bef44130e71ee91bd51d4e75e2733',
 *		  role_name: 'Administrator',
 *		  user_full_name: 'admin, otmm',
 *		  user_id: '1001',
 *		  user_role_id: 1,
 *		  validation_key: -2045393682
 *		}
 *	  }
 *	}
 * </code>
 */
const createSession = async (user, pass) => {
    try {
		let link = url + "/v6/sessions";
		console.log("URL: " + link);
		
		let payloadJSON  = {
            "username": user,
            "password": pass
        };
		let payload =  (new URLSearchParams(payloadJSON)).toString()

        const resp = await axios.post(link, payload, {
			headers: {
				'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
			}
		});
        
		return resp.data;
    } catch (err) {
        console.error(err);
        return null;
    }
};
```

### Obtener la lista de colecciones del usuario actual

A continuación vamos a implementar el método `GET` `/v6/collections` que nos permite
obtener la lista de colecciones del usuario actual.

Necesitaremos la información que nos devolvió el método `POST` `/v6/sessions`

```js
/**
 * Get list of collections for current user
 * @see <a href="https://masteringjs.io/tutorials/axios/headers">Setting Request Headers with Axios</a>
 */
const getListOfCollectionsForCurrentUser = async(session) => {
    try {		
		let link = url + "/v6/collections";
		console.log("URL: " + link);
		
        let result = await axios.get(link, 
			{
				headers: {
					"X-Requested-By": session.session_resource.session.id,
					"Authorization":  "Bearer otmmToken " + session.session_resource.session.message_digest,
					"otmmauthtoken":  session.session_resource.session.message_digest
				}
			});
				
        //console.info(result);
		
        return result.data;
    } catch (error) {
        console.error("Error: " + error);
		return null;
    }
};
```

### Probando nuestra implementación

Por último, vamos a juntarlo todo para hacer una llamada y ver el resultado de nuestra invocación:

```js
createSession(user, pass).then( session => {
    console.log(session);
	
	getListOfCollectionsForCurrentUser(session).then(collections => {
		console.log( JSON.stringify(collections) );
	});	
});

```

Desde un terminal podemos ejecutar nuestro pequeño programa y ver como devuelve la lista de colecciones del usuario:

```shell
node otmmapi.js
```

## Código fuente en github

El código fuente de este ejemplo esta disponible en el repositorio 
[access-to-otmm-api-from-node-js](https://github.com/joaquinOnSoft/access-to-otmm-api-from-node-js) 
de **github**.
