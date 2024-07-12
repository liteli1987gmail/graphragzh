# 问题生成 ❔

## 基于实体的问题生成

[问题生成](https://github.com/microsoft/graphrag/blob/main//graphrag/query/question_gen/) 方法将知识图谱中的结构化数据与输入文档中的非结构化数据结合起来，生成与特定实体相关的候选问题。

## 方法论
给定一组先前的用户问题，问题生成方法使用与 [本地搜索](1-local_search) 中使用的上下文构建方法相同的方法，提取和优先考虑相关的结构化和非结构化数据，包括实体、关系、协变量、社区报告和原始文本块。然后将这些数据记录适配到一个单一的 LLM 提示中，以生成代表数据中最重要或紧急信息内容或主题的候选后续问题。

## 配置

以下是 [问题生成类](https://github.com/microsoft/graphrag/blob/main//graphrag/query/question_gen/local_gen.py) 的关键参数：
* `llm`：用于响应生成的 OpenAI 模型对象
* `context_builder`：用于从知识模型对象集合中准备上下文数据的 [上下文构建器](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/mixed_context.py) 对象，使用与本地搜索相同的上下文构建器类
* `system_prompt`：用于生成候选问题的提示模板。默认模板可以在 [system_prompt](https://github.com/microsoft/graphrag/blob/main//graphrag/query/question_gen/system_prompt.py) 中找到
* `llm_params`：要传递给 LLM 调用的其他参数（例如，温度、最大令牌数）的字典
* `context_builder_params`：在构建问题生成提示的上下文时，要传递给 [`context_builder`](https://github.com/microsoft/graphrag/blob/main//graphrag/query/structured_search/local_search/mixed_context.py) 对象的其他参数的字典
* `callbacks`：可选的回调函数，可用于为 LLM 的完成流事件提供自定义事件处理程序

## 如何使用


示例问题生成函数的代码可以在以下 [笔记本](https://microsoft.github.io/graphrag/posts/query/notebooks/local_search_nb) 中找到。
