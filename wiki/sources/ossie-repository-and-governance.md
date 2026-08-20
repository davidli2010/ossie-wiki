---
type: Source Summary
title: "Apache Ossie 仓库主页与贡献指南（GitHub README + CONTRIBUTING.md）"
description: "apache/ossie GitHub 仓库 2026-08-14 快照与 CONTRIBUTING.md：仓库结构、参与指标、Apache Way 治理、投票规则、规范变更流程与角色职责。"
resource: https://github.com/apache/ossie
tags: [ossie, github, governance, apache-way]
sources:
  - { id: gh-readme, resource: ../../raw/apache-ossie-github-readme.md, title: "GitHub README（2026-08-14 快照）", last_modified: 2026-08-14 }
  - { id: contributing, resource: ../../raw/ossie-contributing-guide.md, title: "CONTRIBUTING.md", last_modified: 2026-08-14 }
usage_window: { from: 2026-08-14, to: 2026-08-14 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 仓库主页与贡献指南

> 快照 2026-08-14。仓库已从 `open-semantic-interchange/OSI` 迁移至 ASF 名下 `apache/ossie`。

## 仓库内容（README）
- `core-spec/`：核心规范（`spec.md`）、机器可读 schema（`spec.yaml`、`osi-schema.json`）及配套文档。
- `converters/`：在 Ossie 与其他语义格式之间转换的参考转换器（dbt、GoodData、Polaris、Salesforce 等）。
- `examples/`：示例语义模型，含完整 TPC-DS 模型。
- `validation/`：schema 验证工具。
- `docs/`：项目文档与概览。[^gh-readme]

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

> 对比掘金快照（2026-07-17）：stars ~1,056 → 1.9k，forks 142 → 235，open issues 57 → 44，commits 210 → 241。转换器从 8 个目录增至 11 个。[^gh-readme]

## Apache Way 治理（CONTRIBUTING.md）
ASF 治理下孵化，遵循 The Apache Way。**idée-force**：*如果没发生在 mailing list，就等于没发生*。[^contributing]

### Mailing lists（主要沟通渠道）
- **dev@ossie.apache.org**：开发与社区讨论（订阅 dev-subscribe@）。
- **commits@ / issues@**：自动化通知。
- **private@ossie.apache.org**：PPMC 私有列表（仅机密事务，如 committer 提名）。
- GitHub Issues/Discussions、Slack 为次要渠道，不替代列表。[^contributing]

### 决策与投票规则
- 目标 **lazy consensus**（无人反对即推进），无法达成一致时退回正式投票（在 dev@ 进行）。
- **代码/文档/工具**：至少一个 binding **+1** 且无否决。
- **规范变更**：**至少三个 binding +1 且无否决**（更高标准）。提议→最少 7 天讨论期（formal `[VOTE]` 为 72 小时）→投票。
- Committer/PPMC 提名：私有列表 lazy consensus（72 小时无 -1）。
- Binding 投票由 PPMC 成员投出；所有人可投非 binding 票。[^contributing]

### Releases（孵化项目两阶段审批）
1. dev@ `[VOTE]`：PPMC 至少三个 binding +1 且 +1 多于 -1。
2. general@incubator.apache.org 二次 `[VOTE]`：IPMC 至少三个 binding +1。
Release 为源码版，经 ASF 渠道分发。[^contributing]

### 角色
Contributors → Committers（写权限、binding 投票权，须 ICLA）→ PPMC（方向与健康）→ Mentors（Incubator 指派）→ IPMC（监督全部 podling，批准发布与季度报告）。[^contributing]

### Committer 提名标准
基于跨代码/规范反馈/文档/社区支持/工作组参与的广度与质量；无固定公式与最短时间要求。首个非平凡贡献合入前及获得提交访问权前须有 ICLA 存档。[^contributing]

### 其他要点
- 全仓库内容按 Apache License 2.0 许可。
- 遵循 ASF Code of Conduct；AI 辅助贡献由提交者负个人责任（ASF Generative Tooling Guidance）。[^contributing]

## 相关实体
- [Apache Ossie](../entities/apache-ossie.md)（Apache Way 采用的背景：[名称与治理迁移](ossie-community-updates.md)）

## 与已有知识的关联
- 对应掘金概览已覆盖的基本信息，补充**仓库迁移后的官方指标快照**与**治理规则细节**。[^gh-readme][^contributing]
- ROADMAP.md 中『未来努力』的治理/验证板块与本页投票机制衔接。[^contributing]

[^contributing]: CONTRIBUTING.md
[^gh-readme]: GitHub README（2026-08-14 快照）
