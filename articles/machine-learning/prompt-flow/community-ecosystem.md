---
title: Prompt flow ecosystem (preview)
titleSuffix: Azure Machine Learning
description: Introduction to the Prompt flow ecosystem, which includes the Prompt flow open source project, tutorials, SDK, CLI and VS Code extension.
services: machine-learning
ms.service: machine-learning
ms.subservice: prompt-flow
ms.topic: conceptual
author: jiaochenlu
ms.author: chenlujiao
ms.reviewer: lagayhar
ms.date: 09/12/2023
---

# Prompt flow ecosystem (preview)

The Prompt flow ecosystem aims to provide a comprehensive set of tutorials, tools and resources for developers who want to leverage the power of Prompt flow to experimentally tune their prompts and develop their LLM-based application in pure local environment, without any dependencies on Azure resources binding. This article provides an overview of the key components within the ecosystem, which include:
 - **Prompt flow open source project** in GitHub.
 - **Prompt flow SDK and CLI** for seamless flow execution and integration with CI/CD pipeline.
 - **VS Code extension** for convenient flow authoring and development within a local environment.

> [!IMPORTANT]
> Prompt flow is currently in public preview. This preview is provided without a service-level agreement, and are not recommended for production workloads. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## Prompt flow SDK/CLI

The Prompt flow SDK/CLI empowers developers to use code manage credentials, initialize flows, develop flows, and execute batch testing and evaluation of prompt flows locally.

It's designed for efficiency, allowing simultaneous trigger of large dataset-based flow tests and metric evaluations. Additionally, the SDK/CLI can be easily integrated into your CI/CD pipeline, automating the testing process.

To get started with the Prompt flow SDK, explore and follow the [SDK quick start notebook](https://github.com/microsoft/promptflow/blob/main/examples/tutorials/get-started/quickstart.ipynb) in steps.

## VS Code extension

The ecosystem also provides a powerful VS Code extension designed for enabling you to easily and interactively develop prompt flows, fine-tune your prompts, and test them with a user-friendly UI.

:::image type="content" source="./media/community-ecosystem/prompt-flow-vs-code-extension-flatten.png" alt-text="Screenshot of the Prompt flow extension in the VS Code showing the UI. "lightbox = "./media/community-ecosystem/prompt-flow-vs-code-extension-flatten.png":::

To get started with the Prompt flow VS Code extension, navigate to the extension marketplace to install and read the details tab.

:::image type="content" source="./media/community-ecosystem/prompt-flow-vs-code-extension.png" alt-text="Screenshot of the Prompt flow extension in the VS Code marketplace. "lightbox = "./media/community-ecosystem/prompt-flow-vs-code-extension.png":::

## Transition to production in cloud

After successful development and testing of your prompt flow within our community ecosystem, the subsequent step you're considering might involve transitioning to a production-grade LLM application. We recommend Azure Machine Learning for this phase to ensure security, efficiency, and scalability.

You can seamlessly shift your local flow to your Azure resource to leverage large-scale execution and management in the cloud. To achieve this, see [Integration with LLMOps](how-to-integrate-with-llm-app-devops.md#go-back-to-studio-ui-for-continuous-development).

## Community support

The community ecosystem thrives on collaboration and support. Join the active community forums to connect with fellow developers, and contribute to the growth of the ecosystem.

[GitHub Repository: promptflow](https://github.com/microsoft/promptflow)

For questions or feedback, you can [open GitHub issue directly](https://github.com/microsoft/promptflow/issues/new) or reach out to pf-feedback@microsoft.com.


## Next steps

The prompt flow community ecosystem empowers developers to build interactive and dynamic prompts with ease. By using the Prompt flow SDK and the VS Code extension, you can create compelling user experiences and fine-tune your prompts in a local environment.

- Join the [Prompt flow community on GitHub](https://github.com/microsoft/promptflow).
