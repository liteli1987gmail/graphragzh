
# Indexer CLI

GraphRAG 索引器 CLI 允许无代码使用 GraphRAG 索引器。

```bash
python -m graphrag.index --verbose --root </workspace/project/root> --config <custom_config.yml>
--resume <timestamp> --reporter <rich|print|none> --emit json,csv,parquet
--nocache
```

## CLI 参数
--verbose（-v）——在运行过程中添加额外的日志信息。
--root <data-project-dir> ——数据根目录。这个目录应该包含一个带有输入数据的“input”目录，以及一个带有环境变量的“.env”文件。这些在下面进行了描述。
--init ——这将使用引导配置和提示覆盖来初始化指定根目录的数据项目目录。
--resume <output-timestamp> ——如果指定，则流水线将尝试恢复先前的运行。来自先前运行的 Parquet 文件将作为输入加载到系统中，并且生成这些文件的工作流将被跳过。输入值应为时间戳的输出文件夹，例如“20240105-143721”。
--config <config_file.yml> ——这将选择退出默认配置模式并执行自定义配置。如果使用此选项，则下面的环境变量都不适用。
--reporter <reporter> ——这将指定要使用的进度报告器。默认值为“rich”。有效值为“rich”、“print”和“none”。
--emit <types> ——这将指定流水线应该输出的表格输出格式。默认值为“parquet”。有效值为“parquet”、“csv”和“json”，以逗号分隔。
--nocache ——这将禁用缓存机制。这对于调试和开发很有用，但不应在生产中使用。
