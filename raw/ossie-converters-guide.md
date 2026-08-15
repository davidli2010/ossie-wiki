---
title: "Apache Ossie 转换器指南 converters/README.md"
source: "https://github.com/apache/ossie/blob/main/converters/README.md"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Ossie 转换器权威文档：hub-and-spoke 架构、转换器职责、核心构造映射、编写转换器步骤、边界情况与 round-trip fidelity。"
tags:
  - "ossie"
  - "converters"
  - "hub-and-spoke"
  - "clippings"
---
> 抓取时间：2026-08-14（main 分支）。

# Apache Ossie Converters

## Overview
Ossie Converter 在 Ossie 语义模型格式与特定厂商语义实现之间翻译。团队可以一次在 Ossie 标准中编写语义模型，再自动生成对应的厂商特定表示。

## Hub-and-Spoke Model
- **Hub**: Ossie 核心规范作为中心、厂商中立的格式。
- **Spokes**: 每个 converter 处理与特定厂商格式之间的转换。

```
            Snowflake ──┐
                        ├── Ossie ──┼── dbt
                        │           ├── Salesforce
            Databricks ─┘           └── ...
```

避免厂商两两点对点转换。N 个厂商：点对点需要 N*(N-1) 个转换器；以 Ossie 为 hub 只需 2*N（每厂商一个 import 一个 export），与所有其它厂商的互操作免费获得。

## Converter Responsibilities（两个方向）
- **Export (Ossie → Vendor)**：读 Ossie 模型，产出等价厂商表示。例：Ossie → Snowflake semantic model；Ossie → dbt `semantic_models` YAML；Ossie → Tableau data source / Salesforce semantic layer；Ossie → Databricks semantic layer。
- **Import (Vendor → Ossie)**：读厂商模型产出有效 Ossie 模型，包括把厂商特定元数据映射进 `custom_extensions`。

## Supported Vendors
| Vendor | Description |
|--------|-------------|
| `SNOWFLAKE` | Snowflake semantic model |
| `SALESFORCE` | Salesforce / Tableau semantic layer |
| `DBT` | dbt semantic models |
| `DATABRICKS` | Databricks semantic layer |
| `OMNI` | Omni semantic model |
| `WISDOM` | WisdomAI domain |
| `NVIDIA_GSF` | NVIDIA Generative Semantic Fabric standalone YAML |

每个厂商可通过 `custom_extensions` 定义核心规范无等价物的厂商特定元数据。

## Mapping Core Constructs（核心构造映射）
### Semantic Model
| Ossie Field | Converter Consideration |
|-----------|------------------------|
| `name` | 映射到厂商的 model/project name |
| `description` | 大多厂商支持描述字段 |
| `ai_context` | 厂商支持 AI/LLM 注解时映射 |
| `datasets` | 见 dataset 映射 |
| `relationships` | 见 relationship 映射 |
| `metrics` | 见 metric 映射 |
| `custom_extensions` | 提取匹配目标厂商的扩展 |

### Datasets
| Ossie Field | Converter Consideration |
|-----------|------------------------|
| `name` | 映射到 table/entity/model name |
| `source` | 解析为厂商特定 catalog/schema/table 组件 |
| `primary_key` | 映射厂商 PK 语法；复合键用数组如 `[order_id, line_number]` |
| `unique_keys` | 厂商支持唯一约束时映射 |
| `fields` | 见 field 映射 |
| `ai_context` | 厂商支持语义注解时映射 |
| `custom_extensions` | 提取匹配目标厂商的扩展 |

### Fields
> 注意：`datatype`（Field 与 Metric 上）声明字段逻辑数据类型；`dimension.is_time` 是独立时间角色标记。两者可兼有、可只有其一、可都无。数据类型问题用 `datatype`；角色问题用 `is_time`。`is_time` 未设时，若 datatype 是 Date/Time/DateTime/DateTimeTz 则默认 true，否则 false；显式 `is_time` 优先。

| Ossie Field | Converter Consideration |
|-----------|------------------------|
| `name` | 映射到 column/attribute name |
| `expression.dialects` | 选择匹配目标厂商的方言；回退 `ANSI_SQL` |
| `datatype` | 建议参考 datatype 判断字段逻辑类型；Decimal 为精确十进制（精度标度未指定），Float 近似；优先用时间成员（Date/Time/DateTime/DateTimeTz）分类时间维度；未知省略；可移植词汇外类型用 Opaque + custom_extensions |
| `dimension.is_time` | 映射厂商时间维度标记；is_time 解析为 true（显式或时间 datatype 默认）时建议分类为时间维度；显式 false 抑制时间维度分类 |
| `label` / `description` / `ai_context` | 厂商支持时映射 |

**表达式方言选择**：提供多方言时，Snowflake converter 应选 `SNOWFLAKE` 方言，否则回退 `ANSI_SQL`。

