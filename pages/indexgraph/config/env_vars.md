# 默认配置模式（使用环境变量）

## 文本嵌入自定义

默认情况下，GraphRAG 索引器仅会生成查询方法所需的嵌入。然而，该模型对于所有明文字段都定义了嵌入，并且可以通过将 `GRAPHRAG_EMBEDDING_TARGET` 环境变量设置为 `all` 来生成这些嵌入。

如果嵌入目标为 `all`，且你只想嵌入其中的一部分字段，你可以使用下面描述的 `GRAPHRAG_EMBEDDING_SKIP` 参数指定要跳过的嵌入。

### 嵌入字段

- `text_unit.text`
- `document.raw_content`
- `entity.name`
- `entity.description`
- `relationship.description`
- `community.title`
- `community.summary`
- `community.full_content`

## 输入数据

我们的流程可以从输入文件夹中摄取 `.csv` 或 `.txt` 格式的数据。这些文件可以嵌套在子文件夹中。要配置如何处理输入数据、映射哪些字段以及如何解析时间戳，请查看下面以 `GRAPHRAG_INPUT_` 开头的配置值。一般来说，基于 CSV 的数据提供了最强大的自定义能力。每个 CSV 文件至少应该包含一个 `text` 字段（可以使用环境变量进行映射），但如果它们还有 `title`、`timestamp` 和 `source` 字段会更有帮助。还可以包含额外的字段，这些字段也会作为附加字段出现在 `Document` 表上。

## 基本 LLM 设置

这些是用于配置 LLM 连接的主要设置。

| 参数                        | 是否必需                             | 描述                                                                                                                              | 类型  | 默认值      |
| --------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----- | ----------- |
| `GRAPHRAG_API_KEY`          | **OpenAI 必需，AOAI 可选**              | API 密钥（注意：`OPENAI_API_KEY` 也将用作后备）。如果在使用 AOAI 时没有定义，将使用托管的标识。                                              | `str` | `None`      |
| `GRAPHRAG_API_BASE`         | **对于 AOAI 来说必需**                  | API 基本 URL                                                                                                                       | `str` | `None`      |
| `GRAPHRAG_API_VERSION`      | **对于 AOAI 来说必需**                  | AOAI API 版本                                                                                                                      | `str` | `None`      |
| `GRAPHRAG_API_ORGANIZATION` |                                       | AOAI 组织                                                                                                                          | `str` | `None`      |
| `GRAPHRAG_API_PROXY`        |                                       | AOAI 代理                                                                                                                          | `str` | `None`      |

## 文本生成设置

这些设置控制流程中使用的文本生成模型。如果基本 LLM 设置可用，任何带有后备的设置将使用基本 LLM 设置。

