---
title: Monitoring Azure Table storage
description: Learn how to monitor the performance and availability of Azure Table storage. Monitor Azure Table storage data, learn about configuration, and analyze metric and log data.
author: normesta
services: storage
ms.service: azure-table-storage
ms.topic: conceptual
ms.date: 08/08/2023
ms.author: normesta
ms.reviewer: fryu
ms.devlang: csharp
ms.custom: monitoring, devx-track-csharp, devx-track-azurepowershell
---

# Monitoring Azure Table storage

When you have critical applications and business processes that rely on Azure resources, you want to monitor those resources for their availability, performance, and operation. This article describes the monitoring data that's generated by Azure Table storage and how you can use the features of Azure Monitor to analyze alerts on this data.

## Monitor overview

The **Overview** page in the Azure portal for each Table storage resource includes a brief view of the resource usage, such as requests and hourly billing. This information is useful, but only a small amount of the monitoring data is available. Some of this data is collected automatically and is available for analysis as soon as you create the resource. You can enable additional types of data collection with some configuration.

## What is Azure Monitor?
Azure Table storage creates monitoring data by using [Azure Monitor](../../azure-monitor/overview.md), which is a full stack monitoring service in Azure. Azure Monitor provides a complete set of features to monitor your Azure resources and resources in other clouds and on-premises. 

Start with the article [Monitoring Azure resources with Azure Monitor](../../azure-monitor/essentials/monitor-azure-resource.md) which describes the following:

- What is Azure Monitor?
- Costs associated with monitoring
- Monitoring data collected in Azure
- Configuring data collection
- Standard tools in Azure for analyzing and alerting on monitoring data

The following sections build on this article by describing the specific data gathered from Azure Storage. Examples show how to configure data collection and analyze this data with Azure tools.

## Monitoring data

Azure Table storage collects the same kinds of monitoring data as other Azure resources, which are described in [Monitoring data from Azure resources](../../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data). 

See [Azure Table storage monitoring data reference](monitor-table-storage-reference.md) for detailed information on the metrics and logs metrics created by Azure Table storage.

Metrics and logs in Azure Monitor support only Azure Resource Manager storage accounts. Azure Monitor doesn't support classic storage accounts. If you want to use metrics or logs on a classic storage account, you need to migrate to an Azure Resource Manager storage account. See [Migrate to Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-overview.md).

You can continue using classic metrics and logs if you want to. In fact, classic metrics and logs are available in parallel with metrics and logs in Azure Monitor. The support remains in place until Azure Storage ends the service on legacy metrics and logs.

## Collection and routing

Platform metrics and the Activity log are collected automatically, but can be routed to other locations by using a diagnostic setting. 

Resource Logs are not collected and stored until you create a diagnostic setting and route them to one or more locations.

To collect resource logs, you must create a diagnostic setting. When you create the setting, choose **table** as the type of storage that you want to enable logs for. Then, specify one of the following categories of operations for which you want to collect logs. 

| Category | Description |
|:---|:---|
| StorageRead | Read operations on objects. |
| StorageWrite | Write operations on objects. |
| StorageDelete | Delete operations on objects. |

