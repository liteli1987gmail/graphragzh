# 自定义配置模式

以下是索引引擎管道的主要配置部分。每个配置部分都可以用 Python（用于 Python API 模式）或 YAML 来表示，但为简洁起见，这里展示了 YAML。

使用自定义配置是一个高级用例。大多数用户将希望使用 [默认配置](/posts/config/overview)。

## 索引引擎示例




# 示例目录



[这里](https://github.com/microsoft/graphrag/blob/main/examples/) 是一些使用_indexing engine_和_自定义配置_的示例。



大部分示例中包含两种不同的运行流程，都包含在示例的 `run.py` 文件中。



1. 使用 Python API

2. 使用流程配置文件



要运行示例：



- 运行 `poetry shell` 激活所需的虚拟环境。

- 运行 `PYTHONPATH="$(pwd)" python examples/path_to_example/run.py` 从根目录运行。



例如，要运行 `single_verb` 示例，你需要运行以下命令：



```bash

poetry shell

```



```sh

PYTHONPATH="$(pwd)" python examples/single_verb/run.py

```



# 配置部分



# > extends



此配置允许你扩展一个或多个基础配置文件。



```yaml

# 单个基础配置

extends: ../base_config.yml

```



```yaml

# 多个基础配置

extends:

  - ../base_config.yml

  - ../base_config2.yml

```



# > root_dir



此配置允许你设置流程的根目录。所有的数据输入和输出都是相对于此路径的。



```yaml

root_dir: /workspace/data_project

```



# > storage



此配置允许你定义流程的输出策略。



- `type`: 存储类型。选项有`file`、`memory`和`blob`。

- `base_dir` (`type: file` only): 存储数据的基本目录。此目录相对于配置的根目录。

- `connection_string` (`type: blob` only): 用于blob存储的连接字符串。

- `container_name` (`type: blob` only): 用于blob存储的容器名。



# > cache



此配置允许你定义流程的缓存策略。



- `type`: 缓存类型。选项有`file`、`memory`和`blob`。

- `base_dir` (`type: file` only): 存储缓存的基本目录。此目录相对于配置的根目录。

- `connection_string` (`type: blob` only): 用于blob存储的连接字符串。

- `container_name` (`type: blob` only): 用于blob存储的容器名。



# > reporting



此配置允许你定义流程的报告策略。报告文件是生成的文档，用于总结流程的性能指标，并输出任何错误消息。



- `type`: 报告类型。选项有`file`、`memory`和`blob`。

- `base_dir` (`type: file` only): 存储报告的基本目录。此目录相对于配置的根目录。

- `connection_string` (`type: blob` only): 用于blob存储的连接字符串。

- `container_name` (`type: blob` only): 用于blob存储的容器名。



# > workflows



此配置部分定义了流程的工作流DAG。在这里，我们定义了一个工作流的数组，并在步骤中表达它们的相互依赖关系。



- `name`: 工作流的名称。这用于在配置的其他部分引用该工作流。

- `steps`: 该工作流的DataShaper步骤。如果一个步骤以`workflow:<workflow_name>`的形式定义输入，则假设它依赖于该工作流的输出。



```yaml

workflows:

  - name: workflow1

    steps:

      - verb: derive

        args:

          column1: "col1"

          column2: "col2"

  - name: workflow2

    steps:

      - verb: derive

        args:

          column1: "col1"

          column2: "col2"

        input:

          # 在此处建立依赖关系

          source: workflow:workflow1
```

# > input

- `type`: 使用的输入类型。选项为`file`或`blob`。
- `file_type`: 文件类型字段，用于区分不同的输入类型。选项为`csv`和`text`。
- `base_dir`: 从中读取输入文件的基本目录。相对于配置文件的位置。
- `file_pattern`: 用于匹配输入文件的正则表达式。正则表达式必须为文件过滤器中的每个字段指定命名组。
- `post_process`: 在执行主要工作流之前应用于输入数据的DataShaper工作流定义。
- `source_column`（仅`type: csv`）: 包含数据来源/作者的列
- `text_column`（仅`type: csv`）: 包含数据文本的列
- `timestamp_column`（仅`type: csv`）: 包含数据时间戳的列
- `timestamp_format`（仅`type: csv`）: 时间戳的格式

```yaml
input:
  type: file
  file_type: csv
  base_dir: ../data/csv # 包含CSV文件的目录，相对于配置文件的位置
  file_pattern: '.*[\/](?P<source>[^\/]+)[\/](?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})_(?P<author>[^_]+)_\d+\.csv$' # 用于匹配CSV文件的正则表达式
  # 使用文件过滤器的附加文件过滤器，使用file_pattern的命名组进一步过滤文件
  # file_filter:
  #   # source: (source_filter)
  #   year: (2023)
  #   month: (06)
  #   # day: (22)
  source_column: "author" # 包含数据来源/作者的列
  text_column: "message" # 包含数据文本的列
  timestamp_column: "date(yyyyMMddHHmmss)" # 可选，包含数据时间戳的列
  timestamp_format: "%Y%m%d%H%M%S" # 可选，时间戳的格式
  post_process: # 可选，用于在进入工作流之前处理数据的步骤集
    - verb: filter
      args:
        column: "title",
        value: "My document"
```

```yaml
input:
  type: file
  file_type: csv
  base_dir: ../data/csv # 包含CSV文件的目录，相对于配置文件的位置
  file_pattern: '.*[\/](?P<source>[^\/]+)[\/](?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})_(?P<author>[^_]+)_\d+\.csv$' # 用于匹配CSV文件的正则表达式
  # 使用文件过滤器的附加文件过滤器，使用file_pattern的命名组进一步过滤文件
  # file_filter:
  #   # source: (source_filter)
  #   year: (2023)
  #   month: (06)
  #   # day: (22)
  post_process: # 可选，用于在进入工作流之前处理数据的步骤集
    - verb: filter
      args:
        column: "title",
        value: "My document"
```
