---
title: Azure Service Fabric Platform Level Monitoring 
description: Learn about platform level events and logs used to monitor and diagnose Azure Service Fabric clusters.
ms.topic: conceptual
ms.author: tomcassidy
author: tomvcassidy
ms.service: service-fabric
services: service-fabric
ms.date: 07/14/2022
---

# Monitoring the cluster

It is important to monitor at the cluster level to determine whether or not your hardware and cluster are behaving as expected. Though Service Fabric can keep applications running during a hardware failure, but you still need to diagnose whether an error is occurring in an application or in the underlying infrastructure. You also should monitor your clusters to better plan for capacity, helping in decisions about adding or removing hardware.

Service Fabric exposes several structured platform events, as [Service Fabric events](service-fabric-diagnostics-events.md), through the EventStore and various log channels out-of-the-box. 

On Windows, Service Fabric events are available from a single ETW provider with a set of relevant `logLevelKeywordFilters` used to pick between Operational and Data & Messaging channels - this is the way in which we separate out outgoing Service Fabric events to be filtered on as needed.

* **Operational**
High-level operations performed by Service Fabric and the cluster, including events for a node coming up, a new application being deployed, or an upgrade rollback, etc. See the full list of events [here](service-fabric-diagnostics-event-generation-operational.md).  

* **Operational - detailed**  
Health reports and load balancing decisions.

The operation channel can be accessed through a variety of ways including ETW/Windows Event Logs, the [EventStore](service-fabric-diagnostics-eventstore.md) (available on Windows in versions 6.2 and later for Windows clusters). The EventStore gives you access to your cluster's events on a per entity basis (entities including cluster, nodes, applications, services, partitions, replicas, and containers) and exposes them via REST APIs and the Service Fabric client library. Use the EventStore to monitor your dev/test clusters, and for getting a point-in-time understanding of the state of your production clusters.

* **Data & Messaging**  
Critical logs and events generated in the messaging (currently only the ReverseProxy) and data path (reliable services models).

* **Data & Messaging - detailed**  
Verbose channel that contains all the non-critical logs from data and messaging in the cluster (this channel has a very high volume of events).

In addition to these, there are two structured EventSource channels provided, as well as logs that we collect for support purposes.

* [Reliable Services events](service-fabric-reliable-services-diagnostics.md)  
Programming model specific events.

* [Reliable Actors events](service-fabric-reliable-actors-diagnostics.md)  
Programming model specific events and performance counters.

* Support logs  
System logs generated by Service Fabric only to be used by us when providing support.

These various channels cover most of the platform level logging that is recommended. To improve platform level logging, consider investing in better understanding the health model and adding custom health reports, and adding custom **Performance Counters** to build a real-time understanding of the impact of your services and applications on the cluster.

In order to take advantage of these logs, it is highly recommended to leave "Diagnostics" enabled during cluster creation in the Azure Portal. 
By turning on diagnostics, when the cluster is deployed, Windows Azure Diagnostics is able to acknowledge the Operational, 
Reliable Services, and Reliable actors channels, and store the data as explained further 
in [Aggregate events with Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).

## Azure Service Fabric health and load reporting

Service Fabric has its own health model, which is described in detail in these articles:

- [Introduction to Service Fabric health monitoring](service-fabric-health-introduction.md)
- [Report and check service health](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Add custom Service Fabric health reports](service-fabric-report-health.md)
- [View Service Fabric health reports](service-fabric-view-entities-aggregated-health.md)

Health monitoring is critical to multiple aspects of operating a service, especially during an application upgrade. After each upgrade domain of the service is upgraded, the upgrade domain must pass health checks before the deployment moves to the next upgrade domain. If OK health status cannot be achieved, the deployment is rolled back, so that the application remains in a known OK state. Although some customers might be affected before the services are rolled back, most customers won't experience an issue. Also, a resolution occurs relatively quickly without having to wait for action from a human operator. The more health checks that are incorporated into your code, the more resilient your service is to deployment issues.

