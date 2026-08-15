---
type: Source Summary
title: "Apache Ossie 规范正文（core-spec + 表达式语言提案 + ontology 规范）"
description: "Ossie 三份规范正文：核心元数据规范 spec.md（semantic model/datasets/relationships/fields/metrics/custom_extensions/ai_context）、表达式语言提案 OSSIE_SQL_2026（可移植 SQL 子集与函数清单）、本体规范 ontology.md（EntityType/ValueType、关系与多重性、映射）。"
resource: https://github.com/apache/ossie/tree/main/core-spec
tags: [ossie, spec, core-spec, expression-language, ontology]
sources:
  - { id: core-spec, resource: ../../raw/ossie-core-spec.md, title: "Apache Ossie 核心元数据规范 spec.md", last_modified: 2026-08-15 }
  - { id: expr-lang, resource: ../../raw/ossie-expression-language-proposal.md, title: "表达式语言提案 OSSIE_SQL_2026", last_modified: 2026-08-15 }
  - { id: ontology-spec, resource: ../../raw/ossie-ontology-spec.md, title: "本体规范 ontology.md", last_modified: 2026-08-15 }
usage_window: { from: 2026-08-15, to: 2026-08-15 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 规范正文

> 快照 2026-08-15（GitHub main 分支）。版本 `0.2.0.dev0`（Draft，schema 可能在 0.2.0 发布前变更）。三份文档共同构成规范的文本与提案层。

## 一、核心元数据规范 spec.md

- **目标**：Standardization（统一语言与结构）、Extensibility（领域特定扩展保持核心兼容）、Interoperability（跨 AI/BI 应用交换复用）。[^core-spec]
- **版本**：0.2.0.dev0（未发布）；历史版本 0.1.1（2025-12-11 Initial release）。[^core-spec]

### 枚举
- **Dialects（7 个）**：ANSI_SQL、SNOWFLAKE、MDX、TABLEAU、DATABRICKS、MAQL、BIGQUERY。[^core-spec]
- **DataType（10 个）**：String/Integer/Decimal/Float/Boolean/Date/Time/DateTime/DateTimeTz/Opaque。共享名与 ontology 内置值类型对齐；Time/DateTimeTz/Opaque 为核心额外类型。[^core-spec]

### Semantic Model（顶层容器）
`name`（必填）、`description`、`ai_context`、`datasets`（必填）、`relationships`、`metrics`、`custom_extensions`。[^core-spec]

### Datasets（逻辑数据集 = 业务实体/概念）
`name`（必填）、`source`（必填，`database.schema.table` 或查询）、`primary_key`、`unique_keys`（数组，每条单列/复合）、`description`、`ai_context`、`fields`、`custom_extensions`。[^core-spec]

### Relationships（外键连接）
`name`（必填）、`from`/`to`（多侧/一侧）、`from_columns`/`to_columns`（列顺序对应、数量必须相同）、`ai_context`、`custom_extensions`。支持简单（单列）与复合（多列）键。[^core-spec]

### Fields（行级属性）
`name`（必填）、`expression`（必填，`dialects[]` 多方言）、`dimension`（含 `is_time`）、`label`、`description`、`datatype`、`ai_context`、`custom_extensions`。[^core-spec]

**type vs role 关键分离**：
- `datatype` = 字段承载什么类型的值（Date/Integer/...）；
- `dimension.is_time` = 时间角色标志，独立于数据类型；
- **is_time 默认**：datatype 为 Date/Time/DateTime/DateTimeTz 时默认 `true`，否则 `false`；显式 is_time 始终优先。可用 `is_time: false` 退出时间默认（如审计 `created_at`）。[^core-spec]
- Precedent：镜像 Snowflake Semantic Views `time_dimensions:`、LookML `dimension_group`。[^core-spec]

### Metrics（模型级量化度量，可跨数据集）
`name`（必填）、`expression`（必填）、`description`、`datatype`、`ai_context`、`custom_extensions`。跨数据集示例：`SUM(orders.amount) / COUNT(DISTINCT customers.id)`。[^core-spec]

### Custom Extensions
`vendor_name`（自由格式字符串，无需改核心规范即可扩展）+ `data`（JSON 字符串）。已知 vendor：COMMON、SNOWFLAKE、SALESFORCE、DBT、DATABRICKS、GOODDATA、HONEYDEW、WISDOM。[^core-spec]

### AI Context Structure
简单字符串或结构化对象：`instructions`（使用指令）、`synonyms`（替代名称/术语）、`examples`（示例问题）。[^core-spec]

## 二、表达式语言提案 OSSIE_SQL_2026（Proposed Final）

- **范围限定在 Logical Layer**（metrics/fields/filters）；ontology 层表达式单独提案。[^expr-lang]
- 提案变更：新增 dialect `Ossie_SQL_2026`，并设为**默认方言**。[^expr-lang]
- 基础：**ANSI SQL:2003 Core**（ISO/IEC 9075-2:2003），看重跨库采用（Snowflake/Databricks/PostgreSQL/BigQuery）、语义明确、现代分析特性（window/CTE）。[^expr-lang]
- WG 阵容：Will Pugh（Snowflake）牵头，参与含 Malloy、AtScale、Salesforce、dbt Labs、Relational AI、Databricks、Cube、ThoughtSpot、Lightdash、Starburst、The ASF、Denodo。[^expr-lang]

### 支持与不支持构造
- 支持：算术/比较/逻辑运算符、BETWEEN、IN（仅值列表非子查询）、LIKE/ILIKE、IS NULL、CASE WHEN、聚合、窗口、标量函数、括号。[^expr-lang]
- **不支持**：SELECT/FROM/JOIN（语义层处理）、GROUP BY（grain 控制）、WHERE（用 filter）、子查询/CTE（用字段引用或 EXISTS_IN）、UNION/INTERSECT/EXCEPT、DDL/DML。[^expr-lang]

### 标识符
- 正常标识符大小写不敏感；`"quoted"` 精确匹配；标准 ANSI SQL 标识符，最长 128 字符。归一化形式用于匹配。命名空间支持多点 `dataset.field`。[^expr-lang]

### 函数族（REQUIRED/SHOULD/MAY 三级合规）
| 类别 | 关键函数 | 备注 |
| --- | --- | --- |
| 聚合 REQUIRED | SUM/COUNT/COUNT(*)/COUNT(DISTINCT)/AVG/MIN/MAX | 附「可分解性」标注（Distributive/Algebraic/Holistic）[^expr-lang] |
| 统计 REQUIRED | STDDEV/STDDEV_POP/VARIANCE/VAR_POP 等 | [^expr-lang] |
| 百分位 REQUIRED | MEDIAN/PERCENTILE_CONT/DISC | Holistic [^expr-lang] |
| 近似 RECOMMENDED | APPROX_COUNT_DISTINCT（~2% 误差）、APPROX_PERCENTILE（~1%） | sketch-based；PostgreSQL 不支持 [^expr-lang] |
| 条件聚合 REQUIRED | DISTINCT 修饰 + CASE 过滤聚合 | [^expr-lang] |
| 日期/时间 REQUIRED | CURRENT_*、YEAR/MONTH/DAY/HOUR、EXTRACT/DATE_PART、DATE_TRUNC/DATEADD/DATEDIFF、TO_DATE/TO_TIMESTAMP | format-string 解析为 EXPERIMENTAL（跨引擎 token 差异大）[^expr-lang] |
| 日期格式化 EXPERIMENTAL | TO_CHAR + 可移植 token 核心（YYYY/MM/MON/...） | 名称 token 依赖 locale [^expr-lang] |
| 字符串 REQUIRED | CONCAT/LENGTH/LOWER/UPPER/TRIM/LEFT/RIGHT/SUBSTRING/REPLACE/SPLIT_PART、POSITION/CONTAINS/STARTSWITH、LIKE/ILIKE/REGEXP_LIKE | regex 系列为 RECOMMENDED [^expr-lang] |
| 数学 REQUIRED | ABS/ROUND/FLOOR/CEIL/TRUNC/MOD/SIGN/POWER/SQRT/EXP/LN/LOG、GREATEST/LEAST | 三角为 RECOMMENDED [^expr-lang] |
| 条件 REQUIRED | CASE/IF/IFF/NULLIF/COALESCE/IFNULL/NVL/NVL2/ZEROIFNULL | [^expr-lang] |
| 窗口 REQUIRED | ROW_NUMBER/RANK/DENSE_RANK/NTILE/LAG/LEAD/FIRST_VALUE/LAST_VALUE/NTH_VALUE + 聚合窗口 | [^expr-lang] |
| 类型转换 REQUIRED | CAST（VARCHAR/INTEGER/DECIMAL/FLOAT/BOOLEAN/DATE/TIMESTAMP/TIME） | TRY_CAST RECOMMENDED [^expr-lang] |

- **跨工具映射表**：Ossie 标准函数 ↔ Tableau/Looker Studio/DAX 齐全对照（如 COUNT(DISTINCT x) ↔ COUNTD/COUNT_DISTINCT/DISTINCTCOUNT）。[^expr-lang]
- **合规级别**：MUST=REQUIRED 全部；SHOULD=RECOMMENDED；MAY=方言扩展。[^expr-lang]

## 三、本体规范 ontology.md（0.2.0.dev0）

### 概念类型
| ConceptType | 说明 |
| --- | --- |
| `EntityType` | 必须用其它信息引用的现实世界概念（如人用社保号引用） |
| `ValueType` | 带附加语义的数据类型（如"社保号是恰好九位数字的字符串"） |[^ontology-spec]

内置概念：`Any`（最一般实体类型）+ `Boolean/Date/DateTime/Decimal/Float/Integer/String`。Multiplicity：`ManyToOne`（最后 role 由其它 roles 唯一确定）、`OneToOne`（二元特例，两方向皆 many-to-one）。[^ontology-spec]

### Ontologies 结构
`name`（必填）/`description`/`ai_context`/`ontology`（必填）。每个关系归到扮演其第一个 role 的概念下。概念字段含 `extends`、`derived_by`、`identify_by`、`requires`、`relationships`。[^ontology-spec]

### Extends
用户概念延伸一个/多个概念成子类型。值类型必须（直接/间接）延伸内建值类型；实体类型只能延伸实体类型且隐式延伸 `Any`。[^ontology-spec]

### Relationships
set 语义（非 bag）、链接不含 null。字段：name/description/`multiplicity`/roles/derived_by/requires/`verbalizes`（必填）。每个关系用"所属概念名.关系名"唯一标识（如 `Person.earns`），支持 dot-join 导航。[^ontology-spec]

### 关键机制
- **identify_by**：实体对象不能直接引用，须借助一个/多个关系（如 Person.nr 用社保号）。[^ontology-spec]
- **derived_by**：派生关系/概念视为视图（ancestor_of 递归派生、Employee = `EXISTS(Person.earns)`）。[^ontology-spec]
- **requires**：对群体必须成立的条件（概念级如 `0 < SocialSecurityNr`；关系级如 `Amount > 0.0`）。[^ontology-spec]

### Ontology mappings
- **concept_mappings**：把逻辑模型字段映射到本体对象/链接，`object_mappings` 与 `link_mappings` 二选一。[^ontology-spec]
- **object_mappings**：值类型/简单标识实体用单个 SQL 表达式；复合标识用 referent mappings（可嵌套，如 OrderLineItem：nr 表达式 + order 嵌套映射 CustOrder）。[^ontology-spec]
- **link_mappings**：树形组织避免重复，`object_mapping`（必填）+ `relationship` + `children`；层级须与关系 arity 一致。[^ontology-spec]

### 版本历史
0.2.0.dev0（2026-05-29）：Basic support for ontologies and logical schema mappings。[^ontology-spec]

## 相关实体
- [Apache Ossie](/entities/apache-ossie.md)

## 引入/强化的概念
- [语义元数据交换](/concepts/semantic-metadata-interchange.md)、[语义层](/concepts/semantic-layer.md)

## 与已有知识的关联
- 掘金概览将其描述为"验证链"的输入（data 由 spec 定义）；本页是 spec 的规范级完整编目，含 `is_time` 默认、表达式语言合规级别等细节。[^core-spec][^expr-lang]
- ontology.md 正文与本批次另建的 [ossie-ontology-tooling](/sources/ossie-ontology-tooling.md)（ontology.json schema + flights 示例）互补。[^ontology-spec]

[^core-spec]: Apache Ossie 核心元数据规范 spec.md
[^expr-lang]: 表达式语言提案 OSSIE_SQL_2026
[^ontology-spec]: 本体规范 ontology.md
