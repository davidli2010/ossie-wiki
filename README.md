# Apache Ossie 知识库

一个基于 [LLM Knowledge Bases](https://github.com/karpathy/llm-knowledge-bases) 模式构建的个人研究型 wiki，追踪 **Apache Ossie (incubating)** —— 一个行业级规范努力，旨在标准化在分析、AI 与 BI 平台之间交换语义元数据的方式，提供厂商中立、单一可信源的语义数据（前身为 Open Semantic Interchange / OSI）。

本仓库通过 LLM 驱动的工作流增量编译原始资料为互相链接、可溯源、可判新的 Markdown 知识库，遵循 Open Knowledge Format (OKF) v0.2 规范。

## 目录结构

```
├── AGENTS.md              # 知识库 schema 与操作规则（写入前必读）
├── README.md
├── raw/                   # 原始资料快照（源文档，只读）
├── wiki/                  # 持久化知识（互相链接的 Markdown 页面）
│   ├── index.md           # 全库索引
│   ├── log.md             # 操作日志
│   ├── entities/          # 实体页：项目、组织、工具、人物
│   ├── concepts/          # 概念页：抽象概念、架构、方法论
│   ├── sources/           # 来源摘要页：单篇原始资料的摘要
│   ├── comparisons/       # 对比分析页
│   └── playbooks/         # 工作流与操作指南
└── output/                # 一次性交付物（报告、指南、图表等）
```

## 页面类型与 frontmatter

| type | 目录 | 说明 |
|------|------|------|
| `Entity` | `wiki/entities/` | 项目、组织、工具、人物 |
| `Concept` | `wiki/concepts/` | 抽象概念、架构、方法论 |
| `Source Summary` | `wiki/sources/` | 单篇原始资料的摘要页 |
| `Comparison` | `wiki/comparisons/` | 对比分析 |
| `Playbook` | `wiki/playbooks/` | 工作流与操作指南 |

每页必填 `type`；概念与摘要页登记 `sources[]`；事实性论断用 footnote 指向来源。frontmatter 规范详见 `AGENTS.md`。

## 主要研究方向

- 语义层规范（datasets / fields / relationships / metrics / ai_context / custom_extensions）
- Hub-and-Spoke 转换器架构与 round-trip fidelity
- AI Context 与 Text-to-SQL / 数据 Agent 治理
- 生态采用（dbt、GoodData、Snowflake、Salesforce、Omni、OrionBelt、Polaris、Honeydew 等 60+ 组织）
- Apache 孵化流程、版本与成熟度演进

## 使用方式

- **查询**：从 `wiki/index.md` 开始，依据索引定位页面后深入阅读。
- **摄入**：新资料放入 `raw/` → 写来源摘要页 → 增量更新相关页面 → 更新 `index.md` → 追加 `log.md`。
- **产出**：持久化知识写入 `wiki/`，一次性交付物写入 `output/`。

完整的写入规范、actor 约定与操作规则见 [AGENTS.md](AGENTS.md)。