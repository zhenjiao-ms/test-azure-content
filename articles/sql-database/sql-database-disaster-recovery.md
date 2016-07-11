<properties 
   pageTitle="SQL Database disaster recovery | Microsoft Azure" 
   description="Learn how to recover a database from a regional datacenter outage or failure with the Azure SQL Database Active Geo-Replication, and Geo-Restore capabilities." 
   services="sql-database" 
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-bcdr" 
   ms.date="06/16/2016"
   ms.author="carlrab"/>

# Restore an Azure SQL Database or failover to a secondary

Azure SQL Database offers the following capabilities for recovering from an outage:

- [Active Geo-Replication](sql-database-geo-replication-overview.md)
- [Geo-Restore](sql-database-recovery-using-backups.md#point-in-time-restore)

To learn about preparing for disaster and when to recover your database, see [Business continuity](sql-database-business-continuity.md) and [Business continuity design and recovery scenarios.md](). 

## When to initiate recovery

The recovery operation impacts the application. It requires changing the SQL connection string and could result in permanent data loss. Therefore, it should be done only when the outage is likely to last longer than your application's RTO. When the application is deployed to production you should perform regular monitoring of the application health and use the following data points to assert that the recovery is warranted:

1.	Permanent connectivity failure from the application tier to the database.
2.	The Azure Portal shows an alert about an incident in the region with broad impact.
3.	The Azure SQL Database Server is marked as degraded. 

Depending on your application tolerance to downtime and possible business liability you can consider the following recovery options.

Use the [Get Recoverable Database](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) to get the latest Geo-replicated restore point.

## Wait for service recovery

The Azure teams work diligently to restore service availability as quickly as possible but depending on the root cause it can take hours or days.  If your application can tolerate significant downtime you can simply wait for the recovery to complete. In this case, no action on your part is required.  You can see the current service status on our [Azure Service Health Dashboard](https://azure.microsoft.com/status/). After the recovery of the region your application’s availability will be restored.

## Failover to geo-replicated secondary database

If your application’s downtime can result in business liability you should be using geo-replicated database(s) in your application. It will enable the application to quickly restore availability in a different region in case of an outage. Learn how to [configure Geo-Replication](sql-database-geo-replication-portal.md).

To restore availability of the database(s) you need to initiate the failover to the geo-replicated secondary using one of the supported methods. 


Use one of the following guides to failover to a geo-replicated secondary database:

- [Failover to a geo-replicated secondary using Azure Portal](sql-database-geo-replication-portal.md)
- [Failover to a geo-replicated secondary using PowerShell](sql-database-geo-replication-powershell.md)
- [Failover to a geo-replicated secondary using T-SQL](sql-database-geo-replication-transact-sql.md) 



## Recover using Geo-Restore

If your application’s downtime does not result in business liability you can use Geo-Restore as a method to recover your application database(s). It creates a copy of the database from its latest geo-redundant backup. 

Use one of the following guides to geo-restore a database into a new region:

- [Geo-Restore a database to a new region using Azure Portal](sql-database-geo-restore-portal.md)
- [Geo-Restore a database to a new region using PowerShell](sql-database-geo-restore-powershell.md) 


## Configure your database after recovery

If you are using either geo-replication failover or geo-restore to recover from an outage, you must make sure that the connectivity to the new databases is properly configured so that the normal application function can be resumed. This is a checklist of tasks to get your recovered database production ready.

### Update Connection Strings

Because your recovered database will reside in a different server, you need to update your application’s connection string to point to that server.

For more information about changing connection strings, see the appropriate development language for your [connection library](sql-database-libraries.md).

### Configure Firewall Rules

You need to make sure that the firewall rules configured on server and on the database match those that were configured on the primary server and primary database. For more information, see [How to: Configure Firewall Settings (Azure SQL Database)](sql-database-configure-firewall-settings.md).


### Configure Logins and Database Users

You need to make sure that all the logins used by your application exist on the server which is hosting your recovered database. For more information, see [Security Configuration for Geo-Replication](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] You should configure and test your server firewall rules and logins (and their permissions) during a disaster recovery drill. These server-level objects and their configuration may not be available during the outage. For more information, see [Performing Disaster Recovery Drills](sql-database-disaster-recovery-drills.md).

### Setup Telemetry Alerts

You need to make sure your existing alert rule settings are updated to map to the recovered database and the different server. 

For more information about database alert rules, see [Receive Alert Notifications](../azure-portal/insights-receive-alert-notifications.md) and [Track Service Health](../azure-portal/insights-service-health.md).

### Enable Auditing

If auditing is required to access your database, you need to enable Auditing after the database recovery. A good indicator that auditing is required is that client applications use secure connection strings in a pattern of *.database.secure.windows.net. For more information, see [Get started with SQL database auditing](sql-database-auditing-get-started.md). 


## Next steps

- To learn about Azure SQL Database automated backups, see [SQL Database automated backups](sql-database-automated-backups.md)
- To learn about business continuity design and recovery scenarios, see [Continuity scenarios](sql-database-business-continuity-scenarios.md)
- To learn about using automated backups for recovery, see [restore a database from the service-initiated backups](sql-database-recovery-using-backups.md)
- To learn about faster recovery options, see [Active-Geo-Replication](sql-database-geo-replication-overview.md)  
- To learn about using automated backups for archiving, see [database copy](sql-database-copy.md)
