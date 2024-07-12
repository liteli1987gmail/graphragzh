# 配置GraphRAG索引概述

GraphRAG 系统具有高度可配置性。本页面提供了有关 GraphRAG 索引引擎可用配置选项的概述。

## 默认配置模式

默认配置模式是使用 GraphRAG 系统的最简单方法。它旨在通过最小的配置即可立即使用。下面描述了索引引擎管道的主要配置部分。使用默认配置模式设置 GraphRAG 的主要方法如下：

- [Init 命令](/posts/config/init)（推荐）
- [纯使用环境变量](/posts/config/env_vars)
- [使用 JSON 或 YAML 进行更深层次的控制](/posts/config/json_yaml)


---
title: GraphRAG 索引化 🤖
navtitle: 索引概述
layout: page
tags: [post]
---

GraphRAG 索引化包是一个数据流水线和转换套件，旨在使用 LLM 从非结构化文本中提取有意义的结构化数据。

索引流水线是可配置的。它由工作流、标准步骤和自定义步骤、提示模板以及输入/输出适配器组成。我们的标准流水线旨在：

- 从原始文本中提取实体、关系和主张
- 对实体进行社区检测
- 在多个粒度级别生成社区摘要和报告
- 将实体嵌入到图向量空间中
- 将文本块嵌入到文本向量空间中

流水线的输出可以以多种格式存储，包括 JSON 和 Parquet，或者可以通过 Python API 手动处理。

## 入门指南

### 要求

请参阅【开始开发】中的【要求】部分，详细了解如何设置开发环境。

索引引擎可以在默认配置模式或自定义流水线模式下使用。
要配置 GraphRAG，请参阅【配置】文档。
在拥有配置文件之后，你可以使用 CLI 或 Python API 运行流水线。

## 使用方法

### CLI

---bash
# 通过Poetry
poetry run poe cli --root <data_root> # 默认配置模式
poetry run poe cli --config your_pipeline.yml # 自定义配置模式

# 通过Node
yarn run:index --root <data_root> # 默认配置模式
yarn run:index --config your_pipeline.yml # 自定义配置模式

```

### Python API

```python
from graphrag.index import run_pipeline
from graphrag.index.config import PipelineWorkflowReference

workflows: list[PipelineWorkflowReference] = [
    PipelineWorkflowReference(
        steps=[
            {
                # 内置动词
                "verb": "derive",  # https://github.com/microsoft/datashaper/blob/main/python/datashaper/datashaper/engine/verbs/derive.py
                "args": {
                    "column1": "col1",  # 从上面获取
                    "column2": "col2",  # 从上面获取
                    "to": "col_multiplied",  # 新列名
                    "operator": "*",  # 两列相乘
                },
                # 由于我们正在尝试对默认输入进行操作，因此不需要显式指定输入
            }
        ]
    ),
]

dataset = pd.DataFrame([{"col1": 2, "col2": 4}, {"col1": 5, "col2": 10}])
outputs = []
async for output in await run_pipeline(dataset=dataset, workflows=workflows):
    outputs.append(output)
pipeline_result = outputs[-1]
print(pipeline_result)
```

## 进一步阅读

---
title: Prompt Tuning ⚙️
navtitle: 概述
layout: page
tags: [post, tuning]
date: 2024-06-13
---

## 默认提示

默认提示是使用 GraphRAG 系统的最简单方法。它被设计成开箱即用，只需进行最少的配置即可。你可以在以下链接中找到有关这些提示的更多详细信息：

- [实体/关系提取](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/graph/prompts.py)
- [实体/关系描述摘要](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/summarize/prompts.py)
- [索赔提取](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/claims/prompts.py)
- [社区报告](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/community_reports/prompts.py)

## 自动模板化

自动模板化利用你的输入数据和 LLM 交互来创建领域自适应模板，用于生成知识图谱。强烈建议运行它，以获得在执行索引运行时更好的结果。有关如何使用它的更多详细信息，请参阅 [自动模板化](/posts/prompt_tuning/auto_prompt_tuning) 文档。


---
title: 查询引擎笔记本
navtitle: 查询引擎笔记本
layout: page
tags: [post, notebook]
---

关于运行查询的示例，请参考以下笔记本：

- [全局搜索笔记本](/posts/query/notebooks/global_search_nb)
- [本地搜索笔记本](/posts/query/notebooks/local_search_nb)

这些笔记本的测试数据集可以在 [此处](/data/operation_dulce/dataset.zip) 找到。 
---
title: 查询引擎  🔎
navtitle: 概述
tags: [post]
layout: page
---

查询引擎是图形 RAG 图书馆的检索模块之一。它是图形 RAG 图书馆的两个主要组成部分之一，另一个是索引管道（请参阅 [索引管道](/posts/index/overview)）。
它负责以下任务：

- [本地搜索](#local-search)
- [全局搜索](#global-search)
- [问题生成](#question-generation)

## 本地搜索

本地搜索方法通过将 AI 提取的知识图谱中的相关数据与原始文档的文本块结合起来生成答案。这种方法适用于需要理解文档中提到的特定实体的问题（例如，洋甘菊具有哪些疗效？）。

有关本地搜索的详细信息，请参阅 [本地搜索](/posts/query/1-local_search) 文档。

## 全局搜索

全局搜索方法以 MapReduce 的方式在所有 AI 生成的社区报告上进行搜索生成答案。这是一种资源密集型方法，但对于需要整体理解数据集的问题通常能提供良好的响应（例如，这个笔记本中提到的草药的最重要价值是什么？）。

更多关于此的信息可以在 [全局搜索](/posts/query/0-global_search) 文档中查看。

## 问题生成



自定义配置模式是一个高级用法。大多数用户将使用默认配置。索引引擎管道的主要配置部分如下所述。有关如何使用自定义配置的详细信息，请参阅 [自定义配置模式](/posts/config/custom) 文档。


- 要开始在 GraphRAG 项目中进行开发，请参阅 [入门指南](/posts/developing/)
- 要了解索引库的基本概念和执行模型，请参阅 [体系结构文档](/posts/index/0-architecture/)
- 要使用一系列示例开始运行，请参阅 [示例文档](https://github.com/microsoft/graphrag/blob/main/examples/README.md)
- 要了解更多关于配置索引引擎的信息，请参阅 [配置文档](/posts/config/overview)




手动配置是一个高级用例。大多数用户将希望使用自动模板功能。有关如何使用手动配置的详细信息，请参阅 [手动提示配置](/posts/prompt_tuning/manual_prompt_tuning) 文档。

这个功能接收一个用户查询的列表，并生成下一个候选问题。这对于在对话中生成后续问题或者为调查人员生成深入研究数据集的问题列表非常有用。

关于问题生成的工作原理的信息可以在 [问题生成](/posts/query/2-question_generation) 文档页面找到。
