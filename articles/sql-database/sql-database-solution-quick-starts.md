<properties
   pageTitle="Azure SQL Database Solution Quick Starts | Microsoft Azure"
   description="Learn about Azure SQL Database Solutions"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="06/22/2016"
   ms.author="carlrab"/>

# Explore Azure SQL Database Solution Quick Starts

This article contains an overview of the Azure SQL Database Solution Quick Starts. These Quick Starts demonstrate the use of SQL Database in complete solution based on real world scenarios. For simple step-by-step tutorials demonstrating the use of a particular Azure SQL Database feature, see [Explore Azure SQL Database Tutorials](sql-database-explore-tutorials.md).

## WingTipTickets Demo and Hands-On Lab

[Azure SQL Database WingTipTickets](https://github.com/microsoft/wingtiptickets) demo and hands on lab These files comprise a hands-on-lab that demostrates an Azure SQL Database and Azure Search based sample application used to selling concert tickets

## Collect and monitor resource usage data across multiple pools

This Solution Quick Start provides a solution for collecting and monitoring Azure SQL Database resource usage accross multiple pools in a subscription. When you have a large number of databases in a subscription, it is cumbersome to monitor each elastic pool separately. To solve this, you can combine SQL database PowerShell cmdlets and T-SQL queries to collect resource usage data from multiple pools and their databases for monitoring and analysis of resource usage. 

[Manage Mulitiple Elastic Pools in SQL Database Using PowerShell and Power BI](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) in the GitHub SQL Server samples repository provides a set of powershell scripts and T-SQL queries along with documentation on what it does and how to use it.

## Get started using Elastic Pools in a SaaS scenario

This Solution Quick Start provides a solution for a Softwware-as-a-Solution (SaaS) scenario that leverages Elastic Pools to provide a cost-effective, scalable database back-end of a SaaS application. In this solution, you will walk-though the implementation of a web app that lets you visualize the load created on an Elastic Pool by a load generator using a custom dashboard that supplements the Azure Portal.

[Elastic Pool Custom Dashboard for Saas](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) in the GitHub SQL Server samples repository provides a load generator and monitoring web app along with the documentation on what it does and how to use it.

## Creating an Azure SQL Database using the Entity Framework and COde First Development

This video and sample provides an introduction to Code First development targeting a new database. This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too. Code First allows you to define your model using C# or VB.Net classes. Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API. See [Code First to a New Database](https://msdn.microsoft.com/data/jj193542.aspx). 

## Integrate Elastic Database Tools into an Entity Framework Application

This sample shows the changes in an Entity Framework application that are needed to integrate with [Elastic Database tools](sql-database-elastic-scale-get-started.md). The focus is on composing [shard map management](sql-database-elastic-scale-shard-map-management.md) and [data-dependent routing](sql-database-elastic-scale-data-dependent-routing.md) with the Entity Framework Code First approach. The [Code First – New Database sample for EF](http://msdn.microsoft.com/data/jj193542.aspx) serves as our running example throughout this sample. The sample code accompanying this document is part of elastic database tools' set of samples in the Visual Studio Code Samples. See [Elastic Database client library with Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md).

## Multi-tenant applications with elastic database tools and row-level security

This sample shows the changes to an Entity Framework application that are needed to integrate [Elastic Database tools](sql-database-elastic-scale-get-started.md) with [row-level security](https://msdn.microsoft.com/library/dn765131). This sample illustrates how to use these technologies together to build an application with a highly scalable data tier that supports multi-tenant shards, using ADO.NET SqlClient and/or Entity Framework. This sample extends the [Elastic Database client library with Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) by adding support for multi-tenant shard databases. It builds a simple console application for creating blogs and posts, with four tenants and two multi-tenant shard databases. See [Multi-tenant applications with elastic database tools and row-level security](sql-database-elastic-tools-multi-tenant-row-level-security.md).

## Tailspin Surveys application

This sample is a multitenant web application, called Surveys, that allows users to create online surveys. The sample demonstrates some key concerns when managing user identities in a multitenant application, including sign-up, authentication, authorization, and app roles. To run this sample, see [How to run the Tailspin Surveys sample application](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

## Next steps

[Explore Azure SQL Database Tutorials](sql-database-explore-tutorials.md)
