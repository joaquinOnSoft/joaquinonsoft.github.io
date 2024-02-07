---
title: "Improving call transcription quality with Add Words in NTE and Explore"
header:
  image: /images/2024-02-05-improving-quality-of-call-transcription-with-add-words-in-nte-4-7-1-and-explore/24-nte-transcribe-2nd-time.png
  og_image: /images/2024-02-05-improving-quality-of-call-transcription-with-add-words-in-nte-4-7-1-and-explore/24-nte-transcribe-2nd-time.png
tags:
  - OpenText
  - Explore
  - NTE
last_modified_at: 2024-02-05T15:39:28-04:00
---

Let's see how to improve the quality and accuracy of the transcription made in OpenText Explore 
for later analysis, which will allow us to perform classifications, identify trends...

## Access to the NTE client

> Recommended reading: [How to configure Nuance Transcription Engine (NTE) for OpenText Explore](/how-to-configure-nuance-transcription-engine-for-opentext-explore/)

First we need to access the *NTE Client* at the address `http://172.31.18.241:3000/` (IP and port can vary).

![NTE client web access](/images/2024-02-05-improving-quality-of-call-transcription-with-add-words-in-nte-4-7-1-and-explore/09-access-nte.png)

Now we are ready to test the quality of the transcripts and help the system to improve them.


## Transceiving our first call

We are going to load a wav file with the recording of the phone conversation. To do this, drag the 
.wav file onto the `Drag or choose WAV/JSON files here.` box, or click on **choose** and select 
an audio file:

![Select audio file for transcription](/images/2024-02-05-improving-quality-of-call-transcription-with-add-words-in-nte-4-7-1-and-explore/21-load-audio-file-to-transcribe.png)


We already have the audio file we want to transcribe loaded:

![Audio file loaded for transcription](/images/2024-02-05-improving-quality-of-call-transcription-with-add-words-in-nte-4-7-1-and-explore/22-transcribe-audio-file.png)

Next, click on the `Transcribe` button and wait a few moments. Finally we get the 
transcript of the call.

![Transcribe a call with NTE](/images/2024-02-05-improving-quality-of-call-transcription-with-add-words-in-nte-4-7-1-and-explore/25-transcribe-call-with-nte.png)

The transcript may contain errors, due to various reasons such as background noise, 
low quality of the recording, overlapping of speakers (in calls recorded in mono)...

We will provide the correct sentence for the detected errors. To do so, we will deploy the `Add Words` block
block and add the words or phrases we want to teach the system. In our example we will add two frease:

   * hello my name is Ana
   * nice to meet you