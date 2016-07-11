<properties
   pageTitle="Managing security recommendations in Azure Security Center  | Microsoft Azure"
   description="This document walks you through how recommendations in Azure Security Center help you protect your Azure resources and stay in compliance with security policies."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/05/2016"
   ms.author="terrylan"/>

# Managing security recommendations in Azure Security Center

This document walks you through how to use recommendations in Azure Security Center to help you protect your Azure resources.

> [AZURE.NOTE] The information in this document applies to the preview release of Azure Security Center. This document introduces the service by using an example deployment.  This is not a step-by-step guide.

## What is Azure Security Center?
Azure Security Center helps you prevent, detect, and respond to threats with increased visibility into and control over the security of your Azure resources. It provides integrated security monitoring and policy management across your subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.

## What are security recommendations?
Security Center periodically analyzes the security state of your Azure resources. When Security Center identifies potential security vulnerabilities, it creates recommendations. The recommendations guide you through the process of configuring the needed controls.

## Implementing security recommendations

### Set recommendations

In [Setting security policies in Azure Security Center](security-center-policies.md), you learn to:

- Configure security policies.
- Turn on data collection.
- Choose which recommendations to see as part of your security policy.

Current policy recommendations center around system updates, baseline rules, antimalware programs, [network security groups](../virtual-network/virtual-networks-nsg.md) on subnets and network interfaces, SQL database auditing, SQL database transparent data encryption, and web application firewalls.  [Setting security policies](security-center-policies.md) provides a description of each recommendation option.

### Monitor recommendations
After setting a security policy, Security Center analyzes the security state of your resources to identify potential vulnerabilities. The **Recommendations** tile on the **Security Center** blade lets you know the total number of recommendations identified by Security Center.

![][2]

To see the details of each recommendation:

1. Click the **Recommendations tile** on the **Security Center** blade. The **Recommendations** blade opens.

The recommendations are shown in a table format where each line represents one particular recommendation. The columns of this table are:

- **DESCRIPTION**: Explains the recommendation and what needs to be done to address it.
- **RESOURCE**: Lists the resources to which this recommendation applies.
- **STATE**: Describes the current state of the recommendation:
    - **Open**: The recommendation hasn't been addressed yet.
    - **In Progress**: The recommendation is currently being applied to the resources, and no action is required by you.
    - **Resolved**: The recommendation has already been completed (in this case, the line will be grayed out).
