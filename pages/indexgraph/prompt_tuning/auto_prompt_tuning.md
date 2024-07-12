# 自动Prompt调优⚙️

GraphRAG 提供了创建领域适应性模板用于生成知识图的功能。这一步骤是可选的，但建议运行它，因为在执行索引运行时会产生更好的结果。

模板是通过加载输入，将它们分割成块（文本单元），然后运行一系列 LLM 调用和模板替换来生成最终的提示。我们建议使用脚本提供的默认值，但本页面中你将找到每个值的详细信息，以便在需要进一步探索和调整模板生成算法时使用。

## 先决条件

在运行自动模板生成之前，请确保你已经使用 `graphrag.index --init` 命令初始化了你的工作区。这将创建必要的配置文件和默认提示。有关初始化过程的更多信息，请参阅 [初始化文档](/posts/config/init)。

## 用法

你可以在命令行上使用各种选项运行主脚本：

```bash
python -m graphrag.prompt_tune [--root ROOT] [--domain DOMAIN]  [--method METHOD] [--limit LIMIT] [--language LANGUAGE] [--max-tokens MAX_TOKENS] [--chunk-size CHUNK_SIZE] [--no-entity-types] [--output OUTPUT]
```

## 命令行选项

- `--root`（可选）：数据项目根目录，包括配置文件（YML、JSON或.env）。默认为当前目录。

- `--domain`（可选）：与你的输入数据相关的领域，例如'太空科学'、'微生物学'或'环境新闻'。如果留空，将从输入数据中推断领域。

- `--method`（可选）：选择文档的方法。选项为all、random或top。默认为random。

- `--limit`（可选）：在使用随机或top选择时加载文本单元的限制。默认为15。

- `--language`（可选）：用于输入处理的语言。如果与输入的语言不同，LLM将进行翻译。默认为""，表示将自动从输入中检测语言。

- `--max-tokens`（可选）：提示生成的最大标记数。默认为2000。

- `--chunk-size`（可选）：用于从输入文档生成文本单元的标记大小。默认为200。

- `--no-entity-types`（可选）：使用未标记的实体提取生成。当你的数据涵盖许多主题或高度随机化时，我们建议使用此选项。

- `--output`（可选）：保存生成的提示的文件夹。默认为"prompts"。

## 示例用法

```bash
python -m graphrag.prompt_tune --root /path/to/project --domain "环境新闻" --method random --limit 10 --language English --max-tokens 2048 --chunk-size 256 --no-entity-types --output /path/to/output
```

或者，只进行最小配置（建议）：

```bash
python -m graphrag.prompt_tune --root /path/to/project --no-entity-types
```

## 文档选择方法

自动模板功能输入数据，然后将其分割成块大小的文本单元。
然后，它使用以下选择方法之一选择一个样本进行模板生成：

- `random`：随机选择文本单元。这是默认和推荐的选项。
- `top`：选择前n个文本单元。
- `all`：使用所有文本单元进行生成。仅适用于小型数据集；通常不推荐此选项。

## 修改Env变量



运行自动模板化后，你应该修改以下环境变量（或配置变量）以使用索引运行中的新提示。注意：请确保更新生成的提示的正确路径，在此示例中，我们使用默认的 "prompts" 路径。

- `GRAPHRAG_ENTITY_EXTRACTION_PROMPT_FILE` = "prompts/entity_extraction.txt"

- `GRAPHRAG_COMMUNITY_REPORT_PROMPT_FILE` = "prompts/community_report.txt"

- `GRAPHRAG_SUMMARIZE_DESCRIPTIONS_PROMPT_FILE` = "prompts/summarize_descriptions.txt"