The **audit** resource log category group allows you to collect the baseline of resource logs that Microsoft deems necessary for auditing your resource. What's collected is dynamic, and Microsoft may change it over time as new resource log categories become available. If you choose the **audit** category group, you can't specify any other resource categories, because the system will decide which logs to collect. For more information, see [Diagnostic settings in Azure Monitor: Resource logs](../../azure-monitor/essentials/diagnostic-settings.md#resource-logs).

See [Create diagnostic setting to collect platform logs and metrics in Azure](../../azure-monitor/platform/diagnostic-settings.md) for the detailed process for creating a diagnostic setting using the Azure portal, CLI, and PowerShell. You can also find links to information about how to create a diagnostic setting by using an Azure Resource Manager template or an Azure Policy definition.

## Destination limitations

For general destination limitations, see [Destination limitations](../../azure-monitor/essentials/diagnostic-settings.md#destination-limitations). The following limitations apply only to monitoring Azure Storage accounts.

- You can't send logs to the same storage account that you are monitoring with this setting. 

  This would lead to recursive logs in which a log entry describes the writing of another log entry. You must create an account or use another existing account to store log information.

- You can't set a retention policy. 

  If you archive logs to a storage account, you can manage the retention policy of a log container by defining a lifecycle management policy. To learn how, see [Optimize costs by automating Azure Blob Storage access tiers](../blobs/lifecycle-management-overview.md).

  If you send logs to Log Analytics, you can manage the data retention period of Log Analytics at the workspace level or even specify different retention settings by data type. To learn how, see [Change the data retention period](../../azure-monitor/logs/data-retention-archive.md).

## Analyzing metrics

For a list of all Azure Monitor support metrics, which includes Azure Table storage, see [Azure Monitor supported metrics](../../azure-monitor/essentials/metrics-supported.md).

### [Azure portal](#tab/azure-portal)

You can analyze metrics for Azure Storage with metrics from other Azure services by using Metrics Explorer. Open Metrics Explorer by choosing **Metrics** from the **Azure Monitor** menu. For details on using this tool, see [Analyze metrics with Azure Monitor metrics explorer](../../azure-monitor/essentials/analyze-metrics.md). 

This example shows how to view **Transactions** at the account level.

![Screenshot of accessing metrics in the Azure portal](./media/monitor-table-storage/access-metrics-portal.png)

For metrics that support dimensions, you can filter the metric with the desired dimension value. This example shows how to view **Transactions** at the account level on a specific operation by selecting values for the **API Name** dimension.

![Screenshot of accessing metrics with dimension in the Azure portal](./media/monitor-table-storage/access-metrics-portal-with-dimension.png)

For a complete list of the dimensions that Azure Storage supports, see [Metrics dimensions](monitor-table-storage-reference.md#metrics-dimensions).

Metrics for Azure Table storage are in these namespaces: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/tableServices

### [PowerShell](#tab/azure-powershell)

#### List the metric definition

You can list the metric definition of your storage account or the Table storage service. Use the [Get-AzMetricDefinition](/powershell/module/az.monitor/get-azmetricdefinition) cmdlet.

In this example, replace the `<resource-ID>` placeholder with the resource ID of the entire storage account or the resource ID of the Table storage service.  You can find these resource IDs on the **Properties** pages of your storage account in the Azure portal.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### Reading metric values

You can read account-level metric values of your storage account or the Table storage service. Use the [Get-AzMetric](/powershell/module/Az.Monitor/Get-AzMetric) cmdlet.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

#### Reading metric values with dimensions

When a metric supports dimensions, you can read metric values and filter them by using dimension values. Use the [Get-AzMetric](/powershell/module/Az.Monitor/Get-AzMetric) cmdlet.

```powershell
$resourceId = "<resource-ID>"
$dimFilter = [String](New-AzMetricFilter -Dimension ApiName -Operator eq -Value "QueryEntities" 3> $null)
Get-AzMetric -ResourceId $resourceId -MetricName Transactions -TimeGrain 01:00:00 -MetricFilter $dimFilter -AggregationType "Total"
```

### [Azure CLI](#tab/azure-cli)

#### List the account-level metric definition

You can list the metric definition of your storage account or the Table storage service. Use the [az monitor metrics list-definitions](/cli/azure/monitor/metrics#az-monitor-metrics-list-definitions) command.
 
In this example, replace the `<resource-ID>` placeholder with the resource ID of the entire storage account or the resource ID of the Table storage service. You can find these resource IDs on the **Properties** pages of your storage account in the Azure portal.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### Read account-level metric values

You can read the metric values of your storage account or the Table storage service. Use the [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) command.

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

#### Reading metric values with dimensions

When a metric supports dimensions, you can read metric values and filter them by using dimension values. Use the [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) command.

```azurecli
az monitor metrics list --resource <resource-ID> --metric "Transactions" --interval PT1H --filter "ApiName eq 'QueryEntities' " --aggregation "Total" 
```

### [.NET](#tab/dotnet)

Azure Monitor provides the [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) to read metric definition and values. The [sample code](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) shows how to use the SDK with different parameters. You need to use `0.18.0-preview` or a later version for storage metrics.
 
In these examples, replace the `<resource-ID>` placeholder with the resource ID of the entire storage account or the Table storage service. You can find these resource IDs on the **Properties** pages of your storage account in the Azure portal.

Replace the `<subscription-ID>` variable with the ID of your subscription. For guidance on how to obtain values for `<tenant-ID>`, `<application-ID>`, and `<AccessKey>`, see [Use the portal to create a Microsoft Entra application and service principal that can access resources](../../active-directory/develop/howto-create-service-principal-portal.md). 

#### List the account-level metric definition

The following example shows how to list a metric definition at the account level:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### Reading account-level metric values

The following example shows how to read `UsedCapacity` data at the account level:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### Reading multidimensional metric values

For multidimensional metrics, you need to define metadata filters if you want to read metric data on specific dimension values.

The following example shows how to read metric data on the metric supporting multidimension:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for table storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

---

## Analyzing logs

You can access resource logs either as a blob in a storage account, as event data, or through Log Analytic queries. For information about how to find those logs, see [Azure resource logs](../../azure-monitor/essentials/resource-logs.md).

All resource logs in Azure Monitor have the same fields followed by service-specific fields. The common schema is outlined in [Azure Monitor resource log schema](../../azure-monitor/essentials/resource-logs-schema.md). The schema for Azure Table Storage resource logs is found in [Azure Table storage monitoring data reference](monitor-table-storage-reference.md).

To get the list of SMB and REST operations that are logged, see [Storage logged operations and status messages](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

Log entries are created only if there are requests made against the service endpoint. For example, if a storage account has activity in its file endpoint but not in its table or queue endpoints, only logs that pertain to the Azure Blob Storage service are created. Azure Storage logs contain detailed information about successful and failed requests to a storage service. This information can be used to monitor individual requests and to diagnose issues with a storage service. Requests are logged on a best-effort basis.

The [Activity log](../../azure-monitor/essentials/activity-log.md) is a type of platform log located in Azure that provides insight into subscription-level events. You can view it independently or route it to Azure Monitor Logs, where you can do much more complex queries using Log Analytics. 

### Log authenticated requests

 The following types of authenticated requests are logged:

- Successful requests
- Failed requests, including timeout, throttling, network, authorization, and other errors
- Requests that use a shared access signature (SAS) or OAuth, including failed and successful requests
- Requests to analytics data (classic log data in the **$logs** container and class metric data in the **$metric** tables)

Requests made by the Table storage service itself, such as log creation or deletion, aren't logged. For a full list of the logged data, see [Storage logged operations and status messages](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and [Storage log format](monitor-table-storage-reference.md).

### Log anonymous requests

 The following types of anonymous requests are logged:

- Successful requests
- Server errors
- Time out errors for both client and server
- Failed GET requests with the error code 304 (Not Modified)

All other failed anonymous requests aren't logged. For a full list of the logged data, see [Storage logged operations and status messages](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and [Storage log format](monitor-table-storage-reference.md).

### Sample Kusto queries

If you send logs to Log Analytics, you can access those logs by using Azure Monitor log queries. For more information, see [Log Analytics tutorial](../../azure-monitor/logs/log-analytics-tutorial.md).

Here are some queries that you can enter in the **Log search** bar to help you monitor your Blob storage. These queries work with the [new language](../../azure-monitor/logs/log-query-overview.md).

> [!IMPORTANT]
> When you select **Logs** from the storage account resource group menu, Log Analytics is opened with the query scope set to the current resource group. This means that log queries will only include data from that resource group. If you want to run a query that includes data from other resources or data from other Azure services, select **Logs** from the **Azure Monitor** menu. See [Log query scope and time range in Azure Monitor Log Analytics](../../azure-monitor/logs/scope.md) for details.

Use these queries to help you monitor your Azure Storage accounts:

* To list the 10 most common errors over the last three days.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* To list the top 10 operations that caused the most errors over the last three days.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* To list the top 10 operations with the longest end-to-end latency over the last three days.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* To list all operations that caused server-side throttling errors over the last three days.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* To list all requests with anonymous access over the last three days.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* To create a pie chart of operations used over the last three days.
    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart

    ```

## Alerts

Azure Monitor alerts proactively notify you when important conditions are found in your monitoring data. They allow you to identify and address issues in your system before your customers notice them. You can set alerts on [metrics](../../azure-monitor/alerts/alerts-metric-overview.md), [logs](../../azure-monitor/alerts/alerts-unified-log.md), and the [activity log](../../azure-monitor/alerts/activity-log-alerts.md). 

The following table lists some example scenarios to monitor and the proper metric to use for the alert:

| Scenario | Metric to use for alert |
|-|-|
| Table Storage service is throttled. | Metric: Transactions<br>Dimension name: Response type |
| Table Storage requests are successful 99% of the time. | Metric: Availability<br>Dimension names: Geo type, API name, Authentication |
| Table Storage egress has exceeded 500 GiB in one day. | Metric: Egress<br>Dimension names: Geo type, API name, Authentication |

## FAQ

**Does Azure Storage support metrics for Managed Disks or Unmanaged Disks?**

No. Azure Compute supports the metrics on disks. For more information, see [Per disk metrics for Managed and Unmanaged Disks](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## Next steps

| Guide | Description |
|---|---|
| [Monitor, diagnose, and troubleshoot your Azure Storage](/training/modules/monitor-diagnose-and-troubleshoot-azure-storage/) | Troubleshoot storage account issues (contains step-by-step guidance). |
| [Monitor storage with Azure Monitor Storage insights](../common/storage-insights-overview.md) | A unified view of storage performance, capacity, and availability |
| [Getting started with Azure Metrics Explorer](../../azure-monitor/essentials/metrics-getting-started.md) | A tour of Metrics Explorer. 
| [Overview of Log Analytics in Azure Monitor](../../azure-monitor/logs/log-analytics-overview.md) | A tour of Log Analytics. |
| [Azure Monitor Metrics overview](../../azure-monitor/essentials/data-platform-metrics.md) | The basics of metrics and metric dimensions  |
| [Azure Monitor Logs overview](../../azure-monitor/logs/data-platform-logs.md)| The basics of logs and how to collect and analyze them |
| [Transition to metrics in Azure Monitor](../common/storage-metrics-migration.md) | Move from Storage Analytics metrics to metrics in Azure Monitor. |
| [Azure Table storage monitoring data reference](monitor-table-storage-reference.md)| A reference of the logs and metrics created by Azure Table Storage |
| [Troubleshoot performance issues](../common/troubleshoot-storage-performance.md?toc=/azure/storage/tables/toc.json)| Common performance issues and guidance about how to troubleshoot them. |
| [Troubleshoot availability issues](../common/troubleshoot-storage-availability.md?toc=/azure/storage/tables/toc.json)| Common availability issues and guidance about how to troubleshoot them.|
| [Troubleshoot client application errors](../common/troubleshoot-storage-client-application-errors.md?toc=/azure/storage/tables/toc.json)| Common issues with connecting clients and how to troubleshoot them.|