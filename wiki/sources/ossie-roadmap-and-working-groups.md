---
type: Source Summary
title: "Apache Ossie 路线图与工作组（ROADMAP.md + working_groups.md）"
description: "社区知情路线图（当前三工作组/未来努力/增量增强，含 GitHub 讨论编号引用）与当前四个工作组的 Lead 与频道。"
resource: https://github.com/apache/ossie/blob/main/ROADMAP.md
tags: [ossie, roadmap, working-group]
sources:
  - { id: roadmap, resource: ../../raw/ossie-roadmap.md, title: "ROADMAP.md", last_modified: 2026-08-14 }
  - { id: wg, resource: ../../raw/ossie-working-groups.md, title: "docs/working_groups.md", last_modified: 2026-08-14 }
usage_window: { from: 2026-08-14, to: 2026-08-14 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 路线图与工作组

> 快照 2026-08-14。路线图综合自 Ossie GitHub Discussions 的社区讨论与投票信号，分为三类。

## 当前努力 / 工作组

### 1. Metric Semantics & Core Semantic Model
- **目标**：使语义模型表现力强、可组合、定义清晰，具备明确的实体/关系/粒度语义。
- **动机**：缺少对不同粒度指标、过滤器、聚合语义与指标间关系的充分支持。
- 关键讨论：#29（metrics vs measures）、#19（结构化 aggregation_method）、#12（entity/grain 一等概念）、#24/#4/#50（关系语义/复杂关系/基数）、#5（语义过滤器）、#15/#119（主键 vs 唯一键）。
- **交付物**：标准指标规范语言；一等聚合/关系/粒度语义；派生与累计指标；显式实体建模；可复用语义过滤器。[^roadmap]

### 2. Catalog Integration & Semantic Services
- **目标**：与数据 catalog 集成，实现集中式语义服务。
- **交付物**：与 catalog（如 Polaris）集成模式；独立语义服务/注册中心；模型的发现、版本化与访问控制。
- 相关：Issue #107（ontology-query 作为 Ontology Access Layer）。[^roadmap]

### 3. Ontology & Semantic Interoperability
- **动机**：解决结构性互操作之上的**概念互操作**（不同模型用不同名称描述同一业务概念）。
- **交付物**：本体层；ontology 概念与 datasets/fields 的 schema 映射；关系本体与非表格数据模型支持。
- 关键讨论：#22、#101、#108、#68。[^roadmap]

> 注：working_groups.md（当前实际 4 组）与 ROADMAP.md 的『当前努力/工作组』分类重合但组织方式不同，见下方。二者均为 2026-08-14 快照，反映同一时期的收敛。

## 当前工作组（docs/working_groups.md，2026-08-14）
| 工作组 | Lead | Slack 频道 |
| --- | --- | --- |
| Metric Language and Relationships | Will Pugh | `#ossie-metric-language-wg` |
| Catalog | Shubham Bhargav（Atlan） | `#ossie-catalog-wg` |
| Ontology | Kurt（Relational AI） | `#ossie-ontology-wg` |
| Financial Services Common Semantics | John Heisler（Snowflake） | `#ossie-financial-services-common-semantics` |

> 从启动时 5 个 WG（Advanced Metrics、Composability、Catalog、Ontology、Converters，见 2026-04 更新）演进为当前 4 个（Metric Language、Catalog、Ontology、FSI Common Semantics），反映路线收敛与 FSI 行业工作组落地。[^wg]

## 未来努力
- **Dataset Abstraction & Logical Modeling**：解耦语义定义与物理存储（#49/#61/#23/#109/#103；Issue #104 文件型数据集）。
- **Semantic Query Language & Reference Engine**：标准查询接口 + 参考编译器。
- **SQL Dialect, Expressions, and Execution Boundaries**：澄清 SQL 角色（#16/#28/#62/#6；Issue #52 每文档单方言）。
- **Dimensions, Hierarchies, and Time Semantics**：层级维度、标准化时间语义、日历（#21/#20/#17/#44/#47；Issue #84 datatype）。
- **AI-Native Semantic Layer**：AI 上下文元数据、verified queries、暴露控制（#32/#14/#9/#82）。
- **Governance, Identity, and Validation**：稳定标识符、验证标准、治理框架（#31/#53/#13/#67/#35；Issue #102/#92/#87）。
- **Industry / Domain-Specific Semantic Models**：可复用领域模型模板。[^roadmap]

## 增量增强（不改变规范基础）
- **Naming/Terminology/UX**（#33/#34/#36/#37）。
- **Data Types & Field Semantics**：unit/currency 一等注解、字段类型分类学（#42/#43/#55/#110；#58/#59 PII）。
- **Extended Metadata**（Issue #100）：measurement/display_format/semantic_type/default_aggregation/desired_direction/default_sort/semantic_mappings，超 custom_extensions。
- **Developer Experience & Doc**：ai_context 指南、建模最佳实践、description Markdown。
- **Specialized Capabilities**：空间类型、Date Spine、audience/segment。
- **Tooling & Ecosystem**（Issue #121 converter/common Java binding、#111 Snowflake 映射）。[^roadmap]

## 相关实体
- [Apache Ossie](../entities/apache-ossie.md)

## 与已有知识的关联
- 掘金概览所列路线图未完成项（指标粒度、语义查询语言、参考编译器、Registry、verified queries）在本页有完整编号与动机展开，并揭示实际的当前工作组建置。[^roadmap]

[^roadmap]: ROADMAP.md
[^wg]: docs/working_groups.md
