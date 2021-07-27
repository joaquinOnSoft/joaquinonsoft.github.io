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


```yaml    
defaultLanguage: 'spa-ESP'
```

![stopSTeMS service](/images/04-change-production-yaml.png)


### Change appsettings json ("Language": "spa-ESP")  
![stopSTeMS service](/images/05-change-appsettings-json.png)


### Start transcription engine  
![stopSTeMS service](/images/06-start-transcription-engine.png)


### transcription engine started  
![stopSTeMS service](/images/07-transcription-engine-started.png)


### Start transcription web client  - This is wrong - start DLM Client to create DLM 
![stopSTeMS service](/images/08-start-transcription-web-client.png)


### Access NTE 3030 web page
![stopSTeMS service](/images/09-access-nte.png)
