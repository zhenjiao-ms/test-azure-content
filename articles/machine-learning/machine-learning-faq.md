<properties
	pageTitle="Azure Machine Learning FAQ | Microsoft Azure"
	description="Azure Machine Learning introduction: FAQ covering billing, capabilities, and limitations of a cloud service for streamlined predictive modeling."
	keywords="machine learning introduction,predictive modeling,what is machine learning"
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="06/13/2016"
	ms.author="garye"/>

# Azure Machine Learning Frequently Asked Questions (FAQ): Billing, capabilities, limitations, and support

This FAQ answers questions about Azure Machine Learning, a cloud service for developing predictive models and operationalizing solutions through web services. This FAQ covers questions about using the service, including the billing model, capabilities, limitations, and support.

## General questions

**What is Azure Machine Learning?**

Azure Machine Learning is a fully managed service that you can use to create, test, operate, and manage predictive analytic solutions in the cloud. With only a browser, you can sign-in, upload data, and immediately start machine learning experiments. Drag-and-drop predictive modeling, a large pallet of modules, and a library of starting templates makes common machine learning tasks simple and quick.  For more information, see the [Azure Machine Learning service overview](https://azure.microsoft.com/services/machine-learning/). For a machine learning introduction covering key terminology and concepts, see [Introduction to Azure Machine Learning](machine-learning-what-is-machine-learning.md).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**What is Machine Learning Studio?**

Machine Learning Studio is a workbench environment you access through a web browser. Machine Learning Studio hosts a pallet of modules with a visual composition interface that enables you to build an end-to-end, data science workflow in the form of an experiment.

For more information about Machine Learning Studio, see [What is Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

**What is the Machine Learning API service?**

The Machine Learning API service enables you to deploy predictive models, such as those built in Machine Learning Studio, as scalable, fault-tolerant, web services. The web services created by the Machine Learning API service are REST APIs that provide an interface for communication between external applications and your predictive analytics models.

See [Connect to a Machine Learning web service](machine-learning-connect-to-azure-machine-learning-web-service.md) for more information.


## Billing questions

**How does Machine Learning billing work?**

For billing and pricing information, see [Machine Learning Pricing](https://azure.microsoft.com/pricing/details/machine-learning/).

**Does Machine Learning have a free trial?**

 Azure Machine Learning has a free subscription option (see [Machine Learning Pricing](https://azure.microsoft.com/pricing/details/machine-learning/) for details), and Machine Learning Studio has an 8-hour quick evaluation trial available (log in to [Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2) for this trial).
 
 In addition, when you sign up for an Azure free trial, you can try any Azure services for a month. To learn more about the Azure free trial, visit [Azure Free Trial FAQ](/pricing/free-trial-faq/).

## Machine Learning Studio questions

### Creating an experiment

**Is there version control or Git integration for experiment graphs?**

No, however Machine Learning Studio retains each iteration of an experiment that cannot be modified by other users.
For more information, see [Manage experiment iterations in Machine Learning Studio](machine-learning-manage-experiment-iterations.md).

### Importing and exporting data for Machine Learning

**What data sources does Machine Learning support?**

Data can be loaded into a Machine Learning Studio experiment in one of three ways: by uploading a local file as a dataset, by using a module to import data from cloud data services, or by importing a dataset saved from another experiment. See [Import training data into Machine Learning Studio](machine-learning-data-science-import-data.md) to learn more about supported file formats.


#### <a id="ModuleLimit"></a>How large can the data set be for my modules?

Modules in Machine Learning Studio support datasets of up to 10 GB of dense numerical data for common use cases. If a module takes more than one input, the 10 GB is the total of all input sizes. You can also sample larger datasets via Hive or Azure SQL Database queries, or by Learning by Counts pre-processing, before ingestion.  

The following types of data can expand into larger datasets during feature normalization, and are limited to less than 10 GB:

- Sparse
- Categorical
- Strings
- Binary data

The following modules are limited to datasets less than 10GB:

- Recommender modules
- SMOTE module
- Scripting modules: R, Python, SQL
- Modules where the output data size can be larger than input data size, such as Join or Feature Hashing.
- Cross-validation, Tune Model Hyperparameters, Ordinal Regression and One-vs-All Multiclass, when number of iterations is very large.

For datasets larger than a few GB, you should upload data to Azure storage or Azure SQL Database or use HDInsight, rather than directly uploading from local file.


####<a id="UploadLimit"></a>What are the limits for data upload?
For datasets larger than a couple GB, upload data to Azure storage or Azure SQL Database or use HDInsight, rather than directly uploading from local file.

**Can I read data from Amazon S3?**

If you have a small amount of data and want to expose it via an http URL, then you can use the [Import Data][import-data] module. For any larger amounts of data to transfer it to Azure Storage first and then use the [Import Data][import-data] module to bring it into your experiment.
<!--
<SEE CLOUD DS PROCESS>
-->

**Is there a built-in image input capability?**

You can learn about image input capability in the [Import Images][image-reader] reference.

### Modules

**The algorithm, data source, data format, or data transformation operation I am looking for isn't in Azure Machine Learning Studio. What are my options?**

You can visit the [user feedback forum](http://go.microsoft.com/fwlink/?LinkId=404231) to see feature requests that we are tracking. Add your vote to a request if a capability you're looking for has already been requested. If the capability you're looking for doesn't exist, create a new request. You can view the status of your request in this forum too. We track this list closely and update the status of feature availability frequently. In addition, with the built-in support for R and Python custom transformations can be created as needed.


**Can I bring my existing code into Machine Learning Studio?**

Yes, you can bring your existing R or Python code into Machine Learning Studio, run it in the same experiment with Azure Machine Learning learners, and deploy the solution as a web service via Azure Machine Learning. For more information, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md) and [Execute Python machine learning scripts in Azure Machine Learning Studio](machine-learning-execute-python-scripts.md).

**Is it possible to use something like [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language)  to define a model?**

No, that is not supported, however custom R and Python code can be used to define a module.

**How many modules can I execute in parallel in my experiment?**  

You can execute up to 4 modules in parallel in an experiment.


### Data processing

**Is there an ability to visualize data (beyond R visualizations) interactively within the experiment?**

By clicking on the output of a module you can visualize the data and get statistics.

**When previewing results or data in the browser, the number of rows and columns is limited, why?**

Since the data is being transmitted to the browser and may be large, the data size is limited to prevent slowing down Machine Learning Studio. To visualize all the data/result, it's better to download the data and use Excel or another tool.

### Algorithms

**What existing algorithms are supported in Machine Learning Studio?**

Machine Learning Studio provides state of the art algorithms, such as Scalable Boosted Decision trees, Bayesian Recommendation systems, Deep Neural Networks, and Decision Jungles developed at Microsoft Research. Scalable open-source machine learning packages like Vowpal Wabbit are also included. Machine Learning Studio supports machine learning algorithms for multiclass and binary classification, regression, and clustering. See the complete list of [Machine Learning Modules][machine-learning-modules].

**Do you automatically suggest the right Machine Learning algorithm to use for my data?**

No, however there are a number of ways in Machine Learning Studio to compare the results of each algorithm to determine the right one for your problem.

**Do you have any guidelines on picking one algorithm over another for the provided algorithms?**
See [How to choose an algorithm ](machine-learning-algorithm-choice.md).

**Are the provided algorithms written in R or Python?**

No, these algorithms are mostly written in compiled languages to provide higher performance.

**Are any details of the algorithms provided?**

The documentation provides some information about the algorithms, and the parameters provided for tuning are described to optimize the algorithm for your use.  

**Is there any support for online learning?**

No, currently only programmatic retraining is supported.

**Can I visualize the layers of a Neural Net Model using the built-in Module?**

No.

**Can I create my own modules in C# or some other language?**

Currently new custom modules can only be created in R.

### R module

**What R packages are available in Machine Learning Studio?**

Machine Learning Studio supports 400+ CRAN R packages today, and here is the [current list](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) of all included packages. Also, see [Extend your experiment with R ](machine-learning-extend-your-experiment-with-r.md) to learn how to retreive this list yourself. If the package you want is not in this list, provide the name of package at [user feedback forum](http://go.microsoft.com/fwlink/?LinkId=404231).

**Is it possible to build a custom R module?**

Yes, see [Author custom R modules in Azure Machine Learning](machine-learning-custom-r-modules.md) for more information.

**Is there a REPL environment for R?**

No, there is no REPL environment for R in the studio.

### Python module

**Is it possible to build a custom Python module?**

Not currently, but you can use one or more [Execute Python Script][python] modules to get the same result.

**Is there a REPL environment for Python?**

You can use the Jupyter Notebooks in Machine Learning Studio. For more information, see [Introducing Jupyter Notebooks in Azure Machine Learning Studio] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## Web service

###Retrainining Models Programmatically

**How do I Retrain Azure Machine Learning models programmatically?**

Use the Retraining APIs. See [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) for more information. Sample code is also available in the [Microsoft Azure Maching Learning Retraining Demo](https://azuremlretrain.codeplex.com/).

### Create

**Can I deploy the model locally or in an application without an internet connection?**

No.


**Is there a baseline latency that is expected for all web services?**

See the [Azure subscription limits](../azure-subscription-service-limits.md)

### Use

**When would I want to run my predictive model as a Batch Execution service versus a Request Response service?**

The Request Response service (RRS) is a low-latency, high-scale web service that is used to provide an interface to stateless models that are created and deployed from the experimentation environment. The Batch Execution service (BES) is a service for asynchronously scoring a batch of data records. The input for BES is similar to data input used in RRS. The main difference is that BES reads a block of records from a variety of sources, such as the Blob service and Table service in Azure, Azure SQL Database, HDInsight (hive query), and HTTP sources. For more information, see [How to consume Machine Learning web services](machine-learning-consume-web-services.md).

**How do I update the model for the deployed web service?**

Updating a predictive model for an already deployed service is as simple as modifying and re-running the experiment that you used to author and save the trained model. Once you have a new version of the trained model available, Machine Learning Studio will ask you if you want to update your web service. See [Deploy a Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md) for details on how to update a deployed web service.

You can also use the Retraining APIs.
See [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) for more information. Sample code is also available in the [Microsoft Azure Maching Learning Retraining Demo](https://azuremlretrain.codeplex.com/).

**How do I monitor my web service deployed in production?**

Once a predictive model has been deployed, you can monitor it from the Azure classic portal. Each deployed service has its own dashboard where you can see monitoring information for that service. For more information on managing your deployed web services, see [Manage an Azure Machine Learning workspace](machine-learning-manage-workspace.md).

**Is there a place where I can see the output of my RRS/BES?**

For RRS, the web service response is typically where you see the result. You can also write it to Azure blob storage. For BES, the output is written to a blob by default. You can also write the output to a database or table using the [Export Data][export-data] module.

**Can I create web services only from models created in Machine Learning Studio?**

No, you can also create web services directly from Jupyter Notebooks and RStudio.

**Where can I find information about error codes?**

See [Machine Learning Module Error Codes](https://msdn.microsoft.com/library/azure/dn905910.aspx) for a list of error codes and descriptions.

## Scalability

**What is the scalability of the web service?**

Currently, the default endpoint is provisioned with 20 concurrent RRS requests per endpoint. You can scale this to 200 concurrent requests per endpoint and you can scale each web service to 10,000 endpoints per web service as described in [Scaling API endpoints](machine-learning-scaling-endpoints.md). For BES, each endpoint allows processing 40 requests at a time and additional requests beyond 40 requests are queued. These queued requests will run automatically as the queue drains.


**Are R jobs spread across nodes?**

No.  


**How much data can I use for training?**

Modules in Machine Learning Studio support datasets of up to 10 GB of dense numerical data for common use cases. If a module takes more than one input, the total size for all inputs together is 10 GB. You can also sample larger datasets via Hive or Azure SQL Database queries, or by pre-processing with [Learning with Counts][counts] modules before ingestion.  

The following types of data can expand into larger datasets during feature normalization, and are limited to less than 10 GB:

- sparse
- categorical
- strings
- binary data

The following modules are limited to datasets less than 10GB:

- Recommender modules
- SMOTE module
- Scripting modules: R, Python, SQL
- Modules where the output data size can be larger than input data size, such as Join or Feature Hashing.
- Cross-Validate, Tune Model Hyperparameters, Ordinal Regression, and One-vs-All Multiclass, when number of iterations is very large.

For datasets larger than a few GB, you should upload data to Azure storage or Azure SQL Database, or use HDInsight, rather than directly uploading from a local file.


**Are there any vector size limitations?**

Rows and columns are each limited to the .NET limitation of Max Int: 2,147,483,647.

**Can the size of the virtual machine being used to run the web service be adjusted?**

No.  

## Security and availability

**Who has access to the http endpoint for the web service by default? How do I restrict access to the endpoint?**

After a web service is deployed, a default endpoint is created for that service. The default endpoint can be called using its API Key. Additional endpoints can be added with their own keys from the Azure classic portal or programmatically using the Web Service Management APIs. Access keys are needed to make calls to the web service. For more information, see [Connect to a Machine Learning web service](machine-learning-connect-to-azure-machine-learning-web-service.md).


**What happens if my Asure storage account can't be found?**

Machine Learning Studio relies on a user supplied Azure storage account to save intermediary data when executing the workflow. This storage account is provided to Machine Learning Studio at the time a workspace is created. After the workspace is created, if the storage account is deleted and can no longer be found, the workspace will stop functioning and all experiments in that workspace will fail.

If you accidentally deleted the storage account, the only way to recover from it is to recreate the storage account with the same name in the same region as the deleted storage account. After that, re-sync the Access Key.


**What happens if my storage account access key is out of sync?**

Machine Learning Studio relies on a user supplied Azure storage account to save intermediary data when executing the workflow. This storage account is provided to Machine Learning Studio at the time a workspace is created and the Access Keys are associated with that workspace. After the workspace is created, if the Access Keys are changed, that workspace can no longer access the storage account - it will stop functioning and all experiments in that workspace will fail.

If you have changed storage account Access Keys, re-sync the Access Keys in the workspace using the Azure classic portal.  


## Azure Marketplace

See the [FAQ for publishing and using apps in the Machine Learning Marketplace](machine-learning-marketplace-faq.md).

## Support and training

**Where can I get training for Azure Machine Learning?**

[Azure Machine Learning Documentation Center](https://azure.microsoft.com/services/machine-learning/) hosts video tutorials as well as how-to guides. These step-by-step guides provide an introduction to the services and walk through the data science life cycle of importing data, cleaning data, building predictive models and deploying them in production with Azure Machine Learning.

We will be adding new material to the Machine Learning Center on an ongoing basis. You can submit requests for additional learning material on Machine Learning Center at the [user feedback forum](https://windowsazure.uservoice.com/forums/257792-machine-learning).

You can also find training at [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**How do I get support for Azure Machine Learning?**

To get technical support for Azure Machine Learning, go to [Azure Support](/support/options/) and select **Machine Learning**.

Azure Machine Learning also has a community forum on MSDN where you can ask questions related to Azure Machine Learning. The forum is monitored by the Azure Machine Learning team. Visit [Azure Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
