---
title: "Como entrenar Nuance Transcription Engine (NTE) para OpenText Explore"
header:
  image: /images/00-nuance-transcription-engine-nte.png
  og_image: /images/00-nuance-transcription-engine-nte.png
tags:
  - OpenText
  - Explore
last_modified_at: 2021-07-27T09:33:57-04:00
---

OpenText(TM) Explore es una solución de "descubrimiento" que permite a los profesionales de negocios y 
Centros de Atención al Cliente ver las interacciones a través de diversos canales colectivamente para 
obtener una imagen completa de los comportamientos y las relaciones de los clientes.

Explore analiza las grabaciones de llamadas y las sesiones de chat, combinándolas con datos de comportamiento 
de las redes sociales, blogs de todo el mundo, foros y cobertura de noticias. Explore, busca de forma dinámica 
los resultados en busca de significado subyacente, ofrece información procesable sobre el comportamiento 
de los clientes casi en tiempo real, lo que permite resolver problemas de manera más efectiva o mejorar las 
ventas en todos los canales.


## Como entrenar el sistema para mejorar la calidad de las transcripciones

> NOTA: Las rutas referidas en este artículo hacen referencia a mi entorno. 
> Pueden variar según el directorio elegido para la instalación


### Parar el servicio OpenText STeMS service  
En primer lugar, tras acceder a la aplicación **Microsoft Management Console** debemos detener el servicio **OpenText STeMS service**.
Basta con hacer clic con el botón derecho, sobre el nombre del servicio, y pulsar *stop*.

![stopSTeMS service](/images/01-stop-STeMS-service.png)


### Actuliazar el fichero development.yaml  

A continuación, vamos a definir el idioma que queremos usar con el motor de transcripción. 
Por defecto, el sitema esta configurado en ingles.  Vamos a configurarlo en español. Para ello accedermos 
al fichero **develoment.yaml**

> El fichero **develoment.yaml** esta ubicado en 
> **C:\ProgramData\Nuance\Transcription Engine\config\develoment.yaml**


```yaml    
languagePack:
     name: 'nte-spa-ESP-4.0.01-8kHz'
     allowSharedMemory: false
```

![stopSTeMS service](/images/02-change-development-yaml.png)

El valor del atributo **name** se corresponde con uno de los packs de idioma instalados en nuestro servidor, habitualmente 
localizados en el directorio **C:\ProgramData\Nuance\Transcription Engine\language_packs**

En el mismo fichero, **develoment.yaml**, también debemos cambiar el valor del atributo **startAsGenerator** a *false*.

```yaml
domainLMGeneration:
    startAsGenerator: false  
```

![stopSTeMS service](/images/03-change-development-yaml.png)


### Actuliazar el fichero production yaml (defaultLanguage: 'spa-ESP')  

También vamos a definir el idioma que queremos usar con el motor de transcripción en el fichero **production.yaml**

> El fichero **production.yaml** esta ubicado en 
> **C:\ProgramData\Nuance\Transcription Engine\config\production.yaml**

En este caso debemos utiliza el código de idioma y código de país de 3 letras:

```yaml    
defaultLanguage: 'spa-ESP'
```

![stopSTeMS service](/images/04-change-production-yaml.png)


### Actualizar el fichero appsettings.json ("Language": "spa-ESP")  

El último fichero de configuración que debemos actualizar es **appsettings.json**. Donde debemos asignar el nuevo valor **"spa-ESP"**
al atributo **"Language"**. Nuestro fichero de configuración deberá tener un aspecto similar a este:


```json
 ...
 
 "Nuance": {
    "NTE": {
      "IP": "172.31.18.241",
      "httpPort": "8000",
      "httpsPort": "7000",
      "UseShellExecute": "true",
      "CreateNoWindow": "false",
      "application": "C:\\Program Files\\Nuance\\Transcription Engine\\startEngine.bat",
      "applicationName": "nte"
    },
    "NTECallbackURL": "http://172.31.18.241/api/STeMS/NTECallback",
    "InstallType": "OnPrem",
    "QueueWatermark": "2",
    "QueueOverflowMultiplier": ".5",
    "Language": "spa-ESP",
    "HouseCleaningTimeout": "60",
    "CoreOverride": "8",
    "NoAudioText": "Transcription for all of the segments has failed",
    "LogStatistics": "",
	"AddWords": [],
    "AWSTableName":  "STeMSTracker",
	"DomainLM": {
      "Name": "",
      "Url": "",
      "Weight": "High"
    }

```

![stopSTeMS service](/images/05-change-appsettings-json.png)


### Start transcription engine  

Una vez finalizada la configuración debemos ejecutar el script **startEngine.bat** 
ubicado en **C:\Program Files\Nuance\Transcription Engine**

![stopSTeMS service](/images/06-start-transcription-engine.png)

Cuando el servidio se haya inicializado correctamente veremos una pantalla similar a esta:
 
![stopSTeMS service](/images/07-transcription-engine-started.png)


### Start transcription web client  - This is wrong - start DLM Client to create DLM 

También debemos arrancar el cliente web de NTE. Para ello ejecutaremos el script **start-server.bat** 
ubicado en **C:\Program Files\Nuance\Transcription Webclient**

![stopSTeMS service](/images/08-start-transcription-web-client.png)


### Acceso al cliente NTE

Por último accederemos al *Cliente NTE* en la dirección `http://172.31.18.241:3000/`

![stopSTeMS service](/images/09-access-nte.png)

Ahora ya estamos listos para probar las calidad de las trasncripciones y ayudar al sistema a mejorarlas, 
pero eso lo contaré en otro artículo.