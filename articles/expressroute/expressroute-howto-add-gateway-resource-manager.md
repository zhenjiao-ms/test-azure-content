<properties
   pageTitle="Adding a VNet gateway to a virtual network for ExpressRoute using Resource Manager and PowerShell | Microsoft Azure"
   description="This article walks you through adding a Vnet gateway to an already created Resource Manager VNet for ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="04/06/2016"
   ms.author="charwen"/>

# Configure a virtual network gateway for ExpressRoute using Resource Manager and PowerShell


> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - Classic](expressroute-howto-add-gateway-classic.md)


This article will walk you through the steps to add, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet. The steps for this configuration are specifically for VNets that were created using the **Resource Manager deployment model** and that will be be used in an ExpressRoute configuration. 

**About Azure deployment models**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Before beginning

Verify that you have installed the Azure PowerShell cmdlets needed for this configuration (1.0.2 or later). If you haven't installed the cmdlets, you'll need to do so before beginning the configuration steps. For more information about installing Azure PowerShell, see [How to install and configure Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

	
## Next steps

After you have created the VNet gateway, you can link your VNet to an ExpressRoute circuit. See [Link a Virtual Network to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).
