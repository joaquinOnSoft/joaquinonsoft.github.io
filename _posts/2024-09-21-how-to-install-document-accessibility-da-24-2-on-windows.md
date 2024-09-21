---
title: "How to install Document Accessibility (DA)24.2 on Windows"
header:
  image: /images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/150-auto-tag.png
  og_image: /images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/150-auto-tag.png
tags:
  - OpenText
  - Document Accessibility
  - DA
last_modified_at: 2024-09-21T06:54:18-04:00
---

This article will show you **how to install Document Accessibility (DA24.2 on Windows** step by step.

> OpenText™ Document Accessibility makes it easy to manually remediate ad hoc PDF documents for 
> persons with disabilities who leverage assistive technologies. Document Accessibility offers 
> an intuitive user interface and AI capabilities to enable organizations to easily comply with 
> accessibility standards and regulations, such as Section 508, ADA and AODA, and create 
> accessible content for all audiences.

## Prerequisites

You must complete the following prerequisites before installing OTSManager:

 - Install **Output Transformation Server 24.2** 

> **Recommended reading**: [How to install Output Transformation Server 24.2 on Windows?](/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows)

 - Install **OTS Manager 24.2** 

> **Recommended reading**: [How to install OTSManager 24.2 on Windows?](/how-to-install-otsmanager-24-2-on-windows)

 - Install [PostgreSQL 15](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

> The tutorial doesn’t describe the full  PostgreSQL installation.
>
> During the installation, remember to set superuser password.

## Document Accessibility (DA) installation

DA needs a database to improve the auto tagging, to store the tagging logic.

The docs are stored on disk.

Just follow these steps to complete the installation:

 - Open `File Explore`
 - Browse to  `<OTS_BASE>\maint`, e.g. **C:\OpenText\OTS 24.2\maint**
 - Double click on `SetupDocumentAccessibility.bat`
 
 ![SetupDocumentAccessibility.bat](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/142-setup-document-accessibility.png)

`SetupDocumentAccessibility.bat` is going a make several questions. Use the default answers, except for these questions:

 - **Database name**: DA
 - **Storage Service Root Path**: `C:\OpenText\OTS 24.2\BaseRepositories\tomcat\ots\common`

> Remember, previously we set the password for super user in PostgreSQL installation

![SetupDocumentAccesibility.bat](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/143-setup-document-accesibility.png)

Once the setup is complete you will see a message like this:

![SetupDocumentAccesibility.bat](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/144-setup-document-accesibility.png)

## Remediate with Document Accessibility (DA)

Open a web browser and browse to http://localhost:8080/remediation:

 - **User**: Administrator
 - **Password**: `<no password by default>`

> Remember to config a password to access DA

![Document Accessibility (DA)](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/145-remediation.png)


## DA installation troubleshooting

> If you see an error page when you access to http://localhost:8080/remediation for first time you can fix it in two different ways

### OPTION 1

Access to `RESOURCES > SERVICES` on http://localhost:8080/OTSManager/, and check that the 3 first services are up and running.


![RESOURCES > SERVICES](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/146-resources-services.png)

### OPTION 2: 

First, restart tomcat and complete these actions:

 - Go to `<OTS_BASE>\TomcatBase` on File Explore, e.g. **C:\OpenText\OTS 24. 2\TomcatBase**
 - Double click on `start-ots.bat`

> **NOTE**: You must stop OTS if is previously running

![start-ots.bat](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/147-start-ots.png)

## Enabling AI capabilities

Open a web browser and navigate to http://localhost:8080/OTSManager/

 - **User**: Administrator
 - **Password**: `<no password by default>`
 
![OTDSManager login](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/165-OTDSManager-login.png)
 

 - Browse to `RESOURCES > FILE SYSTEM`
 - Select `Mounted file System > otda > otda-1.xRemediationService`


![otda-1.xRemediationService](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/166-otda-1.xRemediationService.png)
 
### Enabling AI capabilities: creating DocumentAccessibilityTrainers group 

 - Click on `Open` button


![otda-1.xRemediationService](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/167-open-otda-1.xRemediationService.png)

 - Add this text between `<groups>… </groups>`  tags to create a new group:

```xml 
 <trainers>DocumentAccessibilityTrainers</trainers>
```

 - Click on `Save`
 - Click on `Close`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<RemediationService po-class="com.xenos.remediation.service.parm.RemediationServiceParm"
                    preflightService="otda/otda-1.xPreflightService"
                    storageService="otda/otda-1.xStorageService"
                    persistentStorageName="OTDAStorage">
    <autoDetect enabled="true"
                trainingDataLocation=""
                isolateModels="false">
        <backgroundProcessing enabled="true"
                              maxWaitTime="10000"
                              pageInterval="0"/>
    </autoDetect>
    <encryption enableUpload="true"/>
    <emailService mailSender=""/>
    <supportPackaging enabled="false"
                      to=""
                      from=""
                      contentType="text/html"
                      subject="A new support package is waiting."
                      body="A new support package is available."
                      networkShare=""/>
    <groups>
        <administrators>Administrators</administrators>
        <trainers>DocumentAccessibilityTrainers</trainers>
    </groups>
</RemediationService>

```

### Enabling AI capabilities: assigning users to group

 - On OTSManager, browse to `RESOURCES > FILE SYSTEM`
 - Double click on `Mounted file System > _resources > authentication > user.xAuthScheme`

![user xauthscheme](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/168-user-xauthscheme.png)

 - Add this text

```xml 
<memberOf>DocumentAccessibilityTrainers</memberOf>
```

between

```xml 
<users name="Administrator“ password="þÿ"> 
   <!-- … -->
</users> 
```

tags to create a new group

 - Click on `Save`
 - Click on `Close`

### Enabling AI capabilities: changes taking effect

To apply our changes
 - Click on `user icon` on the top-right corner
 - Click on `Log out`

![OTSManager log out](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/169-otsmanager-log-out.png)

 - Press `Ctlr + C` in the console running `start-ots.bat` to stop the server
 - Go to `<OTS_BASE>\TomcatBase` on *File Explore*, e.g. **C:\OpenText\OTS 24.2\TomcatBase**
 - Double click on `start-ots.bat` to start the server
 - Browse to http://localhost:8080/remediation 
 - Login as `Administrator`
 - A new menu entry called `Training` is shown

![DA training](/images/2024-09-21-how-to-install-document-accessibility-da-24-2-on-windows/170-training.png)