| 参数                                         | 是否必需                | 描述                                                                                            | 类型    | 默认值              |
| ---------------------------------------------| ----------------------- | ----------------------------------------------------------------------------------------------  | ------- | ------------------- |
| `GRAPHRAG_LLM_TYPE`                            | **对于 AOAI 来说必需**   | LLM 操作类型。可以是 `openai_chat` 或 `azure_openai_chat`                                            | `str`   | `openai_chat`       |
| `GRAPHRAG_LLM_DEPLOYMENT_NAME`                 | **对于 AOAI 来说必需**   | AOAI 模型部署名称                                                                                | `str`   | `None`              |
| `GRAPHRAG_LLM_API_KEY`                         | 必需（使用后备）       | API 密钥。如果在使用 AOAI 时没有定义，将使用托管的标识                                              | `str`   | `None`              |
| `GRAPHRAG_LLM_API_BASE`                        | 对于 AOAI（使用后备）   | API 基本 URL                                                                                      | `str`   | `None`              |
| `GRAPHRAG_LLM_API_VERSION`                     | 对于 AOAI（使用后备）   | AOAI API 版本                                                                                    | `str`   | `None`              |
| `GRAPHRAG_LLM_API_ORGANIZATION`                | 对于 AOAI（使用后备）   | AOAI 组织                                                                                        | `str`   | `None`              |
| `GRAPHRAG_LLM_API_PROXY`                       |                        | AOAI 代理                                                                                        | `str`   | `None`              |
| `GRAPHRAG_LLM_MODEL`                           |                        | LLM 模型                                                                                         | `str`   | `gpt-4-turbo-preview` |
| `GRAPHRAG_LLM_MAX_TOKENS`                      |                        | 最大标记数                                                                                      | `int`   | `4000`              |
| `GRAPHRAG_LLM_REQUEST_TIMEOUT`                 |                        | 等待从聊天客户端收到响应的最大秒数                                                              | `int`   | `180`               |
| `GRAPHRAG_LLM_MODEL_SUPPORTS_JSON`             |                        | 指示给定模型是否支持 JSON 输出模式。`True` 为启用                                                     | `str`   | `None`              |
| `GRAPHRAG_LLM_THREAD_COUNT`                    |                        | 用于 LLM 并行化的线程数                                                                            | `int`   | 50                  |
| `GRAPHRAG_LLM_THREAD_STAGGER`                  |                        | 在启动每个线程之间等待的时间（以秒为单位）                                                        | `float` | 0.3                 |
| `GRAPHRAG_LLM_CONCURRENT_REQUESTS`             |                        | 允许并发请求的数目                                                                              | `int`   | 25                  |
| `GRAPHRAG_LLM_TOKENS_PER_MINUTE`               |                        | 允许 LLM 客户端每分钟使用的标记数。0 = Bypass                                                      | `int`   | 0                   |
| `GRAPHRAG_LLM_REQUESTS_PER_MINUTE`             |                        | 允许 LLM 客户端每分钟的请求数。0 = Bypass                                                         | `int`   | 0                   |
| `GRAPHRAG_LLM_MAX_RETRIES`                     |                        | 在请求失败时尝试的最大次数                                                                      | `int`   | 10                  |
| `GRAPHRAG_LLM_MAX_RETRY_WAIT`                  |                        | 在重试之间等待的最大秒数                                                                        | `int`   | 10                  |
| `GRAPHRAG_LLM_SLEEP_ON_RATE_LIMIT_RECOMMENDATION` |                      | 是否根据速率限制推荐进行休眠。 （仅适用于 Azure）                                                | `bool`  | `True`              |
| `GRAPHRAG_LLM_TEMPERATURE`                     |                        | 使用生成的温度                                                                                  | `float` | 0                   |
| `GRAPHRAG_LLM_TOP_P`                           |                        | 用于抽样的 top_p 值                                                                                | `float` | 1                   |
| `GRAPHRAG_LLM_N`                               |                        | 生成响应的数量                                                                                  | `int`   | 1                   |

## 文本嵌入设置

这些设置控制流程中使用的文本嵌入模型。如果基本 LLM 设置可用，任何带有后备的设置将使用基本 LLM 设置。

