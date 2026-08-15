---
type: Concept
title: "语义层"
description: "位于数据存储（数据仓库/数据湖）与分析消费层（BI、AI Agent、指标平台）之间的统一业务语义抽象，提供单一可信源。"
tags: [semantic-layer, metrics, analytics]
sources:
  - { id: juejin-ossie, resource: ../sources/apache-ossie-juejin-overview.md, title: "每天一个开源项目#40 Apache Ossie", last_modified: 2026-07-17 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-14T00:00:00Z }
status: stable
stale_after: 2027-01-01
---

# 语义层

## 定义
位于原始数据存储与分析/消费层之间的一层统一业务语义抽象：用一份可交换的"业务定义"表达指标、关系、字段与 AI 消费约束，避免同一口径在多个工具中被重复、不一致地定义。[^juejin-ossie]

## 为什么需要
现代数据栈中"收入"这类指标可能在 dbt、Snowflake、Salesforce、Omni 与 AI 问数助手中各写一遍，过滤条件、时间口径或关联路径稍有差异，仪表盘互相打架，Agent 在冲突上下文中生成看似合理实则错误的 SQL。语义层的价值在于提供厂商中立、单一可信源。[^juejin-ossie]

## 在 Ossie 中的体现
Ossie 把数据集、字段、关系、指标、SQL 方言与 AI 上下文组织进一个可版本化、可验证的中间表示，作为语义交换枢纽。语义层规范对象：`datasets` / `fields` / `relationships` / `metrics` / `ai_context` / `custom_extensions`。[^juejin-ossie]

## 与相邻抽象的关系
- Parquet/Arrow 统一数据存储格式，ODBC/JDBC 统一访问接口，[语义元数据交换](/concepts/semantic-metadata-interchange.md)（语义层的交换协议）瞄准更上层的业务含义与指标逻辑。[^juejin-ossie]
- 语义层不是查询引擎：Ossie 负责定义、交换与验证，不连接数据库执行查询。[^juejin-ossie]

## 相关概念
- [语义元数据交换](/concepts/semantic-metadata-interchange.md)
- [Hub-and-Spoke 转换架构](/concepts/hub-and-spoke.md)
- [AI Context](/concepts/ai-context.md)
- [Apache Ossie](/entities/apache-ossie.md)

## 未解问题
- 指标粒度、关系基数、复合键业务语义、跨数据集安全聚合仍缺规范定义与 conformance suite。[^juejin-ossie]

[^juejin-ossie]: 每天一个开源项目#40 Apache Ossie（掘金，2026-07-17）