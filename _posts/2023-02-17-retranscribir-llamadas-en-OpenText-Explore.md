---
title: "Retranscribir llamadas en OpenText Explore"
header:
  image: /images/2023-02-17-retranscribir-llamadas-en-opentext-explore/explore_call_status.png
  og_image: /images/2023-02-17-retranscribir-llamadas-en-opentext-explore/explore_call_status.png
tags:
  - Qfiniti
  - Explore
  - OpenText Explore
last_modified_at: 2023-02-17T23:32:22-04:00
---

En ocasiones, podemos necesitar **retranscribir** llamadas en **OpenText Explore**. 
Tan solo hemos de seguir estos pasos para volver a transcribir llamadas previamente importadas en el sistema:


  1. Detener el servicio `Qfiniti Startup Server` 

  ![Qfiniti Explore services](/images/2023-02-17-retranscribir-llamadas-en-opentext-explore/qfiniti-explore-services.png)

  2. Actualizar el valor del campo `status` a `10` (*New*) para todas las llamadas de la 
  tabla `explore_calls` 
  
  ```sql
    USE [Qfiniti_platform] 
    GO
    
    UPDATE [dbo].[explore_call]
      SET [status] = 10 
    GO
  ```

> **NOTA:** El significado del status de las llamadas esta 
> descrito en la tabla `explore_call`:
>
> | Status    | Descripción                        |
> |-----------|------------------------------------|
> | 10        | New                                |
> | 20        | Marked for Transcription           |
> | 21        | Transcription in Progress          |
> | 22        | Transcription Complete             |
> | 24        | Transcription Failed0              |
> | 30        | Post Transcription in Progress     |
> | 32        | Post Transcription Complete        |
> | 34        | Post Transcription Failed          |
> | 36        | Sent to Explore Survey             |
> | 40        | Ingestion in Progress              |
> | 42        | Ingestion Complete                 |
> | 44        | Ingestion Failed                   |
> | 50        | Script Adherence in Progress       |
> | 52        | Script Adherence Complete          |
> | 54        | Script Adherence Failed            |
> | 60        | Marked for Deletion (SQL and IDOL) |
> | 61        | Marked for Deletion (SQL ONLY)     |
> | 62        | Disabled                           |
> | 63        | Deletion in Progress               |
> | 64        | Deletion from IDOL in Progress     |
> | 65        | Skipped by Transcription Rule      |

  3. Truncar la tabla `explore_transerver_language_last_call_control`

  ```sql
  USE [Qfiniti_platform] 
  GO

  TRUNCATE TABLE [dbo].[explore_transerver_language_last_call_control] 
  GO
  ```

  4. Reiniciar los servicios

Una vez reiniciados los servicios las llamadas se transcribirán y sobrescribirán en *Solr*.

## Reimportación a *Solr* sin volver a transcribir la llamada

Hemos de seguir estos pasos para volver a importar a *Solr* 

  1. Borrar todos los registros de *Solr*

    - Desde la línea de comandos:

    ```console
    cd D:\SolrCloud\solr-7.3.3\example\exampledocs

    java -Dc=interaction -Ddata=args -Dcommit=true - jar post.jar "<delete><query>*</query></delete>"
    ```

    - También puede hacerse desde la interfaz de usuario de Solr

  ![Solr delete documents](/images/2023-02-17-retranscribir-llamadas-en-opentext-explore/solr-delete-documents.png)
    
  2. Actualizar el estado a `32` (*Post Transcription Complete*) para las llamadas 
  en estado `42` (*Ingestion Complete*). No cambies el estado para las llamadas en 
  estado `99`

  ```sql
  USE [Qfiniti_platform]
  GO
  
  UPDATE [dbo].[explore_call]
    SET [status] = 32 WHERE status < 42
  GO
  ```
