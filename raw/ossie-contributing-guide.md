---
title: "Apache Ossie 贡献指南 CONTRIBUTING.md"
source: "https://github.com/apache/ossie/blob/main/CONTRIBUTING.md"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Apache Ossie 贡献流程：Apache Way 治理、参与方式、规范变更流程、投票规则、角色与职权、committer 流程。"
tags:
  - "ossie"
  - "contributing"
  - "apache"
  - "governance"
  - "clippings"
---
> 抓取时间：2026-08-14（main 分支）。

# Contributing to Apache Ossie (incubating)

Apache Ossie 在 ASF 治理下孵化。由 [The Apache Way](https://www.apache.org/theapacheway/) 治理——ASF 关于构建开放、厂商中立社区的准则集合。

## 参与方式
- **规范反馈**：评审提议的规范变更，在 mailing list、GitHub PR 与 issues 上分享观点。
- **用例讨论**：分享组织如何使用语义模型与面临挑战。
- **代码贡献**：验证工具、转换器、示例等。
- **文档**：改进文档。
- **社区支持**：回答问题、参与讨论、帮助新贡献者。

## 沟通（mailing lists 是主要渠道）
Apache Way 准则：*如果没发生在 mailing list，就等于没发生*。

- **dev@ossie.apache.org**：开发与社区讨论。订阅：发邮件至 dev-subscribe@ossie.apache.org。
- **commits@ossie.apache.org**：commits、PR 自动通知。
- **issues@ossie.apache.org**：issues 通知。
- **private@ossie.apache.org**：PPMC 私有列表，仅用于机密事务（如 committer 提名）。

次要非正式渠道（决策时不替代列表）：GitHub Issues/Discussions、Slack。

## 入门
1. 订阅 dev 列表并自我介绍。
2. 阅读核心规范 core-spec/spec.md。
3. 查看 TPC-DS 示例 examples/tpcds_semantic_model.yaml。
4. 浏览 issues 或开发列表提议主题。
5. 提 PR：fork、修改、提交 PR，走下述评审流程。

## CLA
所有贡献按 Apache License 2.0 许可。首个非平凡贡献合入前、以及获得提交访问权前，必须有 ICLA 存档。代表雇主贡献时可能需要 CCLA。保持 commits signed-off 且作者正确，确保来源清晰。

## AI 辅助贡献
- 无论如何生成，你对提交的代码负个人责任。
- 详见 ASF Generative Tooling Guidance。

## 工作流
规范仓库托管于 github.com/apache/ossie（从 ASF 基础设施 GitBox 镜像）。

### 代码、文档与工具（非规范）
标准 GitHub PR 评审：
1. 非琐碎事项先开 issue 或 dev@ 线程讨论方案。
2. fork 建主题分支。
3. 提 PR，说明动机与变更。
4. committer 评审并在至少一个 committer **+1** 且无未解决 **-1** 时合入。项目遵循 review-then-commit (RTC) 模型。

### 规范变更（更高标准）
1. **提议**：在 dev@ 公告，开 GitHub PR 说明动机、变更与对现有实现的影响。
2. **讨论期**：最少 7 天（正式 `[VOTE]` 则为 72 小时）；复杂变更可更长。
3. **投票**：讨论结束后在 dev 列表发起 `[VOTE]` 线程。

## 决策与投票
目标是 **lazy consensus**（无人反对即推进），无法达成一致时退回正式投票。投票在 dev@ 公开进行。

- **+1**：赞成。**0**：弃权。**-1**：反对——对代码与规范变更是**否决**，必须有技术论证；有效否决只能通过解决所述问题化解。
- **规则**：
  - 代码与其它变更：lazy consensus——至少一个 binding **+1** 且无否决。
  - **规范变更：至少三个 binding +1 且无否决**。
  - Committer 与 PPMC 提名：私有列表，lazy consensus（72 小时无 **-1**）。
  - 程序性投票（如采纳策略）：简单多数，不可否决。
- Binding 投票由 PPMC 成员投出；所有人鼓励投票，非 binding 投票是有价值输入。

### Releases（孵化项目：两阶段批准）
1. dev@ 发起 `[VOTE]`：至少 PPMC 三个 binding +1 且 +1 多于 -1。
2. general@incubator.apache.org 二次 `[VOTE]`：至少 Incubator PMC (IPMC) 三个 binding +1。

Release 为源码版，经官方 ASF 渠道分发，须符合 ASF release policy。

## 社区价值观——Apache Way
- **Community over code**：健康欢迎的社区是最重要资产。
- **Meritocracy**：以贡献论功绩，永不过期；代码、文档、测试、社区支持、规范评审都算。
- **Peer-based**：每位参与者无论雇主或资历都被视为同行。
- **Consensus decision making**：追求共识，无法达成时正式投票。
- **Open Communications**：技术讨论、设计决策、规范变更在 mailing lists 与公开资源中开放进行；异步决策让各时区贡献者都能参与。
- **Responsible oversight**：社区集体确保规范与工具保持高质量、安全且与使命一致。
- **Vendor neutrality**：项目独立于任何单一厂商或组织运行。

## 角色与职责
- **Contributors**：任何形式贡献者；可参与所有讨论与投票，投票非 binding 但有价值。
- **Committers**：通过持续高质量贡献获得写权限；评审合入 PR，技术决策有 binding 投票权；须有 ICLA。
- **PPMC**（Podling Project Management Committee）：负责 podling 整体方向与健康——技术方向、社区成长、发布监督；投票 binding。孵化期与导师协作，毕业时成为项目 PMC。
- **Mentors**：Incubator 指派指导 podling 的有经验 ASF 成员；帮助学习 Apache Way，护送发布投票至 IPMC。
- **IPMC**：监督所有 podlings，批准发布并评审季度报告，直至项目毕业为顶级项目。

## Committer 流程
- **标准**：基于跨代码/规范反馈/文档/社区支持/工作组参与的广度与质量；无固定公式、无最短时间要求。
- **过程**：任何 PPMC 成员可在私有列表发起提名 → PPMC 讨论投票（lazy consensus，72 小时无 -1 通过）→ 通过后邀请成为 committer → ICLA 存档、Apache 账号建立、写权限授予 → 向社区公告并列入项目文档。

## 行为准则
遵循 [ASF Code of Conduct](https://www.apache.org/foundation/policies/conduct)。关切可私下向 PPMC（private@ossie.apache.org）或按策略向 ASF 提出。

## 商标
Apache Ossie、Ossie、Apache、Apache feather logo、Apache Ossie 项目 logo 是 ASF 商标。

## License
全仓库内容（代码、规范、文档）按 Apache License 2.0 许可。