| 参数                                                 | 是否必需               | 描述                                                                                       | 类型    | 默认值                  |
| ----------------------------------------------------- | ---------------------- | ----------------------------------------------------------------------------------------- | ------- | ----------------------- |
| `GRAPHRAG_EMBEDDING_TYPE`                             | **对于 AOAI 来说必需**  | 要使用的嵌入客户端。可以是 `openai_embedding` 或 `azure_openai_embedding`                   | `str`   | `openai_embedding`       |
| `GRAPHRAG_EMBEDDING_DEPLOYMENT_NAME`                  | **对于 AOAI 来说必需**  | AOAI 部署名称                                                                               | `str`   | `None`                   |
| `GRAPHRAG_EMBEDDING_API_KEY`                          | 必需（使用后备）       | 用于嵌入客户端的 API 密钥。如果在使用 AOAI 时没有定义，将使用托管的标识                         | `str`   | `None`                   |
| `GRAPHRAG_EMBEDDING_API_BASE`                         | 对于 AOAI（使用后备）   | API 基本 URL                                                                                 | `str`   | `None`                   |
| `GRAPHRAG_EMBEDDING_API_VERSION`                      | 对于 AOAI（使用后备）   | 用于嵌入客户端的 AOAI API 版本                                                                | `str`   | `None`                   |
| `GRAPHRAG_EMBEDDING_API_ORGANIZATION`                 | 对于 AOAI（使用后备）   | 用于嵌入客户端的 AOAI 组织                                                                    | `str`   | `None`                   |
| `GRAPHRAG_EMBEDDING_API_PROXY`                        |                       | 用于嵌入客户端的 AOAI 代理                                                                     | `str`   | `None`                   |
| `GRAPHRAG_EMBEDDING_MODEL`                            |                       | 用于嵌入客户端的模型                                                                         | `str`   | `text-embedding-3-small` |
| `GRAPHRAG_EMBEDDING_BATCH_SIZE`                       |                       | 一次嵌入的文本数量。[(Azure 限制为 16)](https://learn.microsoft.com/en-us/azure/ai-ce)         | `int`   | 16                      |
| `GRAPHRAG_EMBEDDING_BATCH_MAX_TOKENS`                 |                       | 每批的最大标记数。[(Azure 限制为 8191)](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference) | `int`   | 8191                    |
| `GRAPHRAG_EMBEDDING_TARGET`                           |                       | 要嵌入的目标字段。可以是 `required` 或 `all`                                                   | `str`   | `required`              |
| `GRAPHRAG_EMBEDDING_SKIP`                             |                       | 要跳过嵌入的字段的逗号分隔列表。例如：'relationship.description'                             | `str`   | `None`                  |
| `GRAPHRAG_EMBEDDING_THREAD_COUNT`                     |                       | 用于嵌入并行化的线程数                                                                       | `int`   |                         |
| `GRAPHRAG_EMBEDDING_THREAD_STAGGER`                   |                       | 开始每个线程之间等待的时间（以秒为单位）                                                     | `float` | 50                      |
| `GRAPHRAG_EMBEDDING_CONCURRENT_REQUESTS`              |                       | 允许嵌入客户端的并发请求的数量                                                               | `int`   | 25                      |
| `GRAPHRAG_EMBEDDING_TOKENS_PER_MINUTE`                |                       | 每分钟允许嵌入客户端使用的标记数。0 = Bypass                                                | `int`   | 0                       |
| `GRAPHRAG_EMBEDDING_REQUESTS_PER_MINUTE`              |                       | 每分钟允许嵌入客户端的请求数。0 = Bypass                                                   | `int`   | 0                       |
| `GRAPHRAG_EMBEDDING_MAX_RETRIES`                      |                       | 请求失败时尝试的最大次数                                                                     | `int`   | 10                      |
| `GRAPHRAG_EMBEDDING_MAX_RETRY_WAIT`                   |                       | 重试之间等待的最大秒数                                                                       | `int`   | 10                      |
| `GRAPHRAG_EMBEDDING_TARGET`                           |                       | 要嵌入的目标字段。可以是 `required` 或 `all`                                                   | `str`   | `required`              |
| `GRAPHRAG_EMBEDDING_SLEEP_ON_RATE_LIMIT_RECOMMENDATION` |                     | 是否根据速率限制推荐进行休眠。 （仅适用于 Azure）                                               | `bool`  | `True`                  |

## 输入设置



                这些设置控制流程使用的输入数据。如果有回退设置，将使用基本 LLM 设置（如果可用）。

### Plaintext 输入数据（`GRAPHRAG_INPUT_FILE_TYPE` = text）

| 参数                          | 描述                                                                                         | 类型   | 必需或可选 | 默认值      |
| ------------------------------ | -------------------------------------------------------------------------------------------- | ------ | ---------- | ----------- |
| `GRAPHRAG_INPUT_FILE_PATTERN`  | 读取输入目录中的输入文件时要使用的文件模式正则表达式。                                        | `str`  | 可选       | `.*\.txt$` |

### CSV 输入数据（`GRAPHRAG_INPUT_FILE_TYPE` = csv）

| 参数                                          | 描述                                                                                                                      | 类型   | 必需或可选 | 默认值      |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ----------- |
| `GRAPHRAG_INPUT_TYPE`                          | 在读取文件时要使用的输入存储类型。 （“file”或 “blob”）                                                                    | `str`  | 可选       | `file`      |
| `GRAPHRAG_INPUT_FILE_PATTERN`                  | 读取输入目录中的输入文件时要使用的文件模式正则表达式。                                                                     | `str`  | 可选       | `.*\.txt$`  |
| `GRAPHRAG_INPUT_SOURCE_COLUMN`                 | 在读取 CSV 输入文件时要使用的“source”列。                                                                                  | `str`  | 可选       | `source`    |
| `GRAPHRAG_INPUT_TIMESTAMP_COLUMN`              | 在读取 CSV 输入文件时要使用的“timestamp”列。                                                                               | `str`  | 可选       | `None`      |
| `GRAPHRAG_INPUT_TIMESTAMP_FORMAT`              | 在解析时间戳列中的时间戳时要使用的时间戳格式。                                                                             | `str`  | 可选       | `None`      |
| `GRAPHRAG_INPUT_TEXT_COLUMN`                   | 在读取 CSV 输入文件时要使用的“text”列。                                                                                    | `str`  | 可选       | `text`      |
| `GRAPHRAG_INPUT_DOCUMENT_ATTRIBUTE_COLUMNS`    | 要包含为文档字段的 CSV 列的逗号分隔列表。                                                                                      | `str`  | 可选       | `id`        |
| `GRAPHRAG_INPUT_TITLE_COLUMN`                  | 在读取 CSV 输入文件时要使用的“title”列。                                                                                   | `str`  | 可选       | `title`     |
| `GRAPHRAG_INPUT_STORAGE_ACCOUNT_BLOB_URL`      | 用于“blob”模式和使用托管身份验证时要使用的 Azure 存储 blob 终结点。将具有格式“https://<storage_account_name>.blob.core.windows.net” | `str`  | 可选       | `None`      |
| `GRAPHRAG_INPUT_CONNECTION_STRING`             | 用于从 Azure Blob 存储中读取 CSV 输入文件的连接字符串。                                                                       | `str`  | 可选       | `None`      |
| `GRAPHRAG_INPUT_CONTAINER_NAME`                | 用于从 Azure Blob 存储中读取 CSV 输入文件的容器名称。                                                                          | `str`  | 可选       | `None`      |
| `GRAPHRAG_INPUT_BASE_DIR`                      | 从中读取输入文件的基本目录。                                                                                              | `str`  | 可选       | `None`      |

## 数据映射设置

| 参数                          | 描述                                                  | 类型   | 必需或可选 | 默认值 |
| ------------------------------ | ----------------------------------------------------- | ------ | ---------- | ------- |
| `GRAPHRAG_INPUT_FILE_TYPE`      | 输入数据类型，`csv` 或 `text`                           | `str`  | 可选       | `text`  |
| `GRAPHRAG_INPUT_ENCODING`       | 读取 CSV /文本输入文件时要应用的编码。                  | `str`  | 可选       | `utf-8` |

## 数据分块

| 参数                          | 描述                                                            | 类型   | 必需或可选 | 默认值 |
| ------------------------------ | --------------------------------------------------------------- | ------ | ---------- | ------- |
| `GRAPHRAG_CHUNK_SIZE`          | 用于文本分块分析窗口的标记数的块大小。                           | `str`  | 可选       | 1200    |
| `GRAPHRAG_CHUNK_OVERLAP`       | 用于文本分块分析窗口的标记数的重叠。                             | `str`  | 可选       | 100     |
| `GRAPHRAG_CHUNK_BY_COLUMNS`    | 执行 TextUnit 分块时要按组的文档属性的逗号分隔列表。                 | `str`  | 可选       | `id`    |

## 提示覆盖

| 参数                                         | 描述                                                                       | 类型     | 必需或可选 | 默认值                                                          |
| --------------------------------------------- | -------------------------------------------------------------------------- | -------- | ---------- | ---------------------------------------------------------------- |
| `GRAPHRAG_ENTITY_EXTRACTION_PROMPT_FILE`      | 实体提取提示模板文本文件的路径（相对于根目录）。                          | `str`    | 可选       | `None`                                                           |
| `GRAPHRAG_ENTITY_EXTRACTION_MAX_GLEANINGS`    | 提取实体时调用的最大重试次数（gleanings）。                                | `int`    | 可选       | 1                                                                |
| `GRAPHRAG_ENTITY_EXTRACTION_ENTITY_TYPES`     | 要提取的实体类型的逗号分隔列表。                                         | `str`    | 可选       | `organization,person,event,geo`                                  |
| `GRAPHRAG_SUMMARIZE_DESCRIPTIONS_PROMPT_FILE` | 描述汇总提示模板文本文件的路径（相对于根目录）。                          | `str`    | 可选       | `None`                                                           |
| `GRAPHRAG_SUMMARIZE_DESCRIPTIONS_MAX_LENGTH`  | 每个描述汇总要生成的标记数的最大数量。                                    | `int`    | 可选       | 500                                                              |
| `GRAPHRAG_CLAIM_EXTRACTION_ENABLED`           | 是否启用该流程的主张提取。                                              | `bool`   | 可选       | `False`                                                          |
| `GRAPHRAG_CLAIM_EXTRACTION_DESCRIPTION`       | 要使用的 claim_description 提示参数。                                     | `string` | 可选       | " Any claims or facts that could be relevant to threat analysis." |
| `GRAPHRAG_CLAIM_EXTRACTION_PROMPT_FILE`       | 要使用的 claim 提取提示。                                                  | `string` | 可选       | `None`                                                           |
| `GRAPHRAG_CLAIM_EXTRACTION_MAX_GLEANINGS`     | 提取循环中提取 claim 时调用的最大重试次数（gleanings）。                    | `int`    | 可选       | 1                                                                |
| `GRAPHRAG_COMMUNITY_REPORTS_PROMPT_FILE`      | 要使用的社区报告提取提示。                                               | `string` | 可选       | `None`                                                           |
| `GRAPHRAG_COMMUNITY_REPORTS_MAX_LENGTH`       | 每个社区报告要生成的标记数的最大数量。                                    | `int`    | 可选       | 1500                                                             |

## 存储

此部分控制流程用于发出输出表的存储机制。

| 参数                                          | 描述                                                                                                                      | 类型   | 必需或可选 | 默认值 |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ------- |
| `GRAPHRAG_STORAGE_TYPE`                        | 要使用的报告器类型。选项为“file”、“memory”或“blob”                                                                      | `str`  | 可选       | `file`  |
| `GRAPHRAG_STORAGE_STORAGE_ACCOUNT_BLOB_URL`    | 用于“blob”模式和使用托管身份验证时要使用的 Azure 存储 blob 终结点。将具有格式“https://<storage_account_name>.blob.core.windows.net” | `str`  | 可选       | None    |
| `GRAPHRAG_STORAGE_CONNECTION_STRING`           | 用于“blob”模式时使用的 Azure 存储连接字符串。                                                                                | `str`  | 可选       | None    |
| `GRAPHRAG_STORAGE_CONTAINER_NAME`              | 在“blob”模式下使用的 Azure 存储容器名称。                                                                                        | `str`  | 可选       | None    |
| `GRAPHRAG_STORAGE_BASE_DIR`                    | 数据输出的基本路径。                                                                                                        | `str`  | 可选       | None    |

## 缓存

此部分控制流程使用的缓存机制。这用于缓存 LLM 调用结果。

| 参数                                         | 描述                                                                                                                      | 类型   | 必需或可选 | 默认值 |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ------- |
| `GRAPHRAG_CACHE_TYPE`                         | 要使用的缓存类型。选项为“file”、“memory”、“none”或“blob”                                                                   | `str`  | 可选       | `file`  |
| `GRAPHRAG_CACHE_STORAGE_ACCOUNT_BLOB_URL`     | 用于“blob”模式和使用托管身份验证时要使用的 Azure 存储 blob 终结点。将具有格式“https://<storage_account_name>.blob.core.windows.net” | `str`  | 可选       | None    |
| `GRAPHRAG_CACHE_CONNECTION_STRING`            | 用于“blob”模式时使用的 Azure 存储连接字符串。                                                                                | `str`  | 可选       | None    |
| `GRAPHRAG_CACHE_CONTAINER_NAME`               | 在“blob”模式下使用的 Azure 存储容器名称。                                                                                        | `str`  | 可选       | None    |
| `GRAPHRAG_CACHE_BASE_DIR`                     | 报告输出的基本路径。                                                                                                        | `str`  | 可选       | None    |

## 报告

此部分控制流程所使用的报送机制，用于常见事件和错误消息。默认情况下，报告将写入输出目录中的文件中。但是，你也可以选择将报告写入控制台或 Azure Blob Storage 容器中。

| 参数                                             | 描述                                                                                                                      | 类型   | 必需或可选 | 默认值 |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ------- |
| `GRAPHRAG_REPORTING_TYPE`                         | 要使用的报告器类型。选项为“file”、“console” 或 “blob”                                                                    | `str`  | 可选       | `file`  |
| `GRAPHRAG_REPORTING_STORAGE_ACCOUNT_BLOB_URL`     | 用于“blob”模式和使用托管身份验证时要使用的 Azure 存储 blob 终结点。将具有格式“https://<storage_account_name>.blob.core.windows.net” | `str`  | 可选       | None    |
| `GRAPHRAG_REPORTING_CONNECTION_STRING`            | 用于“blob”模式时使用的 Azure 存储连接字符串。                                                                                | `str`  | 可选       | None    |
| `GRAPHRAG_REPORTING_CONTAINER_NAME`               | 在“blob”模式下使用的 Azure 存储容器名称。                                                                                        | `str`  | 可选       | None    |
| `GRAPHRAG_REPORTING_BASE_DIR`                     | 报告输出的基本路径。                                                                                                        | `str`  | 可选       | None    |

## Node2Vec 参数



| 参数名                          | 描述                                      | 类型   | 必填或可选           | 默认值  |
| ------------------------------- | ----------------------------------------- | ------ | -------------------- | ------- |
| `GRAPHRAG_NODE2VEC_ENABLED`     | 是否启用 Node2Vec                           | `bool` | 可选                 | False   |
| `GRAPHRAG_NODE2VEC_NUM_WALKS`   | 进行 Node2Vec 的步行次数                       | `int`  | 可选                 | 10      |
| `GRAPHRAG_NODE2VEC_WALK_LENGTH` | Node2Vec 的步行长度                           | `int`  | 可选                 | 40      |
| `GRAPHRAG_NODE2VEC_WINDOW_SIZE` | Node2Vec 的窗口大小                           | `int`  | 可选                 | 2       |
| `GRAPHRAG_NODE2VEC_ITERATIONS`  | 运行 node2vec 的迭代次数                         | `int`  | 可选                 | 3       |
| `GRAPHRAG_NODE2VEC_RANDOM_SEED` | 用于 node2vec 的随机种子                        | `int`  | 可选                 | 597832  |

## 数据快照



| 参数                               | 描述                                       | 类型   | 必需或可选 | 默认值 |
| --------------------------------- | ------------------------------------------ | ------ | ---------- | ------ |
| `GRAPHRAG_SNAPSHOT_GRAPHML`       | 是否启用 GraphML 快照。                     | `bool` | 可选       | False   |
| `GRAPHRAG_SNAPSHOT_RAW_ENTITIES`  | 是否启用原始实体快照。                     | `bool` | 可选       | False   |
| `GRAPHRAG_SNAPSHOT_TOP_LEVEL_NODES`| 是否启用顶层节点快照。                     | `bool` | 可选       | False   |

# 其他设置

| 参数                          | 描述                                                                   | 类型   | 必需或可选 | 默认值         |
| ---------------------------- | --------------------------------------------------------------------- | ------ | ---------- | -------------- |
| `GRAPHRAG_ASYNC_MODE`        | 使用的异步模式，可以是 `asyncio` 或 `threaded`。                             | `str`  | 可选       | `asyncio`     |
| `GRAPHRAG_ENCODING_MODEL`    | 文本编码模型，用于在 tiktoken 中对文本进行编码。                             | `str`  | 可选       | `cl100k_base` |
| `GRAPHRAG_MAX_CLUSTER_SIZE`  | 单个 Leiden 聚类中包含的最大实体数。                                         | `int`  | 可选       | 10             |
| `GRAPHRAG_SKIP_WORKFLOWS`    | 要跳过的工作流名称列表，用逗号分隔。                                       | `str`  | 可选       | `None`         |
| `GRAPHRAG_UMAP_ENABLED`      | 是否启用 UMAP 布局。                                                    | `bool` | 可选       | False           |
