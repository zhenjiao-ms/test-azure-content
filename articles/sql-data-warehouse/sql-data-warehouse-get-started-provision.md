<properties
   pageTitle="Create a SQL Data Warehouse in the Azure Portal | Microsoft Azure"
   description="Learn how to create an Azure SQL Data Warehouse in the Azure Portal"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/20/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Create an Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In this tutorial you will use the Azure Portal to create a SQL Data Warehouse that contains an AdventureWorksDW sample database.


[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]


1. Sign in to the [Azure Portal](https://portal.azure.com).

2. Click **+ New** > **Data + Storage** > **SQL Data Warehouse**.

    ![Create](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. In the **SQL Data Warehouse** blade, fill in the information needed, then press 'Create' to create.

    ![Create database](./media/sql-data-warehouse-get-started-provision/create-database.png)

	- **Server**: We recommend you select your server first.  You can select an existing server or [create a new one](./sql-data-warehouse-get-started-new-server.md). 

	- **Database name**:The name that will be used to reference the SQL Data Warehouse.  It must be unique to the server.
	
    - **Performance**: We recommend starting with 400 DWUs. You can move the slider to the left or right to adjust the performance of your data warehouse, or scale up or down after creation.  To learn more about DWUs see our documentation on [scaling](./sql-data-warehouse-manage-compute-overview.md) or our [pricing page](https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/). 

    - **Subscription**: Select the subscription that this SQL Data Warehouse will bill to.

    - **Resource group**: Resource groups are containers designed to help you manage a collection of Azure resources. Learn more about [resource groups](../azure-portal/resource-group-portal.md).

    - **Select source**: Click **Select source** > **Sample**. Since there is only one sample database available at this time, when you select Sample Azure automatically populates the **Select sample** option with AdventureWorksDW.

4. Click **Create** to create your SQL Data Warehouse.

5. Wait for a few minutes and your SQL Data Warehouse will be ready. When finished, you should be returned to the [Azure portal](https://portal.azure.com). You can find your SQL Data Warehouse on your dashboard, listed under your SQL Databases, or in the resource group that you used to create it. 

    ![Portal view](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL DataBase create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## Next steps

Now that you have created a SQL Data Warehouse, you are ready to [Connect](./sql-data-warehouse-connect-overview.md) and begin querying.

To load data into SQL Data Warehouse, see the [loading overview](./sql-data-warehouse-overview-load.md).

If you are trying to migrate an existing database to SQL Data Warehouse, see the [Migration overview](./sql-data-warehouse-overview-migrate.md) or use [Migration Utility](./sql-data-warehouse-migrate-migration-utility.md).

