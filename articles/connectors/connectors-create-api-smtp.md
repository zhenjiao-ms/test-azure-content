<properties
    pageTitle="Add the SMTP Connector in your Logic Apps | Microsoft Azure"
    description="Overview of the SMTP Connector with REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="05/19/2016"
   ms.author="mandia"/>

# Get started with the SMTP Connector
Connect to an SMTP server to send email. The SMTP Connector can be used from:

- Logic apps (discussed in this topic)
- PowerApps (see the [PowerApps connections list](https://powerapps.microsoft.com/tutorials/connections-list/) for the complete list)

>[AZURE.NOTE] This version of the article applies to logic apps 2015-08-01-preview schema version.

With SMTP, you can:

- Build your business flow that includes sending email using SMTP. 
- Use an action to send email. This action gets a response, and then makes the output available for other actions. For example, when there is a new file on your FTP server, you can take that file and email it as an attachment using SMTP. 

To add an operation in logic apps, see [Create a logic app](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Triggers and actions
The SMTP Connector has the following action available. There are no triggers.

|Triggers | Actions|
|--- | ---|
|None | Send email|

All connectors support data in JSON and XML formats. 

## Create a connection to SMTP

>[AZURE.INCLUDE [Steps to create a SMTP connection](../../includes/connectors-create-api-smtp.md)] 

After you create the connection, you enter the SMTP properties, like the To or CC values. The **REST API reference** in this topic describes these properties.

>[AZURE.TIP] You can use this same SMTP connection in other logic apps.

## Swagger REST API reference
Applies to version: 1.0.

### Send Email
Sends an email to one or more recipients.  
```POST: /SendEmail```

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|emailMessage| many|yes|body|none |Email message|

## Object definitions

#### Email: SMTP email

| Name | Data Type | Required|
|---|---|---|
|To|string|no|
|CC|string|no|
|Subject|string|no|
|Body|string|no|
|From|string|no|
|IsHtml|boolean|no|
|Bcc|string|no|
|Importance|string|no|
|Attachments|array|no|


#### Attachment: Email attachment

| Name | Data Type |Required|
|---|---|---|
|FileName|string|no|
|ContentId|string|no|
|ContentData|string|yes|
|ContentType|string|yes|
|ContentTransferEncoding|string|yes|


## Next steps
[Create a logic app](../app-service-logic/app-service-logic-create-a-logic-app.md).
