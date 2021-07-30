---
title: "Como configurar Nuance Transcription Engine (NTE) para OpenText Explore"
header:
  image: /images/00-nuance-transcription-engine-nte.png
  og_image: /images/00-nuance-transcription-engine-nte.png
tags:
  - OpenText
  - Explore
  - NTE  
last_modified_at: 2021-07-27T09:33:57-04:00
---

**OpenText(TM) Explore** es una solución de "descubrimiento" que permite a los profesionales de negocio y 
Centros de Atención al Cliente ver las interacciones a través de diversos canales colectivamente para 
obtener una imagen completa de los comportamientos y las relaciones de los clientes.

Explore analiza las grabaciones de llamadas y las sesiones de chat, combinándolas con datos de comportamiento 
de las redes sociales, blogs de todo el mundo, foros y cobertura de noticias. Explore, busca de forma dinámica 
los resultados en busca de significado subyacente, ofrece información procesable sobre el comportamiento 
de los clientes casi en tiempo real, lo que permite resolver problemas de manera más efectiva o mejorar las 
ventas en todos los canales.

Aunque la caliad de las transcripciones ofrecida por defecto es alta, podemos mejorar entrenar a 
*Nuance Transcription Engine (NTE)*, el motot de transcripción utilizado por *Explore*, para mejorar la 
calidad y preción de las transcripciones. Vamos a ver que pasos tenemos que dar para configurar el sistema y abordar esta tarea.

## Como configurar el sistema para mejorar la calidad de las transcripciones

> NOTA: Las rutas referidas en este artículo hacen referencia a mi entorno. 
> Pueden variar según el directorio elegido para la instalación de Explore.


### Parar el servicio OpenText STeMS service  

> NTE es administrado por el componente de **Opentext STeMS**, que debe instalarse en los mismos 
> servidores que ejecutan NTE. STeMS lanza y administra una instancia NTE por núcleo en el servidor.

En primer lugar, tras acceder a la aplicación **Microsoft Management Console** debemos detener el servicio **OpenText STeMS service**.
Basta con hacer clic con el botón derecho, sobre el nombre del servicio, y pulsar *Stop*.

![stopSTeMS service](/images/01-stop-STeMS-service.png)


### Actuliazar el fichero development.yaml  

A continuación, vamos a definir el idioma que queremos usar con el motor de transcripción. 
Por defecto, el sitema esta configurado en ingles.  Vamos a configurarlo en español. 
Para ello accedermos al fichero **develoment.yaml**

> El fichero **develoment.yaml** esta ubicado en 
> **C:\ProgramData\Nuance\Transcription Engine\config\develoment.yaml**


```yaml    
languagePack:
     name: 'nte-spa-ESP-4.0.01-8kHz'
     allowSharedMemory: false
```

![Actualizar develoment.yaml - pack de idioma](/images/02-change-development-yaml.png)

El valor del atributo **name** se corresponde con uno de los packs de idioma instalados en nuestro servidor, 
habitualmente localizados en el directorio **C:\ProgramData\Nuance\Transcription Engine\language_packs**

En el mismo fichero, **develoment.yaml**, también debemos cambiar el valor del atributo 
**startAsGenerator** a *false*.

```yaml
domainLMGeneration:
    startAsGenerator: false  
```

![Actualizar develoment.yaml - startAsGenerator](/images/03-change-development-yaml.png)


### Actuliazar el fichero production yaml (defaultLanguage: 'spa-ESP')  

También vamos a definir el idioma que queremos usar con el motor de transcripción en el fichero 
**production.yaml**

> El fichero **production.yaml** esta ubicado en 
> **C:\ProgramData\Nuance\Transcription Engine\config\production.yaml**

En este caso debemos utiliza el código de idioma y código de país de 3 letras:

```yaml    
defaultLanguage: 'spa-ESP'
```

![Actualizar idioma en production.yaml](/images/04-change-production-yaml.png)


### Actualizar el fichero appsettings.json ("Language": "spa-ESP")  

El último fichero de configuración que debemos actualizar es **appsettings.json**. Donde 
debemos asignar el nuevo valor **"spa-ESP"** al atributo **"Language"**. Nuestro fichero 
de configuración deberá tener un aspecto similar a este:


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

![Actualizar appsettings.json](/images/05-change-appsettings-json.png)


### Start transcription engine  

Una vez finalizada la configuración debemos ejecutar el script **startEngine.bat** 
ubicado en **C:\Program Files\Nuance\Transcription Engine**

![startEngine.bat](/images/06-start-transcription-engine.png)

Cuando el servicio se haya inicializado correctamente veremos una pantalla similar a esta:
 
![Transcription Engine inicializado](/images/07-transcription-engine-started.png)


### Start transcription web client

También debemos arrancar el **Cliente Web de NTE**. Para ello ejecutaremos el script **start-server.bat** 
ubicado en **C:\Program Files\Nuance\Transcription Webclient**

![Inicializar Cliente Web de NTE](/images/08-start-transcription-web-client.png)


### Acceso al cliente NTE

Por último accederemos al *Cliente NTE* en la dirección `http://172.31.18.241:3000/` (la IP y el puerto pueden variar).

![Acceso web al cliente NTE](/images/09-access-nte.png)

Ahora ya estamos listos para probar las calidad de las trasncripciones y ayudar al sistema a mejorarlas.

> Lectura recomendada: [Mejorando la calidad de la transcripción de las llamadas con Add Words en NTE y Explore](/mejorando-la-calidad-de-la-transcripcion-de-las-llamadas-con-add-words-en-nte-y-explore)


### Deteniendo los servidores

Una vez finalizado el trabajo mejora de la calidad de la transcrición es aconsejable detener los servidores:

   * **Detener el Cliente Domain Language Model  (DomainLM Cliente)**: Basta con acceder a 
   **C:\Program Files\Nuance\Transcription Webclient** y ejecutar el script **stop-server.bat**
   ![Detener el Domain Language Model Client](/images/16-stop-transcription-DomainLM-Client-ctrl-c.png)

   * **Detener el servidor NTE (Transcription Engine)**: Debemos pulsar `Ctrl + C` en la consola que ejecuta el 
   servidor NTE
   ![Detener el servidor NTE](/images/17-stop-server-transcription-engine.png)

