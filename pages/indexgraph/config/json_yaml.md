# 默认配置模式（使用JSON/YAML）

默认配置模式可以通过在数据项目根目录中使用 `config.json` 或 `config.yml` 文件进行配置。如果与此配置文件一起存在 `.env` 文件，则会加载该文件，并且其中定义的环境变量将可用于使用 `${ENV_VAR}` 语法在配置文档中进行令牌替换。

例如：

```
# .env
API_KEY=some_api_key

# config.json
{
    "llm": {
        "api_key": "${API_KEY}"
    }
}
```

# 配置段落

## input

### 字段

- `type` **file|blob** - 要使用的输入类型。默认值=`file`
- `file_type` **text|csv** - 要加载的输入数据类型。可以是 `text` 或 `csv`。默认为 `text`
- `file_encoding` **str** - 输入文件的编码。默认为 `utf-8`
- `file_pattern` **str** - 用于匹配输入文件的正则表达式。如果处于csv模式，则默认为 `.*\.csv$` ，如果处于文本模式，则默认为 `.*\.txt$` 。
- `source_column` **str** - （仅限CSV模式）源列名。
- `timestamp_column` **str** - （仅限CSV模式）时间戳列名。
- `timestamp_format` **str** - （仅限CSV模式）源格式。
- `text_column` **str** - （仅限CSV模式）文本列名。
- `title_column` **str** - （仅限CSV模式）标题列名。
- `document_attribute_columns` **list[str]** - （仅限CSV模式）要包含的其他文档属性。
- `connection_string` **str** - （仅限blob）Azure Storage连接字符串。
- `container_name` **str** - （仅限blob）Azure Storage容器名称。
- `base_dir` **str** - 相对于根目录读取输入的基本目录。
- `storage_account_blob_url` **str** - 要使用的存储帐户Blob URL。

## llm

这是基本的LLM配置部分。其他步骤可能使用自己的LLM配置覆盖此配置。

### 字段

- `api_key` **str** - 要使用的OpenAI API密钥。
- `type` **openai_chat|azure_openai_chat|openai_embedding|azure_openai_embedding** - 要使用的LLM类型。
- `model` **str** - 模型名称。
- `max_tokens` **int** - 输出令牌的最大数量。
- `request_timeout` **float** - 每个请求的超时时间。
- `api_base` **str** - 要使用的API基本URL。
- `api_version` **str** - API版本
- `organization` **str** - 客户端组织。
- `proxy` **str** - 要使用的代理URL。
- `cognitive_services_endpoint` **str** - 认知服务的URL端点。
- `deployment_name` **str** - 要使用的部署名称（Azure）。
- `model_supports_json` **bool** - 模型是否支持JSON模式输出。
- `tokens_per_minute` **int** - 设置令牌每分钟的漏桶限制。
- `requests_per_minute` **int** - 设置每分钟的请求漏桶限制。
- `max_retries` **int** - 使用的最大重试次数。
- `max_retry_wait` **float** - 最大退避时间。
- `sleep_on_rate_limit_recommendation` **bool** - 是否遵守休眠建议（Azure）。
- `concurrent_requests` **int** - 允许同时打开的请求数。
- `temperature` **float** - 要使用的温度。
- `top_p` **float** - 要使用的top-p值。
- `n` **int** - 要生成的完成数。

## parallelization

### 字段

- `stagger` **float** - 线程间隔值。
- `num_threads` **int** - 最大工作线程数。

## async_mode

**asyncio|threaded** 要使用的异步模式。可以是 `asyncio` 或 `threaded`。

## embeddings

### 字段

- `llm`（参见LLM顶级配置）
- `parallelization`（参见Parallelization顶级配置）
- `async_mode`（参见Async Mode顶级配置）
- `batch_size` **int** - 要使用的最大批处理大小。
- `batch_max_tokens` **int** - 最大批处理的令牌数量。
- `target` **required|all** - 确定要发出的嵌入集。
- `skip` **list[str]** - 要跳过的嵌入。
- `strategy` **dict** - 完全覆盖文本嵌入策略。

## chunks

### 字段



- `size` **int** - 每个块的最大令牌数量。
- `overlap` **int** - 块之间的重叠令牌数量。
- `group_by_columns` **list[str]** - 在分块之前按字段对文档进行分组。
- `strategy` **dict** - 完全覆盖分块策略。

