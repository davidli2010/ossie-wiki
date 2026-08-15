---
type: Source Summary
title: "Apache Ossie 官网（首页 / 社区 / 生态 / Updates 索引）"
description: "ossie.apache.org 四个页面快照：首页定位与示例、社区参与渠道、60+ 生态组织名单与 Snowflake 集成详情、Updates 时间线索引（2025-09 至 2026-07）。"
resource: https://ossie.apache.org
tags: [ossie, website, ecosystem, community]
sources:
  - { id: homepage, resource: ../../raw/ossie-apache-org-homepage.md, title: "官网首页", last_modified: 2026-08-14 }
  - { id: community, resource: ../../raw/ossie-apache-org-community.md, title: "官网社区页", last_modified: 2026-08-14 }
  - { id: ecosystem, resource: ../../raw/ossie-apache-org-ecosystem.md, title: "官网生态页", last_modified: 2026-08-14 }
  - { id: updates-index, resource: ../../raw/ossie-apache-org-updates-index.md, title: "官网 Updates 索引", last_modified: 2026-08-14 }
usage_window: { from: 2026-08-14, to: 2026-08-14 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 官网

> 快照 2026-08-14。Material for MkDocs 构建。

## 首页定位（Write Once, Query Anywhere）
- 口号：*Stop redefining "Revenue" in every dashboard.* ——声明式 YAML 定义指标、维度与 join，使数据栈每个工具与 agent 基于同一可信源工作。
- 标签：100% Vendor Neutral / YAML Configuration / Apache 2.0 Open Source / AI Ready Semantic Context。
- 四大支柱：Interoperability、Consistency、Vendor-Agnostic、Efficiency。
- 问题：Metric Drift / Manual Translation / Hallucinations / Integration Debt；解决方案：Single Source of Truth、Native Interoperability、Trusted AI Grounding、Reduced TCO。[^homepage]
- Core Classes：Semantic Model（顶层容器）、Datasets（逻辑业务实体）、Fields（行级属性）、Metrics（跨数据集聚合）、Dimensions（类别属性）、Relationships（外键连接，简单与复合键）。[^homepage]
- 首页示例 YAML：`ecommerce_analytics` 语义模型（orders dataset + `is_time: true` 的 order_date + ANSI_SQL 的 `SUM(orders.amount)` 指标）。[^homepage]

## 社区页
- 渠道：View Repository（GitHub）、GitHub Discussions、apache-ossie Slack、dev@/commits@/issues@ mailing lists、Google Calendar。
- 工作组（官网口径，5 个）：Advanced Metrics & Expression Language、Composability、Catalog Integration、Ontology Representation、Model Converters & Developer Tools。[^community]

## 生态页（60+ 组织）
- 名单：Alation, Ataccama, Anomalo, Atlan, AtScale, Bigeye, BlackRock, Blue Yonder, CARTO, Cloudera, Coalesce, Collate, Collibra, Cogniti, Count, Credible, Cube, Dataiku, Databricks, DataHub, Denodo, dbt Labs, Dremio, Domo, Elementum AI, Entropy Data, Firebolt, GoodData, Hex, Honeydew, Informatica, Instacart, JetBrains, Kyvos, Lightdash, Metabase, Mistral AI, Omni, Oracle, Preset, PuppyGraph, Qlik, RelationalAI, Salesforce, Select Star, ServiceNow, Sigma, Snowflake, Starburst Data, Strategy, Sundial, ThoughtSpot, Zeta Global（logo 名单，按字母序）。[^ecosystem]
- **Snowflake 是唯一有 detail 的组织**：
  - Export to Ossie：Snowflake Semantic View → Ossie YAML（`SYSTEM_READ_OSSIE_YAML_FROM_SEMANTIC_VIEW`）。
  - Import from Ossie：Ossie YAML → Snowflake Semantic View（`SYSTEM_CREATE_SEMANTIC_VIEW_FROM_OSSIE_YAML`）。
  - 另维护 apache/ossie 仓库中的 Snowflake Converter。[^ecosystem]
- 加入方式：向 ossie-website 仓库提 PR（logo + 组织工作描述）。[^ecosystem]
- 生态名单是官网声明，第三方实现成熟度需逐个审计。[^ecosystem]

## Updates 时间线（2025-09 → 2026-07）
- 2026-07-10 更名 Apache Ossie（此前条目为 OSI 阶段）。
- 2026-06-04 FSI 工作组上线；2026-04-28 社区更新；2026-03-24 Denodo；2026-02-10 Databao（JetBrains）；2026-02-03 Collate；2026-01-27 AtScale / dbt Labs / Qlik / Salesforce / Snowflake 五篇；2025-12-08 DataHub；2025-11-25 Domo；2025-11-13 Collibra / Snowflake（expand）/ Starburst / Strategy；2025-10-28 dbt MetricFlow 开源；2025-10-15 Preset；2025-09-23 发布日 17 家 launch partners（Snowflake/Omni/Salesforce/ThoughtSpot/Honeydew/RelationalAI/Alation/Atlan/Cube/Elementum/Select Star/Sigma 等公告）。[^updates-index]

## 相关实体
- [Apache Ossie](/entities/apache-ossie.md)

## 与已有知识的关联
- 官网生态名单（60+，2026-08-14）对 README 宣称的 50+ 参与组织（2026-07-17）做了增量；Snowflake 的系统函数与 [转换器生态](/sources/ossie-converter-ecosystem.md) 的 snowflake converter 对应。[^ecosystem]
- 首页 Core Classes 与 [核心规范页](/sources/ossie-core-spec.md) 的 semantic_model 结构一致。[^homepage]

[^community]: 官网社区页
[^ecosystem]: 官网生态页
[^homepage]: 官网首页
[^updates-index]: 官网 Updates 索引
