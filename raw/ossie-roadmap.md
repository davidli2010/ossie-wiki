---
title: "Apache Ossie 路线图 ROADMAP.md"
source: "https://github.com/apache/ossie/blob/main/ROADMAP.md"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Apache Ossie 社区知情路线图：当前工作组（指标语义、Catalog 集成、本体）、未来努力（逻辑建模、语义查询语言、SQL 方言、维度时间、AI 原生、治理、行业模型）与增量增强。"
tags:
  - "ossie"
  - "roadmap"
  - "working-group"
  - "clippings"
---
> 抓取时间：2026-08-14（main 分支）。

# Apache Ossie Roadmap (Community-Informed)

路线图综合自 Ossie GitHub Discussions 的社区讨论与投票信号，分为三类：**当前努力/工作组**、**未来努力**、**增量增强**。

## 当前努力 / 工作组

### Metric Semantics & Core Semantic Model
**目标**：使语义模型表达力强、可组合、定义清晰，具备明确的实体、关系与粒度语义。
**动机**：当前模型缺乏对不同粒度指标、过滤器、聚合语义与指标间关系的足够支持；实体、join 与粒度表示上的歧义限制互操作性。

关键讨论（节选）：Top-level "metrics" vs. dataset-level "measures"（#29）、Cumulative 指标扩展（#39）、结构化 aggregation_method（#19）、entity/grain 一等概念（#12）、指标显式数据集引用（#18）、Relationship Semantics（#24）、复杂关系定义（#4）、显式关系基数（#50）、inner join（#11）、跨数据集维度与单数据集 measure（#27）、语义过滤器（#5）、metrics trees（#40）、主键 vs 唯一键冗余（#15、#119）。

**路线图交付物**：标准指标规范语言；一等聚合、关系与粒度语义（含已对齐期望行为的规范文档）；派生与累计指标支持；显式实体建模；增强关系定义；跨域建模；可复用语义过滤器定义。

