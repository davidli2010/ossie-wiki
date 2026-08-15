---
title: "Apache Ossie 核心元数据规范 spec.md"
source: "https://github.com/apache/ossie/blob/main/core-spec/spec.md"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Ossie 核心规范正文：语义模型、数据集、关系、字段、指标、自定义扩展、AI Context 结构、数据类型与版本历史（0.2.0.dev0）。"
tags:
  - "ossie"
  - "spec"
  - "core-spec"
  - "clippings"
---
> 抓取时间：2026-08-14（main 分支）。

# Apache Ossie - Core Metadata Specification

> **DRAFT version** — 开发中，schema 可能在 0.2.0 发布前变更。
> **Version:** 0.2.0.dev0

## Goals
- **Standardization**: 为语义模型定义建立统一语言与结构，确保跨工具系统的一致性与易解释性。
- **Extensibility**: 支持领域特定扩展，同时保持核心兼容性。
- **Interoperability**: 支持跨不同 AI 与 BI 应用交换与复用。

## 目录
1. Enumerations
2. Semantic Model
3. Datasets
4. Relationships
5. Fields
6. Metrics
7. Examples

## Enumerations

### Dialects（指标与字段定义支持的 SQL 与表达式语言方言）
| Dialect | Description |
|---------|-------------|
| `ANSI_SQL` | Standard SQL dialect |
| `SNOWFLAKE` | Snowflake SQL |
| `MDX` | Multi-Dimensional Expressions |
| `TABLEAU` | Tableau calculations |
| `DATABRICKS` | Databricks SQL |
| `MAQL` | GoodData MAQL |
| `BIGQUERY` | Google BigQuery (GoogleSQL) |

### Data types
`DataType` 声明字段或指标的逻辑值类型，独立于其角色与物理表示。共享名与 ontology 规范的内置值类型对齐；`Time`、`DateTimeTz`、`Opaque` 为额外核心数据类型。

| DataType | Description |
|----------|-------------|
| `String` | 变长 Unicode 字符数据；长度与 collation 未指定。 |
| `Integer` | 精确整数；宽度与符号性未指定。 |
| `Decimal` | 精确十进制；精度与标度未指定。 |
| `Float` | 近似浮点数。 |
| `Boolean` | 逻辑二值真值类型。 |
| `Date` | 无时间分量的日历日期。 |
| `Time` | 无日期或时区的日间时间。 |
| `DateTime` | 无时区或偏移的本地/民用日期与时间。 |
| `DateTimeTz` | 含足够偏移或时区上下文以识别时刻的日期与时间；不保证保留命名时区标识。 |
| `Opaque` | 可移植词汇外的已知类型；用 `custom_extensions` 做厂商特定细化。类型未知或未指定时省略 `datatype`。 |

## Semantic Model（顶层容器）

### Schema
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 语义模型的唯一标识 |
| `description` | string | No | 人类可读描述 |
| `ai_context` | string/object | No | AI 工具的附加上下文（如自定义指令） |
| `datasets` | array | Yes | 逻辑数据集集合（fact 与 dimension 表） |
| `relationships` | array | No | 逻辑数据集如何连接 |
| `metrics` | array | No | 定义为逻辑数据集字段聚合表达式的量化度量 |
| `custom_extensions` | array | No | 厂商特定属性 |

### Example
```yaml
semantic_model:
  - name: sales_analytics
    description: Sales and customer analytics model
    ai_context:
      instructions: "Use this model for sales analysis and customer insights"
    datasets:
      - name: orders
        source: sales.public.orders
    relationships: []
    metrics: []
    custom_extensions:
      - vendor_name: DBT
        data: '{"project_name": "tpcds_analytics", "models_path": "models/semantic"}'
```

## Datasets（逻辑数据集 = 业务实体/概念，fact 与 dimension 表）

### Schema
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 数据集唯一标识 |
| `source` | string | Yes | 底层物理表/视图引用（如 `database.schema.table`）或查询 |
| `primary_key` | array | No | 唯一标识行的主键列（单个或复合） |
| `unique_keys` | array of arrays | No | 唯一键定义数组（每条可单列或复合） |
| `description` | string | No | 人类可读描述 |
| `ai_context` | string/object | No | AI 工具的附加上下文（如同义词、常用术语） |
| `fields` | array | No | 用于分组、过滤与指标表达式的行级属性 |
| `custom_extensions` | array | No | 厂商特定属性 |

### 键示例
```yaml
primary_key: [customer_id]        # 简单主键
primary_key: [order_id, line_number]   # 复合主键
unique_keys:
  - [email]                    # 简单唯一键
  - [first_name, last_name]    # 复合唯一键
```

## Relationships（外键约束连接逻辑数据集，支持简单与复合键）

### Schema
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 关系唯一标识 |
| `from` | string | Yes | 关系多侧（many side）的逻辑数据集 |
| `to` | string | Yes | 关系一侧（one side）的逻辑数据集 |
| `from_columns` | array | Yes | from 数据集的列名数组（外键列） |
| `to_columns` | array | Yes | to 数据集的列名数组（主键或唯一键列） |
| `ai_context` | string/object | No | AI 工具附加上下文 |
| `custom_extensions` | array | No | 厂商特定属性 |

### 注意事项
- `from_columns` 的列顺序必须与 `to_columns` 对应；
- 两个数组列数必须相同；
- 简单关系用单列 `[column1]`；复合关系用多列 `[column1, column2]`。

### 示例
```yaml
# 简单
- name: orders_to_customers
  from: orders
  to: customers
  from_columns: [customer_id]
  to_columns: [id]

# 复合（order_lines.product_id = products.id AND order_lines.variant_id = products.variant_id）
- name: order_lines_to_products
  from: order_lines
  to: products
  from_columns: [product_id, variant_id]
  to_columns: [id, variant_id]
```

