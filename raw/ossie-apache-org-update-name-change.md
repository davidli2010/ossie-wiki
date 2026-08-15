---
title: "Apache Ossie（孵化中）：Open Semantic Interchange 的新名称"
source: "https://ossie.apache.org/updates/ossie-enters-apache-incubator/"
author:
  - "[[Josh Klahr]]"
published: 2026-07-10
created: 2026-08-14
description: "OSI 项目被 Apache 孵化器接纳并更名 Apache Ossie (incubating) 的公告：更名原因、治理迁移与接下来方向。"
tags:
  - "ossie"
  - "apache-incubator"
  - "update"
  - "clippings"
---
> 抓取时间：2026-08-14。作者 Josh Klahr，3 min read。

# Apache Ossie (Incubating): The New Name for Open Semantic Interchange

Apache Ossie 目前在 Apache 软件基金会 (ASF) 接受孵化。

## 为什么更名

原项目名为 Open Semantic Interchange（OSI）。社区为进入孵化做准备时，决定更名为 "Ossie"，以避免与开源生态中共享 OSI 缩写（OSI = Open Source Initiative）的其它项目混淆。

- 吉祥物：一只袋鼠，育儿袋里装着语义元数据，在数据栈系统之间跳跃。
- 今后项目名为 **Apache Ossie (Incubating)**；"OSI" 作为项目名的引用为历史性；
- 用于定义指标、维度与关系的规范及基于 YAML 的格式不变。
- 如果已基于 Open Semantic Interchange 构建，无需任何改动——名称变了，规范没变。

## Ossie 是什么

Ossie 是同时针对 semantic layer 与 ontology 的开放规范。它定义一种厂商中立格式，表达业务指标、维度及其关系，以及更广泛的业务概念与规则。任何工具（BI 平台、查询引擎或 AI agent）都可以在无意义损失的情况下消费与产出语义定义。

解决的问题：同一业务概念（如 "Monthly Active Users"）常在组织的 CRM、数据仓库与 BI 工具中被不一致地定义。分析人员或 AI agent 查询时不该猜测哪个定义正确。Ossie 提供共享的机器可读格式，编码的不仅是数据，还有其背后的意图与业务含义。

## 为什么选择 Apache

在 ASF 孵化确保它保持开放标准、没有单一控制实体。Ossie 目标是为语义数据提供行业级标准化，厂商中立的立足点至关重要。

孵化期间：公开 mailing lists、基于 GitHub 的开发、规范变更的正式讨论与投票流程、通过贡献而非雇主关系获得的 committership。作为迁移的一部分，所有引用"Open Semantic Interchange"的 mailing lists 将被退役，成员应改用 ASF 提供的项目资源。

## 社区背景

Ossie 不是单一公司项目。自 2025 年 11 月仓库开放以来：

- 来自 Snowflake、Dremio、Salesforce、Databricks、dbt Labs、RelationalAI、GoodData 与 Honeydew 的贡献者已合入 **100+ commits 与 35 个 merged PRs**；
- 参与联盟从 **17 个 launch partners 增长到 50+ 组织**；
- 三个工作组（Metric Language、Catalog、Ontology）有专属 lead、会议与公开渠道；
- Ossie-to-dbt Semantic Layer 转换器与 Apache Polaris 转换器已合入。

## 接下来

Snowflake 是 OSI 的创始组织之一，将在 ASF 治理下继续作为 Apache Ossie 的活跃贡献者。

社区将共同决定方向。路线图（ROADMAP.md）中有大量潜在增项，Snowflake 期待推动的领域：

- 深化规范表达能力：表达式语言规范、高级指标逻辑、窗口函数、复杂关系；
- 为更多平台与框架构建转换器，让采用 Ossie 不必推翻已有工具；
- 任何引擎都能支持的标准语义查询规范；
- 与 Apache Polaris 集成，使语义模型可从 catalog 直接发现。

以上都未经预设，将走与项目其它事项相同的公开讨论与投票流程。

## 参与方式

- View Repository / Start a Discussion / Join Slack
- Subscribe to dev@（主开发与社区讨论列表）