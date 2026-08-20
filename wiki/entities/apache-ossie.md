---
type: Entity
title: "Apache Ossie"
description: "Apache 孵化项目（原 Open Semantic Interchange / OSI），行业级规范努力，标准化在分析、AI 与 BI 平台间交换语义元数据的方式。"
resource: https://ossie.apache.org
entity_type: project
domain: [semantic-layer, apache, bi, ai]
tags: [ossie, semantic-layer, apache-incubator]
sources:
  - { id: juejin-ossie, resource: ../sources/apache-ossie-juejin-overview.md, title: "每天一个开源项目#40 Apache Ossie", last_modified: 2026-07-17 }
  - { id: conv-ecosystem, resource: ../sources/ossie-converter-ecosystem.md, title: "Apache Ossie 转换器生态", last_modified: 2026-08-15 }
  - { id: ossie-tooling, resource: ../sources/ossie-tooling.md, title: "Apache Ossie 工具链与机器可读工件", last_modified: 2026-08-15 }
  - { id: ontology-tooling, resource: ../sources/ossie-ontology-tooling.md, title: "Apache Ossie 本体规范与示例", last_modified: 2026-08-15 }
  - { id: core-spec, resource: ../sources/ossie-core-spec.md, title: "Apache Ossie 核心元数据规范", last_modified: 2026-08-15 }
  - { id: repo-governance, resource: ../sources/ossie-repository-and-governance.md, title: "仓库主页与贡献指南", last_modified: 2026-08-14 }
  - { id: roadmap-wg, resource: ../sources/ossie-roadmap-and-working-groups.md, title: "路线图与工作组", last_modified: 2026-08-14 }
  - { id: conv-guide, resource: ../sources/ossie-converters-guide.md, title: "转换器指南", last_modified: 2026-08-14 }
  - { id: website, resource: ../sources/ossie-website.md, title: "官网（首页/社区/生态/Updates）", last_modified: 2026-08-14 }
  - { id: community-updates, resource: ../sources/ossie-community-updates.md, title: "官网社区更新（更名/2026-04/FSI）", last_modified: 2026-07-10 }
  - { id: launch-ann, resource: ../sources/ossie-launch-announcements.md, title: "OSI 发布与采用公告", last_modified: 2025-09-23 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie

## 基本信息
- 状态：Apache 孵化项目（incubating）
- 前身：Open Semantic Interchange (OSI)
- 一句话定位：语义层的"Parquet"——让数据平台、BI 与 AI Agent 交换同一套业务意义，而非替代任一执行引擎。[^juejin-ossie]
- GitHub: [apache/ossie](https://github.com/apache/ossie)，官网: ossie.apache.org
- License: Apache License 2.0；主语言 Python 75.1% / Java 24.9%（主要为参考转换器，规范本体是 YAML/JSON/Markdown）。[^juejin-ossie]
- 快照数据（2026-07-17，[详情](../sources/apache-ossie-juejin-overview.md)）：Stars ~1,055、Forks 142、Open Issues 57、创建于 2025-11-18、GitHub Trending #1/17。[^juejin-ossie]
- 快照数据（2026-08-14，[仓库页](../sources/ossie-repository-and-governance.md)）：Stars **1.9k**、Forks 235、Open Issues 44、Commits 241、Watchers 46；被 ASF 接纳后已迁移至 `apache/ossie`。[^repo-governance]
- 生态：官网生态页（2026-08-14）列出 **60+ 组织**；[详情](../sources/ossie-website.md)。[^website]

## 治理（Apache Way / ASF 孵化）
- 参与以 mailing lists 为主渠道（dev@/commits@/issues@/private@）；GitHub 与 Slack 为次要。[^repo-governance]
- 决策目标 lazy consensus；代码变更至少 1 个 binding +1 无否决；**规范变更至少 3 个 binding +1 无否决**（discussion ≥7 天）。[^repo-governance]
- Release 两阶段审批：dev@ PPMC ≥3 binding +1 → general@incubator IPMC ≥3 binding +1。[^repo-governance]
- Milestone：2026-07-10 进入 Apache 孵化器并更名 Apache Ossie（更名动机、治理迁移与 Snowflake 后续方向详见 [community-updates](../sources/ossie-community-updates.md)）。[^community-updates]

## 要解决的问题（四类碎片化）
1. **Metric Drift**：不同工具同名 KPI 算法不同；
2. **Manual Translation**：人工迁移、对账语义定义；
3. **AI Hallucinations**：Agent 在冲突/缺失业务逻辑下生成不可靠答案；
4. **Integration Debt**：每引入一套工具就多一组脆弱的点对点连接。[^juejin-ossie]

## 核心架构
- 语义模型五类对象：datasets / fields / relationships / metrics / ai_context / custom_extensions。[^juejin-ossie]
- Hub-and-Spoke 转换器：以 Ossie 为中心，每平台只需 import/export 两条路径（复杂度 `N×(N-1)` → `2×N`）。见 [Hub-and-Spoke 转换架构](../concepts/hub-and-spoke.md)。[^juejin-ossie]
- 验证链：JSON Schema + 语义引用检查 + sqlglot 方言语法检查；不是完整语义编译器。[^juejin-ossie]

## 版本与成熟度
- 开发版本 `0.2.0.dev0 Draft`；latest released 文档称 `0.1.1`，但快照日 GitHub API 无正式 Release，仅见 `osi-0.1.1-rc1` 标签。[^juejin-ossie]
- 路线图未完成项：指标粒度、关系基数、语义查询语言、Registry、参考 SQL 编译器、verified queries 与暴露控制。[^juejin-ossie]
- 成熟度判断：短期增长动能高、行业影响潜力高、规范稳定性中低、生产成熟度处于早期试点阶段。[^juejin-ossie]

## 生态与转换器

### 转换器全景（2026-08-15 快照，11 个）
| 转换器 | 实现 | 目标格式 | round-trip 策略 |
| --- | --- | --- | --- |
| dbt | Python | MetricFlow Semantic Interface | MSI→OSI 有损（4 类 issue 记录），OSI→MSI 尽力重建[^conv-ecosystem] |
| Databricks | Python | Unity Catalog Metric View v1.1 | MV→OSI→MV 无损；Export 丢无 MV 槽位字段[^conv-ecosystem] |
| GoodData | Python | 声明式 LDM JSON | 扩展保留 labels/granularity；**metrics 不转换**[^conv-ecosystem] |
| NVIDIA GSF | Python | GsfModelDocument YAML | 扩展内嵌整份原生文档换 full-cycle 复用[^conv-ecosystem] |
| Honeydew | Python | workspace YAML 目录 | `HONEYDEW` 扩展；unique_keys↔primary_key 归一[^conv-ecosystem] |
| Omni | Python | 多文件模型目录 | Omni→OSI→Omni 无损；Export 丢 unique_keys 等[^conv-ecosystem] |
| OrionBelt | Python | OBML v1.0（+ontology 导出） | measures/metrics 分解 + 双 vendor 扩展[^conv-ecosystem] |
| Polaris | Java | Iceberg REST Catalog | field 级 `POLARIS` 扩展保留精确 Iceberg 类型[^conv-ecosystem] |
| Salesforce | Java | Salesforce Semantic Model JSON | `SALESFORCE` 扩展；metrics 当前不导出[^conv-ecosystem] |
| Snowflake | Python | Cortex Analyst YAML | 单向（OSI→SF），开发中[^conv-ecosystem] |
| WisdomAI | Python | Domain export JSON | 基数折叠进方向；ai_context 恢复 ONE_TO_ONE/多对多[^conv-ecosystem] |

要点：各厂用 `vendor_name` 区分扩展（DATABRICKS/OMNI/NVIDIA_GSF/...），OrionBelt 额外用 `OSI` vendor 存 OBML 无法表达的 OSI 字段；违反 requirement 的输入抛 `ConversionError` 而非静默猜测；方言一致回退「厂商方言 > ANSI_SQL > 警告」。[^conv-ecosystem]

README 宣称 50+ 参与组织（Databricks、dbt Labs、GoodData、Mistral AI、Salesforce、Snowflake、ThoughtSpot 等），为生态参与名单，不等同生产级兼容认证。[^juejin-ossie]

### 工具链
- **官方校验器** `validation/validate.py`：JSON Schema（Draft 2020-12）→ 唯一性 → 关系引用 → sqlglot 语法四层校验；`MDX/TABLEAU/MAQL` 跳过 SQL 校验。[^ossie-tooling]
- **Go CLI**（`ossie`，cobra）：`convert`/`validate`/`plugin`（install/list/remove）子命令；转换器以 subprocess 插件运行，JSON 信封 stdin/stdout 交换；`convert` 等主体实现仍是 `not yet implemented`。[^ossie-tooling]
- **Python 参考实现** PyPI 包 `apache-ossie`（Pydantic v2），全部 Python 转换器的共享基础。[^ossie-tooling]
- **机器可读工件**：`core-spec/osi-schema.json`（Draft 2020-12，DataType 枚举权威清单）、`core-spec/spec.yaml`（结构骨架）、`ontology/ontology.json`。[^ossie-tooling]

### 本体（ontology）
- 独立于 core-spec 的第二文档类型（`0.2.0.dev0`）：EntityType/ValueType 概念、关系的 roles/multiplicity、object/link mappings。[^ontology-tooling]
- 官方示例 `examples/flights.yaml` 是 ontology 文档：ValueType 通过 `extends` 叠加单元语义（NrFeet extends Decimal），`requires` 承载业务不变量断言（如 `DegreesLatitude <= 90`）。[^ontology-tooling]

## 工作组成置与路线图（2026-08-14 快照）
- 当前 **4 个工作组**（[详情](../sources/ossie-roadmap-and-working-groups.md)）：Metric Language（Will Pugh）、Catalog（Shubham Bhargav/Atlan）、Ontology（Kurt/Relational AI）、Financial Services Common Semantics（John Heisler/Snowflake）。
- 演进：2026-04 的 5 组（Advanced Metrics、Composability、Catalog、Ontology、Converters）→ 当前 4 组。[^roadmap-wg][^community-updates]
- **FSI 工作组**（2026-06-03 首次会议）：参与者含 Northern Trust、DTCC、LSEG、Verisk、BlackRock、S&P Global、AIG、TIAA 等；三项倡议 Architecture / Commercial Impact 度量 / Structure。[^community-updates]
- 路线图三类：当前努力（Metric Semantics、Catalog 集成、Ontology）、未来努力（逻辑建模、语义查询语言、SQL 方言、时间语义、AI-Native、治理、行业模型）、增量增强（命名、unit/currency、Extended Metadata #100 等）。详见 [roadmap](../sources/ossie-roadmap-and-working-groups.md)。[^roadmap-wg]

## 相关实体
- 上游组织：[Apache Software Foundation](apache-software-foundation.md)（待建）

## 相关概念
- [语义层](../concepts/semantic-layer.md)、[语义元数据交换](../concepts/semantic-metadata-interchange.md)、[Hub-and-Spoke 转换架构](../concepts/hub-and-spoke.md)、[AI Context](../concepts/ai-context.md)

[^juejin-ossie]: 每天一个开源项目#40 Apache Ossie（掘金，2026-07-17）
[^conv-ecosystem]: [Apache Ossie 转换器生态](../sources/ossie-converter-ecosystem.md)（2026-08-15 快照）
[^ossie-tooling]: [Apache Ossie 工具链与机器可读工件](../sources/ossie-tooling.md)（2026-08-15 快照）
[^ontology-tooling]: [Apache Ossie 本体规范与示例](../sources/ossie-ontology-tooling.md)（2026-08-15 快照）
[^repo-governance]: [Apache Ossie 仓库主页与贡献指南](../sources/ossie-repository-and-governance.md)（2026-08-14）
[^roadmap-wg]: [Apache Ossie 路线图与工作组](../sources/ossie-roadmap-and-working-groups.md)（2026-08-14）
[^community-updates]: [Apache Ossie 官网社区更新](../sources/ossie-community-updates.md)（2026-07-10）
[^website]: [Apache Ossie 官网](../sources/ossie-website.md)（2026-08-14）
