

# 查询 CLI

GraphRAG 查询 CLI 允许无代码使用 GraphRAG 查询引擎。

```bash
python -m graphrag.query --data <path-to-data> --community_level <comunit-level> --response_type <response-type> --method <"local"|"global"> <query>
```

## CLI 参数

- `--data <path-to-data>` - 包含从运行索引器的`.parquet`输出文件的文件夹。
- `--community_level <community-level>` - 社区层次在Leiden社区层次结构中，从这个层级加载社区报告，较大的值表示我们使用较小社区的报告。默认值：2
- `--response_type <response-type>` - 描述响应类型和格式的自由文本，可以是任何内容，例如`多段落`，`单段落`，`单句`，`3-7点的列表`，`单页`，`多页报告`。默认值：`多段落`。
- `--method <"local"|"global">` - 用于回答查询的方法，其中之一为local或global。有关更多信息，请参阅[概览](overview.md)

## 环境变量



需要的环境变量来执行：
- `GRAPHRAG_API_KEY` - 用于执行模型的API密钥，如果没有提供，则将回退到`OPENAI_API_KEY`。
- `GRAPHRAG_LLM_MODEL` - 要用于聊天完成的模型。
- `GRAPHRAG_EMBEDDING_MODEL` - 要用于嵌入的模型。

你还可以通过提供这些环境变量进一步自定义执行：

- `GRAPHRAG_LLM_API_BASE` - API基本URL。默认值：`None`
- `GRAPHRAG_LLM_TYPE` - LLM操作类型。可以是`openai_chat`或`azure_openai_chat`。默认值：`openai_chat`
- `GRAPHRAG_LLM_MAX_RETRIES` - 请求失败时尝试的最大重试次数。默认值：`20`
- `GRAPHRAG_EMBEDDING_API_BASE` - API基本URL。默认值：`None`
- `GRAPHRAG_EMBEDDING_TYPE` - 要使用的嵌入客户端。可以是`openai_embedding`或`azure_openai_embedding`。默认值：`openai_embedding`
- `GRAPHRAG_EMBEDDING_MAX_RETRIES` - 请求失败时尝试的最大重试次数。默认值：`20`
- `GRAPHRAG_LOCAL_SEARCH_TEXT_UNIT_PROP` - 上下文窗口用于相关文本单位的比例。默认值：`0.5`
- `GRAPHRAG_LOCAL_SEARCH_COMMUNITY_PROP` - 上下文窗口用于社区报告的比例。默认值：`0.1`
- `GRAPHRAG_LOCAL_SEARCH_CONVERSATION_HISTORY_MAX_TURNS` - 包括在对话历史中的最大轮次数。默认值：`5`
- `GRAPHRAG_LOCAL_SEARCH_TOP_K_ENTITIES` - 从实体描述嵌入存储中检索的相关实体数。默认值：`10`
- `GRAPHRAG_LOCAL_SEARCH_TOP_K_RELATIONSHIPS` - 控制将多少个非网络关系引入上下文窗口。默认值：`10`
- `GRAPHRAG_LOCAL_SEARCH_MAX_TOKENS` - 根据你的模型的标记限制进行更改（如果你使用的模型具有8k限制，则好的设置可能是5000）。默认值：`12000`
- `GRAPHRAG_LOCAL_SEARCH_LLM_MAX_TOKENS` - 根据你的模型的标记限制进行更改（如果你使用的模型具有8k限制，则好的设置可能为1000=1500）。默认值：`2000`
- `GRAPHRAG_GLOBAL_SEARCH_MAX_TOKENS` - 根据你的模型的标记限制进行更改（如果你使用的模型具有8k限制，则好的设置可能为5000）。默认值：`12000`
- `GRAPHRAG_GLOBAL_SEARCH_DATA_MAX_TOKENS` - 根据你的模型的标记限制进行更改（如果你使用的模型具有8k限制，则好的设置可能为5000）。默认值：`12000`
- `GRAPHRAG_GLOBAL_SEARCH_MAP_MAX_TOKENS` - 默认值：`500`
- `GRAPHRAG_GLOBAL_SEARCH_REDUCE_MAX_TOKENS` - 根据你的模型的标记限制进行更改（如果你使用的模型具有8k限制，则好的设置可能为1000-1500）。默认值：`2000`
- `GRAPHRAG_GLOBAL_SEARCH_CONCURRENCY` - 默认值：`32`


