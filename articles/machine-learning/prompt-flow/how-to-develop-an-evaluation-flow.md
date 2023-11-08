---
title: Develop an evaluation flow in Prompt flow (preview)
titleSuffix: Azure Machine Learning
description: Learn how to customize or create your own evaluation flow tailored to your tasks and objectives, and then use in a batch run as an evaluation method in Prompt flow with Azure Machine Learning studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: ZikeiWong
ms.author: ziqiwang
ms.reviewer: lagayhar
ms.date: 09/12/2023
---

# Develop an evaluation flow (preview)

Evaluation flows are special types of flows that assess how well the outputs of a run align with specific criteria and goals.

In Prompt flow, you can customize or create your own evaluation flow tailored to your tasks and objectives, and then use in a batch run as an evaluation method. This document you'll learn:

- How to develop an evaluation method
  - Customize built-in evaluation Method
  - Create new evaluation Flow from Scratch
- Understand evaluation in Prompt flow
  - Inputs
  - Outputs and Metrics Logging

> [!IMPORTANT]
> Prompt flow is currently in public preview. This preview is provided without a service-level agreement, and is not recommended for production workloads. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## Starting to develop an evaluation method

There are two ways to develop your own evaluation methods:

- **Customize a Built-in Evaluation Flow:** Modify a built-in evaluation method based on your needs.
- **Create a New Evaluation Flow from Scratch:**  Develop a brand-new evaluation method from the ground up.

The process of customizing and creating evaluation methods is similar to that of a standard flow.

### Customize a built-in evaluation method to measure the performance of a flow

Find the built-in evaluation methods by selecting the  **"Create"**  button on the homepage and navigating to the Create from gallery -\> Evaluation tab. You can view more details about an evaluation method by selecting  **"View details"**.

If you want to customize this evaluation method, you can select the **"Clone"** button.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/create-from-gallery.png" alt-text="Screenshot of the Prompt flow gallery with the evaluation tab selected. " lightbox = "./media/how-to-develop-an-evaluation-flow/create-from-gallery.png":::

By the name of the flow, you can see an **"evaluation"** tag, indicating you're building an evaluation flow. Similar to cloning a sample flow from gallery, you'll be able to view and edit the flow and the codes and prompts of the evaluation method.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/evaluation-tag.png" alt-text="Screenshot of Classification Accuracy Evaluation with the evaluation tag underlined. " lightbox = "./media/how-to-develop-an-evaluation-flow/evaluation-tag.png":::

Alternatively, you can customize a built-in evaluation method from a completed run by selecting the  **"Clone"**  icon when viewing its snapshot from the run detail page.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/clone-from-snapshot.png" alt-text="Gif of cloning a Web Classification batch run. " lightbox = "./media/how-to-develop-an-evaluation-flow/clone-from-snapshot.png":::

### Create new evaluation flow from scratch

To create your evaluation method from scratch, select the  **"Create"** button on the homepage and select  **"Evaluation"** as the flow type. You'll enter the flow authoring page.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/create-by-type.png" alt-text="Screenshot of tiles from the Prompt flow gallery with the create button highlighted on evaluation flow. " lightbox = "./media/how-to-develop-an-evaluation-flow/create-by-type.png":::

Then, you can see a template of evaluation flow containing two nodes: line_process and aggregate.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/evaluation-template.png" alt-text="Screenshot of evaluation flow created with template. " lightbox = "./media/how-to-develop-an-evaluation-flow/evaluation-template.png":::

## Understand evaluation in Prompt flow

In Prompt flow, a flow is a sequence of nodes that process an input and generate an output. Evaluation flows also take required inputs and produce corresponding outputs.

Some special features of evaluation methods are:

1. They usually run after the run to be tested, and receive outputs from that run.
2. Apart from the outputs from the run to be tested, they can receive an optional additional dataset which may contain corresponding ground truths. 
3. They may have an aggregation node that calculates the overall performance of the flow being tested based on the individual scores.
4. They can log metrics using log_metric() function.

We'll introduce how the inputs and outputs should be defined in developing evaluation methods.

### Inputs

An evaluation runs after another run to assess how well the outputs of that run align with specific criteria and goals. Therefore, evaluation receives the outputs generated from that run.

Other inputs may also be required, such as ground truth, which may come from a dataset. By default, evaluation will use the same dataset as the test dataset provided to the tested run. However, if the corresponding labels or target ground truth values are in a different dataset, you can easily switch to that one.  

Therefore, to run an evaluation, you need to indicate the sources of these required inputs. To do so, when submitting an evaluation, you'll see an  **"input mapping"**  section.

- If the data source is from your run output, the source is indicated as "${run.output.[OutputName]}"
- If the data source is from your test dataset, the source is indicated as "${data.[ColumnName]}"


