

# 手动提示调优⚙️

默认情况下，GraphRAG 索引器会使用一些旨在在广泛的知识发现环境中运行良好的提示。然而，通常希望调整提示以更好地适应你的特定用例。我们提供了一种方法，允许你指定自定义提示文件，该文件将在内部使用一系列的标记替换。

可以通过编写纯文本的自定义提示文件来覆盖这些提示。我们使用形式为 `{token_name}` 的标记替换，并且可用标记的描述如下所示。

## 实体/关系提取

[提示来源](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/graph/prompts.py)

### 标记（由提取器提供的值）

- **{input_text}** - 要处理的输入文本。
- **{entity_types}** - 实体类型的列表
- **{tuple_delimiter}** - 用于在元组内分隔值的分隔符。单个元组用于表示单个实体或关系。
- **{record_delimiter}** - 用于分隔元组实例的分隔符。
- **{completion_delimiter}** - 用于指示生成完成的指示器。

## 摘要实体/关系描述

[提示来源](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/summarize/prompts.py)

### 标记（由提取器提供的值）

- **{entity_name}** - 实体的名称或关系的源/目标对。
- **{description_list}** - 实体或关系的描述列表。

## 提取主张

[提示来源](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/claims/prompts.py)

### 标记（由提取器提供的值）

- **{input_text}** - 要处理的输入文本。
- **{tuple_delimiter}** - 用于在元组内分隔值的分隔符。单个元组用于表示单个实体或关系。
- **{record_delimiter}** - 用于分隔元组实例的分隔符。
- **{completion_delimiter}** - 用于指示生成完成的指示器。

注意：在主张提取中使用了 `Claim Description` 的额外参数。默认值为

`"任何可能与信息发现相关的主张或事实。"`

有关如何更改此配置的详细信息，请参阅 [配置文档](/indexgraph/config/overview/)。

## 生成社区结构报告

[提示来源](http://github.com/microsoft/graphrag/blob/main/graphrag/index/graph/extractors/community_reports/prompts.py)

### 标记（由提取器提供的值）
