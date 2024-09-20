---
title: "How to train a custom autotagging model in Opentext Document Accessibility?"
header:
  image: /images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/181-auto-tag-with.png
  og_image: /images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/181-auto-tag-with.png
tags:
  - OpenText
  - Document Accessibility
  - DA
last_modified_at: 2024-09-22T00:04:14-04:00
---

This article will show you **how to train a custom autotagging model in Opentext Document Accessibility** step by step.

> OpenText™ Document Accessibility makes it easy to manually remediate ad hoc PDF documents for 
> persons with disabilities who leverage assistive technologies. **OpentextDocument Accessibility** offers 
> an intuitive user interface and AI capabilities to enable organizations to easily comply with 
> accessibility standards and regulations, such as *Section 508*, *ADA* and *AODA*, and create 
> accessible content for all audiences.

## Submit As Sample

As a `**document remediator** we can submit the remediated document as a sample, so, and Admin can use it to train the system to learn how to auto tag a type of document.

So, just follow this steps:

 - Click on `Submit As Sample` on the top menu
 
 ![Submit As Sample](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/163-submit-as-example.png)
 
 
 - When the `Submit As Sample`pop-up is shown, click on `Yes

 ![Submit As Sample](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/164-submit-as-sample.png)

## Add a model

As Administrator, click on `Training icon` on the top-left menu.

 ![Training](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/170-training.png)


 - Click on `Add model` icon `(+)` under `Available models` sections
 - Provide a name, e.g. *"OpenText Solution Overview"*
 - Press `Enter`

 ![Add model](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/170b-add-model.png)

 - Select `Create a model based on`
 - Select `Default`
 - Click `OK`

 ![Create a model based on](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/174-new-training-model.png)

## Add sample to model

> **NOTE:** If you double-click on the document/s under `Pending review`, you can see the work done by the Document remediator

As *Administrator*, click on Training icon on the top-left corner.

 - Click on `Expand icon` on the bottom-right corner, on `Pending review` section.

 ![Create a model based on](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/171-expand.png)

 - Mark check box close to your document i.e. *opentext-slo-document-accessibility-en.pdf* 

 - Click on `Add to Model` menu entry.

 ![Add to Model](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/172-add-to-model.png)

 - Select *"OpenText Solution Overview"* on the Model drop down list

 ![Add sample to Model](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/175-add-samples-to-model.png)

 - Click OK

## Update Available model

 - Click on `Expand icon` on the bottom-right corner, on Available models section

 ![Expand available models](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/176-expand-available-modesl.png)

 - Double click on the model name, *"OpenText Solution Overview"*  in our example

 ![Available models](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/177-available-modesl.png)

 - Click on `Train Model` on the top menu

 ![Train model](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/178-train-model.png)

 - The training is scheduled

 ![Training scheduled](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/179-training-scheduled.png)

 - Once the training is completed you can use it.

## Auto tagging with a custom model 

 - Browse to DA main page
 - Drag and drop a document, i.e. *"opentext-slo-document-accessibility-en-v2.pdf"*

 ![New document](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/180-new-document.png)

 - Double click on *"opentext-slo-document-accessibility-en-v2.pdf"*
 - Click on `Auto Tag With...`

 ![Auto Tag With…](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/181-auto-tag-with.png)

 - Select *"OpenText Solution Overview"* on the `Auto Tag pop-up`
 - Click `OK`

 ![Auto tag](/images/2024-09-22-how-to-train-a-custom-autotagging-model-in-opentext-document-accessibility/182-auto-tag.png)