## cache

### 字段

- `type` **file|memory|none|blob** - 要使用的缓存类型。默认值=`file`
- `connection_string` **str** - (仅限blob) Azure存储连接字符串。
- `container_name` **str** - (仅限blob) Azure存储容器名。
- `base_dir` **str** - 相对于根目录的基本目录，用于写入缓存。
- `storage_account_blob_url` **str** - 要使用的存储帐户Blob URL。

## storage

### 字段

- `type` **file|memory|blob** - 要使用的存储类型。默认值=`file`
- `connection_string` **str** - (仅限blob) Azure存储连接字符串。
- `container_name` **str** - (仅限blob) Azure存储容器名。
- `base_dir` **str** - 相对于根目录的基本目录，用于写入报告。
- `storage_account_blob_url` **str** - 要使用的存储帐户Blob URL。

## reporting

### 字段

- `type` **file|console|blob** - 要使用的报告类型。默认值=`file`
- `connection_string` **str** - (仅限blob) Azure存储连接字符串。
- `container_name` **str** - (仅限blob) Azure存储容器名。
- `base_dir` **str** - 相对于根目录的基本目录，用于写入报告。
- `storage_account_blob_url` **str** - 要使用的存储帐户Blob URL。

## entity_extraction

### 字段

- `llm` (请参阅LLM顶级配置)
- `parallelization` (请参阅并行化顶级配置)
- `async_mode` (请参阅异步模式顶级配置)
- `prompt` **str** - 要使用的提示文件。
- `entity_types` **list[str]** - 要识别的实体类型。
- `max_gleanings` **int** - 要使用的最大获取周期数。
- `strategy` **dict** - 完全覆盖实体抽取策略。

## summarize_descriptions

### 字段

- `llm` (请参阅LLM顶级配置)
- `parallelization` (请参阅并行化顶级配置)
- `async_mode` (请参阅异步模式顶级配置)
- `prompt` **str** - 要使用的提示文件。
- `max_length` **int** - 每个摘要的最大输出令牌数量。
- `strategy` **dict** - 完全覆盖摘要描述策略。

## claim_extraction

### 字段

- `enabled` **bool** - 是否启用索赔提取。默认值=False
- `llm` (请参阅LLM顶级配置)
- `parallelization` (请参阅并行化顶级配置)
- `async_mode` (请参阅异步模式顶级配置)
- `prompt` **str** - 要使用的提示文件。
- `description` **str** - 描述我们想要提取的索赔类型。
- `max_gleanings` **int** - 要使用的最大获取周期数。
- `strategy` **dict** - 完全覆盖索赔提取策略。

## community_reports

### 字段

- `llm` (请参阅LLM顶级配置)
- `parallelization` (请参阅并行化顶级配置)
- `async_mode` (请参阅异步模式顶级配置)
- `prompt` **str** - 要使用的提示文件。
- `max_length` **int** - 每个报告的最大输出令牌数量。
- `max_input_length` **int** - 生成报告时要使用的最大输入令牌数量。
- `strategy` **dict** - 完全覆盖社区报告策略。

## cluster_graph

### 字段

- `max_cluster_size` **int** - 要生成的最大聚类大小。
- `strategy` **dict** - 完全覆盖聚类图策略。

## embed_graph

### 字段




- `enabled` **bool** - 是否启用图嵌入。
- `num_walks` **int** - node2vec 的行走次数。
- `walk_length` **int** - node2vec 的行走长度。
- `window_size` **int** - node2vec 的窗口大小。
- `iterations` **int** - node2vec 的迭代次数。
- `random_seed` **int** - node2vec 的随机种子。
- `strategy` **dict** - 完全覆盖嵌入图的策略。

## umap

### Fields

- `enabled` **bool** - 是否启用 UMAP 布局。

## snapshots

### Fields

- `graphml` **bool** - 生成 graphml 快照。
- `raw_entities` **bool** - 生成原始实体快照。
- `top_level_nodes` **bool** - 生成顶级节点快照。

## encoding_model

**str** - 要使用的文本编码模型。默认为 `cl100k_base`。

## skip_workflows



**list[str]** - 要跳过的工作流名称列表。
