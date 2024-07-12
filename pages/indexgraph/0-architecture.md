---
title: 索引架构
navtitle: 架构
tags: [帖子, 索引]
layout: 页面
date: 2023-01-01
---

## 关键概念

### 知识模型

为了支持 GraphRAG 系统，索引引擎的输出（在默认配置模式下）与我们称之为“GraphRAG 知识模型”的知识模型对齐。
该模型旨在抽象出底层数据存储技术，并为 GraphRAG 系统提供一个公共接口以进行交互。
在正常使用情况下，GraphRAG 索引器的输出将加载到数据库系统中，并且 GraphRAG 的查询引擎将使用知识模型数据存储类型与数据库进行交互。

### DataShaper 工作流程

GraphRAG 的索引管道建立在我们的开源库 [DataShaper](https://github.com/microsoft/datashaper) 之上。
DataShaper 是一个数据处理库，允许用户使用明确定义的模式以声明方式表达数据管道、模式和相关资产。
DataShaper 在 JavaScript 和 Python 中有实现，并且被设计为可扩展到其他语言。

在 DataShaper 中，核心资源类型之一是 [Workflow](https://github.com/microsoft/datashaper/blob/main/javascript/schema/src/workflow/WorkflowSchema.ts)。
工作流程被表达为一系列步骤，我们称之为 [动词](https://github.com/microsoft/datashaper/blob/main/javascript/schema/src/workflow/verbs.ts)。
每个步骤都有一个动词名称和配置对象。
在 DataShaper 中，这些动词模拟关系概念，例如 SELECT、DROP、JOIN 等。每个动词转换输入数据表，并将该表传递到流水线。

![DataShaper 工作流程](img/LLM-based Workflow Steps.png)

### 基于 LLM 的工作流步骤

GraphRAG 的索引管道在我们的 DataShaper 库提供的标准关系动词之上实现了一些自定义动词。这些动词使我们能够利用 LLM（如 GPT-4）的强大功能，以丰富结构化数据的方式增强文本文档。我们在标准工作流程中利用这些动词提取实体、关系、声明、社区结构和社区报告摘要。此行为是可定制的，并可以扩展以支持许多种基于 AI 的数据增强和提取任务。

### 工作流程图

由于我们的数据索引任务的复杂性，我们需要能够将数据流水线表示为多个相互依赖的工作流程的系列。
在 GraphRAG 索引管道中，每个工作流程可能对其他工作流程有依赖关系，从而形成一个有向无环图（DAG）的工作流程，然后用于调度处理。

![](/img/Dataframe Message Format.png)

### Dataframe 消息格式

工作流程之间和工作流程步骤之间的通信主要是 `pandas.DataFrame` 的实例。
尽管可能存在副作用，我们的目标是以数据为中心、以表为中心的方式处理数据。
这使我们能够轻松地推理我们的数据，并利用基于数据帧的生态系统的功能。
我们的底层数据帧技术可能随时间而变化，但我们的主要目标是支持 DataShaper 工作流程模式，同时保持单机使用和开发人员的舒适性。

### LLM 缓存 
GraphRAG 库在设计时考虑了与 LLM 的交互，而使用 LLM API 时常见的一个问题是由于网络延迟、限流等原因导致的各种错误。考虑到这些潜在的错误情况，我们在 LLM 交互周围添加了一个缓存层。当使用相同的输入集（提示和调优参数）进行完成请求时，如果存在缓存结果，我们会返回该结果。这使得我们的索引器能够更好地应对网络问题，实现幂等操作，并提供更高效的最终用户体验。