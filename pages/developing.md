# 开发 GraphRAG

## 环境配置

| 名称                  | 安装                                           | 目的                                                                                |
| ------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| Python 3.10-3.12    | [下载](https://www.python.org/downloads/)                  | 该库基于 Python 开发。                                                                   |
| Poetry              | [使用说明](https://python-poetry.org/docs/#installation) | Poetry 用于 Python 代码库的包管理和虚拟环境管理。 |

# 入门指南

## 安装依赖

```sh
# 安装Python依赖。
poetry install
```

## 执行索引引擎

```sh
poetry run poe index <...args>
```

## 执行查询

```sh
poetry run poe query <...args>
```

# Azurite

某些单元测试和冒烟测试使用Azurite模拟Azure资源。可以通过以下命令启动：

```sh
./scripts/start-azurite.sh
```

或者在终端中直接运行`azurite`（如果已全局安装）。有关安装和使用Azurite的更多信息，请参阅[Azurite文档](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite)。

# 生命周期脚本

我们的Python包使用Poetry来管理依赖关系，并使用[poethepoet](https://pypi.org/project/poethepoet/)来管理构建脚本。

可用的脚本有：

- `poetry run poe index` - 运行索引CLI
- `poetry run poe query` - 运行查询CLI
- `poetry build` - 调用`poetry build`，将构建一个wheel文件和其他可分发的构件。
- `poetry run poe test` - 执行所有测试。
- `poetry run poe test_unit` - 执行单元测试。
- `poetry run poe test_integration` - 执行集成测试。
- `poetry run poe test_smoke` - 执行冒烟测试。
- `poetry run poe check` - 对包进行一系列静态检查，包括：
  - 格式化
  - 文档格式化
  - 代码检查
  - 安全模式
  - 类型检查
- `poetry run poe fix` - 对包应用任何可用的自动修复。通常这只是格式修复。
- `poetry run poe fix_unsafe` - 对包应用任何可用的自动修复，包括那些可能不安全的修复。
- `poetry run poe format` - 明确地在包中运行格式化程序。

## 故障排除

### 运行`poetry install`时出现"RuntimeError: llvm-config failed executing, please point LLVM_CONFIG to the path for llvm-config"错误

确保已安装llvm-9和llvm-9-dev：

`sudo apt-get install llvm-9 llvm-9-dev`

然后在bashrc中添加以下内容：

`export LLVM_CONFIG=/usr/bin/llvm-config-9`

### 运行`poetry install`时出现"numba/\_pymodule.h:6:10: fatal error: Python.h: No such file or directory"错误

确保已安装python3.10-dev或更一般的`python<version>-dev`：

`sudo apt-get install python3.10-dev`

### LLM调用不断超过TPM、RPM或时间限制


`GRAPHRAG_LLM_THREAD_COUNT`和`GRAPHRAG_EMBEDDING_THREAD_COUNT`的默认值都设置为50。你可以修改这些值以减少并发性。请参考[配置文档](../config/overview)。
