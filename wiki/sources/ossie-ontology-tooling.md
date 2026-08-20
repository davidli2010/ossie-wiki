---
type: Source Summary
title: "Apache Ossie 本体（ontology）规范与示例"
description: "Ossie ontology 体系：ontology.md 规范正文、ontology.json 机器可读 schema、flights.yaml 本体示例（含 ValueType/requires 约束断言）与 OBML→OSI ontology 导出边界。"
resource: https://github.com/apache/ossie/tree/main/ontology
tags: [ossie, ontology, spec, value-type]
sources:
  - { id: ontology-json, resource: ../../raw/ossie-ontology.json, title: "ontology/ontology.json", last_modified: 2026-08-15 }
  - { id: example-flights, resource: ../../raw/ossie-example-flights.yaml, title: "examples/flights.yaml", last_modified: 2026-08-15 }
  - { id: orionbelt-ontomapping, resource: ../../raw/ossie-orionbelt-ontology-mapping-analysis.md, title: "OBML → OSI Ontology Mapping Analysis", last_modified: 2026-08-15 }
usage_window: { from: 2026-08-15, to: 2026-08-15 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 本体（ontology）规范与示例

> 快照 2026-08-15（GitHub main 分支）。raw/ossie-ontology-spec.md（本体规范正文）另有摄入；本文侧重机器可读 schema 与示例落点。

## ontology.json 结构（0.2.0.dev0）

- Draft 2020-12 schema，`$id` 指向 apache/ossie 仓库；必填 `version`/`name`/`ontology`，`additionalProperties: false`。[^ontology-json]
- 顶层构件：`ai_context`（可引用 core-spec 的 `$defs/AIContext`）、`requires`（约束人口数量的 Expression 数组）、`ontology`（OntologyComponent 数组，minItems 1）、`ontology_mappings`（logical model 到概念的本体映射）。[^ontology-json]
- OntologyComponent：单个 `concept` + 以该 concept 为主键的关系；含 `type`（ConceptType）。[^ontology-json]
- ontology_mappings：概念映射（object_mappings/`expression`）与链接映射（link_mappings）、`semantic_model` 嵌入。[^ontology-json]

## flights.yaml —— 官方本体示例

- 根字段即 ontology 文档结构：`version/name/description/requires/ontology`（外加 `ontology_mappings`）。[^example-flights]
- **ValueType 是核心抽象**：`NrFeet extends [Decimal]`、`Distance extends [NrMiles]`、`Capacity extends [NrPounds]` 等，通过继承叠加语义约束。[^example-flights]
- **`requires` 约束断言**是本体层的校验表达式：`COUNT[Airport] > 0`、`DegreesLatitude <= 90`、`CancelationCode == 'A' OR 'B' OR 'C' OR 'D'`、`1 <= DistanceGroup <= 10`，表达在物理数据上施加的业务不变量。[^example-flights]
- flight ontology 命名域：Airport/Carrier/fleet（含 RunwayGeometry/runway crafting）等实体概念 + 上述值概念。[^example-flights]

## OBML → OSI ontology 导出的边界（OrionBelt 实证）

- 本本体示例模式与 OrionBelt 的 ontology 导出遵循同一体系（EntityType 概念、relationship role/multiplicity、object/link mapping）。[^orionbelt-ontomapping]
- 已知缺口：ValueType 建模（flights 用 ValueType 定义单元类型）在 OBML 导出中被 deferred；measures/metrics 不进入 ontology 层。[^orionbelt-ontomapping]

## 相关实体
- [Apache Ossie](../entities/apache-ossie.md)

## 引入/强化的概念
- [语义元数据交换](../concepts/semantic-metadata-interchange.md)（本体层是结构性互操作之上的概念互操作）

## 与已有知识的关联
- 本体规范正文（raw/ossie-ontology-spec.md）已汇总概念类型/内置概念/多重性；本页补充 schema 与示例细节。[^ontology-json]

[^example-flights]: examples/flights.yaml
[^ontology-json]: ontology/ontology.json
[^orionbelt-ontomapping]: OBML → OSI Ontology Mapping Analysis
