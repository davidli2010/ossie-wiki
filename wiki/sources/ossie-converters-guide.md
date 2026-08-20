---
type: Source Summary
title: "Apache Ossie 转换器指南（converters/README.md）"
description: "converters/ 总指南：hub-and-spoke 架构、转换器双向职责、核心构造（datasets/fields/relationships/metrics/custom_extensions/ai_context）映射、编写转换器步骤、边界情况与 round-trip fidelity 要求。"
resource: https://github.com/apache/ossie/blob/main/converters/README.md
tags: [ossie, converters, hub-and-spoke, roundtrip]
sources:
  - { id: conv-guide, resource: ../../raw/ossie-converters-guide.md, title: "converters/README.md", last_modified: 2026-08-14 }
usage_window: { from: 2026-08-14, to: 2026-08-14 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 转换器指南

> 快照 2026-08-14。converters/ 总指南（权威文档之总纲），厂商级展开见 [转换器生态页](ossie-converter-ecosystem.md)。

## Hub-and-Spoke 模型
- **Hub**：Ossie 核心规范为中心厂商中立格式；**Spokes**：各厂商 converter 处理与 Ossie 的双向转换。
- 避免点对点：N 厂商点对点需 N×(N-1) 个转换器；以 Ossie 为 hub 只需 **2×N**（每厂商一个 import 一个 export）。[^conv-guide]

## 转换器职责
- **Export（Ossie → Vendor）**：读 Ossie 模型产等价厂商表示（→Snowflake semantic model、→dbt `semantic_models` YAML、→Tableau/Salesforce、→Databricks）。
- **Import（Vendor → Ossie）**：读厂商模型产有效 Ossie 模型，厂商特定元数据映射进 `custom_extensions`。[^conv-guide]

## 已支持厂商
`SNOWFLAKE`、`SALESFORCE`、`DBT`、`DATABRICKS`、`OMNI`、`WISDOM`、`NVIDIA_GSF`（每厂商可用 `custom_extensions` 定义核心规范无等价物的元数据）。[^conv-guide]

## 核心构造映射要点
- **Datasets**：`source` 解析为厂商 catalog/schema/table；复合主键用数组 `[order_id, line_number]`；`unique_keys` 厂商支持时映射。
- **Fields**：`datatype` vs `dimension.is_time`——数据类型问题用 datatype，角色问题用 is_time；is_time 未设时时间 datatype 默认 true，显式优先。
- **Relationships**：`from`=多侧、`to`=一侧；复合键 `from_columns: [product_id, variant_id]` ↔ `to_columns: [id, variant_id]` 列位置对应、基数必须相同，须生成等价多列 join。
- **Metrics**：`expression.dialects` 选方言回退 ANSI_SQL；跨数据集指标（如 CLV = SUM/sales ÷ COUNT(DISTINCT customer)）须正确解析数据集引用并建立所需 join。
- **Custom Extensions**：Export 提取 vendor_name 匹配的扩展并应用；Import 把无核心等价的厂商设置存为 custom_extension。
- **AI Context**：每层都出现（model/dataset/field/relationship/metric）；厂商不支持时 import 存 `vendor_name: COMMON` 扩展防丢失。[^conv-guide]

## 编写转换器的 9 步
1. 用 Ossie JSON Schema + validation 脚本验证输入 → 2. 解析并迭代 `semantic_model` → 3. 映射 datasets（解析 source 到厂商 catalog 结构）→ 4. 字段映射 + 方言选择（厂商方言→ANSI_SQL→警告/报错）→ 5. 关系映射（保留复合键列顺序）→ 6. 指标映射 → 7. 应用 custom extensions → 8. 保留 AI context → 9. 按厂商 schema 验证输出。[^conv-guide]

## 边界情况与推荐处理
| 场景 | 处理 |
| --- | --- |
| 字段/指标缺厂商方言 | 回退 ANSI_SQL，记录警告 |
| 计算字段需厂商专用 SQL | 源模型含厂商方言；厂商+ANSI 皆无则报错 |
| 复合主键 | 厂商不支持则展平或文档化限制 |
| 跨数据集指标 | 确保被引用数据集存在且定义了关系；解析限定列名 |
| 未知厂商的 custom extension | **忽略但不丢弃**——为 round-trip 保留 |
| 不支持 ai_context 的厂商 | 存 `vendor_name: COMMON` 的扩展以保留 round-trip |[^conv-guide]

## Round-Trip Fidelity 三法则（Vendor A → Ossie → Vendor A）
1. **绝不静默丢弃信息**——厂商特定属性无核心等价物时存 `custom_extensions`。
2. 尽量保留字段顺序（部分厂商对声明顺序敏感）。
3. 为所有厂商保留 `custom_extensions`——单一 Ossie 模型可同时携带多厂商元数据。[^conv-guide]

## 新增转换器清单
稳定 `vendor_name` → 定义厂商 custom extension schema → 实现 export → 实现 import → 用 TPC-DS 示例模型作基线测试 → 文档化限制。[^conv-guide]

## 相关实体
- [Apache Ossie](../entities/apache-ossie.md)

## 引入/强化的概念
- [Hub-and-Spoke 转换架构](../concepts/hub-and-spoke.md)——本页是该概念在规范层的权威定义与实现规范

## 与已有知识的关联
- 与 [转换器生态页](ossie-converter-ecosystem.md)（11 厂商实证）互为总纲与细节；`is_time` 默认规则与 [核心规范页](ossie-core-spec.md) 一致。[^conv-guide]

[^conv-guide]: converters/README.md
