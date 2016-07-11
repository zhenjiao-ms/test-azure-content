<properties 
	pageTitle="How To Control Inbound Traffic to an App Service Environment" 
	description="Learn about how to configure network security rules to control inbound traffic to an App Service Environment." 
	services="app-service" 
	documentationCenter="" 
	authors="ccompy" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/01/2016" 
	ms.author="stefsch"/>	

# How To Control Inbound Traffic to an App Service Environment

## Overview ##
An App Service Environment is always created in a subnet of a regional classic "v1" [virtual network][virtualnetwork].  A new regional classic "v1" virtual network and new subnet can be defined at the time an App Service Environment is created.  Alternatively, an App Service Environment can be created in a pre-existing regional classic "v1" virtual network and pre-existing subnet.  With a recent change made in June 2016, ASEs can now be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e. private addresses).  For more details on creating an App Service Environment see [How To Create an App Service Environment][HowToCreateAnAppServiceEnvironment].

**Note:**  An App Service Environment cannot be created in a "v2" ARM-managed virtual network.

An App Service Environment must always be created within a subnet because a subnet provides a network boundary which can be used to lock down inbound traffic behind upstream devices and services such that HTTP and HTTPS traffic is only accepted from specific upstream IP addresses.

Inbound and outbound network traffic on a subnet is controlled using a [network security group][NetworkSecurityGroups]. Currently only network security groups created in the classic deployment model are supported for App Service Environment. Controlling inbound traffic requires creating network security rules in a network security group, and then assigning the network security group the subnet containing the App Service Environment.

Once a network security group is assigned to a subnet, inbound traffic to apps in the App Service Environment is allowed/blocked based on the allow and deny rules defined in the network security group.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## Network Ports Used in an App Service Environment ##
Before locking down inbound network traffic with a network security group, it is important to know the set of required and optional network ports used by an App Service Environment.  Accidentally closing off traffic to some ports can result in loss of functionality in an App Service Environment.

The following is a list of ports used by an App Service Environment:

- 454:  **Required port** used by Azure infrastructure for managing and maintaining App Service Environments.  Do not block traffic to this port.
- 455:  **Required port** used by Azure infrastructure for managing and maintaining App Service Environments.  Do not block traffic to this port.
- 80:  Default port for inbound HTTP traffic to apps running in App Service Plans in an App Service Environment
- 443: Default port for inbound SSL traffic to apps running in App Service Plans in an App Service Environment
- 21:  Control channel for FTP.  This port can be safely blocked if FTP is not being used.
- 10001-10020: Data channels for FTP.  As with the control channel, these ports can be safely blocked if FTP is not being used   
- 4016: Used for remote debugging with Visual Studio 2012.  This port can be safely blocked if the feature is not being used.
- 4018: Used for remote debugging with Visual Studio 2013.  This port can be safely blocked if the feature is not being used.
- 4020: Used for remote debugging with Visual Studio 2015.  This port can be safely blocked if the feature is not being used.

## Outbound Connectivity and DNS Requirements ##
For an App Service Environment to function properly, it requires outbound access to Azure Storage worldwide as well as Sql Database in the same Azure region.  If outbound Internet access is blocked in the virtual network, App Service Environments will not be able to access these Azure endpoints.

App Service Environments also require a valid DNS infrastructure configured for the virtual network.  If for any reason the DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment to pick up the new DNS configuration.  Triggering a rolling environment reboot using the "Restart" icon located at the top of the App Service Environment management blade in the [Azure portal][NewPortal] will cause the environment to pick up the new DNS configuration.

The following list details the connectivity and DNS requirements for an App Service Environment:

-  Outbound network connectivity to Azure Storage endpoints worldwide.  This includes endpoints located in the same region as the App Service Environment, as well as storage endpoints located in **other** Azure regions.  Azure Storage endpoints resolve under the following DNS domains: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* and *file.core.windows.net*.  
-  Outbound network connectivity to Sql DB endpoints located in the same region as the App Service Environment.  Sql DB endpoints resolve under the following domain:  *database.windows.net*.
-  Outbound network connectivity to the Azure management plane endpoints (both ASM and ARM endpoints).  This includes outbound connectivity to both *management.core.windows.net* and *management.azure.com*. 
-  Outbound network connectivity to *ocsp.msocsp.com*, *mscrl.microsoft.com* and *crl.microsoft.com*.  This is needed to support SSL functionality.
-  The DNS configuration for the virtual network must be capable of resolving all of the endpoints and domains mentioned in the earlier points.  If these endpoints cannot be resolved, App Service Environment creation attempts will fail, and existing App Service Environments will be marked as unhealthy.
-  If a custom DNS server exists on the other end of a VPN gateway, the DNS server must be reachable from the subnet containing the App Service Environment. 
-  The outbound network path cannot travel through internal corporate proxies, nor can it be force tunneled to on-premises.  Doing so changes the effective NAT address of outbound network traffic from the App Service Environment.  Changing the NAT address of an App Service Environment's outbound network traffic will cause connectivity failures to many of the endpoints listed above.  This results in failed App Service Environment creation attempts, as well as previously healthy App Service Environments being marked as unhealthy.  
-  Inbound network access to required ports for App Service Environments must be allowed as described in this [article](app-service-app-service-environment-control-inbound-traffic.md).

