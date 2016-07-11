<properties
   pageTitle="Load balance an Azure Container Service cluster | Microsoft Azure"
   description="Load balance an Azure Container Service cluster."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/18/2016"
   ms.author="rogardle"/>

# Load balance an Azure Container Service cluster

In this article, we'll set up a web front end, which you can scale up to deliver services behind Azure Load Balancer.

## Prerequisites

[Deploy an instance of Azure Container Service](container-service-deployment.md) with orchestrator type DC/OS and [ensure that your client can connect to your cluster](container-service-connect.md). Also, do the following steps.
[AZURE.INCLUDE [install the DC/OS command-line interface (CLI)](../../includes/container-service-install-dcos-cli-include.md)]


## Load balancing

There are two load-balancing layers in a Container Service cluster: Azure Load Balancer for the public entry points (the ones that end users will hit), and the underlying Marathon Load Balancer (marathon-lb) that routes inbound requests to container instances that service requests. As we scale the containers that provide the service, marathon-lb dynamically adapts.

## Marathon Load Balancer

Marathon Load Balancer dynamically reconfigures itself based on the containers that you've deployed. It's also resilient to the loss of a container or an agent--if this occurs, Mesos will simply restart the container elsewhere and reconfigure Marathon Load Balancer.

To install Marathon Load Balancer, run the following command from your client machine:

```bash
dcos package install marathon-lb
```

Now that we have the marathon-lb package, we can deploy a simple web server by using the following configuration:


```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "tutum/hello-world",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

The key parts of this are:
  * Set the value of HAProxy_0_VHOST to the FQDN of the load balancer for your agents. This is in the form `<acsName>agents.<region>.cloudapp.azure.com`. For example, if you create a Container Service cluster with name `myacs` in region `West US`, the FQDN would be `myacsagents.westus.cloudapp.azure.com`. You can also find this by looking for the load balancer with "agent" in the name when you're looking through the resources in the resource group that you created for Container Service in the [Azure portal](https://portal.azure.com).
  * Set the servicePort to a port >= 10,000. This identifies the service that is being run in this container--marathon-lb uses this to identify services that it should balance across.
  * Set the HAPROXY_GROUP label to "external".
  * Set hostPort to 0. This means that Marathon will arbitrarily allocate an available port.

Copy this JSON into a file called `hello-web.json`, and use it to deploy a container:

```bash
dcos marathon app add hello-web.json
```

## Azure Load Balancer

By default, Azure Load Balancer exposes ports 80, 8080, and 443. If you're using one of these three ports (as we do in the above example), then there is nothing you need to do. You should be able to hit your agent load balancer's FQDN--and each time you refresh, you'll hit one of your three web servers in a round-robin fashion. However, if you use a different port, you need to add a round-robin rule and a probe on the load balancer for the port that you used. You can do this from the [Azure CLI](../xplat-cli-azure-resource-manager.md), with the commands `azure lb rule create` and `azure lb probe create`.


## Additional scenarios

You could have a scenario where you use different domains to expose different services. For example:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

To achieve this, check out [virtual hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), which provide a way to associate domains to specific marathon-lb paths.

Alternatively, you could expose different ports and remap them to the correct service behind marathon-lb. For example:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## Next steps

Check out this [load balancing blog post](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/) for more information about Marathon Load Balancer.