:::image type="content" source="./media/how-to-develop-an-evaluation-flow/bulk-test-evaluation-input-mapping.png" alt-text="Screenshot of evaluation input mapping." lightbox = "./media/how-to-develop-an-evaluation-flow/bulk-test-evaluation-input-mapping.png":::

> [!NOTE]
> If your evaluation doesn't require data from the dataset, you do not need to reference any dataset columns in the input mapping section, indicating the dataset selection is an optional configuration. Dataset selection won't affect evaluation result.

### Input description

To remind what inputs are needed to calculate metrics, you can add a description for each required input. The descriptions are displayed when mapping the sources in batch run submission.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/input-description.png" alt-text="Screenshot of evaluation input mapping with the answers description highlighted. " lightbox = "./media/how-to-develop-an-evaluation-flow/input-description.png":::

To add descriptions for each input, select **Show description** in the input section when developing your evaluation method. And you can select "Hide description" to hide the description.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/add-description.png" alt-text="Screenshot of Classification Accuracy Evaluation with hide description highlighted. " lightbox = "./media/how-to-develop-an-evaluation-flow/add-description.png":::

Then this description is displayed to when using this evaluation method in batch run submission.

### Outputs and metrics

The outputs of an evaluation are the results that measure the performance of the flow being tested. The output usually contains metrics such as scores, and may also include text for reasoning and suggestions.

#### Instance-level scores — outputs

In Prompt flow, the flow processes each sample dataset one at a time and generates an output record. Similarly, in most evaluation cases, there will be a metric for each output, allowing you to check how the flow performs on each individual data.

To record the score for each data sample, calculate the score for each output, and log the score **as a flow output** by setting it in the output section. This authoring experience is the same as defining a standard flow output.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/eval-output.png" alt-text="Screenshot of the outputs section showing a name and value. " lightbox = "./media/how-to-develop-an-evaluation-flow/eval-output.png":::

We calculate this score in `line_process` node, which you can create and edit from scratch when creating by type. You can also replace this python node with an LLM node to use LLM to calculate the score.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/line-process.png" alt-text="Screenshot of line process node in the template. " lightbox = "./media/how-to-develop-an-evaluation-flow/line-process.png":::

When this evaluation method is used in a batch run, the instance-level score can be viewed in the **Overview ->Output** tab.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/evaluation-output-bulk.png" alt-text="Screenshot of the output tab with evaluation result appended and highlighted. " lightbox = "./media/how-to-develop-an-evaluation-flow/evaluation-output-bulk.png":::

#### Metrics logging and aggregation node

In addition, it's also important to provide an overall score for the run. You can check the  **"set as aggregation"** of a Python node in an evaluation flow to turn it into a "reduce" node, allowing the node to take in the inputs **as a list** and process them in batch.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/set-as-aggregation.png" alt-text="Screenshot of the Python node heading pointing to an unchecked checked box. " lightbox = "./media/how-to-develop-an-evaluation-flow/set-as-aggregation.png":::

In this way, you can calculate and process all the scores of each flow output and compute an overall result for each variant.

You can log metrics in an aggregation node using **Prompt flow_sdk.log_metrics()**. The metrics should be numerical (float/int). String type metrics logging isn't supported.

We calculate this score in `aggregate` node, which you can create and edit from scratch when creating by type. You can also replace this python node with a LLM node to use LLM to calculate the score. See the following example for using the log_metric API in an evaluation flow:


```python
from typing import List
from promptflow import tool, log_metric

@tool
def calculate_accuracy(grades: List[str], variant_ids: List[str]):
    aggregate_grades = {}
    for index in range(len(grades)):
        grade = grades[index]
        variant_id = variant_ids[index]
        if variant_id not in aggregate_grades.keys():
            aggregate_grades[variant_id] = []
        aggregate_grades[variant_id].append(grade)

    # calculate accuracy for each variant
    for name, values in aggregate_grades.items():
        accuracy = round((values.count("Correct") / len(values)), 2)
        log_metric("accuracy", accuracy, variant_id=name)

    return aggregate_grades
```

As you called this function in the Python node, you don't need to assign it anywhere else, and you can view the metrics later. When this evaluation method is used in a batch run, the instance-level score can be viewed in the **Overview->Metrics** tab.

:::image type="content" source="./media/how-to-develop-an-evaluation-flow/evaluation-metrics-bulk.png" alt-text="Screenshot of the metrics tab that shows the metrics logged by log metric. " lightbox = "./media/how-to-develop-an-evaluation-flow/evaluation-metrics-bulk.png":::

## Next steps

- [Iterate and optimize your flow by tuning prompts using variants](how-to-tune-prompts-using-variants.md)
- [Submit batch run and evaluate a flow](how-to-bulk-test-evaluate-flow.md)