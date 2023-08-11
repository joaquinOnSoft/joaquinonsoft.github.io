---
title: "Send SMS with OpenText Notifications"
header:
  image: /images/2023-08-11-send-sms-with-opentext-notifications/01-myportal-login.png
  og_image: /images/2023-08-11-send-sms-with-opentext-notifications/01-myportal-login.png
tags:
  - Notifications
last_modified_at: 2023-08-11T18:37:48-43:38
---

In this article we'll learn how to send SMS with OpenText Notifications.

> OpenText™ Notifications brings email, SMS, push, voice and fax messaging channels together 
> into a single, cloud-based messaging platform, eliminating siloed communication services. 
> Whether sending one or millions of messages, Notifications makes it easy to deliver 
> personalized communications to customers’ channel of choice to strengthen relationships, 
> expand visibility and fuel sales.

## Use case

A new product has been launched, so we want to inform our clients, through SMS, about a special promotion for the early birds. For this, I will use a recipient list and also use variables for personalization. 

## Client list

Our client list is a *CSV* file that looks like this:

```csv
REF,ADDR,INS_1,INS_2,INS_3
Summer campaign 2023,699-999-999,Joaquin,Garzon,joaquin@joaquinonsft.com
```

## Login

Browse to your `Notifications` server and introduce your credentials (user name/password)


 ![New Communication](../images/2023-08-11-send-sms-with-opentext-notifications/01-myportal-login.png)	  	

Click on the `Sign in` button to log in.

### User interface desired language

You can change the language used in the user interface doing click on the `world` icon at the bottom of the login page.

 ![Select desired language](../images/2023-08-11-send-sms-with-opentext-notifications/02-myportal-select-desired-language.png)	
 At the moment of writing this article, there are 8 languages supported by default.

## Upload a client list

Let's create a new client list from our CSV file. 

Click on `Lists > Manage list > Upload list (icon)`
 
 ![Upload list](../images/2023-08-11-send-sms-with-opentext-notifications/03-myportal-upload-list.png)

The `Upload list` pop-up is shown. Just provide the required information:

 - **List name**: UpdateApp.SMS 
 - **Level**: Customer
 - **List type**: Sms List
 - **Character set**: Unicode (UTF-8)
 - **Choose file**: customer-update_app_sms.csv
 
 ![Upload list pop-up](../images/2023-08-11-send-sms-with-opentext-notifications/04-myportal-upload-list-pop-up.png)

Click on `Upload` button to create the list.

When the import is finished, a pop-up window will appear with a summary of the number of imported records. You can close it by clicking the `Close window` button.

 ![Client list import summary](../images/2023-08-11-send-sms-with-opentext-notifications/05-myportal-upload-list-import-summary.png)



