---
title: Estimating Consumption plan costs in Azure Functions
description: Learn how to better estimate the costs that you may incur when running your function app in a Consumption plan in Azure.
ms.date: 9/20/2019
ms.topic: conceptual
# Customer intent: As a cloud developer, I want to understand the overall costs of running my code in Azure Functions so that I can make better architectural and business decisions.
---

# Estimating Consumption plan costs

There are currently three types of hosting plans for an app that runs in Azure Functions, with each plan having its own pricing model: 

| Plan | Description |
| ---- | ----------- |
| [**Consumption**](consumption-plan.md) | You're only charged for the time that your function app runs. This plan includes a [free grant][pricing page] on a per subscription basis.|
| [**Premium**](functions-premium-plan.md) | Provides you with the same features and scaling mechanism as the Consumption plan, but with enhanced performance and VNET access. Cost is based on your chosen pricing tier. To learn more, see [Azure Functions Premium plan](functions-premium-plan.md). |
| [**Dedicated (App Service)**](dedicated-plan.md) <br/>(basic tier or higher) | When you need to run in dedicated VMs or in isolation, use custom images, or want to use your excess App Service plan capacity. Uses [regular App Service plan billing](https://azure.microsoft.com/pricing/details/app-service/). Cost is based on your chosen pricing tier.|

You chose the plan that best supports your function performance and cost requirements. To learn more, see [Azure Functions scale and hosting](functions-scale.md).

This article deals only with the Consumption plan, since this plan results in variable costs. This article supersedes the [Consumption plan cost billing FAQ](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ) article.

Durable Functions can also run in a Consumption plan. To learn more about the cost considerations when using Durable Functions, see [Durable Functions billing](./durable/durable-functions-billing.md).

## Consumption plan costs

The execution *cost* of a single function execution is measured in *GB-seconds*. Execution cost is calculated by combining its memory usage with its execution time. A function that runs for longer costs more, as does a function that consumes more memory. 

Consider a case where the amount of memory used by the function stays constant. In this case, calculating the cost is simple multiplication. For example, say that your function consumed 0.5 GB for 3 seconds. Then the execution cost is `0.5GB * 3s = 1.5 GB-seconds`. 

Since memory usage changes over time, the calculation is essentially the integral of memory usage over time.  The system does this calculation by sampling the memory usage of the process (along with child processes) at regular intervals. As mentioned on the [pricing page], memory usage is rounded up to the nearest 128-MB bucket. When your process is using 160 MB, you're charged for 256 MB. The calculation takes into account concurrency, which is multiple concurrent function executions in the same process.

> [!NOTE]
> While CPU usage isn't directly considered in execution cost, it can have an impact on the cost when it affects the execution time of the function.

For an HTTP-triggered function, when an error occurs before your function code begins to execute you aren't charged for an execution. This means that 401 responses from the platform due to API key validation or the App Service Authentication / Authorization feature don't count against your execution cost. Similarly, 5xx status code responses aren't counted when they occur in the platform prior to a function processing the request. A 5xx response generated by the platform after your function code has started to execute is still counted as an execution, even if the error isn't raised by your function code.

## Other related costs

When estimating the overall cost of running your functions in any plan, remember that the Functions runtime uses several other Azure services, which are each billed separately. When calculating pricing for function apps, any triggers and bindings you have that integrate with other Azure services require you to create and pay for those additional services. 

For functions running in a Consumption plan, the total cost is the execution cost of your functions, plus the cost of bandwidth and additional services. 

When estimating the overall costs of your function app and related services, use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/?service=functions). 

| Related cost | Description |
| ------------ | ----------- |
| **Storage account** | Each function app requires that you have an associated General Purpose [Azure Storage account](../storage/common/storage-introduction.md#types-of-storage-accounts), which is [billed separately](https://azure.microsoft.com/pricing/details/storage/). This account is used internally by the Functions runtime, but you can also use it for Storage triggers and bindings. If you don't have a storage account, one is created for you when the function app is created. To learn more, see [Storage account requirements](storage-considerations.md#storage-account-requirements).|
| **Application Insights** | Functions relies on [Application Insights](../azure-monitor/app/app-insights-overview.md) to provide a high-performance monitoring experience for your function apps. While not required, you should [enable Application Insights integration](configure-monitoring.md#enable-application-insights-integration). A free grant of telemetry data is included every month. To learn more, see [the Azure Monitor pricing page](https://azure.microsoft.com/pricing/details/monitor/). |
| **Network bandwidth** | You can incur costs for data transfer depending on the direction and scenario of the data movement. To learn more, see [Bandwidth pricing details](https://azure.microsoft.com/pricing/details/bandwidth/). |

## Behaviors affecting execution time

The following behaviors of your functions can impact the execution time:

+ **Triggers and bindings**: The time taken to read input from and write output to your [function bindings](functions-triggers-bindings.md) is counted as execution time. For example, when your function uses an output binding to write a message to an Azure storage queue, your execution time includes the time taken to write the message to the queue, which is included in the calculation of the function cost. 

+ **Asynchronous execution**: The time that your function waits for the results of an async request (`await` in C#) is counted as execution time. The GB-second calculation is based on the start and end time of the function and the memory usage over that period. What is happening over that time in terms of CPU activity isn't factored into the calculation. You may be able to reduce costs during asynchronous operations by using [Durable Functions](durable/durable-functions-overview.md). You're not billed for time spent at awaits in orchestrator functions.

## Viewing cost-related data

In [your invoice](../cost-management-billing/understand/download-azure-invoice.md), you can view the cost-related data of **Total Executions - Functions** and **Execution Time - Functions**, along with the actual billed costs. However, this invoice data is a monthly aggregate for a past invoice period. 

### Function app-level metrics

To better understand the cost impact of your functions, you can use Azure Monitor to view cost-related metrics currently being generated by your function apps. 

[!INCLUDE [functions-monitor-metrics-consumption](../../includes/functions-monitor-metrics-consumption.md)]

### Function-level metrics

Function execution units are a combination of execution time and your memory usage, which makes it a difficult metric for understanding memory usage. Memory data isn't a metric currently available through Azure Monitor. However, if you want to optimize the memory usage of your app, can use the performance counter data collected by Application Insights.  

If you haven't already done so, [enable Application Insights in your function app](configure-monitoring.md#enable-application-insights-integration). With this integration enabled, you can [query this telemetry data in the portal](analyze-telemetry-data.md#query-telemetry-data). 

You can use either [Azure Monitor metrics explorer](../azure-monitor/essentials/metrics-getting-started.md) in the [Azure portal] or REST APIs to get Monitor Metrics data.

[!INCLUDE [functions-consumption-metrics-queries](../../includes/functions-consumption-metrics-queries.md)]

## Next steps

> [!div class="nextstepaction"]
> [Learn more about Monitoring function apps](functions-monitoring.md)

[pricing page]:https://azure.microsoft.com/pricing/details/functions/
[Azure portal]: https://portal.azure.com