## Fields（行级属性，可分组、过滤、用于指标表达式；简单列引用或计算表达式）

### Schema
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 数据集内字段唯一标识 |
| `expression` | object | Yes | 带方言支持的表达式定义 |
| `dimension` | object | No | 维度元数据（如 `is_time` 标志） |
| `label` | string | No | 分类标签 |
| `description` | string | No | 人类可读描述 |
| `datatype` | string (enum) | No | 该字段的逻辑数据类型 |
| `ai_context` | string/object | No | AI 工具附加上下文（如同义词） |
| `custom_extensions` | array | No | 厂商特定属性 |

### Expression Object（支持多 SQL 方言）
```yaml
expression:
  dialects:
    - dialect: ANSI_SQL  # 必须是 dialects 枚举值之一
      expression: "customer_id"  # 标量 SQL 表达式
```
要点：使用标量 SQL 表达式（无聚合）；可为简单列引用或计算表达式；同字段可提供多方言版本。

### Dimension Object
| Field | Type | Description |
|-------|------|-------------|
| `is_time` | boolean | 时间角色标记。为 `true` 时，能区分时间维度的消费者（时间序列分析、时间过滤）应把该字段作为时间维度。这是*角色*标志，独立于字段数据类型。 |

### DataType 与 is_time：type vs. role
- **`datatype`** 描述字段的数据类型（Date、Integer、String、DateTimeTz 等）：字段承载什么类型的值。
- **`dimension.is_time`** 是时间角色标记：无论数据类型为何，该字段是否应被视为时间维度。
- **`is_time` 默认值**：未显式设置时，若 `datatype` 是 Date/Time/DateTime/DateTimeTz 之一则默认 `true`，否则 `false`。显式 `is_time` 始终优先。可在时间类型列（如不想上时间轴的审计 `created_at`）设 `is_time: false` 退出默认。

组合示例：
| 列示例 | datatype | is_time | 有效角色 | 原因 |
|---|---|---|---|---|
| `d_date` | Date | 省略 | 时间维度 | 时间 datatype；默认 true |
| `order_timestamp` | DateTimeTz | 省略 | 时间维度 | 同上 |
| `created_at`（审计时间戳） | DateTime | false | 常规维度 | 显式退出时间默认 |
| `d_year`（整型年份粒度） | Integer | true | 时间维度 | 非时间 datatype，显式标角色 |
| `d_quarter_name` | String | true | 时间维度 | 字符串时间粒度 |
| `customer_id` | Integer | 省略 | 常规维度 | 非时间 datatype；默认 false |

> **Precedent.** 类型/角色分离镜像 Snowflake Semantic Views 的 YAML authoring form（结构性 `time_dimensions:` 集合可携带任意 `data_type`）；LookML 通过 `dimension_group` 支持类似拆分。

**Consumer guidance**：
- 数据类型问题（casting、序列化、下游类型推断）：优先 `datatype`；若仅设 `is_time: true`，不要从中推断特定标量类型。
- 角色问题（查询 UI 中分类时间维度、生成时间序列输出段、选择时间感知聚合）：当 `is_time` 解析为 `true`（显式或由时间 datatype 默认）时把字段视为时间维度。

## Metrics（模型级量化度量，可跨多个数据集）

### Schema
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 指标唯一标识 |
| `expression` | object | Yes | 带方言支持的表达式定义 |
| `description` | string | No | 指标衡量内容的人类可读描述 |
| `datatype` | string (enum) | No | 指标的逻辑数据类型 |
| `ai_context` | string/object | No | AI 工具附加上下文（如同义词） |
| `custom_extensions` | array | No | 厂商特定属性 |

### 示例
```yaml
# 简单聚合
- name: total_revenue
  expression:
    dialects:
      - dialect: ANSI_SQL
        expression: SUM(orders.amount)
  description: Total revenue across all orders
  datatype: Decimal
  ai_context:
    synonyms: ["total sales", "revenue"]

# 跨数据集指标
- name: avg_orders
  expression:
    dialects:
      - dialect: ANSI_SQL
        expression: SUM(orders.amount) / COUNT(DISTINCT customers.id)
  description: Average orders
  datatype: Decimal
```

## Custom Extensions（厂商附加平台特定元数据而不破坏核心兼容）
```yaml
custom_extensions:
  - vendor_name: string  # 自由格式字符串，标识厂商
    data: string         # 含厂商特定数据的 JSON 字符串
```
`vendor_name` 为自由格式字符串，允许任何厂商定义扩展而无须修改核心规范。已知示例：COMMON、SNOWFLAKE、SALESFORCE、DBT、DATABRICKS、GOODDATA、HONEYDEW、WISDOM。

（完整示例见规范原文，含 Snowflake/Salesforce/DBT/Databricks 扩展 JSON。）

## AI Context Structure
`ai_context` 可以是简单字符串或结构化对象：
```yaml
ai_context: "orders, purchases, sales"
# 或
ai_context:
  instructions: "Use this for sales analysis"
  synonyms: ["orders", "purchases", "sales"]
  examples: ["Show total sales last month", "What's the revenue by region?"]
```
推荐字段：`instructions`（AI 如何使用该实体的指令）、`synonyms`（替代名称与术语）、`examples`（示例问题或用例）。

## Version History
- **0.2.0.dev0**（Unreleased）：开发中下一个 minor release。Schema 可变；生产环境勿依赖此版本。
- **0.1.1**（2025-12-11）：Initial release
  - 核心语义模型结构；datasets、relationships、fields、metrics 支持；多方言指标表达式；厂商扩展框架；agent 上下文。

## License
见 LICENSE 文件。