### Relationships
| Ossie Field | Converter Consideration |
|-----------|------------------------|
| `name` | 映射厂商 join/relationship name |
| `from` | 多侧数据集（引用表） |
| `to` | 一侧数据集（被引用表） |
| `from_columns` / `to_columns` | 位置对应；复合键处理；两数组基数必须相同 |

复合关系示例：`from_columns: [product_id, variant_id]`、`to_columns: [id, variant_id]` 表示 `from.product_id = to.id AND from.variant_id = to.variant_id`，须在目标厂商格式生成等价多列 join。

### Metrics
| Ossie Field | Converter Consideration |
|-----------|------------------------|
| `name` | 映射厂商 measure/KPI name |
| `expression.dialects` | 选择适当方言；回退 `ANSI_SQL` |
| `datatype` | 建议参考 datatype 声明聚合结果类型；数值 measure 区分精确 Decimal/Integer 与近似 Float |
| `description` / `ai_context` | 厂商支持时映射 |

跨数据集指标示例：`customer_lifetime_value = SUM(store_sales.ss_ext_sales_price) / COUNT(DISTINCT customer.c_customer_sk)`。converter 必须确保数据集引用在目标厂商格式正确解析并建立所需 join。

### Custom Extensions
converter 应：1) Export 时提取 `vendor_name` 匹配目标厂商的扩展、解析 data JSON、应用厂商特定设置；2) Import 时把无 Ossie 核心等价的厂商设置存为 custom_extension。

### AI Context
`ai_context` 出现在每个层级（model/dataset/field/relationship/metric）。目标厂商支持等价构造时映射；不支持时可在 import 时编码进 `custom_extensions`（`vendor_name: COMMON`）避免数据丢失。

## Writing a Converter（编写步骤）
1. 用 Ossie JSON Schema 与 validation 脚本验证输入 Ossie 模型。
2. 解析 Ossie 模型，迭代顶层 `semantic_model`。
3. 映射 datasets（name/source/primary_key/unique_keys/fields；解析 source 字符串为厂商 catalog 结构）。
4. 映射字段并做方言选择：优先厂商方言 → 回退 ANSI_SQL → 两者皆无则警告或报错。
5. 映射关系为厂商 join 语法；保留复合键列顺序。
6. 映射指标（同方言选择）；把数据集引用解析为厂商限定列格式。
7. 应用 custom extensions（提取匹配 vendor_name 并应用）。
8. 保留 AI context。
9. 按厂商自身 schema 或工具验证输出。

## 边界情况
| 场景 | 推荐处理 |
|----------|---------------------|
| 字段/指标缺厂商方言 | 回退 ANSI_SQL，记录警告 |
| 厂商特定 SQL 语法的计算字段 | 源 Ossie 模型需含厂商方言；厂商与 ANSI 皆无则报错 |
| 复合主键 | 厂商不支持则展平或文档化限制 |
| 引用多表的跨数据集指标 | 确保被引用数据集存在且有关系定义；解析限定列名 |
| 未知厂商的 custom extension | 忽略但不丢弃——为 round-trip 保留 |
| 不支持 ai_context 的厂商 | 存为 `vendor_name: COMMON` 的 custom_extension 以保留 round-trip |

## Round-Trip Fidelity
```
Vendor A model → [Import] → Ossie model → [Export] → Vendor A model
```
达成要点：
- **绝不静默丢弃信息**。厂商特定属性无 Ossie 核心等价物时存入 `custom_extensions`。
- 尽量保留字段顺序（部分厂商对声明顺序敏感）。
- 为所有厂商保留 `custom_extensions`，不止目标厂商——单一 Ossie 模型可同时携带多厂商元数据。

## 概念转换流程示例（TPC-DS → Snowflake）
1. 读 `tpcds_retail_model`。
2. 建 Snowflake 模型 `tpcds_retail_model`。
3. 映射 datasets（store_sales/date_dim/customer/item/store）为 Snowflake 表引用，解析 `tpcds.public.store_sales`。
4. 字段选 ANSI_SQL 方言表达式。
5. 关系转为 Snowflake join 定义。
6. 指标如 `total_sales`（SUM(store_sales.ss_ext_sales_price)）转为 Snowflake 指标定义。
7. 提取 SALESFORCE 与 DBT custom extensions——不应用到 Snowflake 输出，但若模型后续导回 Ossie 应保留。

## 新增转换器
1. 每个 converter 发出的 custom extension 用稳定 `vendor_name`。
2. 定义厂商 custom extension schema。
3. 实现 export（Ossie → Vendor）。
4. 实现 import（Vendor → Ossie）。
5. 用 TPC-DS 示例模型为基线加测试。
6. 文档化限制或不支持构造。