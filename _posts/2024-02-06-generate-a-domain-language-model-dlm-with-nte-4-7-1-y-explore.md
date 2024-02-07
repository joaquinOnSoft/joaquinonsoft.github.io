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
predict the words a person said in a conversation, based on the surrounding words.

We can improve, or in other words: "train" the language model, using text from the business domain of interest. 
From the text, the language pack can learn new words specific to our domain and which combinations of words to use. 
NTE compiles these new words and/or word combinations into a *"Domain Language Model" (DLM)*. This DLM complements the 
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

```yaml
domainLMGeneration:
    startAsGenerator: true  
```

### Start transcription engine  

Once the configuration is finished we must execute the **startEngine.bat** script 
located in **C:\Program Files\Nuance\Transcription Engine**.

![startEngine.bat](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/06-start-transcription-engine.png)

When the service has successfully initialized we will see a screen similar to this:
 
![Transcription Engine initialized](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/07-transcription-engine-started.png)


### Start transcription web client

We must also start the **Transcription DomainLM Client**. To do so, run the script **startServer.bat** 
located at **C:\Program Files\Nuance\Transcription DomainLM Client**.

![Initialize NTE Web Client](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/starServer-bat-transcription-domain-lm-client.png)


### Access to the Transcription DomainLM Client

Finally we will access the *Transcription DomainLM Client* at the address `http://172.31.18.241:3030/` (IP and port can vary).

![Transcription DomainLM Client web access](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/transcription-domainlm-client.png)

Now we are ready to generate a *Domain Language Model*.

### Provide training data

Training data usually consists of text files containing fragments of conversations, or full transcripts of conversations. 
conversations, or complete transcripts of calls that we have made manually.
A training file might look something like this (but with many more sentences):

```
it was stalled and would not go forward or backward
and that tells you
ok let me do a test
```

We must give a name to our DLM in the **Domain LM name** field. Next we are going to 
drag and drop the text file on the area 
`Drag or choose text files, zipped text files, or domain LM files with tokenization data`.

![Domain Language Model Generator](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/domain-language-model-generator.png)

Finally, click on the `Generate Domain LM` button and after a few moments the DLM will be generated.

![Domain Language Model Generator](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/domain-language-model-generated.png)

In our example the DLM is called "DomainLM-TEST_5532d8a0-f152-11eb-87ec-e377ede745dc".

> The DLM we have just generated is automatically copied to the 
> *domainLM* directory of the Nuance Transcription Engine installation, 
> in my environment, **C:\ProgramData\Nuance Transcription Engine\domainLM**.

### Stopping the servers

Once the work on improving the quality of the transcription has been completed, it is advisable to stop the servers:

   - Stop **Transcription DomainLM Client**: We must press `Ctrl + C` in the console that runs the 
   NTE server

   - Stop **NTE (Transcription Engine) server**: We must press `Ctrl + C` in the console that runs the 
   NTE server
   ![Stop NTE server](/images/2024-02-06-generate-a-domain-language-model-dlm-with-nte-4-7-1-y-explore/17-stop-server-transcription-engine.png)


### Update the development.yaml file.  

Access the **develoment.yaml** file to change the value of the attribute 
**startAsGenerator** to *false*.


````yaml
domainLMGeneration:
    startAsGenerator: false  
```

### Update appsettings.json file ("Language": "spa-ESP")  

The last configuration file to update is **appsettings.json**. Where 
we must assign the new value **"DomainLM-TEST_5532d8a0-f152-11eb-87ec-e377ede745dc "** to the attribute **"Nuance >". 
to the attribute **"Nuance > DomainLM > Name "**. Our configuration file should look something like this:


````json
 ...
 
 "Nuance": {
    "NTE": {
      "IP": "172.31.18.241",
      "httpPort": "8000",
      "httpsPort": "7000",
      "useShellExecute": "true",
      "createNoWindow": "false",
      "application": "C:\Program Files FilesTranscription Engine StartEngine.bat",
      "applicationName": "nte"
    },
    "NTECallbackURL": "http://172.31.18.241/api/STeMS/NTECallback",
    "installType": "onPrem",
    "queueWatermark": "2",
    "queueOverflowMultiplier": ".5",
    "language": "spa-ESP",
    "houseCleaningTimeout": "60",
    "coreOverride": "8",
    "NoAudioText": "Transcription for all of the segments has failed",
    "LogStatistics": "",
	"AddWords": [],
    "AWSTableName": "STeMSTracker",
	"DomainLM": {
      "Name": "DomainLM-TEST_5532d8a0-f152-11eb-87ec-e377ede745dc",
      "Url": "",
      "Weight": "High"
    }

```
