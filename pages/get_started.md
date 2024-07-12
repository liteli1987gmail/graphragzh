
# 快速入门

## 环境配置

[Python 3.10-3.12](https://www.python.org/downloads/)

要开始使用 GraphRAG 系统，你有几个选项：

👉 [使用 GraphRAG Accelerator 解决方案](https://github.com/Azure-Samples/graphrag-accelerator) 

👉 [从 pypi 安装](https://pypi.org/project/graphrag/) 

👉 [从源码使用](/posts/developing)

## 快速入门

为了开始使用 GraphRAG 系统，我们建议尝试 [知识图谱加速](https://github.com/Azure-Samples/graphrag-accelerator) 包。这为使用 Azure 资源的用户提供了一个用户友好的端到端体验。

# 顶级模块

[索引流程概述](/posts/index/overview)
[查询引擎概述](/posts/query/overview)

# 概述

以下是使用 GraphRAG 系统的简单端到端示例。它展示了如何使用系统对一些文本进行索引，然后使用索引数据来回答关于文档的问题。

# 安装 GraphRAG

```bash
pip install graphrag
```

# 运行索引器

现在我们需要设置一个数据项目和一些初始配置。让我们来设置它。我们使用[默认配置模式](/posts/config/overview/)，你可以根据需要自定义一个[配置文件](/posts/config/json_yaml/)（我们建议），或者使用[环境变量](/posts/config/env_vars/)。

首先让我们准备一个示例数据集：

```sh
mkdir -p ./ragtest/input
```

现在让我们从可靠的来源获取查尔斯·狄更斯的《圣诞颂歌》的副本

```sh
curl https://www.gutenberg.org/cache/epub/24022/pg24022.txt > ./ragtest/input/book.txt
```

接下来，我们将注入一些必需的配置变量：

## 设置你的工作区变量

首先，请确保设置必需的环境变量。有关这些环境变量的详细信息以及可用的环境变量，请参阅[变量文档](/posts/config/overview/)。

为了初始化你的工作区，让我们首先运行`graphrag.index --init`命令。由于我们已经在上一步中配置了一个名为\.ragtest`的目录，我们可以运行以下命令：

```sh
python -m graphrag.index --init --root ./ragtest
```

这将在`./ragtest`目录中创建两个文件：`.env`和`settings.yaml`。

- `.env`包含运行GraphRAG流程所需的环境变量。如果你检查该文件，你会看到定义了一个单一的环境变量，`GRAPHRAG_API_KEY=<API_KEY>`。这是用于OpenAI API或Azure OpenAI端点的API密钥。你可以将其替换为你自己的API密钥。
- `settings.yaml`包含流程的设置。你可以修改此文件以更改流程的设置。
  <br/>

#### OpenAI和Azure OpenAI

要在OpenAI模式下运行，请确保将`.env`文件中的`GRAPHRAG_API_KEY`的值更新为你的OpenAI API密钥。

#### Azure OpenAI

此外，Azure OpenAI用户应该在settings.yaml文件中设置以下变量。要查找相应的部分，只需搜索`llm:`配置，你应该会看到两个部分，一个用于聊天端点，一个用于嵌入端点。下面是如何配置聊天端点的示例：

```yaml
type: azure_openai_chat # Or azure_openai_embedding for embeddings
api_base: https://<instance>.openai.azure.com
api_version: 2024-02-15-preview # 你可以自定义此版本
deployment_name: <azure_model_deployment_name>
```

- 有关配置GraphRAG的更多详细信息，请参阅[配置文档](/posts/config/overview/)。
- 要了解更多有关初始化的详细信息，请参阅[初始化文档](/posts/config/init/)。
- 要了解有关使用CLI的更多详细信息，请参阅[CLI文档](/posts/query/3-cli/)。

## 运行索引流程
 
最后我们来运行流程！

```sh
python -m graphrag.index --root ./ragtest
```

![pipeline executing from the CLI](/img/pipeline-running.png)

这个过程需要一些时间来运行。这取决于你的输入数据大小，你使用的模型以及正在使用的文本块大小（可以在`.env`文件中进行配置）。
一旦流程完成，你应该会看到一个名为`./ragtest/output/<timestamp>/artifacts`的新文件夹，其中包含一系列parquet文件。

# 使用查询引擎

## 运行查询引擎


以下是使用此数据集提出一些问题的示例。

使用全局搜索来提出一个高层次问题的示例：

```sh
python -m graphrag.query \
--root ./ragtest \
--method global \
"这个故事的主题是什么？"
```

使用局部搜索来提出一个关于特定角色的更具体的问题的示例：

```sh
python -m graphrag.query \
--root ./ragtest \
--method local \
"Scrooge 这个故事的主人公是谁，他的主要关系是什么？"
```

详细了解如何利用我们的本地搜索和全局搜索机制从数据中提取有意义的见解的方法，请参阅[Query Engine](/posts/query/overview)文档，了解更多详细信息。在索引器执行完毕后如何运用本地和全局搜索机制提取有意义的见解。 