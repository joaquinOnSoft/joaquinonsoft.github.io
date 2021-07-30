---
title: "Mejorando la calidad de la transcripción de las llamadas con Add Words en NTE y Explore"
header:
  image: /images/24-nte-transcribe-2nd-time.png
  og_image: /images/24-nte-transcribe-2nd-time.png
tags:
  - OpenText
  - Explore
  - NTE
last_modified_at: 2021-07-30T15:39:28-04:00
---

Veamos como mejorar la calidad y precisión de las transcripción realizadas en OpenText Explore 
para su posterior análisis, lo que nos permitirá realizar clasificaciones, identificación de tendencias...

## Acceso al cliente NTE

> Lectura recomendada: [Como configurar Nuance Transcription Engine (NTE) para OpenText Explore](/como-configurar-nuance-transcription-engine-para-opentext-explore/)

En primer lugar debemos acceder al *Cliente NTE* en la dirección `http://172.31.18.241:3000/` (la IP y el puerto pueden variar).

![Acceso web al cliente NTE](/images/09-access-nte.png)

Ahora ya estamos listos para probar las calidad de las trasncripciones y ayudar al sistema a mejorarlas.


## Transcibiendo nuestra primera llamada

Vamos a cargar un fichero wav con la grabación de la conversació telefónica. Para ello arrastraremos el fichero 
.wav sobre el cuadro `Drag or choose WAV/JSON files here.`, o bien haremos clic sobre **choose** y seleccionaremos 
un fichero de audio:

![Selección de fichero de audio para su transcripción](/images/21-load-audio-file-to-transcribe.png)


Ya tenemos cargado el fichero de audio que queremos transcribir:

![Fichero de audio cargado para su transcripción](/images/22-transcribe-audio-file.png)

A continuación, pulsaremos en el botón `Transcribe` y esperaremos unos instante. Finalmente obtenemos la 
trasncripción de la llamada.

![Transcribir una llamada con NTE](/images/25-transcribe-call-with-nte.png)

La transcripción puede contener errores, debido a diversos motivos como ruido de fondo, 
baja calidad de la grabación, solapamiento de los hablantes (en llamadas grabadas en mono)...

Vamos a facilitar las frase correctas para los errores detectados. Para ello deplegamos el bloque `Add Words`
y añadimos las palabras o frases que queremos enseñar al sistema. En nuestro ejemplo añadiremos dos frease:
   * hola me llamo Ana
   * encantada de conocerte

![NTE - Add words](/images/23-nte-add-words.png)

Una vez añadidas todas las frases que queremos enseñar al sistema tenemos que volver a transcribir la llamada.

Observaremos que cuando el sistema utiliza alguna de las frases que ha aprendido, las frases se muestran
con guiones bajos, '_', en lugar de espacios.

![Transcripción de la llamadas por 2ª vez](/images/24-nte-transcribe-2nd-time.png)

Podemos repetir este proceso (Add Words - transcribir llamada) tantas veces como consideres oportuno.

Tras una o varias iteracciones, podemos volver a repetir el proceso, pero esta vez con otra llamada diferente. 
Así vamos identificando expresiones habituales en las conversaciones o frases que se transcriben incorrectamente.
Esto nos va a permitir componer una colección de frases que más tarde podremos usar para la elaboración de
nuestro Domain Language Model (DLM).

> Lectura recomendada: [Genera un Domain Language Model (DLM) para NTE y Explore](/genera-un-domain-language-model-dlm-nte-y-explore)


### Deteniendo los servidores

Una vez finalizado el trabajo mejora de la calidad de la transcrición es aconsejable detener los servidores:

   * **Detener el Cliente Domain Language Model  (DomainLM Cliente)**: Basta con acceder a 
   **C:\Program Files\Nuance\Transcription Webclient** y ejecutar el script **stop-server.bat**
   ![Detener el Domain Language Model Client](/images/16-stop-transcription-DomainLM-Client-ctrl-c.png)

   * **Detener el servidor NTE (Transcription Engine)**: Debemos pulsar `Ctrl + C` en la consola que ejecuta el 
   servidor NTE
   ![Detener el servidor NTE](/images/17-stop-server-transcription-engine.png)



