---
title: "Sending an SMS using Notifications REST API with Postman"
header:
  image: /images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/00-sending-an-sms-using-notifications-rest-api-with-postman.jpg
  og_image: /images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/00-sending-an-sms-using-notifications-rest-api-with-postman.jpg
tags:
  - Notifications
  - CCM
  - OpenText  
  - Postman
last_modified_at: 2023-08-21T21:13:28-44:57
---

Let's see how to send an SMS using [Notifications REST API](https://developer.opentext.com/ce/products/notifications) 
with [Postman](https://www.postman.com/).

> **OpenText Notifications** brings email, SMS, push, voice and fax messaging channels together into a single, 
> cloud-based messaging platform, eliminating siloed communication services. Whether sending one or millions of 
> messages, Notifications makes it easy to deliver personalized communications to customers’ channel of choice 
> to strengthen relationships, expand visibility and fuel sales.

> **NOTE**: *Notifications* also provides a 
> [Notifications SOAP API](https://apiforums.easylink.com/emapidocs/).
> Currently (August 2023), SOAP API exposes more functions than the REST API. 

## Postman

We'll use **Postman**, an API platform for building and using APIs, to invoke 
*Notifications REST API* to send an SMS.

> Recomended reading: [Welcome to Postman](https://learning.postman.com/docs/getting-started/overview/)

## Send SMS

### POST - /mra/v1/outbound/sms

`POST` - `/mra/v1/outbound/sms`: This API call submits a request to send a SMS. It requires use of pure JSON request body wherein the SMS metadata (recipient SMS address and user-specified options) and text to be sent in SMS are part of the same JSON construct.	

### Send SMS with Postman

Let's create a new *Collection*. Just follow these steps:

 - Click on `New` button 
 
  ![Postman - New](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/01-postman-new-collection.png)

 - Click on `Collection`
 
  ![Postman - New collection](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/02-postman-new-collection.png)

 - Provide a `Name` for the new collection, i.e. *Notifications*  

  ![Postman - Set name to new collection](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/03-postman-set-name-new-collection.png)
  
 - Click on `Add a request`
 - Provide the basic request information:  
    - **Name**: Send SMS
	- **Method**: POST
	- **URL**: https://api.eu.cloudmessaging.opentext.com/mra/v1/outbound/sms
 - Click on `Authorization` tab
 
> **Authentication** 
> Either HTTP Basic authentication or OAUTH2 authorization is to be used for all requests.	 
> 
> **NOTE**: In this example we'll use basic authentication. 

 - Select `Basic Auth` on the `Type` drop-down list 
 
  ![Postman - basic authentication](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/04-postman-basic-authentication.png) 

 - Provide the authentication information:
    - **Username**
	- **Password**
 - Click on `Body`tab
    - Click on `raw`
    - Select `JSON` from the drop-down list
	
  ![Postman - body RAW JSON](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/05-postman-body-raw-json.png) 

 - Paste this *JSON* into the `Body` tab

```JSON
{
    "options": {
        "billing_code": "SUMMER-PROMO",
        "customer_reference": "JoaquinOnSoft"
    },
    "reports": {
        "delivery_report": {
        "type": "none",
        "report_destinations": [
            {
                "email": "joaquin@joaquinonsoft.com"
            }
        ]
        }
    },
    "destinations": [
        {
            "ref": "Joaquin-REF",
            "sms": "0034666999999"
        }
    ],
    "sms_text": "SMS from notifications REST API using Postman"
}
```

> **NOTE**: You can change different tags like `email`, `sms`, or `sms_text` according to your needs
 
 - Click on `Save` button	
 - Click on `Send` button	
 
The request is done and we receive the Notifications Job Identifier that we can use to track it.
 
  ![Postman - Send SMS response](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/06-postman-request-response.png) 
 
After a few seconds we receive the SMS in our mobile phone.
 
  ![SMS received](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/07-sms-received.jpg)  
 
 
## SMS Status

Once SMS request has been successfully accepted and a job ID is returned, `[GET ]- SMS Status` method allows 
users to request the information about the job as it is being processed by the OpenText messaging service.

Let's follow these steps to check the SMS status with *Postman*:

 - Click on 3 dots icon, close to the `Notifications` collection 
 - Select `Add request` on the drop-down list 
 
  ![Add request](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/08-add-request.png)  
 
 - Provide the basic request information:  
    - **Name**: SMS Status
	- **Method**: GET
	- **URL**: https://api.eu.cloudmessaging.opentext.com/mra/v1/outbound/sms/status/

  ![GET - SMS status](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/09-postman-sms-status.png)  

 - Click on `Authorization` tab	
 - Select `Basic Auth` on the `Type` drop-down list 
 - Provide the authentication information:
    - **Username**
	- **Password**	
 
 - Click on `Params` tab
 - Add a new param:
    - **Key**: job_id
	- **Value**: xsi2-44417320 (In our example)

 - Click on `Save` button	
 - Click on `Send` button	
	
The response, in our examples, looks like this:

```JSON
{
    "job_id": "xsi2-44417320",
    "billing_code_base64": "U1VNTUVSLVBST01P",
    "customer_reference_base64": "Sm9hcXVpbk9uU29mdA==",
    "entry_time": "2023-08-21T09:25:26.000Z",
    "job_state": [
        "Complete",
        "Posted"
    ],
    "deliveries": [
        {
            "ref_base64": "Sm9hcXVpbi1SRUY=",
            "sms": "0034626790770",
            "delivery_attempts": [
                {
                    "state": "Sent",
                    "first_attempt": "2023-08-21T10:25:26.000Z"
                }
            ],
            "events": {
                "sms_delivery_events": [
                    {
                        "event_time": "2023-08-21T10:26:00.000Z",
                        "status": "Confirmed"
                    }
                ]
            }
        }
    ]
}
```

 ![SMS Status response](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/10-postman-sms-status-response.png)  
## Check job status

We can also use `MyPortal` to check the job status. You just need to browse to `Jobs » Check job status` and look for the Job Identifier returned by `Send SMS` response, i.e. *44417320*.

 ![MyPortal - Check job status](/images/2023-08-21-sending-an-sms-using-notifications-rest-api-with-postman/11-myportal-check-jobs-status.png)  