- **SEVERITY**: Describes the severity of that particular recommendation:
    - **High**: A vulnerability exists with a meaningful resource (such as an application, a VM, or a network security group) and requires attention.
    - **Medium**: A vulnerability exists and non-critical or additional steps are required to eliminate it or to complete a process.
    - **Low**: A vulnerability exists that should be addressed but does not require immediate attention. (By default, low recommendations aren't presented, but you can filter on low recommendations if you want to see them.)

Use the table below as a reference to help you understand the available recommendations and what each one will do if you apply it.

> [AZURE.NOTE] You will want to understand the [classic and Resource Manager deployment models](../azure-classic-rm.md) for Azure resources.

|Recommendation|Description|
|-----|-----|
|[Enable data collection for subscriptions](security-center-enable-data-collection.md)|Recommends that you turn on data collection in the security policy for each of your subscriptions and all virtual machines (VMs) in your subscriptions.|
|Resolve mismatch baseline rules|Recommends that you align OS configurations with the recommended baselines, e.g. do not allow passwords to be saved.|
|Apply system updates|Recommends that you deploy missing system security and critical updates to VMs.|
|[Add a web application firewall](security-center-add-web-application-firewall.md)|Recommends that you deploy a web application firewall (WAF) for web endpoints. You can protect multiple web applications in Security Center by adding these applications to your existing WAF deployments. WAF appliances (created using the Resource Manager deployment model) need to be deployed to a separate virtual network. WAF appliances (created using the classic deployment model) are restricted to using a network security group. This support will be extended to a fully customized deployment of a WAF appliance (classic) in the future.|
|Finalize web application firewall setup|To complete the configuration of a WAF, traffic must be rerouted to the WAF appliance. Following this recommendation will complete the necessary setup changes.|
|[Enable Antimalware](security-center-enable-antimalware.md)|Recommends that you provision antimalware programs to VMs (Windows VMs only).|
|Enable Network Security Groups on subnets/network interfaces|Recommends that you enable network security groups (NSGs) on subnets and network interfaces.|
|Restrict access through public external endpoints|Recommends that you configure inbound traffic rules for NSGs.|
|[Enable server SQL Auditing](security-center-enable-auditing-on-sql-servers.md)|Recommends that you turn on auditing for Azure SQL servers (Azure SQL service only; doesn't include SQL running on your virtual machines).|
|[Enable database SQL Auditing](security-center-enable-auditing-on-sql-databases.md)|Recommends that you turn on auditing for Azure SQL databases (Azure SQL service only; doesn't include SQL running on your virtual machines).|
|[Enable Transparent Data Encryption on SQL databases](security-center-enable-transparent-data-encryption.md)|Recommends that you enable encryption for SQL databases (Azure SQL service only).|
|Deploy the VM Agent|Enables you to see which VMs require the VM Agent. The VM Agent must be installed on VMs in order to provision patch scanning, baseline scanning, and antimalware programs. The VM Agent is installed by default for VMs that are deployed from the Azure Marketplace. The article [VM Agent and Extensions – Part 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) provides information on how to install the VM Agent.|
| [Apply disk encryption](security-center-apply-disk-encryption.md) |Recommends that you encrypt your VM disks using Azure Disk Encryption (Windows and Linux VMs). Encryption is recommended for both the OS and data volumes on your VM.|
|Provide security contact details | Recommends that you provide security contact information for each of your subscriptions. Contact information is an email address and phone number. The information will be used to contact you if our security team finds that your resources are compromised. |
| Update OS version | Recommends that you update the operating system (OS) version for your Cloud Service to the most recent version available for your OS family.  To learn more about Cloud Services, see the [Cloud Services overview](../cloud-services/cloud-services-choose-me.md). |

You can filter and dismiss recommendations.

1. Click **Filter** on the **Recommendations** blade. The **Filter** blade opens and you select the severity and state values you wish to see.

    ![][3]

2. If you determine that a recommendation is not applicable, you can dismiss the recommendation and then filter it out of your view. There are two ways to dismiss a recommendation. One way is to right click an item, and then select **Dismiss**. The other is to hover over an item, click the three dots that appear to the right, and then select **Dismiss**. You can view dismissed recommendations by clicking **Filter**, and then selecting **Dismissed**.

    ![][4]

### Apply recommendations
After reviewing all recommendations, decide which one you should apply first. We recommend that you use the severity rating as the main parameter to evaluate which recommendations should be applied first.
Using the **Enable Antimalware** recommendation, let’s walk through an example of how to apply a recommendation.

1. In the **Recommendations** blade, select **Enable Antimalware**.
![][5]

2. In the **Install Antimalware** blade select from the list of virtual machine(s) without antimalware enabled and click **Install Antimalware**.
3. The **New resource** blade opens to allow you to select the antimalware solution you want to use. Select **Microsoft Antimalware**.
4. Additional information about the antimalware solution is displayed. Select **Create**.
5. Enter the required configuration settings on the **Add Extension** blade, and then select **OK**.
![][6]

[Microsoft Antimalware](../security/azure-security-antimalware.md) is now active on the selected virtual machine.


## Next steps
In this document, you were introduced to security recommendations in Security Center. To learn more about Security Center, see the following:

- [Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.
- [Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.
- [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.
- [Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.
- [Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.

<!--Image references-->
[2]: ./media/security-center-recommendations/recommendations-tile.png
[3]: ./media/security-center-recommendations/filter-recommendations.png
[4]: ./media/security-center-recommendations/dismiss-recommendations.png
[5]: ./media/security-center-recommendations/select-enable-antimalware.png
[6]: ./media/security-center-recommendations/install-antimalware.png
