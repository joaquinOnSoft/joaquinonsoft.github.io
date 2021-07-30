---
title: "Generar un Domain Language Model (DLM) para NTE y Explore"
header:
  image: /images/24-nte-transcribe-2nd-time.png
  og_image: /images/24-nte-transcribe-2nd-time.png
tags:
  - OpenText
  - Explore
  - NTE
last_modified_at: 2021-07-30T15:39:28-04:00
---


> NOTA: Las rutas referidas en este artículo hacen referencia a mi entorno. 
> Pueden variar según el directorio elegido para la instalación de Explore.


### Parar el servicio OpenText STeMS service  

> NTE es administrado por el componente de **Opentext STeMS**, que debe instalarse en los mismos 
> servidores que ejecutan NTE. STeMS lanza y administra una instancia NTE por núcleo en el servidor.

En primer lugar, tras acceder a la aplicación **Microsoft Management Console** debemos detener 
el servicio **OpenText STeMS service**. Basta con hacer clic con el botón derecho, sobre el 
nombre del servicio, y pulsar *Stop*.

![stopSTeMS service](/images/01-stop-STeMS-service.png)


### Actuliazar el fichero development.yaml  

> El fichero **develoment.yaml** esta ubicado en 
> **C:\ProgramData\Nuance\Transcription Engine\config\develoment.yaml**


Accederemos al fichero **develoment.yaml** para cambiar el valor del atributo 
**startAsGenerator** a *true*.

```yaml
domainLMGeneration:
    startAsGenerator: true  
```


### Start transcription engine  

Una vez finalizada la configuración debemos ejecutar el script **startEngine.bat** 
ubicado en **C:\Program Files\Nuance\Transcription Engine**

![startEngine.bat](/images/06-start-transcription-engine.png)

Cuando el servicio se haya inicializado correctamente veremos una pantalla similar a esta:
 
![Transcription Engine inicializado](/images/07-transcription-engine-started.png)


### Start transcription web client

También debemos arrancar el **Transcription DomainLM Client**. Para ello ejecutaremos el script **start-server.bat** 
ubicado en **C:\Program Files\Nuance\Transcription DomainLM Client**

![Inicializar Cliente Web de NTE](/images/starServer-bat-transcription-domain-lm-client.png)



### Acceso al Transcription DomainLM Client

Por último accederemos al *Transcription DomainLM Client* en la dirección `http://172.31.18.241:3030/` (la IP y el puerto pueden variar).

![Acceso web Transcription DomainLM Client](/images/transcription-domainlm-client.png)

Ahora ya estamos listos para generar un *Domain Language Model*.

### Facilitar datos de entrenamiento

Los datos de entrenamiento suelen consistir en ficheros de texto que contienen fragmentos de 
conversaciones, o bien transcripciones completas de llamadas que hemos realizado manualmente.
Un fichero de entrenamiento podría ser similar a este (pero con muchas más frases):

```
se quedaba parado y no daba ni para adelante ni para atras
y que te indica
vale dejame hacer una prueba
```

Debemos dar una nombre a nuesto DLM en el campo **Domain LM name**. A continuamión vamoa a 
arrastrar y soltar el fichero de texto sobre el area 
`Drag or choose text files, zipped text files, or domain LM files with tokenization data`

![Domain Language Model Generator](/images/domain-language-model-generator.png)

Por último, haremos clic en el botón `Generate Domain LM` y tras unos instante el DLM se habrá generado.

![Domain Language Model Generator](/images/domain-language-model-generated.png)

En nuestro ejemplo el DLM se llama 'DomainLM-TEST_5532d8a0-f152-11eb-87ec-e377ede745dc'

### Deteniendo los servidores

Una vez finalizado el trabajo mejora de la calidad de la transcrición es aconsejable detener los servidores:

   * **Detener el Cliente Domain Language Model  (DomainLM Cliente)**: Basta con acceder a 
   **C:\Program Files\Nuance\Transcription Webclient** y ejecutar el script **stop-server.bat**
   ![Detener el Domain Language Model Client](/images/16-stop-transcription-DomainLM-Client-ctrl-c.png)

   * **Detener el servidor NTE (Transcription Engine)**: Debemos pulsar `Ctrl + C` en la consola que ejecuta el 
   servidor NTE
   ![Detener el servidor NTE](/images/17-stop-server-transcription-engine.png)