### Catalog Integration & Semantic Services
**目标**：将 Ossie 与数据 catalog 集成，实现集中式语义服务。
**动机**：语义模型需要跨系统可发现、可治理、可共享。
**交付物**：与 catalog（如 Polaris）的集成模式；独立语义服务/注册中心；Ossie 模型的发现、版本化与访问控制。
**相关**：[Issue #107 — Adopt ontology-query as an Ontology Access Layer tool](https://github.com/apache/ossie/issues/107)

### Ontology & Semantic Interoperability
**目标**：使 Ossie 独立于物理数据布局描述业务概念，支持基于本体的语义模型与跨模型概念对齐。
**动机**：许多语义表示（如 Palantir、Goldman Sachs Legend）用本体定义含义，维度语义模型自然分层其上。Ossie 目前解决结构性互操作（任何工具都能读写公共格式），但未解决概念互操作（不同模型可能用不同名称或结构描述同一业务概念）。本体层可让组织独立于数据所在位置定义规范业务概念（Customer、Order、Product 等），并把物理语义模型映射回共享定义。

关键讨论：基于关系本体的规范交换（#22）、本体支持（#101）、共享语义（#108）、非表格数据模型（#68）。

**交付物**：描述物理/逻辑语义模型之上的业务概念的本体层；本体概念与 Ossie datasets/fields 的 schema 映射；关系本体与非表格数据模型支持；实现跨模型概念互操作的共享语义定义。

## 未来努力

### Dataset Abstraction & Logical Modeling
**目标**：把语义定义与物理存储解耦。
**动机**：用户需要独立于底层表或视图的可复用语义模型。
关键讨论：#49（查询定义实体/视图定义）、#61（逻辑数据集与物理表一对多绑定）、#23（结构化数据源）、#109（source 表示）、#103（跨模型可复用数据集）。
**交付物**：逻辑与物理数据集映射层；跨环境可复用语义定义；跨语义模型共享数据集与关系。
**相关**：[Issue #104 — 一等文件型数据集表示（如 Parquet）](https://github.com/apache/ossie/issues/104)

### Semantic Query Language & Reference Engine
**目标**：定义交互 Ossie 模型的标准查询接口，提供规范解释与执行实现的参考引擎。
**动机**：消费者（BI 工具、AI 系统、API）需要独立于底层 SQL 方言的一致查询方式。
**交付物**：标准语义查询语言（Ossie 原生或 SQL 扩展）；语义查询→执行计划映射；指标/维度/过滤器/关系支持；Ossie→SQL 参考编译器；join/聚合/过滤的规范处理；跨实现一致性测试套件。

### SQL Dialect, Expressions, and Execution Boundaries
**目标**：澄清 SQL 与执行在 Ossie 中的角色。
**动机**：可移植性与实际执行需求之间存在张力。
关键讨论：#16（数据集级默认方言）、#28（SQL 方言表达式期望）、#62（templating vs plain yaml）、#6（Jinja Templates）。
**交付物**：显式方言处理策略；语义定义与执行之间清晰边界；可选模板支持。
**相关**：[Issue #52 — 每个 Ossie 文档仅一个方言](https://github.com/apache/ossie/issues/52)

### Dimensions, Hierarchies, and Time Semantics
**目标**：标准化维度与时间建模方式。
**动机**：层级与时间处理不一致影响可用性与互操作性。
关键讨论：#21（维度层级）、#20（维度组）、#17（is_time 改为 dimension_type 枚举）、#44（通用日历）、#47（Date Spine 模型）。
**交付物**：层级维度建模；标准化时间语义；日历抽象。
**相关**：[Issue #84 — 支持字段 datatype 而非 is_time](https://github.com/apache/ossie/issues/84)

### AI-Native Semantic Layer
**目标**：使 Ossie 成为 AI 驱动分析的可靠基础。
**动机**：对结构化语义上下文与 grounded 查询生成的需求日益增长。
关键讨论：#32（不把 "AI Context" 作为键名）、#14（跳过 AI 上下文的关键字）、#9（ai_context 用法指南）、#82（verified_queries 作为核心元素）。
**交付物**：标准化 AI 上下文元数据；verified/curated 查询定义；控制 AI 对语义元素暴露的机制。

### Governance, Identity, and Validation
**目标**：确保信任、稳定与长期互操作。
**动机**：企业采纳需要一致标识符、验证与治理钩子。
关键讨论：#31（稳定标识符而非复用 name）、#53（certified 与认证机构）、#13（治理元数据钩子）、#67（用 LinkML 增加规范严谨性）、#35（Ossie 级验证）。
**交付物**：跨环境稳定标识符；验证与一致性标准；治理与认证框架。
**相关**：[Issue #102 — 核心 spec 语义版本化与 Git release](https://github.com/apache/ossie/issues/102)、[#92 — Trust Control Center](https://github.com/apache/ossie/issues/92)、[#87 — Restricted/Internal 标志](https://github.com/apache/ossie/issues/87)

### Industry / Domain-Specific Semantic Models
**目标**：通过可复用、标准化的领域模型加速采纳。
**动机**：组织反复创建类似语义模型（SaaS、金融、零售）。
**交付物**：精选领域特定语义模型模板；按行业的指标与维度最佳实践；与 Ossie 对齐的互操作模型包。

## 增量增强（不改变规范基础）

### Naming, Terminology, and UX Improvements
动机：当前规范的若干命名与行业术语冲突。
讨论：#33（Field 改名为 Dimension）、#34（Dataset.source 改名）、#36（泛化 description）、#37（Display name）。
**交付物**：反映社区共识的修订术语（如 "Dimension" 优于 "Field"）；一致的命名约定。

### Data Types and Field Semantics
动机：消费系统需要知道字段是否代表货币、物理单位或敏感数据。
讨论：#42（units 原生支持）、#43（currencies 原生支持）、#55（dimension_type/data_type/pii_classification 语义字段类型）、#110（可移植物理元数据）。
**交付物**：measures 与 dimensions 的一等 unit/currency 注解；标准化语义字段类型分类学。
**相关**：[Issue #58 — contain personal data](https://github.com/apache/ossie/issues/58)、[#59 — confidential indicator](https://github.com/apache/ossie/issues/59)

### Extended Metadata for Apache Ossie
动机：Ossie 在可解释性上下文（显示约定、默认聚合行为、KPI 极性、排序偏好、外部语义概念对齐）支持有限。
**交付物**：[Issue #100 Extended Metadata Proposal](https://github.com/apache/ossie/issues/100)——可选、向后兼容元数据字段（`measurement`、`display_format`、`semantic_type`、`default_aggregation`、`desired_direction`、`default_sort`、`semantic_mappings`）；超越 custom_extensions 的更丰富应用特定扩展点；示例值注解。
讨论：#30、#7（sample values）、#13、#41（positive direction）、#115（default_aggregation）。

### Developer Experience & Documentation
**交付物**：带注解示例的综合使用指南（尤其 ai_context）；数据建模最佳实践文档；description 字段 Markdown 支持。
讨论：#9、#8（数据建模信息）、#38（Markdown）。

### Specialized Capabilities
**交付物**：空间字段类型、空间关系与地理层级；Date Spine 模型支持（时间序列对齐与补洞）；audience/segment 定义为一等构造。
讨论：#69（地理空间）、#47（Date Spine）、#51（Audiences）、#114（空间维度类型）。

### Tooling & Ecosystem Support
**交付物**：验证器代码（schema 验证、linting、一致性检查）；参与者↔Ossie 转换器代码。
**现有工件**：JSON Schema (osi-schema.json)、validate.py、Snowflake/GoodData/Salesforce/Polaris/OrionBelt 转换器。
**相关**：[Issue #121 — converter/common 模块（Java binding）](https://github.com/apache/ossie/issues/121)、[#111 — Snowflake YAML 中 ai_context 与 custom_extensions 映射](https://github.com/apache/ossie/issues/111)