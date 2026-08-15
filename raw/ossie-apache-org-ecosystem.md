---
title: "Apache Ossie 官网生态页"
source: "https://ossie.apache.org/ecosystem/"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Apache Ossie 生态组织完整名单（60+）与 Snowflake 集成详情（Export/Import Ossie YAML）。"
tags:
  - "ossie"
  - "ecosystem"
  - "partners"
  - "clippings"
---
> 抓取时间：2026-08-14。

# Apache Ossie 生态

Organizations powered by Apache Ossie (incubating)。官网生态页列出以下组织（logo 名单，按字母序）：

Alation, Ataccama, Anomalo, Atlan, AtScale, Bigeye, BlackRock, Blue Yonder, CARTO, Cloudera, Coalesce, Collate, Collibra, Cogniti, Count, Credible, Cube, Dataiku, Databricks, DataHub, Denodo, dbt Labs, Dremio, Domo, Elementum AI, Entropy Data, Firebolt, GoodData, Hex, Honeydew, Informatica, Instacart, JetBrains, Kyvos, Lightdash, Metabase, Mistral AI, Omni, Oracle, Preset, PuppyGraph, Qlik, RelationalAI, Salesforce, Select Star, ServiceNow, Sigma, Snowflake, Starburst Data, Strategy, Sundial, ThoughtSpot, Zeta Global

> 说明：此为官网声明的生态参与名单；第三方实现成熟度需逐个审计。

## Snowflake 集成详情（生态页唯一有 detail 的组织）

Snowflake 致力于保持 Semantic Views 及其它语义上下文与 Apache Ossie 互操作：

- **Export to Ossie**：将任意 Snowflake Semantic View 转换为 Ossie YAML 文件。[Docs](https://docs.snowflake.com/en/sql-reference/functions/system_read_ossie_yaml_from_semantic_view)
- **Import from Ossie**：从 Ossie YAML 文件创建 Snowflake Semantic View。[Docs](https://docs.snowflake.com/en/sql-reference/stored-procedures/system_create_semantic_view_from_ossie_yaml)
- **Snowflake Converter**：维护在 Ossie 仓库中的程序化转换工具。[GitHub](https://github.com/apache/ossie/tree/main/converters/snowflake)

## 加入方式

"Want to add your organization?" — 打开 [ossie.apache.org PR](https://github.com/apache/ossie-website/pulls)，附 logo 与组织工作描述。