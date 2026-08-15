---
type: Source Summary
title: "Apache Ossie 工具链与机器可读工件（CLI/validate.py/schema/examples）"
description: "apache/ossie 仓库 cli/（Go）、validation/、core-spec/（osi-schema.json、spec.yaml）、ontology/、examples/ 与 python/ 的摘要：官方校验器、Go CLI 插件架构、JSON Schema 结构、示例模型。"
resource: https://github.com/apache/ossie/tree/main
tags: [ossie, tooling, cli, schema, validation, examples]
sources:
  - { id: ossie-cli, resource: ../../raw/ossie-cli-go.txt, title: "Go CLI 源码（cli/）", last_modified: 2026-08-15 }
  - { id: validate-py, resource: ../../raw/ossie-validate.py, title: "validation/validate.py", last_modified: 2026-08-15 }
  - { id: osi-schema-json, resource: ../../raw/ossie-osi-schema.json, title: "core-spec/osi-schema.json", last_modified: 2026-08-15 }
  - { id: core-spec-yaml, resource: ../../raw/ossie-core-spec.yaml, title: "core-spec/spec.yaml", last_modified: 2026-08-15 }
  - { id: ontology-json, resource: ../../raw/ossie-ontology.json, title: "ontology/ontology.json", last_modified: 2026-08-15 }
  - { id: example-flights, resource: ../../raw/ossie-example-flights.yaml, title: "examples/flights.yaml（ontology 示例）", last_modified: 2026-08-15 }
  - { id: example-tpcds, resource: ../../raw/ossie-example-tpcds.yaml, title: "examples/tpcds_semantic_model.yaml", last_modified: 2026-08-15 }
  - { id: python-pkg, resource: ../../raw/ossie-python-package-readme.md, title: "python/README.md（Pydantic v2 参考实现）", last_modified: 2026-08-15 }
usage_window: { from: 2026-08-15, to: 2026-08-15 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 工具链与机器可读工件

> 快照 2026-08-15（GitHub main 分支）。

## 官方校验器 validation/validate.py

- 4 层校验：JSON Schema（Draft 2020-12）→ 唯一性（dataset/field/metric/relationship 重名）→ 关系引用端点存在性 → SQL 语法（sqlglot）。[^validate-py]
- 方言映射：`ANSI_SQL`（sqlglot 默认）、`SNOWFLAKE`→snowflake、`DATABRICKS`→databricks、`BIGQUERY`→bigquery；`MDX`/`TABLEAU`/`MAQL` 无 sqlglot 方言，跳过 SQL 校验。[^validate-py]
- `--schema` 参数可指向 ontology JSON，是核心 spec 与 ontology 文档各自校验同一脚本。[^validate-py]

## Go CLI（cli/，cobra）

- 子命令：`convert`（方向互斥 `--from`/`--to`，默认输出 `./ossie-output/<plugin>/<direction>`，含 `--plugin` 路径绕过、`--timeout`、`--max-input-size`）、`validate`（`--strict` 警告转错误、`--output=text|json`）、`plugin`（install/list/remove）。[^ossie-cli]
- **2016 状态观察：`convert`、`validate`、`plugin install`、`plugin remove` 主体仍是 `not yet implemented`**；已实现的是 `plugin list` + 插件发现/注入。CLI 是「转换器通过 subprocess 插件」（plugin.yaml 声明 invoke 命令，JSON 信封 stdin/stdout 交换 `{files:{}}`/`{files,issues}`），非内嵌 Go 转换逻辑。[^ossie-cli]
- 插件目录解析：`$OSSIE_PLUGIN_DIR` 优先，否则 `~/.ossie/plugins/`；`PersistentPreRunE` 负责 `EnsurePluginDir()`。[^ossie-cli]
- plugin.yaml 必填：`ossie_plugin_spec`、`ossie_spec_version`、`name`、`convert.to_ossie.invoke`、`convert.to_ossie.accepts`、`convert.from_ossie.invoke`；`platform`/`setup` 可选。宽松解析（未知字段容忍——未来 spec 版本兼容）。[^ossie-cli]
- 设计解码：插件错误通过进程退出码 + stderr 透传；`cmd.Output()` 与显式 stderr writer 不兼容，故用手动 buffer；`exec.CommandContext` 的超时以 `ctx.Err()` 为权威信号。[^ossie-cli]

## core-spec 机器可读工件

- **osi-schema.json**：Draft 2020-12 JSON Schema；DataType 枚举是权威清单（TODO 提示 spec.yaml 需手工同步）。[^osi-schema-json]
- **spec.yaml**：人读注释版结构骨架——`semantic_model[]`（name/description/ai_context/datasets/relationships/metrics/custom_extensions）、`datasets`（name/source/primary_key/unique_keys/description/ai_context/fields/custom_extensions）、relationships（全局、字符串引用、equi-join 语义）、fields、metrics。[^core-spec-yaml]
- dialect 枚举：ANSI_SQL / SNOWFLAKE / MDX / TABLEAU / DATABRICKS / MAQL / BIGQUERY（7 个）。[^core-spec-yaml]

## ontology/ontology.json

- Galaxy 本体文档的机器可读 schema（EntityType/ValueType 概念、内置概念、关系与多重性、identify_by/derived_by/requires）。[^ontology-json]

## 示例模型

- **examples/flights.yaml**：实际是 **ontology 文档** 示例（非 core-spec 语义模型）——ValueType（NrFeet extends Decimal 等）、`ontology:` 概念列表、单元类型（feet/pounds/miles/minutes）、约束用 `requires:` 断言（如 `DegreesLatitude <= 90`、`CancelationCode == 'A' OR ...`）。[^example-flights]
- **examples/tpcds_semantic_model.yaml**：core-spec 语义模型旗舰示例——`tpcds_retail_model`，含 store_sales/date_dim/customer/item/store 数据集，各转换器的 TPC-DS 基线测试基准。[^example-tpcds]

## python/（参考实现）

- PyPI 包 `apache-ossie`（Python 3.11+，uv 构建）：Pydantic v2 模型，是**所有 Python 转换器的解析/构造/校验/序列化共享基础**。[^python-pkg]

## 相关实体
- [Apache Ossie](/entities/apache-ossie.md)

## 引入/强化的概念
- [Hub-and-Spoke 转换架构](/concepts/hub-and-spoke.md)——插件化 CLI 是 hub 的工程化载体

## 与已有知识的关联
- 与掘金概览（2026-07-17）描述的「JSON Schema + 语义引用 + sqlglot」验证链一致，validate.py 是本验证链的直接源码。[^validate-py]

[^core-spec-yaml]: core-spec/spec.yaml
[^example-flights]: examples/flights.yaml（ontology 示例）
[^example-tpcds]: examples/tpcds_semantic_model.yaml
[^ontology-json]: ontology/ontology.json
[^osi-schema-json]: core-spec/osi-schema.json
[^ossie-cli]: Go CLI 源码（cli/）
[^python-pkg]: python/README.md（Pydantic v2 参考实现）
[^validate-py]: validation/validate.py
