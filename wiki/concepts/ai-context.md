---
type: Concept
title: "AI Context"
description: "Ossie 语义模型中的结构化元数据字段（instructions / synonyms / examples），为 Text-to-SQL 与分析 Agent 提供消歧、约束与示范。"
tags: [ai-context, text-to-sql, governance, llm]
sources:
  - { id: juejin-ossie, resource: ../sources/apache-ossie-juejin-overview.md, title: "每天一个开源项目#40 Apache Ossie", last_modified: 2026-07-17 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-14T00:00:00Z }
status: stable
stale_after: 2027-01-01
---

# AI Context

## 定义
`ai_context` 可挂在模型、数据集、字段、关系和指标等层级，内容既可以是文本，也可以是结构化的 `instructions`、`synonyms` 与 `examples`。它是 AI 语义治理接口，而非注释。[^juejin-ossie]

## 三层价值
- **消歧**：把"GMV、成交额、销售额"等自然语言映射到认证指标；
- **约束**：告诉模型某字段什么场景该用、什么场景不能用；
- **示范**：用已审核问题或查询给 Agent 提供检索与生成锚点。[^juejin-ossie]

## 边界与风险
这些字段只是**可传递的上下文元数据**，不自动构成权限控制或防幻觉机制。路线图中的 verified queries、暴露控制、认证与治理钩子尚未完全落地；生产系统的硬约束仍需落在查询网关、数据权限与结果校验层。[^juejin-ossie]

## 相关概念
- [语义层](/concepts/semantic-layer.md)
- [语义元数据交换](/concepts/semantic-metadata-interchange.md)
- [Apache Ossie](/entities/apache-ossie.md)

[^juejin-ossie]: 每天一个开源项目#40 Apache Ossie（掘金，2026-07-17）