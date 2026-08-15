---
title: "Snowflake 联合行业领导者启动 Open Semantic Interchange 倡议（2025-09-23 发布日主公告）"
source: "https://www.snowflake.com/en/blog/open-semantic-interchange-ai-standard/"
author:
  - "[[Josh Klahr]]"
published: 2025-09-23
created: 2026-08-14
description: "OSI 发布日 Snowflake 主公告：倡议定位、17 家创始伙伴名单、五项指导原则、核心提案（OSI 模型 + 参与者映射模块）。"
tags:
  - "ossie"
  - "osi"
  - "snowflake"
  - "launch"
  - "announcement"
  - "clippings"
---
> 抓取时间：2026-08-14。作者 Josh Klahr +2，4 min read。Snowflake 官方博客（发布日主公告，OSI 阶段）。

# Snowflake Unites Industry Leaders to Unlock AI's Potential with the Open Semantic Interchange Initiative

## 摘要
2025-09-23，Snowflake 宣布参与 Open Semantic Interchange (OSI) 行业倡议，并列出创始生态伙伴：**Alation, Atlan, BlackRock, Blue Yonder, Cube, dbt Labs, Elementum AI, Hex, Honeydew, Mistral AI, Omni, RelationalAI, Salesforce, Select Star, Sigma, ThoughtSpot**（共 17 家含 Snowflake）。

引述（Christian Kleinerman, EVP of Product, Snowflake）："在 Snowflake，我们一直相信互操作性与开放标准是解锁 AI 与数据全部潜力的关键……通过 Open Semantic Interchange 倡议，我们与伙伴们一起带头解决 AI 的基础性挑战——缺乏通用语义标准。"

## 背景论点
- 现代企业 AI 与分析的主要障碍不是缺数据，而是**缺乏共享意义**（lack of shared meaning）。
- 业务逻辑与定义（如 churn rate、net margin）长期锁在专有孤岛中，团队被迫反复重建相同语义上下文，导致不一致与浪费。
- 愿景：建立**通用、厂商无关的语义模型规范与查询 API**，在 AI agents、BI 平台与生态工具间交换时保持一致。

## OSI Member Charter 五项指导原则
1. **Standardization**：建立统一语言与结构，确保跨工具一致性与易解释性。
2. **Interoperability**：在不同数据分析、AI、BI 产品间无缝交换与利用语义模型。
3. **Extensibility**：语义模型可适配自定义，允许组织与厂商按需扩展。
4. **Open Source**：承诺社区参与协作，鼓励创新。
5. **Domain-Specific Models**：标准化语义表示，简化多源数据组合与数据产品共享。

## 关键成果
- **Improved AI/BI Adoption**：跨工具无缝数据交换与共同理解。
- **Streamlined Data Operations**：为语义数据提供公共语言，减少管理多源复杂度。
- **Vendor Neutrality**：独立于任何特定数据平台、AI 工具或 BI 厂商。

## 核心提案（构建基础）
OSI 将提供标准创建、治理与维护的基本框架，含关键组件项目：
1. **OSI 模型**：以标准化 YAML 格式表示语义元数据（项目核心）。
2. **参与者特定模型映射与读写代码模块**：把 OSI 模型转换为参与者特定模型/语言，作为 Apache 开源项目的一部分。

> 历史意义：本公告为 OSI/OSSIE 的奠基文档，后项目更名为 Apache Ossie 并进入 Apache 孵化器。参见 [Apache Ossie 实体页](/entities/apache-ossie.md)。