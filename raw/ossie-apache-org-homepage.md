---
title: "Apache Ossie (incubating) 官网首页"
source: "https://ossie.apache.org/"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Apache Ossie 官方首页：项目定位、核心对象类、解决的问题与解决方案、最新动态。"
tags:
  - "ossie"
  - "official-site"
  - "clippings"
---
> 抓取时间：2026-08-14。页面内容为 ossie.apache.org 首页（Material for MkDocs 构建）。

# Apache Ossie (incubating)

Apache Ossie (incubating) is the universal standard for semantic data.

Apache Ossie (incubating) 是一个行业级规范努力，旨在标准化在分析、AI 与 BI 平台之间交换语义元数据的方式，提供厂商中立、单一可信源的语义数据。Ossie 前身为 Open Semantic Interchange (OSI)。

- View Specification: https://github.com/apache/ossie/blob/main/core-spec/spec.md
- Get Involved: https://ossie.apache.org/community/

标签：100% Vendor Neutral / YAML Configuration / Apache 2.0 Open Source / AI Ready Semantic Context

## 核心口号：Write Once, Query Anywhere

"Stop redefining 'Revenue' in every dashboard." Apache Ossie 用声明式 YAML 标准定义指标、维度与 join，使数据栈中每个工具与 agent 都基于同一可信源工作。

- **Consistent Definitions**：确保营销、财务、销售看到相同数字，消除 metric drift。
- **AI-Ready Context**：为 LLM 提供回答业务问题所需的语义上下文。
- **No Lock-In**：业务逻辑在平台间自由迁移，指标属于你自己。

### 首页示例 YAML

```yaml
semantic_model:
  - name: ecommerce_analytics
    description: Sales and customer analytics
    ai_context:
      instructions: "Use for sales analysis"

    datasets:
      - name: orders
        source: sales.public.orders
        primary_key: [order_id]
        fields:
          - name: order_date
            dimension:
              is_time: true

    metrics:
      - name: total_revenue
        expression:
          dialects:
            - dialect: ANSI_SQL
              expression: SUM(orders.amount)
```

## About Apache Ossie

Apache Ossie 是一个协作型开源努力，致力于在数据分析、AI 与 BI 生态中标准化并简化语义模型定义。四大支柱：

- **Interoperability**：在 AI agents、BI 平台与分析工具之间无缝交换语义模型。
- **Consistency**：在整个生态的每个平台保持一致的数据定义与取值。
- **Vendor-Agnostic**：跨厂商通用的公共标准，消除特定工具的不一致与锁定。
- **Efficiency**：通过统一的、受治理的语义基础减少工程债并加速创新。

### 问题：语义碎片化（Semantic Fragmentation）

- **Metric Drift**：不同仪表盘间的 KPI 不一致。
- **Manual Translation**：昂贵且易错的人工对账。
- **Hallucinations**：基于冲突数据逻辑的不可靠 AI grounding。
- **Integration Debt**：专有工具之间复杂的 N-to-N 自定义集成。

### 解决方案：统一标准

- **Single Source of Truth**：统一的语义与指标定义。
- **Native Interoperability**：平台与 AI agents 之间直接交换。
- **Trusted AI Grounding**：agent 基于业务逻辑准确推理。
- **Reduced TCO**：通过自动化模型交换降低成本。

### Core Classes（规范定义的基础构件）

- **Semantic Model**：顶层容器，表示包含 datasets、relationships 与 metrics 的完整模型。
- **Datasets**：逻辑业务实体 —— fact 与 dimension 表，含字段与结构。
- **Fields**：行级属性，用于分组、过滤与指标表达式。
- **Metrics**：跨多个数据集的聚合计算 —— sums、averages、ratios。
- **Dimensions**：类别属性 —— Where、When、Who。
- **Relationships**：连接数据集的外键约束，支持简单与复合键。

## Latest Updates（2026-08-14 可见）

- **2026-07-10**：Apache Ossie (Incubating): The New Name for Open Semantic Interchange —— OSI 已被 Apache 孵化器接纳并更名，spec、社区与使命不变，但名称、治理归属与长期轨迹改变。
- **2026-06-04**：The FSI Semantic Working Group Is Live —— 金融服务语义工作组举行首次正式会议。
- **2026-04-28**：OSI Community Update: What's New and What's Next —— 规范已上线，工作组成形，14 位新参与者加入。
- **2026-03-24**：Denodo Joins Snowflake to Advance Open Semantic Interchange（外部新闻稿）。
- **2026-02-10**：Databao Becomes a Partner of the Open Semantic Interchange Initiative（JetBrains Databao 成为 OSI 合作伙伴，贡献开源 context engine 与 data agent）。

## 页面元信息

- 版权：Copyright © 2026 The Apache Software Foundation，Apache License 2.0。
- 处于 Apache Incubator 孵化过程，由 Apache Incubator 赞助。
- 说明：孵化状态不必然反映代码的完整性与稳定性，但表明项目尚未获得 ASF 全面背书。