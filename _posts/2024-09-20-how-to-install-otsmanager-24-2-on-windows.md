---
title: "How to install OTSManager 24.2 on Windows?"
header:
  image: /images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/70-ots-manager.png
  og_image: /images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/70-ots-manager.png
tags:
  - OpenText
  - Output Transformation Server
  - OTS
last_modified_at: 2024-09-20T23:54:18-04:00
---

This article will show you **how to install OTSManager 24.2 on Windows** step by step.

> **Output Transformation Server Manager** is a thin client consisting of a number of JSP
> pages that enable users to connect directly to Output Transformation Server
> installed onto any of the supported Java Enterprise Edition (Java EE) application
> servers.
> 

The Output Transformation Server Manager enables a user to do the following:

 - Monitor the current server status.
 - Monitor jobs that are currently running.
 - Stop individual or all current running jobs.
 - Modify, run, view, export, and delete projects.
 - View license files.
 - View individual log files.


## Prerequisites

You must complete the following prerequisites before installing OTSManager:

 - Install **Output Transformation Server 24.2** 

> Recommended reading: [How to install Output Transformation Server 24.2 on Windows?](./2024-09-19-how-to-install-output-transformation-server-24-2-on-windows.md)

 - Install [Tomcat 9](https://tomcat.apache.org/download-90.cgi)
 - Verify that `localhost` is defined in `c:\Windows\System32\drivers\etc\hosts`
 
 ![etc\hosts](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/69-hosts.png)

# OTS Manager installation (graphic UI) 

 - Launch **Designer** app
 - Click on `Welcome` tab
 - Click on `Package and deploy a new instance`

 ![Output Transformation Designer Welcome screen](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/59-output-transformation-designer-welcome.png)

 - Click on `Next` on *Package and deploy wizard* pop-up

![Package and deploy wizard](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/60-package-and-deploy-wizard.png)

 - Set Tomcat root directory in `Server Home` field, e.g., **C:\Program Files\apache-tomcat-9.0.85**
 - Keep the other default values

![Select target application server](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/61-select-target-application-server.png)

 - Click on `Next`

![Package and deploy wizard](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/62-package-and-deploy-wizard.png)
 
 - Click on `Next`

![Package and deploy wizard](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/63-package-and-deploy-wizard.png)
 
 - Click on `Next`

![Package and deploy wizard](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/64-package-and-deploy-wizard.png)
 
 - Click on `Next`

![Package and deploy wizard](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/65-package-and-deploy-wizard.png)
 
 - Click on `Next`

![Package and deploy wizard](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/66-package-and-deploy-wizard.png)
 
 - Click on `Finish`

# Start OTSManager

 - On File Explore browse to `<OTS-HOME>\TomcatBase` i.e. **C:\OpenText\OTS 24.2\TomcatBase**
 - Double click on `startots.bat`

![startots.bat](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/67-startots.png)

 - The script will initialize the OTS engine

> **NOTE**: Don’t close this console

![Initializing OTS engine](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/68-initializing-ots-engine.png)

## Access to OTS Manager

   - Open a web browser and access to [http://localhost:8080/OTSManager/](http://localhost:8080/OTSManager/)
      - **User**: Administrator
      - **Password**: `<no password by default>`
	  
> NOTE: Set a password for a none local environment

   - Click on `Log In`


![OTS Manager](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/70-ots-manager.png)

   - Click on `FILE SYSTEM > RESOURCES`

![OTS Manager > FILE SYSTEM > RESOURCES](/images/2020-04-20-error-en-la-publicacion-de-teamsite-error-while-executing-task-production-deployment/71-ots-manager-resources.png)

> **NOTE:** We can setup a project in Designer and mount it ot OTS web. 
>
> It will be visible  on `RESOURCES` menu.
