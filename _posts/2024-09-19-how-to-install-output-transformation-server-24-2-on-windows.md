---
title: "How to install Output Transformation Server 24.2 on Windows?"
header:
  image: /images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/08-Output-Transformation-Designer.png
  og_image: /images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/08-Output-Transformation-Designer.png
tags:
  - OpenText
  - Output Transformation Server
  - OTS
last_modified_at: 2024-09-19T20:24:18-04:00
---

OpenText *Output Transformation Server (OTS)* enables organizations to transform, store and present 
print-ready documents at scale, as well as in highly complete and structured accessible formats. 
Through high-performance document transformation, organizations can also index and archive 
high-volume documents and reduce paper usage.

## Prerequisites: 3rd party software

Download and install these products in your environment:

   - *Mandatory*
      - [Java SE 8](https://jdk.java.net/java-se-ri/8-MR5), [Java SE 11](https://jdk.java.net/java-se-ri/11-MR2) or [Java SE 17](https://jdk.java.net/java-se-ri/17) (Recommended)
	  - [Tomcat 9](https://tomcat.apache.org/download-90.cgi)

   - *Recommended:*
	  - [Acrobat Reader](https://get.adobe.com/reader/)
	  - [Notepad++](https://notepad-plus-plus.org/downloads/)

## OTS software 

 - Browse to [OpenText My Support](https://support.opentext.com)
 - Click on `Knowledge > Software Download`

![Knowledge software](/images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/01-knowledge-software.png)

   - Select:
      - **Product**: `AI, Analytics & reporting > Output Transformation`
      - **Content type**:  `Sofware Download`

![Filter by Ouput Transformation Server](/images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/02-output-transformation-server-filter.png)

 - Download following componets:

![My Support - OTS](/images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/04-mysupport.opentext.com-ots.png)

|               | Description                                           | Link (My Support)                                                                                              |
| ------------- | ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Base          | Output Transformation Server Base 24.2                | [ots_base.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=debb7ccb47750e10fd2258e5536d4372) |
| Modules       | Output Transformation Server Core 24.2                | [ots_core_24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=b9faf84747750e10fd2258e5536d4306) |
| Modules       | Output Transformation Server Data Transformation 24.2 | [OTS_DataTransformation_24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=cadbb04f47750e10fd2258e5536d4382) |
| Modules       | Document Accessibility 24.2                           | [OTS_DocumentAccessibility_24.2.00_11801.zip](https://support.opentext.com/csm?sys_kb_id=b40c708f47750e10fd2258e5536d43f7&id=kb_article_view&sysparm_rank=1&sysparm_tsqueryId=c82a8b7d470a829018d18ba5536d431e) |
| Modules       | Output Transformation Server ebXMLMessaging 24.2      | [OTS_ebXMLMessaging_24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=afcc740747b50e10fd2258e5536d432c) |
| Modules       | Machine Learning 24.2                                 | [OTS_ml-dl4j_24.2.00_11801.zip](https://support.opentext.com/csm?sys_kb_id=f72c740347b50e10fd2258e5536d43cb&id=kb_article_view&sysparm_rank=1&sysparm_tsqueryId=d5e9c33d470a829018d18ba5536d43c2) |
| Modules       | Output Transformation 24.2                            | [OTS_OutputTransformation_24.2.00_11801.zip](https://support.opentext.com/csm?sys_kb_id=3d5cb44347b50e10fd2258e5536d4320&id=kb_article_view&sysparm_rank=1&sysparm_tsqueryId=5be9936f47724690f69d90a5536d43c1) |
| Modules       | Output Transformation Server 24.2                     | [OTS_OutputTransformationServer_24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=ab29bc0f47350e10fd2258e5536d4322) |
| Modules       | Output Transformation Server Archive Navigator 24.2   | [OTS_ArchiveNavigator_24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=393a380347750e10fd2258e5536d43f9) |
| Connectors    | Output Transportation Server Integration ICN 24.2     | [OTS-integration-icn-24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=d9fcf04747b50e10fd2258e5536d438d) |
| Connectors    | Output Transformation Server ODWEK Integration 24.2   | [OTS-integration-odwek-24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=8b6dbc8747b50e10fd2258e5536d43a4) |
| Connectors    | Output Transformation for InfoArchive 24.2 Windows    | [OutputTransformation-for-InfoArchive-24.2.00_11801.zip](https://support.opentext.com/csm?sys_kb_id=5a2eb0cb47b50e10fd2258e5536d43d5&id=kb_article_view&sysparm_rank=1&sysparm_tsqueryId=a9badfaf47724690f69d90a5536d4363) |
| Connectors    | Output Transformation Server Xenos PDF API 24.2       | [Xenos-PDF-API-24.2.00_11801.zip](https://support.opentext.com/csm?id=kb_article_view&sys_kb_id=d7ae748f47b50e10fd2258e5536d43ce) |

## OTS installation

   - Create the folder `c:\OpenText\OTS 24.2`
   - Unzip `ots_base.zip` into `c:\OpenText\OTS 24.2`
   - Copy all the modules, .zip files, under `<OTS_BASE>\modules`
      - OTS_ArchiveNavigator_24.2.00_11801.zip
      - ots_core_24.2.00_11801.zip
      - OTS_DataTransformation_24.2.00_11801.zip
      - OTS_DocumentAccessibility_24.2.00_11801.zip
      - OTS_ebXMLMessaging_24.2.00_11801.zip
      - OTS_ml-dl4j_24.2.00_11801.zip
      - OTS_OutputTransformation_24.2.00_11801.zip
      - OTS_OutputTransformationServer_24.2.00_11801.zip

![OTS modulos](/images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/03-ots-23_4_modules.png)

   - Copy the license file into `<OTS_BASE>\settings`
 
> **NOTE:** The license is provided by OpenText once you adquire your software license
 
   - Rename it as `license.txt`
   - Browse to `<OTS_BASE>\maint in Windows Explorer`
   - Double click on `OnTimeSetup.bat`
      - Enter `Y` and press `Enter` to accept the EULA
      - The installer expands all the modules
      - Press `Enter` to close the Console once the installation is complete

![OnTimeSetup.bat](/images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/06-OneTimeSetup.bat.png)

## Launch Designer

   - Browse to `<OTS_BASE>\startup` in *Windows File Explorer*
   - Double click on `startDesigner.bat`

![Output Transformation Designer](/images/2024-09-19-how-to-install-output-transformation-server-24-2-on-windows/08-Output-Transformation-Designer.png)

Congrats! You have successfully installed Output Transformation Server (OTS) 24.2 on Windows.