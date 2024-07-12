

# 本地搜索 🔎

## 基于实体的推理

[本地搜索](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/) 方法在查询时将知识图谱的结构化数据与输入文档的非结构化数据相结合, 以相关实体信息增强 LLM 上下文。这种方法非常适合回答需要理解输入文档中提到的特定实体的问题(例如, "洋甘菊的治疗特性是什么?")。

## 方法论

![](/img/1-local_search.png)

给定用户查询和可选的对话历史, 本地搜索方法从知识图谱中识别出与用户输入在语义上相关的一组实体。这些实体作为进入知识图谱的接入点, 能够提取更多相关细节, 如连接的实体、关系、实体协变量和社区报告。此外, 它还从与识别的实体相关的原始输入文档中提取相关文本块。然后对这些候选数据源进行优先排序和过滤, 以适应预定义大小的单一上下文窗口, 用于生成对用户查询的响应。

## 配置

以下是 [LocalSearch 类](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/search.py) 的关键参数:
* `llm`: 用于生成响应的 OpenAI 模型对象
* `context_builder`: 用于从知识模型对象集合准备上下文数据的 [上下文构建器](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/mixed_context.py) 对象
* `system_prompt`: 用于生成搜索响应的提示模板。默认模板可在 [system_prompt](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/system_prompt.py) 中找到
* `response_type`: 描述所需响应类型和格式的自由文本(例如, `多个段落`, `多页报告`)
* `llm_params`: 要传递给 LLM 调用的其他参数(如温度、最大标记数)的字典
* `context_builder_params`: 在构建搜索提示的上下文时传递给 [`context_builder`](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/mixed_context.py) 对象的其他参数字典
* `callbacks`: 可选的回调函数, 可用于为 LLM 的完成流事件提供自定义事件处理程序

## 使用方法

本地搜索场景的示例可以在以下 [笔记本](https://microsoft.github.io/graphrag/posts/query/notebooks/local_search_nb) 中找到。


