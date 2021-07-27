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

### Stop STeMS service  
En primer lugar, tras acceder a la aplicación **Microsoft Management Console** debemos detener el servicio **OpenText STeMS service**.
Basta con hacer clic con el botón derecho, sobre el nombre del servicio, y pulsar *stop*.

![stopSTeMS service](/images/01-stop-STeMS-service.png)

### Change development yaml (package language: spa-ESP)  

A continuación vamos a definir el idioma que queremos usar con el motor de transcripción. Para ello accedermos al fichero **develoment.yaml**

> En mi instalación el fichero **develoment.yaml** esta ubicado en 
> **C:\ProgramData\Nuance\Transcription Engine\config\develoment.yaml**


```yaml    
languagePack:
     name: 'nte-spa-ESP-4.0.01-8kHz'
     allowSharedMemory: false

```

![stopSTeMS service](/images/02-change-development-yaml.png)


### Change development yaml (startAsGenerator: false)  
![stopSTeMS service](/images/03-change-development-yaml.png)

### Change production yaml (defaultLanguage: 'spa-ESP')  
![stopSTeMS service](/images/04-change-production-yaml.png)


### Change appsettings json ("Language": "spa-ESP")  
![stopSTeMS service](05-change-appsettings-json.png)

### Start transcription engine  
![stopSTeMS service](06-start-transcription-engine.png)

### transcription engine started  
![stopSTeMS service](07-transcription-engine-started.png)

### Start transcription web client  - This is wrong - start DLM Client to create DLM 
![stopSTeMS service](/images/08-start-transcription-web-client.png)

### Access NTE 3030 web page
![stopSTeMS service](/images/09-access-nte.png)
