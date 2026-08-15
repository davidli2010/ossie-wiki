---
title: "Apache Ossie GitHub 仓库页面（README）"
source: "https://github.com/apache/ossie"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Apache Ossie GitHub 仓库主页与 README：仓库结构、项目使命、参与方式，以及快照日的 stars/forks/issues 等指标。"
tags:
  - "ossie"
  - "github"
  - "clippings"
---
> 抓取时间：2026-08-14。仓库状态为该日 GitHub 渲染页面快照。

# Apache Ossie GitHub Repository

## 仓库指标（2026-08-14 快照）

| 指标 | 值 |
| --- | --- |
| Stars | **1.9k** |
| Watchers | 46 |
| Forks | 235 |
| Open Issues | 44 |
| Pull Requests | 66 |
| Commits (main) | 241 |
| License | Apache-2.0 |
| Topics | metadata, semantic |
| 官网 | https://ossie.apache.org/ |

## README 正文

# Apache Ossie (incubating)

Apache Ossie 是一个协作型开源努力，致力于在数据分析、AI 与 BI 生态中多样化工具与平台之间标准化并简化语义模型交换与使用。其愿景是建立一个公共、厂商无关的语义模型规范，在参与者中促进无与伦比的互操作性、效率与协作。通过提供单一、一致的可信源，该厂商无关标准确保数据定义与价值在 AI agents、BI 平台与生态中所有其它工具之间交换时保持一致，消除跨工具的差异。

Apache Ossie 前身为 **Open Semantic Interchange (OSI)**。

Apache Ossie 提供单一 JSON 与 YAML 格式规范，任何工具都可读写，解决当今数据栈常见的语义碎片化问题：同一 KPI 在不同工具中定义各异、团队花费大量精力人工对账定义、以及 AI agents 基于不一致业务逻辑产生不可靠输出。

## 仓库内容

- `core-spec/` —— Ossie 核心规范（`spec.md`）、机器可读 schema（`spec.yaml`、`osi-schema.json`）及配套文档。
- `converters/` —— 在 Ossie 与其他语义格式之间转换的参考转换器（如 dbt、GoodData、Polaris、Salesforce）。
- `examples/` —— 示例语义模型，包括完整的 TPC-DS 模型。
- `validation/` —— 针对 Ossie schema 验证语义模型的工具。
- `docs/` —— 项目文档与概览。

## 参与方式

- **Contribute**：见 CONTRIBUTING.md（规范变更提案、代码贡献、社区参与）。
- **Roadmap**：见 ROADMAP.md（当前工作组、未来工作与社区讨论驱动的规划增强）。
- **Discuss**：GitHub Discussions 与 Issues。
- **Slack**：apache-ossie 社区 Slack。

## About

Apache Ossie，industry wide specification effort to standardize how we exchange semantic metadata across analytics, AI and BI platforms, providing a vendor neutral, single source of truth for semantic data.

https://ossie.apache.org/

Topics: metadata, semantic。
Readme · Apache-2.0 license · Code of conduct · Contributing · Security policy。

## 顶层目录结构（快照日 main 分支）

`.github` / `cli` / `compliance` / `converters` / `core-spec` / `docs` / `examples` / `ontology` / `python` / `validation` / `.asf.yaml` / `.editorconfig` / `.gitignore` / `CONTRIBUTING.md` / `DISCLAIMER` / `LICENSE` / `NOTICE` / `README.md` / `ROADMAP.md`

> 对比掘金文章（2026-07-17）快照：当时为 8 个转换器目录、约 175 个非 Git 文件；本次 GitHub 页面仍列出 dbt、GoodData、Polaris、Salesforce 等转换器（README 示例）。stars 从 ~1,056 增长至 1.9k，forks 从 142 增至 235，open issues 从 57 降至 44，commits 从 210 增至 241。—— 抓取对比（2026-08-14）