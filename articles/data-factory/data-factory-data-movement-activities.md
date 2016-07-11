<properties 
	pageTitle="Data movement activities | Microsoft Azure" 
	description="Learn about data movement in Data Factory pipelines: data migration between cloud stores, between on-premises and cloud. Use the Copy Activity." 
	keywords="data movement, data migration, copy data, transfer data"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/31/2016" 
	ms.author="spelluru"/>

# Data movement and the Copy Activity: migrating data to the cloud and between cloud stores
Data movement from a source to a sink (destination) is performed by the [Copy Activity](#copyactivity) in Azure Data Factory. The Copy Activity is powered by a secure, reliable, scalable, and [globally available service](#global). The service automatically chooses the optimal region to perform the data movement, usually the region closest to the sink data store.

Here’s how data migration occurs between two cloud data stores, between an on-premises data store and a cloud data store, and from/to a data store on an Azure Iaas VM.

## Copy data between two cloud data stores
When both the source and sink (destination) data stores reside in the cloud, the copy Activity goes through the following stages to copy/move data from the source to the sink. The service that powers the Copy Activity performs the following: 

1. Reads data from source data store
2.	Performs serialization/deserialization, compression/decompression, column mapping, and type conversion based on the configurations of input dataset, output dataset and the Copy Activity 
3.	Writes data to the destination data store

![cloud-to-cloud copy](.\media\data-factory-data-movement-activities\cloud-to-cloud.png)


## Copy data between an on-premises data store and a cloud data store
To [securely move data between on-premises data stores behind your corporate firewall and a cloud data store](#moveonpremtocloud), you will need to install the Data Management Gateway, which is an agent that enables hybrid data movement and processing, on your on-premises machine. The Data Management Gateway can be installed on the same machine as the data store itself or on a separate machine that has access to reach the data store. In this scenario, the serialization/deserialization, compression/decompression, column mapping, and type conversion are performed by the Data Management Gateway. Data does not flow through Azure Data Factory service is such case. Data Management Gateway directly writes the data to the destination store. 

![onprem-to-cloud copy](.\media\data-factory-data-movement-activities\onprem-to-cloud.png)

## Copy data from/to a data store on an Azure Iaas VM 
You can also move data from/to supported data stores hosted on Azure IaaS VMs (Infrastructure-as-a-Service virtual machines) using the Data Management Gateway. In this case, the Data Management Gateway can be installed on the same Azure VM as the data store itself or on a separate VM that has access to reach the data store. 

## Supported data stores
Copy Activity copies data from a **source** data store to a **sink** data store. Data factory supports the following data stores and **data can from any source can be written to any sink**. Click on a data store to learn how to copy data from/to that store. 

| Sources| Sinks |
|:------- | :---- |
| <ul><li>[Azure Blob](data-factory-azure-blob-connector.md)</li><li>[Azure Table](data-factory-azure-table-connector.md)</li><li>[Azure SQL Database](data-factory-azure-sql-connector.md)</li><li>[Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)</li><li>[Azure DocumentDB (see note below)](data-factory-azure-documentdb-connector.md)</li><li>[Azure Data Lake Store](data-factory-azure-datalake-connector.md)</li><li>[SQL Server On-premises/Azure IaaS](data-factory-sqlserver-connector.md)</li><li>[File System On-premises/Azure IaaS](data-factory-onprem-file-system-connector.md)</li><li>[Oracle Database On-premises/Azure IaaS](data-factory-onprem-oracle-connector.md)</li><li>[MySQL Database On-premises/Azure IaaS ](data-factory-onprem-mysql-connector.md)</li><li>[DB2 Database On-premises/Azure IaaS](data-factory-onprem-db2-connector.md)</li><li>[Teradata Database On-premises/Azure IaaS ](data-factory-onprem-teradata-connector.md)</li><li>[Sybase Database On-premises/Azure IaaS](data-factory-onprem-sybase-connector.md)</li><li>[PostgreSQL Database On-premises/Azure IaaS](data-factory-onprem-postgresql-connector.md)</li><li>[ODBC data sources on-premises/Azure IaaS](data-factory-odbc-connector.md)</li><li>[Hadoop Distributed File System (HDFS) On-premises/Azure IaaS](data-factory-hdfs-connector.md)</li><li>[OData sources](data-factory-odata-connector.md)</li><li>[Web table (table from HTML)](data-factory-web-table-connector.md)</li><li>[GE Historian On-premises/Azure IaaS](data-factory-odbc-connector.md#ge-historian-store)</li></ul> | <ul><li>[Azure Blob](data-factory-azure-blob-connector.md)</li><li>[Azure Table](data-factory-azure-table-connector.md)</li><li>[Azure SQL Database](data-factory-azure-sql-connector.md)</li><li>[Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)</li><li>[Azure DocumentDB (see note below)](data-factory-azure-documentdb-connector.md)</li><li>[Azure Data Lake Store](data-factory-azure-datalake-connector.md)</li><li>[SQL Server On-premises/Azure IaaS](data-factory-sqlserver-connector.md)</li><li>[File System On-premises/Azure IaaS](data-factory-onprem-file-system-connector.md)</li><li>[Oracle Database On-premises/Azure IaaS](data-factory-onprem-oracle-connector.md)</li></ul> |


> [AZURE.NOTE] Copying data to/from Azure DocumentDB from/to on-premises/Azure IaaS data stores are currently not supported. The full matrix for Azure DocumentDB would be enabled shortly.

If you need to move data to/from a data store that is not supported by the **Copy Activity**, you may use the **custom activity** in Data Factory with your own logic for copying/moving the data. See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) article for details on creating and using a custom activity.

## Tutorial
For a quick tutorial on using the Copy Activity, please see [Tutorial: Use Copy Activity in an Azure Data Factory Pipeline](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  In the tutorial, you will use the Copy Activity to copy data from an Azure blob storage to an Azure SQL database.  

## <a name="copyactivity"></a>Copy Activity
Copy Activity copies data from one input dataset (**source**) to one output dataset (**sink**). Data copy is done in a batch fashion according to the schedule specified on the activity. To learn about defining activities in general, see [Understanding Pipelines & Activities](data-factory-create-pipelines.md) article.

Copy Activity provides the following capabilities:

### <a name="global"></a>Globally available data movement
Even though the Azure Data Factory itself is available only in the West US, East US, and North Europe regions, the service powering the Copy Activity is available globally in the following regions and geographies. The globally available topology ensures efficient data movement avoiding cross-region hops in most cases.

The **Data Management Gateway** or the **Azure Data Factory** performs data movement based on the location of source and destination data stores in a copy operation. See the following table for details:  

Source data store location | Destination data store location | Data movement is performed by  
-------------------------- | ------------------------------- | ----------------------------- 
on-premises/Azure VM (IaaS) | cloud |  **Data Management Gateway** on an on-premises computer/Azure VM. The data does not flow through the service in the cloud. <br/><br/>Note: The Data Management Gateway can be on the same on-premises computer/Azure VM as the data store or on a different on-premises computer/Azure VM as long as it can connect to both data stores.
cloud | on-premises/Azure VM (IaaS) |  Same as above. 
on-premises/Azure VM (IaaS) | on-premises/Azure VM | **Data Management Gateway associated with the source**. The data does not flow through the service in the cloud. See the note above.   
cloud | cloud | **The cloud service that powers the Copy Activity**. Azure Data Factory uses the deployment of this service in the region that is closest to the sink location in the same geography. Refer to the following table for mapping: <br/><br/><table><tr><th>Region of the destination data store</th> <th>Region used for data movement</th></tr><tr><td>East US</td><td>East US</td></tr><tr><td>East US 2</td><td>East US 2</td><tr/><tr><td>Central US</td><td>Central US</td><tr/><tr><td>West US</td><td>West US</td></tr><tr><td>North Central US</td><td>North Central US</td></tr><tr><td>South Central US</td><td>South Central US</td></tr><tr><td>North Europe</td><td>North Europe</td></tr><tr><td>West Europe</td><td>West Europe</td></tr><tr><td>Southeast Asia</td><td>South East Asia</td></tr><tr><td>East Asia</td><td>South East Asia</td></tr><tr><td>Japan East</td><td>Japan East</td></tr><tr><td>Japan West</td><td>Japan East</td></tr><tr><td>Brazil South</td><td>Brazil South</td></tr><tr><td>Australia East</td><td>Australia East</td></tr><tr><td>Australia Southeast</td><td>Australia Southeast</td></tr></table>


> [AZURE.NOTE] If the region of the destination data store is not in the list above, the Copy Activity will fail instead of going through an alternative region. 



### <a name="moveonpremtocloud"></a>Securely transfer data between on-premises location and cloud
One of the challenges for modern data integration is to seamlessly transfer data to and from on-premises to cloud. Data management gateway is an agent you can install on-premises to enable hybrid data pipelines. 

The data gateway provides the following capabilities: 

1.	Manage access to on-premises data stores securely.
2.	Model on-premises data stores and cloud data stores within the same data factory and move data.
3.	Have a single pane of glass for monitoring and management with visibility into gateway status with data factory cloud based dashboard.

You should treat your data source as an on-premises data source (that is behind a firewall) even when you use **ExpressRoute** and **use the gateway** to establish connectivity between the service and the data source. 

See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) for more details.


### Reliable and cost effective data movement
Copy Activity is designed to move large volumes of data in a reliable way, resistant to transient errors across a large variety of data sources. Data can be copied in a cost effective way with the option to enable compression over the wire.

### Type conversions across different type systems
Different data stores have different native type systems. Copy Activity performs automatic type conversions from source types to sink types with the following 2 step approach:

1. Convert from native source types to .NET type
2. Convert from .NET type to native sink type

You can find the mapping for a given native type system to .NET for the data store in the respective data store connector articles. You can use these mappings to determine appropriate types while creating your tables so that right conversions are performed during Copy Activity.

### Working with different file formats
Copy Activity supports a variety of file formats including binary, text, Avro, ORC, and JSON formats for file based stores. You can use the Copy Activity to convert data from one format to another. Example: text (CSV) to Avro.  If the data is unstructured, you can omit the **Structure** property in the JSON definition of the [dataset](data-factory-create-datasets.md). 

### Copy Activity properties
Properties like name, description, input and output tables, various policies etc are available for all types of activities. Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type. 

In case of Copy Activity, the **typeProperties** section varies depending on the types of sources and sinks. Click on a source/sink in the [Supported Sources/Sinks](#supported-data-stores) section to learn about type properties supported by the Copy Activity for that data store.   

Each of the data store specific page listed above documents these properties specific to the data store type. 

### Ordered copy
It is possible to run multiple copy operations one after another in a sequential/ordered manner. Say you have two copy activities in a pipeline: CopyActivity1 and CopyActivity with the following input data output datasets.   

CopyActivity1: 
Input: Dataset1
Output Dataset2

CopyActivity2: 
Inputs: Dataset2
Output: Dataset4

CopyActivity2 would run only if the CopyActivity1 has run successfully and Dataset2 is available. 

In the above example, CopyActivity2 can have a different input, say Dataset3, but you will need to specify Dataset2 also as an input to CopyActivity2 so the activity will not run until CopyActivity1 completes. For example: 

CopyActivity1: 
Input: Dataset1
Output Dataset2

CopyActivity2: 
Inputs: Dataset3, Dataset2
Output: Dataset4

When multiple inputs are specified, only the first input dataset is used for copying data but other datasets are used as dependencies. CopyActivity2 would only start executing when the following conditions are met: 

- CopyActivity2 has successfully completed and Dataset2 is available. This dataset will not be used when copying data to Dataset4. It only acts as a scheduling dependency for CopyActivity2.   
- Dataset3 is available. This dataset represents the data that is copied to the destination.  


### Copy Activity Performance & Tuning 
See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) article, which describes key factors that impact performance of data movement (Copy Activity) in Azure Data Factory. It also lists the observed performance during internal testing, and discusses various ways to optimize the performance of the Copy Activity.


## Data Factory Copy Wizard
The **Data Factory Copy Wizard** allows you to create a pipeline to copy data from supported sources to destinations without writing JSON definitions for linked services, datasets, and pipelines. To launch Copy Wizard, click **Copy data** tile on the home page of your data factory. 

![Copy data wizard](./media/data-factory-data-movement-activities/copy-data-wizard.png)

### Features

#### An intuitive and seamless wizard for copying data 
This wizard allows you to easily move data from a source to a destination in minutes with the following easy steps: 

1.	Select the **source**
2.	Select the **destination**
3.	Configure **settings**

![Select data source](./media/data-factory-data-movement-activities/select-data-source-page.png)

#### Rich data exploration and schema mappings
You can browse tables/folders, preview data, map schema, validate expressions and perform simple data transformations within the wizard. 

**Browse tables/folders**
![Browse tables and folders](./media/data-factory-data-movement-activities/browse-tables-folders.png)

#### Scalable experience for diverse data and object types
The experience is designed with big data in mind from the start. It is simple and efficient to author Data Factory pipelines that move hundreds of folders, files or tables.

**Preview data, map schema and perform simple transformations**
![File format settings](./media/data-factory-data-movement-activities/file-format-settings.png)
![Schema mapping](./media/data-factory-data-movement-activities/schema-mapping.png)
![Validate expressions](./media/data-factory-data-movement-activities/validate-expressions.png)

#### Scalable experience for diverse data and object types
The experience is designed with big data in mind from the start. Moving hundreds of folders, files or tables is as simple and efficient to author using the Copy Wizard.

![Select tables to copy data](./media/data-factory-data-movement-activities/select-tables-to-copy-data.png)

#### Richer scheduling options
You can run the copy operation just once or on a schedule (hourly, daily, etc...). Both these options can be used for the breadth of the connectors across on-premises, cloud and local desktop copy. One-time copy enables data movement from a source to a destination just once and applies to data of any size and any supported formats. The scheduled copy enables copying data on a prescribed recurrence. You can leverage rich settings (like retry, timeout, alerts etc) to configure the scheduled copy.

![Scheduling properties](./media/data-factory-data-movement-activities/scheduling-properties.png)


### Try it out 
For a quick walkthrough of using the **Data Factory Copy Wizard** to create a pipeline with a Copy Activity, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).


### Variables in Azure Blob folder path
You can use variables in folder path to copy data from a folder that is determined at runtime based on [WindowStart system variable](data-factory-functions-variables.md#data-factory-system-variables). The supported variables are: **year**, **month**, **day**, **hour**, **minute** and **{custom}**. Example: inputfolder/{year}/{month}/{day}. 

Suppose, you have input folders in the following format: 
	
	2016/03/01/01
	2016/03/01/02
	2016/03/01/03
	...

Click the **Browse** button for the **File or folder**, navigate to one of these folders, say 2016->03->01->02, and click **Choose**. You should see **2016/03/01/02** in the text box now. Now, replace **2016** with **{year}**, **03** with **{month}**, **01** with **{day}**, **02** with **{hour}**, and press **TAB**. You should see drop down lists to select **format** for these four variables as shown below: 

![Using system variables](./media/data-factory-data-movement-activities/blob-standard-variables-in-folder-path.png)   

You can also use a **custom** variable as shown below and use any [supported format strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Make sure that you select a folder with that structure using the Browse button first, replace a value with **{custom}** and press **TAB** to see the text box where you can type the format string.    

![Using custom variable](./media/data-factory-data-movement-activities/blob-custom-variables-in-folder-path.png)

