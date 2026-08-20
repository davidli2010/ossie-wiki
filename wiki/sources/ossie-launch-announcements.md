---
type: Source Summary
title: "OSI 发布与采用公告（Snowflake 三篇 + Salesforce 一篇）"
description: "2025-09-23 OSI 发布日 Snowflake 主公告（17 家创始伙伴、五项指导原则）、2025-11-13 生态扩展与首次工作组会议（MetricFlow 开源参考实现）、2026-01-27 规范 v0.1 上线（8 新成员、基金会治理展望）与 Salesforce 定位解读（metrics-as-code、三项贡献能力）。"
resource: https://www.snowflake.com/en/blog/open-semantic-interchange-ai-standard/
tags: [ossie, osi, snowflake, salesforce, metricflow, launch]
sources:
  - { id: launch, resource: ../../raw/snowflake-osi-launch-announcement.md, title: "Snowflake 发布日主公告（2025-09-23）", last_modified: 2025-09-23 }
  - { id: expands, resource: ../../raw/snowflake-osi-expands-partners.md, title: "OSI 扩展伙伴与首次工作组会议（2025-11-13）", last_modified: 2025-11-13 }
  - { id: spec-live, resource: ../../raw/snowflake-osi-spec-live.md, title: "OSI 规范上线与新成员（2026-01-27）", last_modified: 2026-01-27 }
  - { id: sf-drift, resource: ../../raw/salesforce-ending-semantic-drift.md, title: "Ending Semantic Drift（2026-01-27，Salesforce）", last_modified: 2026-01-27 }
usage_window: { from: 2025-09-23, to: 2026-01-27 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# OSI 发布与采用公告

## 一、发布日主公告（2025-09-23，Snowflake，Josh Klahr）

- 定位：现代企业 AI/分析的主要障碍不是缺数据，而是**缺乏共享意义**。愿景是建立通用、厂商无关的语义模型规范与查询 API。
- **17 家创始伙伴**：Alation、Atlan、BlackRock、Blue Yonder、Cube、dbt Labs、Elementum AI、Hex、Honeydew、Mistral AI、Omni、RelationalAI、Salesforce、Select Star、Sigma、ThoughtSpot + Snowflake。
- **OSI Member Charter 五项指导原则**：Standardization、Interoperability、Extensibility、Open Source、Domain-Specific Models。
- 关键成果：Improved AI/BI Adoption、Streamlined Data Operations、Vendor Neutrality。
- 核心提案（构建基础）：① **OSI 模型**（标准化 YAML 表示语义元数据）；② 参与者特定模型映射与读写代码模块（作为 Apache 开源项目一部分）。[^launch]

## 二、生态扩展与首次工作组会议（2025-11-13）

- 指导原则：Standardization、Interoperability、Extensibility。
- **首次官方工作组会议 2025-10-17**（Snowflake Menlo Park/Bellevue 办公室 + 远程）。
- 新加入工作组：**AWS、Collibra、DataHub、Domo、Firebolt、Informatica、Instacart、JPMC、Preset、Starburst Data、Strategy**（11 家）。
- **MetricFlow 开源**：dbt Labs 在 Coalesce 宣布开源 MetricFlow（Apache 2.0），OSI 工作组用作**初始参考实现**以加速 v1.0 定义。
- 讨论：核心技术规范、治理模型、可量化里程碑。[^expands]

## 三、规范 v0.1 上线与基金会展望（2026-01-27）

- **OSI 规范首个版本上线**，位于 Apache 2.0 许可 Git 仓库（github.com/open-semantic-interchange/OSI）——厂商中立、可扩展的语义层构造表示（data sets、metrics、dimensions、relationships、contexts）。定位为起点非终点。
- 新增工作组成员 8 家：**AtScale、Coalesce、Collate、Credible、Databricks、JetBrains、Lightdash、Qlik**。
- **open-semantic-interchange.org 网站上线**（规范访问、成员目录、社区更新）。
- 展望：随倡议成熟，将过渡到中立的、基金会主导的治理模式（2026-07-10 落地为 Apache 孵化器）。[^spec-live]

## 四、Salesforce 视角：Ending Semantic Drift（2026-01-27，Southard Jones）

- 与 Snowflake、dbt Labs 共同发起 OSI；庆祝 v0.1 规范发布。
- **metrics-as-code**：业务逻辑与特定分析工具解耦，"revenue" 定义版本化、集中治理，而非埋藏在仪表盘/AI prompts/脚本中。
- 规范**原生支持 AI context**：synonyms（"sales"="purchases"）与 instructions（"用于销售分析"）直接嵌入代码。
- 数据（引用 *State of Data & Analytics*）：**89% 的数据领导者认为 agent 互操作很快将成必需，81% 担心分散 schema 限制互操作**。
- **三项核心贡献能力**：① Bi-directional metadata exchange；② Seamless governance propagation；③ Native query logic。
- 承诺把项目产出**捐赠给 Apache Software Foundation**（2026-07-10 已落地）。[^sf-drift]

## 相关实体
- [Apache Ossie](../entities/apache-ossie.md)

## 与已有知识的关联
- 四篇公告构成 OSI→Apache Ossie 的外部叙事与演化时间线（发布 → 工作组规模扩大 → 规范上线 → 更名入孵），是 [官网 Updates 索引](ossie-website.md) 所列外链的原文。[^launch]
- MetricFlow 作为参考实现，与 [转换器生态](ossie-converter-ecosystem.md) 中 dbt converter（MetricFlow Semantic Interface）直接相关。[^expands]
- "SQL Dialect / 表达式语言" 的规范化努力（[核心规范页](ossie-core-spec.md)）呼应 Salesforce 的"Native query logic"。 [^sf-drift]

[^expands]: OSI 扩展伙伴与首次工作组会议（2025-11-13）
[^launch]: Snowflake 发布日主公告（2025-09-23）
[^sf-drift]: Ending Semantic Drift（2026-01-27，Salesforce）
[^spec-live]: OSI 规范上线与新成员（2026-01-27）
