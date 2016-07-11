<properties 
	pageTitle="What are Logic Apps?" 
	description="Learn more about App Service Logic Apps" 
	authors="kevinlam1" 
	manager="dwrede" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article" 
	ms.date="04/07/2016"
	ms.author="klam"/>

#What are Logic Apps?

| Quick Reference |
| --------------- |
| [Logic Apps Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Logic Apps Connector Documentation](../connectors/apis-list.md) |
| [Logic Apps Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Azure App Service is a fully managed PaaS (Platform as a Service) for developers that makes it easier to build web, mobile, and integration apps. Logic Apps are a part of this suite and allow any technical user or developer to automate business process execution and workflow using an easy-to-use visual designer.

Best of all, Logic Apps can be combined with built-in [Managed APIs][managedapis] to help solve even tricky integration scenarios with ease: 

![Flow app designer](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

As mentioned, with logic apps, you can automate business processes. Here are a couple examples:  
 
* You can automatically replicate new records in your SQL DB and then send mail to the front desk.   
* Automatically find negative tweets and send them to a slack channel.

Scenarios such as these can be configured all from the visual designer and without writing a single line of code. Get started [building your logic app now][create].

## Why Logic Apps?

Logic Apps allow developers to design workflows that start from a trigger and then execute a series of steps. Each step invokes an API whilst securely taking care of authentication and best practices, like checkpointing and durable execution.

If you want to automate any business process (e.g. find negative tweets and post to your internal slack channel or replicate new customer records from SQL, as they arrive, into your CRM system), Logic Apps makes integrating disparate data sources, from cloud to on-premises easy. Check out our [managed APIs][managedapis] for more inspiration and [get started][create] now to see what you can do. 

What's more, with our [BizTalk managed APIs][biztalk] you can scale to mature integration scenarios with the power of a [rules engine][rules], [trading partner management][tpm], and more.

- **Easy to use design tools** - Logic Apps can be designed end-to-end in the browser. Start with a trigger - from a simple schedule to whenever a tweet appears about your company. Then orchestrate any number of actions using the rich gallery of connectors.

- **Compose SaaS easily** - Even composition tasks that are easy to describe are difficult to implement in code. Logic Apps make it a cinch to connect disparate systems. Want to create a task in your CRM software that is based on the activity from your Facebook or Twitter accounts? Want to connect your cloud marketing solution to your on-premises billing system? Logic apps are the fastest, most reliable way to deliver solutions to these problems.

- **Get started quickly from templates** - To help you get started we've provided a [gallery of templates][templates] that allow you to rapidly create some common solutions. From advanced BizTalk solutions to simple SaaS connectivity, and even a few that are just 'for fun' - the gallery is the fastest way to understand the power of Logic Apps.

- **Extensibility baked-in** - Don't see the API you need? Logic Apps is designed to work with API apps; you can easily create your own API app to use as a custom API. Build a new app just for you, or share and monetize in the marketplace.

- **Real integration horsepower** - Start easy and grow as you need. Logic Apps can easily leverage the power of BizTalk, Microsoft's industry leading integration solution to enable integration professionals to build the solutions they need. Find out more about the [BizTalk capabilities provided with Logic Apps][biztalk].

## Logic App Concepts

The following are some of the key pieces that comprise the Logic Apps experience. 

- **Workflow** - Logic Apps provides a graphical way to model your business processes as a series of steps or a workflow.
- **Managed APIs** - Your logic apps need access to data and services. Managed APIs are created specifically to aid you when you are connecting to and working with your data. See the list of APIs available now in [managed APIs][managedapis].
- **Triggers** - Some Managed APIs can also act as a trigger. A trigger starts a new instance of a workflow based on a specific event, like the arrival of an e-mail or a change in your Azure Storage account.
-  **Actions** - Each step after the trigger in a workflow is called an action. Each action typically maps to an operation on your managed or custom API apps.
- **BizTalk** - For more advanced integration scenarios, Logic Apps includes capabilities from Biztalk. Biztalk is Microsoft's industry leading integration platform. The BizTalk API apps allow you to easily include validation, transformation, rules, and more in to your Logic App workflows. Find out more in [what are BizTalk API apps][biztalk].

## Getting Started  

 - To get started with Logic Apps, follow the [create a Logic App][create] tutorial.  
 - [View common examples and scenarios](app-service-logic-examples-and-scenarios.md)
 - [You can automate business processes with Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
 - [Learn How to Integrate your systems with Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
- For more information on Azure App Service platform, see [Azure App Service][appservice].

[biztalk]: app-service-logic-what-are-biztalk-api-apps.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-create-a-trading-partner-agreement.md
[rules]: app-service-logic-use-biztalk-rules.md
[templates]: app-service-logic-use-logic-app-templates.md
