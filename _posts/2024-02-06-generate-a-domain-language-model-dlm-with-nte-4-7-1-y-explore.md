---
title: "Generating a Domain Language Model (DLM) for NTE 4.7.1 and Explore 22.4"
header:
  image: /images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/24-nte-transcribe-2nd-time.png
  og_image: /images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/24-nte-transcribe-2nd-time.png
tags:
  - OpenText
  - Explore
  - NTE
last_modified_at: 2024-02-06T15:39:28-04:00
---

Each Nuance Transcription Engine (NTE) language pack contains a "language model", 
which has been trained on the text of thousands of conversations. The language model observes how certain 
combinations of words appear more frequently than others in the text. From this, a language model can 
language model can predict the words a person said in a conversation, based on the surrounding words.

We can improve, or in other words: "train" the language model, using text from the business domain of interest. 
business domain of interest. From the text, the language pack can learn new words specific to our domain and which combinations of words to use. 
specific to our domain and which word combinations are most likely to be used. NTE compiles these 
new words and/or word combinations into a *"Domain Language Model" (DLM)*. This DLM complements the 
generic language model found in the installed language pack.


## Environment configuration

> NOTE: The paths referred in this article refer to my environment. 
> They may vary depending on the directory chosen for the Explore installation.


### Stopping the OpenText STeMS service  

> NTE is managed by the **Opentext STeMS** component, which must be installed on the same 
> servers running NTE. STeMS launches and manages one NTE instance per core on the server.

First of all, after accessing the **Microsoft Management Console** application, we must stop 
the **OpenText STeMS service**. Just right-click on the name of the service and click *Stop*.

![stopSTeMS service](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/01-stop-STeMS-service.png)


### Update the development.yaml file.  

> The **develoment.yaml** file is located in 
> **C:\ProgramDataNuanceTranscription Engine\configuredeveloment.yaml**


We will access the **develoment.yaml** file to change the value of the attribute 
**startAsGenerator** to *true*.

````yaml
domainLMGeneration:
    startAsGenerator: true  
```