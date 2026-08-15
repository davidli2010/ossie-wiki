---
type: Concept
title: "语义元数据交换"
description: "在分析、AI 与 BI 平台间标准化交换语义元数据（数据集、字段、关系、指标、AI 上下文）的规范方式，目标是厂商中立、单一可信源。"
tags: [interchange, standard, interoperability, metadata]
sources:
  - { id: juejin-ossie, resource: ../sources/apache-ossie-juejin-overview.md, title: "每天一个开源项目#40 Apache Ossie", last_modified: 2026-07-17 }
  - { id: ontology-tooling, resource: ../sources/ossie-ontology-tooling.md, title: "Apache Ossie 本体规范与示例", last_modified: 2026-08-15 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# 语义元数据交换

## 定义
用一套厂商中立的中间表示（Ossie 的 YAML/JSON 规范）在分析、AI 与 BI 平台之间交换语义元数据：数据集、字段、关系、指标、多 SQL 方言表达式与 AI 上下文。Ossie 正是这一努力的 Apache 孵化规范。[^juejin-ossie]

## 与既有标准的层级关系
| 层级 | 既有标准 | Ossie 位置 |
| --- | --- | --- |
| 数据格式 | Parquet / Arrow | 更上层 |
| 访问接口 | ODBC / JDBC | 更上层 |
| 业务含义与指标逻辑 | 无既有标准 | **Ossie 瞄准这里** |[^juejin-ossie]

一旦中间规范被多家工具接受，企业不必为每两套产品维护点对点映射，AI Agent 也能拿到经治理的指标定义、同义词与使用约束。

## 关键机制
- 多 SQL 方言表达式（ANSI_SQL / SNOWFLAKE / DATABRICKS / BIGQUERY / MDX / TABLEAU / MAQL），回退顺序"目标方言 > ANSI_SQL > 警告/失败"。[^juejin-ossie]
- `custom_extensions` 承载核心规范无法表达的厂商私有信息，管理 round-trip 的信息损失。[^juejin-ossie]
- 交换与分发经 [Hub-and-Spoke 转换架构](/concepts/hub-and-spoke.md) 完成。[^juejin-ossie]
- **两层互操作**：核心 spec 解决结构性互操作（任何工具都能读写公共格式）；**本体层（ontology）** 解决概念互操作——独立于物理数据布局定义业务概念（Customer/Order/Product 与 ValueType 单元语义），并把物理语义模型经 ontology_mappings 映射回共享定义。ontology 是独立文档类型（0.2.0.dev0），不并入 core-spec。[^ontology-tooling]
- 官方本体示例（examples/flights.yaml）用 ValueType + `requires` 表达跨模型共享的值语义与业务不变量，正是路线图中"概念互操作"的落点。[^ontology-tooling]

## 相关概念
- [语义层](/concepts/semantic-layer.md)
- [Hub-and-Spoke 转换架构](/concepts/hub-and-spoke.md)
- [AI Context](/concepts/ai-context.md)
- [Apache Ossie](/entities/apache-ossie.md)

[^juejin-ossie]: 每天一个开源项目#40 Apache Ossie（掘金，2026-07-17）
[^ontology-tooling]: [Apache Ossie 本体规范与示例](/sources/ossie-ontology-tooling.md)（2026-08-15 快照）