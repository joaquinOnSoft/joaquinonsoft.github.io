---
title: "Sending an SMS using Notifications SOAP API with SOAP UI"
header:
  image: /images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/00-sms-in-mobile-phone.jpg
  og_image: /images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/00-sms-in-mobile-phone.jpg
tags:
  - Notifications
  - CCM
  - OpenText  
last_modified_at: 2023-08-18T20:29:31-33:47
---

Today we'll learn how to send an SMS using [Notifications SOAP API](https://apiforums.easylink.com/emapidocs/) with [SOAP UI](https://www.soapui.org/).

> **OpenText Notifications** is a cloud-based, comprehensive omni-channel messaging platform that 
> increases the effectiveness of message creation and delivery. It is a coordinated solution to 
> deliver essential communications via SMS, voice, email and fax with end-to-end visibility and 
> message control. The solution provides the flexibility and scalability to meet every business 
> need, from individual personalized messages to large-scale broadcast communications.

> **NOTE**: *Notifications* also provides a 
> [REST API](https://developer.opentext.com/ce/products/notifications), 
> but currently (August 2023), exposes fewer functions than the SOAP API. 

## JobSubmit function

The [JobSubmit](https://apiforums.easylink.com/emapidocs/26/JobSubmit/JobSubmit.html) function allows one 
or more documents to be submitted for delivery to one or more destinations. This is one of the most basic, 
and also one of the most complex, functions available through the *Cloud Fax and Notifications API*.

We will use it to send an SMS to our mobile phone.

## SOAP UI

We'll use **SoapUI**, an automated testing tool for SOAP and REST APIs, to invoke 
*Notifications SOAP API* to send an SMS.

Let's create a new SOAP project. Just follow these steps:

 - Click on `File » New SOAP project`
 
 ![New SOAP project](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/01-new-soap-project.png)	  	

 - Provide the required information in the `New SOAP project` pop-up:
    - **Project name**: ws.easylink
	- **Initial WSDL**: http://ws.easylink.com/JobSubmit/2011/01?WSDL
	- **Create Request**: Checked
	
 ![New SOAP project pop-up](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/02-new-soap-project-pop-up.png)	  	
 - Click on `OK` button

A default request is created based on the request definition given in the WSDL:

 ![JobSubmit defaul request](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/03-jobsubmit-default-request.png)	  	
We must modify the *XML* request to provide the required information. Our request will look like this: 

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://ws.easylink.com/RequestResponse/2011/01" xmlns:ns1="http://ws.easylink.com/JobSubmit/2011/01">
   <soapenv:Header>
      <ns:Request>
         <ns:ReceiverKey>https://messagingeu.easylink.com/soap/sync</ns:ReceiverKey>
         <ns:Authentication>            
            <ns:XDDSAuth>
               <ns:RequesterID aliasType="?">USER</ns:RequesterID>
               <ns:Password>PASSWORD</ns:Password>
            </ns:XDDSAuth>
         </ns:Authentication>
      </ns:Request>
   </soapenv:Header>
   <soapenv:Body>
		   <JobSubmitRequest xmlns="http://ws.easylink.com/JobSubmit/2011/01">
				<Message>
				<JobOptions>
				<BillingCode>Joaquin</BillingCode>
				<CustomerReference>JoaquinOnSoft</CustomerReference>
					<SmsOptions>
						<CharacterSet>ISO-8859-1</CharacterSet>
					</SmsOptions>
				</JobOptions>
				<Destinations>
					<Sms><Phone>+34666999999</Phone></Sms>
				</Destinations>
				<Reports>
					<DeliveryReport>
					<DeliveryReportType>none</DeliveryReportType>
					</DeliveryReport>
				</Reports>
				<Contents>
					<Part>
						<Document>
								<DocType>text</DocType>
								<DocData format="text">Sending an SMS using Notifications SOAP API with SOAP UI</DocData>	
								<CharacterSet>ISO-8859-1</CharacterSet> 									
						</Document>
						<Treatment>body</Treatment>
					</Part>
				</Contents>
				</Message>
		</JobSubmitRequest>
   </soapenv:Body>
</soapenv:Envelope>

```

 ![JobSubmit custom request](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/04-jobsubmit-custom-request.png)	  

> **NOTE**: You must provide your own user (tag `ns:RequesterID`) and password 
> (tag `ns:Password`) to complete the authentication.
> Update the `Phone` tag with your mobile phone number to receive the SMS and 
> `DocData` tag with the desired text for your SMS.

 - Copy the URL in the `ns:ReceiverKey` tag
 
```
https://messagingeu.easylink.com/soap/sync
```
 - Paste the URL in the address text box

 ![JobSubmit custom request URL updated](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/05-jobsubmit-custom-request-url-updated.png)	  
 
 - Click on `Submit` button in the top-left menu
 
 ![JobSubmit custom request URL updated](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/06-submit-request.png)	   
 
We can see the response in the left-hand side. The tag `MRN` includes the job identifier, in our example *44376898*.

 ![JobSubmit response](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/07-jobsubmit-response.png)	   

## SMS in our mobile phone

After a few seconds the SMS is received in our mobile phone.
  
 ![SMS sent by OT Notifications](/images/2023-08-18-sending-an-sms-using-notifications-soap-api-with-soap-ui/08-sms-in-mobile-phone.jpg)  
 