---
title: What's new in Azure Communications Gateway?
description: Discover what's new in Azure Communications Gateway for Operator Connect, Teams Phone Mobile and Microsoft Teams Direct Routing. Learn how to get started with the latest features.
author: rcdun
ms.author: rdunstan
ms.service: communications-gateway
ms.topic: whats-new
ms.date: 11/06/2023
---

# What's new in Azure Communications Gateway?

This article covers new features and improvements for Azure Communications Gateway.

## November 2023

### Support for Zoom Phone Cloud Peering

From November 2023, Azure Communications Gateway supports providing PSTN connectivity to Zoom with Zoom Phone Cloud Peering. You can provide Zoom Phone calling services to many customers, each with many users, with minimal disruption to your existing network.

For more information about Zoom Phone Cloud Peering with Azure Communications Gateway, see [Overview of interoperability of Azure Communications Gateway with Zoom Phone Cloud Peering](interoperability-zoom.md). For an overview of deploying and configuring Azure Communications Gateway for Zoom, see [Get started with Azure Communications Gateway](get-started.md).

## October 2023

### Support for multitenant Microsoft Teams Direct Routing

From October 2023, Azure Communications Gateway supports providing PSTN connectivity to Microsoft Teams through Direct Routing. You can provide Microsoft Teams calling services to many customers, each with many users, with minimal disruption to your existing network. Azure Communications Gateway automatically updates the SIP signaling to indicate the correct tenant, without needing changes to your core network to map between numbers and customer tenants.

Azure Communications Gateway can screen Direct Routing calls originating from Microsoft Teams to ensure that the number is enabled for Direct Routing. This screening reduces the risk of caller ID spoofing, because it prevents customer administrators assigning numbers that you haven't allocated to the customer.

For more information about Direct Routing with Azure Communications Gateway, see [Overview of interoperability of Azure Communications Gateway with Microsoft Teams Direct Routing](interoperability-teams-direct-routing.md). For an overview of deploying and configuring Azure Communications Gateway for Direct Routing, see [Get started with Azure Communications Gateway](get-started.md).

## September 2023

### ExpressRoute Microsoft Peering between Azure and operator networks

From September 2023, you can use ExpressRoute Microsoft Peering to connect operator networks to Azure Communications Gateway as an alternative to Peering Services Voice (also known as MAPS for voice). We recommend that most deployments use MAPS for voice unless there's a specific reason that ExpressRoute Microsoft Peering is preferable. For example, you might have existing ExpressRoute connectivity to your network that you can reuse. For details and examples of when ExpressRoute might be preferable to MAPS, see [Using ExpressRoute for Microsoft PSTN Services](../../articles/expressroute/using-expressroute-for-microsoft-pstn.md).

## May 2023

### Integrated Mobile Control Point for Teams Phone Mobile integration

From May 2023, you can deploy Mobile Control Point (MCP) as part of Azure Communications Gateway. MCP is an IMS Application Server that simplifies interworking with Microsoft Phone System for mobile calls. It ensures calls are only routed to the Microsoft Phone System when a user is eligible for Teams Phone Mobile services. This process minimizes the changes you need in your mobile network to route calls into Microsoft Teams. For more information, see [Mobile Control Point in Azure Communications Gateway for Teams Phone Mobile](mobile-control-point.md).

You can add MCP when you deploy Azure Communications Gateway or by requesting changes to an existing deployment. For more information, see [Deploy Azure Communications Gateway](deploy.md) or [Get support or request changes to your Azure Communications Gateway](request-changes.md)

### Authentication with managed identities for Operator Connect APIs

Azure Communications Gateway contains services that need to access the Operator Connect APIs on your behalf. Azure Communications Gateway therefore needs to authenticate with your Operator Connect or Teams Phone Mobile environment.

From May 2023, Azure Communications Gateway automatically provides a managed identity for this authentication. You must set up specific permissions for this managed identity and then add the Application ID of this managed identity to your Operator Connect or Teams Phone Mobile environment. For more information, see [Deploy Azure Communications Gateway](deploy.md).

This new authentication model replaces an earlier model that required you to create an App registration and manage secrets for it. With the new model, you no longer need to create, manage or rotate secrets.

## Next steps

- [Learn more about Azure Communications Gateway](overview.md).
- [Get started with Azure Communications Gateway](get-started.md).