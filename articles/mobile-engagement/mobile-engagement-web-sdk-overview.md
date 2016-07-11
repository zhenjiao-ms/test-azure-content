<properties
	pageTitle="Azure Mobile Engagement Web SDK Overview | Microsoft Azure"
	description="Latest updates and procedures for Web SDK for Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="web"
	ms.devlang="js"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="piyushjo" />


# Web SDK for Azure Mobile Engagement

Start here to get all the details on how to integrate Azure Mobile Engagement in an Web App. If you'd like to give it a try first, make sure you do our [15 minutes tutorial](mobile-engagement-web-app-get-started.md).

## Integration procedures
1. Start here: [How to integrate Mobile Engagement in your Web app](mobile-engagement-web-integrate-engagement.md)

2. Tag plan implementation: [How to use the advanced Mobile Engagement tagging API in your Web app](mobile-engagement-web-use-engagement-api.md)

## Release notes

### 2.0.1 (06/10/2016)

-   SDK disabled on IE8 and IE9.
-   Fixed Opera web browser detection.

For all versions, please see the [complete release notes](mobile-engagement-web-release-notes.md).

## Upgrade procedures

### From 1.2.1 to 2.0.0

The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement. If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.2.1 first and then apply the following procedure.

This version of the Engagement Web SDK doesn't support samsung-tv, OperaTV, webOS and the Reach feature. 

>[AZURE.IMPORTANT] Capptain and Mobile Engagement are not the same services, and the procedure given below only highlights how to migrate the client app. Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers.

#### JavaScript files

Replace the `capptain-sdk.js` file by the `azure-engagement.js` file and update your script imports accordingly.

#### Remove Capptain Reach

This version of Engagement Web SDK doesn't support the Reach feature, if you integrated Capptain Reach in your application then it needs to be removed.

Remove the Reach css import from your page and delete the related css file (capptain-reach.css by default).

Delete the Reach resources: the close image (capptain-close.png by default) and the brand icon (capptain-notification-icon by default).

Remove Reach UI for in-app notifications, the default layout looks like:

	<!-- capptain notification -->
	<div id="capptain_notification_area" class="capptain_category_default">
	  <div class="icon">
	    <img src="capptain-notification-icon.png" alt="icon" />
	  </div>
	  <div class="content">
	    <div class="title" id="capptain_notification_title"></div>
	    <div class="message" id="capptain_notification_message"></div>
	  </div>
	  <div id="capptain_notification_image"></div>
	  <div>
	    <button id="capptain_notification_close">Close</button>
	  </div>
	</div>

Remove Reach UI for text\web announcements and polls, the default layout looks like:

	<div id="capptain_overlay" class="capptain_category_default">
	  <button id="capptain_overlay_close">x</button>
	  <div id="capptain_overlay_title"></div>
	  <div id="capptain_overlay_body"></div>
	  <div id="capptain_overlay_poll"></div>
	  <div id="capptain_overlay_buttons">
	    <button id="capptain_overlay_exit"></button>
	    <button id="capptain_overlay_action"></button>
	  </div>
	</div>

Remove the `reach` object from your configuration if any. It looks like the following:

	window.capptain = {
	  [...]
	  reach: {
	    [...]
	  }
	}

Remove any other Reach customization such as categories.

#### Remove deprecated APIs

Some of the APIs from Capptain are deprecated in the Engagement version of the SDK.

Remove any call to the following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, `agent.sendMessageToDevice`.

Remove the following callbacks, if any, from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, `onPushMessageReceived`.

#### Configuration

Now Engagement uses a connection string to configure the SDK identifiers such as the application identifier.

Replace the application ID by your connection string, also note that the global object for the SDK configuration moves from `capptain` to `azureEngagement`.

Before migration:

	window.capptain = {
	  appId: ...,
	  [...]
	};

After migration:

	window.azureEngagement = {
	  connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
	  [...]
	};

The connection string for your application is displayed on the Azure Portal.

#### JavaScript APIs

the global JavaScript object `window.capptain` has been renamed `window.azureEngagement` but you can use the `window.engagement` alias for API calls (you can't use the alias to define the SDK configuration).

For instance: `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity` etc ...

If you already have integrated an older version of our SDK into your application please consult [Upgrade Procedures](mobile-engagement-web-upgrade-procedure.md).