Another aspect of service health is reporting metrics from the service. Metrics are important in Service Fabric because they are used to balance resource usage. Metrics can also be an indicator of system health. For example, you might have an application that has many services, and each instance reports a requests per second (RPS) metric. If one service is using more resources than another service, Service Fabric moves service instances around the cluster, to try to maintain even resource utilization. For a more detailed explanation of how resource utilization works, see [Manage resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).

Metrics also can help give you insight into how your service is performing. Over time, you can use metrics to check that the service is operating within expected parameters. For example, if trends show that at 9 AM on Monday morning the average RPS is 1,000, then you might set up a health report that alerts you if the RPS is below 500 or above 1,500. Everything might be perfectly fine, but it might be worth a look to be sure that your customers are having a great experience. Your service can define a set of metrics that can be reported for health check purposes, but that don't affect the resource balancing of the cluster. To do this, set the metric weight to zero. We recommend that you start all metrics with a weight of zero, and not increase the weight  until you are sure that you understand how weighting the metrics affects resource balancing for your cluster.

> [!TIP]
> Don't use too many weighted metrics. It can be difficult to understand why service instances are being moved around for balancing. A few metrics can go a long way!

Any information that can indicate the health and performance of your application is a candidate for metrics and health reports. **A CPU performance counter can tell you how your node is utilized, but it doesn't tell you whether a particular service is healthy, because multiple services might be running on a single node.** But, metrics like RPS, items processed, and request latency all can indicate the health of a specific service.

## Service Fabric support logs

If you need to contact Microsoft support for help with your Azure Service Fabric cluster, support logs are almost always required. If your cluster is hosted in Azure, support logs are automatically configured and collected as part of creating a cluster. The logs are stored in a dedicated storage account in your cluster's resource group. The storage account doesn't have a fixed name, but in the account, you see blob containers and tables with names that start with *fabric*. For information about setting up log collections for a standalone cluster, see [Create and manage a standalone Azure Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and [Configuration settings for a standalone Windows cluster](service-fabric-cluster-manifest.md). For standalone Service Fabric instances, the logs should be sent to a local file share. You are **required** to have these logs for support, but they are not intended to be usable by anyone outside of the Microsoft customer support team.

## Measuring performance

Measure performance of your cluster will help you understand how it is able to handle load and drive decisions around scaling your cluster (see more about scaling a cluster [on Azure](service-fabric-cluster-scale-in-out.md), or [on-premises](service-fabric-cluster-windows-server-add-remove-nodes.md)). Performance data is also useful when compared to actions you or your applications and services may have taken, when analyzing logs in the future. 

For a list of performance counters to collect when using Service Fabric, see [Performance Counters in Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Here are two common ways in which you can set up collecting performance data for your cluster:

* **Using an agent**  
This is the preferred way of collecting performance from a machine, since agents usually have a list of possible performance metrics that can be collected, and it is a relatively easy process to choose the metrics you want to collect or change. The Read about the Azure Monitor offering Azure Monitor logs in Service Fabric's [Azure Monitor logs integration](service-fabric-diagnostics-event-analysis-oms.md) and [Setting up the Log Analytics agent](../azure-monitor/agents/agent-windows.md) to learn more about the Log Analytics agent, which is one such monitoring agent that is able to pick up performance data for cluster VMs and deployed containers.

* **Performance counters to Azure Table Storage**  
You can also send performance metrics to the same table storage as the events. This requires changing the Azure Diagnostics configuration to pick up the appropriate performance counters from the VMs in your cluster, and enabling it to pick up docker stats if you will be deploying any containers. Read about configuring [Performance Counters in WAD](service-fabric-diagnostics-event-aggregation-wad.md) in Service Fabric to set up performance counter collection.

## Next steps

* Read about Service Fabric's [Azure Monitor logs integration](service-fabric-diagnostics-event-analysis-oms.md) to collect cluster diagnostics and create custom queries and alerts
* Learn about Service Fabric's in built diagnostic experience, the [EventStore](service-fabric-diagnostics-eventstore.md)
* Walk through some [common diagnostic scenarios](service-fabric-diagnostics-common-scenarios.md) in Service Fabric