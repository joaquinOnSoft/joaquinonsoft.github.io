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
para su porste

> Lectura recomendada: [Como configurar Nuance Transcription Engine (NTE) para OpenText Explore](/como-configurar-nuance-transcription-engine-para-opentext-explore/)

## Acceso al cliente NTE

Por último accederemos al *Cliente NTE* en la dirección `http://172.31.18.241:3000/` (la IP y el puerto pueden variar).

![Acceso web al cliente NTE](/images/09-access-nte.png)

Ahora ya estamos listos para probar las calidad de las trasncripciones y ayudar al sistema a mejorarlas.


## Transcibiendo nuestra primera llamada

Vamos a cargar un fichero wav con la grabación de la conversació telefónica. Para ello arrastraremos el fichero 
.wav sobre el cuadro `Drag or choose WAV/JSON files here.`, o bien haremos clic sobre **choose** y seleccionaremos 
un fichero de audio:

![Selección de fichero de audio para su transcripción](21-load-audio-file-to-transcribe.png)


Ya tenemos cargado el fichero de audio que queremos transcribir:

![Fichero de audio cargado para su transcripción](22-transcribe-audio-file.png)

A conuación, pulsaremos en el botón `Transcribe` y esperaremos unos instante. Finalmente obtenemos la 
trasncripción de la llamada.

La transcripción puede contener errores, debido a diversos motivos como ruido de fondo, 
baja calidad de la grabación, solapamiento de los hablantes (en llamadas grabadas en mono)...

Vamos a facilitar las frase correctas para los errores detectados. Para ello deplegamos el bloque `Add Words`
y añadimos las palabras o frases que queremos enseñar al sistema.

![NTE - Add words](23-nte-add-words.pngg)


