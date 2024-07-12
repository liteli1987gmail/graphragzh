# 初始化GraphRAG索引

要开始使用 GraphRAG，你需要配置系统。`init` 命令是最简单的方法。它将在指定的目录中创建 `.env` 和 `settings.yaml` 文件，其中包含必要的配置设置。它还会输出 GraphRAG 使用的默认 LLM 提示。

## 用法

```sh
python -m graphrag.index [--init] [--root PATH]
```

## 选项

- `--init` - 使用必要的配置文件进行初始化。
- `--root PATH` - 要初始化的根目录。默认为当前目录。

## 示例

```sh
python -m graphrag.index --init --root ./ragtest
```

## 输出

`init`命令将在指定的目录中创建以下文件：

- `settings.yaml` - 配置设置文件。此文件包含GraphRAG的配置设置。
- `.env` - 环境变量文件。这些变量在`settings.yaml`文件中引用。
- `prompts/` - LLM提示文件夹。这包含了GraphRAG使用的默认提示，你可以修改它们或运行[自动提示调优](/indexgraph/prompt_tuning/auto_prompt_tuning)命令以生成适应你的数据的新提示。

## 下一步操作
在初始化你的工作区之后，你可以运行[Prompt Tuning](/indexgraph/prompt_tuning/auto_prompt_tuning)命令来使提示适应你的数据，甚至可以开始运行[Indexing Pipeline](/indexgraph/overview)来索引你的数据。有关配置GraphRAG的更多信息，请参阅[Configuration](/indexgraph/config/overview)文档。