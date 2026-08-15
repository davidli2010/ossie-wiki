---
okf_version: "0.2"
---
# Wiki 索引

Apache Ossie 知识库 —— 追踪 Apache Ossie (incubating) 规范、架构、生态与落地方法。

## 实体

* [Apache Ossie](entities/apache-ossie.md) - Apache 孵化项目（原 Open Semantic Interchange / OSI），行业级规范努力，标准化在分析、AI 与 BI 平台间交换语义元数据的方式 `#project` `#semantic-layer`

## 概念

* [语义层](concepts/semantic-layer.md) - 位于数据存储（数据仓库/数据湖）与分析消费层（BI、AI Agent、指标平台）之间的统一业务语义抽象，提供单一可信源 `#concept`
* [Hub-and-Spoke 转换架构](concepts/hub-and-spoke.md) - 以中心格式为枢纽的转换架构：N 个平台的互转路径从 N×(N-1) 降为 2×N，前提是各平台映射足够完整且 round-trip fidelity 成立 `#architecture`
* [AI Context](concepts/ai-context.md) - Ossie 语义模型中的结构化元数据字段（instructions / synonyms / examples），为 Text-to-SQL 与分析 Agent 提供消歧、约束与示范 `#concept`
* [语义元数据交换](concepts/semantic-metadata-interchange.md) - 在分析、AI 与 BI 平台间标准化交换语义元数据（数据集、字段、关系、指标、AI 上下文）的规范方式，目标是厂商中立、单一可信源 `#standard`

## 来源摘要

* [每天一个开源项目#40 Apache Ossie](sources/apache-ossie-juejin-overview.md) - 掘金文章：Apache Ossie 概览、核心特性、Hub-and-Spoke 架构、验证链、生态与落地方法，快照于 2026-07-17 `#primary-source`
* [Apache Ossie 转换器生态](sources/ossie-converter-ecosystem.md) - 11 个厂商转换器 README + OrionBelt 映射分析：双向映射、round-trip fidelity 策略、各方向信息丢失点，快照于 2026-08-15 `#primary-source`
* [Apache Ossie 工具链与机器可读工件](sources/ossie-tooling.md) - Go CLI、validate.py、osi-schema.json / spec.yaml、ontology.json、示例模型、Python 参考实现，快照于 2026-08-15 `#primary-source`
* [Apache Ossie 本体规范与示例](sources/ossie-ontology-tooling.md) - ontology 体系：schema 结构、flights.yaml ValueType/requires 示例、OBML→OSI ontology 导出边界，快照于 2026-08-15 `#primary-source`
* [Apache Ossie 核心元数据规范](sources/ossie-core-spec.md) - spec.md（semantic model/datasets/relationships/fields/metrics/custom_extensions/ai_context）、表达式语言提案 OSSIE_SQL_2026（方言/函数合规）、ontology 规范正文，快照于 2026-08-15 `#primary-source`
* [Apache Ossie 仓库主页与贡献指南](sources/ossie-repository-and-governance.md) - GitHub README（1.9k stars/241 commits）与 CONTRIBUTING.md：Apache Way 治理、投票规则、角色与 committer 流程，快照于 2026-08-14 `#primary-source`
* [Apache Ossie 路线图与工作组](sources/ossie-roadmap-and-working-groups.md) - ROADMAP.md（当前工作组/未来努力/增量增强，含 GitHub 讨论编号）与当前 4 工作组 Lead，快照于 2026-08-14 `#primary-source`
* [Apache Ossie 转换器指南](sources/ossie-converters-guide.md) - converters/README.md：hub-and-spoke 模型、核心构造映射、编写转换器 9 步、边界情况与 round-trip 三法则，快照于 2026-08-14 `#primary-source`
* [Apache Ossie 官网](sources/ossie-website.md) - 首页定位与示例 YAML、社区渠道、60+ 生态组织与 Snowflake 集成、Updates 时间线索引，快照于 2026-08-14 `#primary-source`
* [Apache Ossie 官网社区更新](sources/ossie-community-updates.md) - 更名 Apache Ossie 公告（进入 Apache 孵化器）、2026-04 社区更新（14 新参与者）、FSI 工作组上线（问题陈述与三项倡议） `#primary-source`
* [OSI 发布与采用公告](sources/ossie-launch-announcements.md) - Snowflake 发布日主公告（17 创始伙伴/五项原则）、2025-11 扩展（MetricFlow 开源参考实现）、2026-01 v0.1 上线与 Salesforce metrics-as-code 视角 `#primary-source`