It is also recommended that any custom DNS servers on the vnet be setup ahead of time prior to creating an App Service Environment.  If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in the App Service Environment creation process failing.  In a similar vein, if a custom DNS server exists on the other end of a VPN gateway, and the DNS server is unreachable or unavailable, the App Service Environment creation process will also fail.

## Creating a Network Security Group ##
For full details on how network security groups work see the following [information][NetworkSecurityGroups].  The details below touch on highlights of network security groups, with a focus on configuring and applying a network security group to a subnet that contains an App Service Environment.

**Note:** Network security groups can be configured graphically using the [Azure Portal](https://portal.azure.com) or through Azure PowerShell.

Network security groups are first created as a standalone entity associated with a subscription. Since network security groups are created in an Azure region, ensure that the network security group is created in the same region as the App Service Environment.

The following demonstrates creating a network security group:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Once a network security group is created, one or more network security rules are added to it.  Since the set of rules may change over time, it is recommended to space out the numbering scheme used for rule priorities to make it easy to insert additional rules over time.

The example below shows a rule that explicitly grants access to the management ports needed by the Azure infrastructure to manage and maintain an App Service Environment.  Note that all management traffic flows over SSL and is secured by client certificates, so even though the ports are opened they are inaccessible by any entity other than Azure management infrastructure.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

When locking down access to port 80 and 443 to "hide" an App Service Environment behind upstream devices or services, you will need to know the upstream IP address.  For example, if you are using a web application firewall (WAF), the WAF will have its own IP address (or addresses) which it uses when proxying traffic to a downstream App Service Environment.  You will need to use this IP address in the *SourceAddressPrefix* parameter of a network security rule.

In the example below, inbound traffic from a specific upstream IP address is explicitly allowed.  The address *1.2.3.4* is used as a placeholder for the IP address of an upstream WAF.  Change the value to match the address used by your upstream device or service.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
If FTP support is desired, the following rules can be used as a template to grant access to the FTP control port and data channel ports.  Since FTP is a stateful protocol, you may not be able to route FTP traffic through a traditional HTTP/HTTPS firewall or proxy device.  In this case you will need to set the *SourceAddressPrefix* to a different value - for example the IP address range of developer or deployment machines on which FTP clients are running. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Note:**  the data channel port range may change during the preview period.)

If remote debugging with Visual Studio is used, the following rules demonstrate how to grant access.  There is a separate rule for each supported version of Visual Studio since each version uses a different port for remote debugging.  As with FTP access, remote debugging traffic may not flow properly through a traditional WAF or proxy device.  The *SourceAddressPrefix* can instead be set to the IP address range of developer machines running Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## Assigning a Network Security Group to a Subnet ##
A network security group has a default security rule which denies access to all external traffic.  The result from combining the network security rules described above, and the default security rule blocking inbound traffic, is that only traffic from source address ranges associated with an *Allow* action will be able to send traffic to apps running in an App Service Environment.

After a network security group is populated with security rules, it needs to be assigned to the subnet containing the App Service Environment.  The assignment command references both the name of the virtual network where the App Service Environment resides, as well as the name of the subnet where the App Service Environment was created.  

The example below shows a network security group being assigned to a subnet and virtual network:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Once the network security group assignment succeeds (the assignment is a long-running operations and can take a few minutes to complete), only inbound traffic matching *Allow* rules will successfully reach apps in the App Service Environment.

For completeness the following example shows how to remove and thus dis-associate the network security group from the subnet:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## Special Considerations for Explicit IP-SSL ##
If an app is configured with an explicit IP address, instead of the default IP address of the App Service Environment, both HTTP and HTTPS traffic flows into the subnet over a different set of ports other than ports 80 and 443.

The individual pair of ports used for each IP-SSL address can be found by clicking through "All settings" --> "IP Addresses" from an App Service Environment's user interface blade.  The "IP Addresses" blade shows a table of all explicitly configured IP-SSL addresses for the App Service Environment, along with the special port pair that is used to route HTTP and HTTPS traffic associated with each IP-SSL address.  It is this port pair that needs to be used for the DestinationPortRange parameters when configuring rules in a network security group.

## Getting started

To get started with App Service Environments, see [Introduction to App Service Environment][IntroToAppServiceEnvironment]

For details around apps securely connecting to backend resource from an App Service Environment, see [Securely connecting to Backend resources from an App Service Environment][SecurelyConnecttoBackend]

For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
