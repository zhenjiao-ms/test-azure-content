<properties
	pageTitle="Manage Azure Key Vault using Azure Automation | Microsoft Azure"
	description="Learn about how the Azure Automation service can be used to manage Azure Key Vault."
	services="Key-Vault, automation"
	documentationCenter=""
	authors="csand-msft"
	manager="eamono"
	editor=""/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/18/2016"
	ms.author="csand"/>



#Managing Azure Key Vault using Azure Automation

This guide will introduce you to the Azure Automation service and how it can be used to simplify management of your keys and secrets in Azure Key Vault.

## What is Azure Automation?

[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation and desired state configuration. Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.

Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs. In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.

Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.


## How can Azure Automation help manage Azure Key Vault?

Key Vault can be managed in Azure Automation by using the [AzureRM Key Vault cmdlets] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx). The Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import the [AzureRM-KeyVault module] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within the service. You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.

With the Azure Key Vault cmdlets you can perform these tasks among others: create and configure a key vault, create or import a key, create or update a secret, update attributes of a key, get a key or secret, delete a key or secret.

Here are some examples of using PowerShell to manage Key Vault:
* [Azure Key Vault - Step by Step] (https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/)
* [Setting Up and Configuring an Azure Key Vault] (https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault/)


## Next Steps

Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Key Vault, follow these links to learn more about Azure Automation.

* See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).
* See the [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-Key-Vault-Powershell-1349b091).
