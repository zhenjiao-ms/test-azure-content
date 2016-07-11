<properties 
	pageTitle="DocumentDB scale and performance testing | Microsoft Azure" 
	description="Learn how to perform scale and performance testing with Azure DocumentDB"
	keywords="performance testing"
	services="documentdb" 
	authors="arramac" 
	manager="jhubbard" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/06/2016" 
	ms.author="arramac"/>

# Performance and scale testing with Azure DocumentDB
Performance and scale testing is a key step in application development. For many applications, the database tier has a significant impact on the overall performance and scalability, and is therefore a critical component of performance testing. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) is purpose-built for elastic scale and predictable performance, and therefore a great fit for applications that need a high-performance database tier. 

This article is a reference for developers implementing performance test suites for their DocumentDB workloads, or evaluating DocumentDB for high-performance application scenarios. It focuses primarily on isolated performance testing of the database, but also includes best practices for production applications.

After reading this article, you will be able to answer the following questions:   

- Where can I find a sample .NET client application for performance testing of Azure DocumentDB?
- What are the key factors that affect end-to-end performance of requests made to Azure DocumentDB? 
- How do I achieve high throughput levels with Azure DocumentDB from my client application?

To get started with code, please download the project from [DocumentDB Performance Testing  Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

## Key client configuration options
DocumentDB is a fast and flexible distributed database that scales seamlessly with guaranteed latency and throughput. You do not have to make major architecture changes or write complex code to scale your database tier with DocumentDB. Scaling up and down is as easy as making a single API call or SDK method call. However, when testing at scale, it is important to note that DocumentDB is accessed via network calls. If you are writing a stanadalone client application to performance test DocumentDB, you must configure it appropriate to counter the impact of network latency on your performance measurements.

In order to get the best end-to-end performance with DocumentDB, consider the following client configuration options:

- **Increase number of threads/tasks**: Since calls to DocumentDB are over the network, you may need to vary the degree of parallelism of your requests so that the client application spends very little time waiting between requests. For example, if you're using .NET's [Task Parallel Library](https://msdn.microsoft.com//library/dd460717.aspx), please create in the order of 100s of Tasks reading or writing to DocumentDB.
- **Test within the same Azure region**: When possible, test from a Virtual Machine or App Service deployed in the same Azure Region. For a ballpark comparison, calls to DocumentDB within the same region complete within 1-2 ms, but the latency between the West and East coast of the US is >50 ms.
- **Increase System.Net MaxConnections per host**: DocumentDB requests are made over HTTPS/REST by default and subject to the default connection limits per hostname or IP address. You may need to set this to a higher value (100-1000) so that the client library can utilize multiple simultaneous connections to DocumentDB. In .NET, this is [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).
- **Turn server-side GC on**: Reducing the frequency of garbage collection may help in some cases. In .NET, set [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) to true.
- **Use Direct Connectivity**: Use [Direct connectivity](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionmode.aspx) for the best performance. 
- **Implement backoff at RetryAfter intervals**: During performance testing, you should increase load until a small rate of requests get throttled. If throttled, the client application should backoff on throttle for the server-specified retry interval. This ensures that you  spend minimal amount of time waiting between retries. See [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
- **Scale out your client-workload**: If you are testing at high throughput levels (>50,000 RU/s), the client application may become the bottleneck due to the machine capping out on CPU or Network utilization. If you reach this point, you can continue to push the DocumentDB account further by scaling out your client applications across multiple servers.

## Get started
The quickest way to get started is to compile and run the .NET sample below, as described in the steps below. You can also review the source code and implement similar configurations to your own client applications.

**Step 1:** Download the project from [DocumentDB Performance Testing  Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), or fork the Github repository.

**Step 2:** Modify the settings for EndpointUrl, AuthorizationKey, CollectionThroughput and DocumentTemplate (optional) in App.config.

> [AZURE.NOTE] Before provisioning collections with high throughput, please refer to the [Pricing Page](https://azure.microsoft.com/pricing/details/documentdb/) to estimate the costs per collection. DocumentDB bills storage and throughput independently on an hourly basis, so you can save costs by deleting or lowering the throughput of your DocumentDB collections after testing.

**Step 3:** Compile and run the console app from the command line. You should see output like the following:

	Summary:
	---------------------------------------------------------------------
	Endpoint: https://docdb-scale-demo.documents.azure.com:443/
	Collection : db.testdata at 50000 request units per second
	Document Template*: Player.json
	Degree of parallelism*: 500
	---------------------------------------------------------------------

	DocumentDBBenchmark starting...
	Creating database db
	Creating collection testdata
	Creating metric collection metrics
	Retrying after sleeping for 00:03:34.1720000
	Starting Inserts with 500 tasks
	Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
	Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
	Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
	Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
	Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
	Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
	Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
	Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
	Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
	Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
	Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
	Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
	Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
	Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
	Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
	Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
	Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
	Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
	Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
	Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
	Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
	Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

	Summary:
	---------------------------------------------------------------------
	Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
	---------------------------------------------------------------------
	DocumentDBBenchmark completed successfully.


**Step 4 (if necessary):** The throughput reported (RU/s) from the tool should be the same or higher than the provisioned throughput of the collection. If not, increasing the DegreeOfParallelism in small increments may help you reach the limit. If the throughput from your client app plateaus, launching multiple instances of the app on the same or different machines will help you reach the provisioned limit across the different instances. If you need help with this step, please reach out to us via [Ask DocumentDB](askdocdb@microsoft.com) or by filing a support ticket.

Once you have the app running, you can try different [Indexing policies](documentdb-indexing-policies.md) and [Consistency levels](documentdb-consistency-levels.md) to understan their impact on throughput and latency. You can also review the source code and implement similar configurations to your own test suites or production applications.

## Summary
In this article, we looked at how you can perform performance and scale testing with DocumentDB using a .NET console app and reviewed key configuration options to get the best performance from Azure DocumentDB. Please refer to the links below for additional information on working with DocumentDB.

* [DocumentDB performance testing sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Server-side partitioning in DocumentDB](documentdb-partition-data.md)
* [DocumentDB collections and performance levels](documentdb-performance-levels.md)
* [DocumentDB .NET SDK documentation on MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET samples](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB blog on performance tips](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
