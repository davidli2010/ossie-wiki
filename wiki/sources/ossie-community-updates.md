---
type: Source Summary
title: "Apache Ossie 官网社区更新（更名公告 / 2026-04 / FSI 工作组）"
description: "三篇官网更新帖：更名 Apache Ossie 公告（进入 Apache 孵化器）、2026-04 社区更新（工作组、路线图、14 新参与者）、FSI 语义工作组上线（问题陈述与三项倡议）。"
resource: https://ossie.apache.org/updates/
tags: [ossie, apache-incubator, community, fsi, working-group]
sources:
  - { id: name-change, resource: ../../raw/ossie-apache-org-update-name-change.md, title: "The New Name for Open Semantic Interchange（2026-07-10）", last_modified: 2026-07-10 }
  - { id: april-2026, resource: ../../raw/ossie-apache-org-update-april-2026.md, title: "OSI Community Update（2026-04-28）", last_modified: 2026-04-28 }
  - { id: fsi-wg, resource: ../../raw/ossie-apache-org-update-fsi-working-group.md, title: "The FSI Semantic Working Group Is Live（2026-06-04）", last_modified: 2026-06-04 }
usage_window: { from: 2026-07-10, to: 2026-07-10 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 官网社区更新

## 一、更名公告（2026-07-10，Josh Klahr）

### 为什么更名
- 原 Open Semantic Interchange（OSI）为避免与开源生态中共享 OSI 缩写（Open Source Initiative）的项目混淆，进入孵化准备时更名为 **Ossie**。
- 吉祥物：袋鼠（育儿袋装着语义元数据，在数据栈系统间跳跃）。
- 今后称 **Apache Ossie (Incubating)**；"OSI" 引用为历史性。**规范与 YAML 格式不变，已基于 OSI 构建无需任何改动**。[^name-change]

### 为什么选择 Apache
- ASF 孵化确保开放标准、无单一控制实体；厂商中立的立足点至关重要。
- 迁移期间：公开 mailing lists、基于 GitHub 开发、规范变更正式讨论与投票、通过贡献（而非雇主关系）获得的 committership；旧 mailing lists 退役，改用 ASF 项目资源。[^name-change]

### 社区背景（自 2025-11 仓库开放以来）
- 来自 Snowflake、Dremio、Salesforce、Databricks、dbt Labs、RelationalAI、GoodData、Honeydew 的贡献者已合入 **100+ commits 与 35 个 merged PRs**。
- 参与联盟从 **17 个 launch partners 增长到 50+ 组织**。
- 三个工作组（Metric Language、Catalog、Ontology）有专属 lead、会议与公开渠道。
- Ossie-to-dbt Semantic Layer 转换器与 Apache Polaris 转换器已合入。[^name-change]

### 接下来（Snowflake 期待推动的领域，未经预设、走公开投票流程）
- 深化规范表达能力：表达式语言规范、高级指标逻辑、窗口函数、复杂关系；
- 更多平台/框架的转换器；
- 标准语义查询规范（任何引擎都能支持）；
- 与 Apache Polaris 集成使语义模型可从 catalog 直接发现。[^name-change]

## 二、2026-04-28 社区更新（Josh Klahr）

- 规范已公开、开发开放进行，GitHub 仓库成为活跃枢纽。
- Discussions 议题：可移植指标/派生逻辑、composability 权衡、与 catalog/metadata 系统对齐、ontologies 表示、开发者体验与工具。
- **工作组 5 个成形**：Advanced Metrics & Expression Language、Composability、Catalog Integration、Ontology Representation、Model Converters & Developer Tools。
- 引入公开路线图（ROADMAP.md）。
- **新增 14 个生态参与者**：Anomalo、Bigeye、CARTO、Cloudera、Coginiti、Count、Dataiku、Denodo、Dremio、GoodData、Metabase、Oracle、Sundial、Zeta Global。[^april-2026]

## 三、FSI 语义工作组（2026-06-04，John Heisler）

- 论点：金融服务业 agentic AI 已从实验进入真实业务；跑得最快的公司先解决语义层。结构性标准给了线格式（wire formats），但没给"数据意味着什么"的共享语言。
- 2026-06-03 首次正式会议（OSI 旗下）。参与者：**Northern Trust、DTCC、Cotality、LSEG、Verisk、BlackRock、S&P Global、AIG、Equilar、TIAA** 等（银行/保险/资管/财富管理/市场基础设施/数据提供商）。
- 批准正式问题陈述：结构性标准给线格式而非共享语言——数据提供商与机构独立建模相同核心概念而无共享词汇锚定。
- 成立三项倡议：
  1. **Architecture**：起步架构（Mermaid + Markdown 假设/设计选择/未决问题），经 OSI GitHub 共享。
  2. **Measurement of Commercial Impact**：度量 OSI 采纳的具体方法并随时间展示商业影响。
  3. **Structure**：建议如何跨金融子行业与功能领域组织语义。[^fsi-wg]

## 相关实体
- [Apache Ossie](../entities/apache-ossie.md)

## 与已有知识的关联
- 更名公告为「项目为何/如何进入 Apache 孵化」的权威叙事，与掘金概览（2026-07-17 快照）中 Apache 名称一致；治理规则细节见 [仓库与治理页](ossie-repository-and-governance.md)。[^name-change]
- 2026-04 的 5 工作组与当前 [工作组快照](ossie-roadmap-and-working-groups.md) 的 4 组形成演进对照；FSI WG 即当前 Financial Services Common Semantics 组的前身。[^april-2026][^fsi-wg]

[^april-2026]: OSI Community Update（2026-04-28）
[^fsi-wg]: The FSI Semantic Working Group Is Live（2026-06-04）
[^name-change]: The New Name for Open Semantic Interchange（2026-07-10）
