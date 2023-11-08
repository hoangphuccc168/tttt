---
title: "NGINX HTTP Server connector for Microsoft Sentinel"
description: "Learn how to install the connector NGINX HTTP Server to connect your data source to Microsoft Sentinel."
author: cwatson-cat
ms.topic: how-to
ms.date: 05/22/2023
ms.service: microsoft-sentinel
ms.author: cwatson
---

# NGINX HTTP Server connector for Microsoft Sentinel

The NGINX HTTP Server data connector provides the capability to ingest [NGINX](https://nginx.org/en/) HTTP Server events into Microsoft Sentinel. Refer to [NGINX Logs documentation](https://nginx.org/en/docs/http/ngx_http_log_module.html) for more information.

## Connector attributes

| Connector attribute | Description |
| --- | --- |
| **Log Analytics table(s)** | NGINX_CL<br/> |
| **Data collection rules support** | Not currently supported |
| **Supported by** | [Microsoft Corporation](https://support.microsoft.com) |

## Query samples

**Top 10 Clients (Source IP)**
   ```kusto
NGINXHTTPServer
 
   | summarize count() by SrcIpAddr
 
   | top 10 by count_
   ```



## Vendor installation instructions


**NOTE:** This data connector depends on a parser based on a Kusto Function to work as expected which is deployed as part of the solution. To view the function code in Log Analytics, open Log Analytics/Microsoft Sentinel Logs blade, click Functions and search for the alias NGINXHTTPServer and load the function code or click [here](https://github.com/Azure/Azure-Sentinel/blob/master/Solutions/NGINX%20HTTP%20Server/Parsers/NGINXHTTPServer.txt).The function usually takes 10-15 minutes to activate after solution installation/update.

1. Install and onboard the agent for Linux or Windows

Install the agent on the NGINX HTTP Server where the logs are generated.

> Logs from NGINX HTTP Server deployed on Linux or Windows servers are collected by **Linux** or **Windows** agents.




2. Configure the logs to be collected

Configure the custom log directory to be collected



1. Select the link above to open your workspace advanced settings 
2. From the left pane, select **Data**, select **Custom Logs** and click **Add+**
3. Click **Browse** to upload a sample of a NGINX HTTP Server log file (e.g. access.log or error.log). Then, click **Next >**
4. Select **New line** as the record delimiter and click **Next >**
5. Select **Windows** or **Linux** and enter the path to NGINX HTTP logs based on your configuration. Example: 
 - **Linux** Directory:  '/var/log/nginx/*.log' 
6. After entering the path, click the '+' symbol to apply, then click **Next >** 
7. Add **NGINX_CL** as the custom log Name and click **Done**



## Next steps

For more information, go to the [related solution](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azuresentinel.azure-sentinel-solution-nginx?tab=Overview) in the Azure Marketplace.