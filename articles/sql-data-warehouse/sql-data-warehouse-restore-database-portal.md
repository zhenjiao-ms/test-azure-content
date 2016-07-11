<properties
   pageTitle="Restore an Azure SQL Data Warehouse (Portal) | Microsoft Azure"
   description="Azure portal tasks for restoring an Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/28/2016"
   ms.author="sonyama;barbkess;nicw"/>

# Restore an Azure SQL Data Warehouse (Portal)

> [AZURE.SELECTOR]
- [Overview][]
- [Portal][]
- [PowerShell][]
- [REST][]

In this article you will learn how to restore an Azure SQL Data Warehouse using the Azure Portal.

## Before you begin

**Verify your DTU capacity.** Each SQL Data Warehouse is hosted by a SQL server logical server.  This logical server has a capacity limit measured in DTU.  Before you can restore a SQL Data Warehouse, it is important to make sure the SQL server logical server hosting your database has enough DTU capacity for the database being restored. See this blog post for more information on [how to view and increase DTU quota][].


## Restore an active or paused database

To restore a database:

1. Log in to the [Azure portal][]
2. On the left side of the screen select **Browse** and then select **SQL servers**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Navigate to your server and select it
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Find the SQL Data Warehouse that you want to restore from and select it
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. At the top of the Data Warehouse blade, click **Restore**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Specify a new **Database name**
7. Select the latest **Restore Point**
    1. Make sure you choose the latest restore point.  Since restore points are shown in UTC, sometimes the default option shown is not the latest restore point.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Click **OK**
9. The database restore process will begin and can be monitored using **NOTIFICATIONS**

>[AZURE.NOTE] After the restore has completed, you can configure your recovered database by following the [Finalize a recovered database][] guide.


## Restore a deleted database

To restore a deleted database:

1. Log in to the [Azure portal][]
2. On the left side of the screen select **Browse** and then select **SQL servers**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Navigate to your server and select it
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Scroll down to the Operations section on your server's blade
5. Click the **Deleted Databases** tile
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Select the deleted database you want to restore
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Specify a new **Database name**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Click **OK**
9. The database restore process will begin and can be monitored using **NOTIFICATIONS**

>[AZURE.NOTE] After the restore has completed, you can configure your recovered database by following the [Finalize a recovered database][] guide.


## Next steps
To learn about the business continuity features of Azure SQL Database editions, please read the [Azure SQL Database business continuity overview][].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ./sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Finalize a recovered database]: ./sql-database-recovered-finalize.md

<!--MSDN references-->

<!--Blog references-->
[how to view and increase DTU quota]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
