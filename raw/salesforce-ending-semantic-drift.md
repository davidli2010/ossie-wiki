---
title: "Ending Semantic Drift：AI 与 BI 首个统一业务逻辑基础（Salesforce）"
source: "https://www.salesforce.com/blog/ending-semantic-drift-unified-business-logic-foundation/"
author:
  - "[[Southard Jones]]"
published: 2026-01-27
created: 2026-08-14
description: "Salesforce 对 OSI v0.1 的定位解读：metrics-as-code、AI context 原生支持、bring your own semantic layer 策略与三项核心贡献能力，预告捐赠给 Apache。"
tags:
  - "ossie"
  - "osi"
  - "salesforce"
  - "semantic-drift"
  - "metrics-as-code"
  - "clippings"
---
> 抓取时间：2026-08-14。作者 Southard Jones（Salesforce Chief Product Officer, Analytics），3 min read。

# Ending Semantic Drift: The First Unified Business Logic Foundation for AI and BI

## 背景
数据与分析行业长期接受一个痛苦事实：每个工具说不同的语言。我们已标准化如何存储与转换数据，但数据的实际含义——驱动决策的关键指标与业务逻辑——仍困在孤立孤岛中。

## OSI 的意义
- 2025-09 与 Snowflake、dbt Labs 及行业领导者共同发起 OSI，为业务数据构建通用标准，让 AI agents 与 BI 工具共享完全相同的受信企业上下文。
- 与伙伴庆祝 **OSI v0.1 规范发布**，并建立 open-semantic-interchange.org 作为社区枢纽。
- 组织终于可以把业务逻辑与特定分析工具**解耦**。这带来 **metrics-as-code** 工作流：像 "revenue" 这样的定义被版本化并集中治理，而非埋藏在仪表盘计算、AI prompts 或自定义脚本中。
- 规范**原生支持 AI context**：工程师可在代码中直接嵌入同义词（如 "sales" = "purchases"）与指令（如"用于销售分析"），使数据开箱即用且 AI-ready。

## 行业联盟
市场需要"通用翻译器"。客户厌倦每次向下游移动数据都要重新治理。正在构建厂商中立的规范：业务逻辑定义一次，处处被继承和理解——无论是 Tableau、Agentforce、Snowflake AI agent 还是其它伙伴平台。

真正激动人心的不是代码本身，而是其后的空前联盟：传统竞争者把客户成功置于 vendor lock-in 之上。从 Salesforce、Snowflake、dbt Labs 和 Google 开始，已成长为行业浪潮。工作组持续扩张，含 Databricks、AtScale 及众多语义层提供商。

## 为 agentic 未来做准备
- CIO/CDAO 的最大恐惧不是数据量，而是 "metric drift"。营销对 "campaign ROI" 的定义与财务不同时，组织信任蒸发。
- 据 Salesforce *State of Data & Analytics* 报告：**89% 的数据领导者认为 agent 互操作很快将成为开展业务的必需，而 81% 担心分散的数据 schema 会限制这种互操作**。
- 部署 OSI 不止为了修复仪表盘，而是确保 agentic AI 革命中的知情决策。AI agents 是企业数据的新主要消费者，它们不能"猜测"你的业务逻辑。

## four 核心贡献能力（Salesforce 聚焦）
Salesforce 聚焦于三项将静态定义变为动态、可信数据流的核心能力：
1. **Bi-directional metadata exchange**（双向元数据交换）：同步业务逻辑，无需冗余的人工工作；
2. **Seamless governance propagation**（无缝治理传播）：在工具生态中维持信任与安全；
3. **Native query logic**（原生查询逻辑）：用源平台的原生运行时生成结果时保持每次计算的完整性。

## 从理论到代码
- v0.1 规范发布是企业工程团队的绿灯——不再是概念，而是可用的标准。
- 为保持 OSI 真正社区治理，承诺把项目产出**捐赠给 Apache Software Foundation**（2026-07-10 已落地）。

> 历史意义：文中"捐赠给 Apache"的承诺于 2026-07-10 实现（进入孵化器、更名 Apache Ossie）。参见 [Apache Ossie 实体页](/entities/apache-ossie.md)。