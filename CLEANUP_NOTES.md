# NL2SQL Server 精简说明

本副本从原始 `DAIL-SQL_server.zip` 筛选文件而来，没有改动任何 Python 源代码。

## 保留功能

保留服务器直接执行 `/api/v1/text-to-sql` 的 OpenAI 调用，以及 VS Code Plugin 所需的 `/api/v1/tokenize`、`/api/v1/generate-prompt`、`/api/v1/validate-sql`、`/api/v1/encode`、`/api/v1/upload-database`、`/api/v1/databases` 和 `/api/v1/health`。同时保留 Schema Linking、Few-shot 选择器、SQL Validator、SQLite 数据库上传，以及生成 masked cache 的脚本与训练数据。大型第三方 Stanford CoreNLP 二进制文件因体积过大没有打包，需从原始 Server 目录复制 `third_party/` 到本副本后再使用。

## 排除内容

历史 ask_llm* 独立实验脚本、旧测试/结果、历史 embedding_cache 和 simple_cache、.git/.github、__pycache__、旧 shell 脚本、配置/实验临时文件，以及需要手动复制回来的大型 third_party/。由于 `.env` 和 `mysql_config.json` 可能包含密钥，出于安全考虑不包含。VS Code 插件不随 Server 压缩包打包。

## 已知限制（原始项目已有，不是精简导致）

1. 原始 ZIP 中不存在 `vector_cache/masked_embeddings_cache.npz`。当前 `MASKED_CACHED` 选择器需要该文件，可在安装依赖后从项目根目录执行 `python create_masked_cache.py` 生成；否则服务器可能回退到简化 Prompt。
2. 原始 ZIP 中也不存在 `dataset/spider/tables.json` 和 Spider 数据库目录，因此 Spider 数据库直接查询不能视作开箱即用。`/api/v1/text-to-sql` 必须指向可加载的数据库，例如通过 `/api/v1/upload-database` 导入 SQLite 数据库。
3. `utils/data_builder.py` 的 `get_tables()` 中有硬编码的绝对 Windows 路径。此处保持原样，避免无测试的功能改写，但后续应修复。
4. 本副本尚未完成 CoreNLP、LLM、SQLite、MySQL、PostgreSQL 与插件的端到端运行验证。Python 源码做了静态语法检查。
5. 请从项目根目录运行 `python api_server.py`，并自行设置 `.env` 环境变量。保留原始 `requirements*.txt`，未擅自变更依赖版本。

## 说明

目前默认选择器仍需 `train_spider.json`、CoreNLP 和模型依赖。请一定从原始目录复制 `third_party/stanford-corenlp-full-2018-10-05/` 到精简后的 Server 目录。切勿将它们视为可随意删除的历史实验资料。
