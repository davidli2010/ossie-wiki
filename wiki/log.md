# 操作日志

## 2026-08-15
* **produce**: 基于 wiki 全库撰写《Apache Ossie 核心规范完全指南》（output/ossie-core-spec-intro.md）——宏观（语义层空白/规范文档全景）+ 微观（spec.md 全字段枚举/is_time 与 datatype 分离/OSSIE_SQL_2026 函数合规/ontology 四机制/验证链与 round-trip 三法则）+ TPC-DS 旗舰示例串讲。聚焦核心规范本体，区别于已有的项目级入门指南。纯综合编译，未引入新知识，无需新增 wiki 页；结构守护者检查通过（索引未变，output 一次交付物不影响 wiki 结构）
* **produce**: 基于全部 16 页 wiki 内容撰写《Apache Ossie 完全入门指南》（output/apache-ossie-intro.md）——宏观（背景/四类碎片化/语义层）+ 微观（六类构建块/YAML 示例/is_time/custom_extensions/AI Context/OSSIE_SQL_2026/ontology）+ 工具链/转换器生态/治理/成熟度评估/上手路径。纯综合编译，未引入新知识，无需新增 wiki 页；结构守护者检查通过（索引新鲜度 16=16，无新增孤立页）
* **query**: 读取 wiki/index.md 定位 16 页，深读 entities/concepts/sources 全部页面后撰写入门材料

## 2026-08-15
* **lint**: 全库健康检查（16 页）。通过：frontmatter type/generated/okf_version、索引新鲜度（16=16）、stale_after 均未过期（最早 2027-01-01）、raw 41 文件全部有摘要页且 resource 存在、实体页 domain 标签合规。修复：①10 个来源摘要页正文引用 `[^id]` footnote 但缺失定义块（约 43 处），已补定义使 label 命中 `sources[].id`；②孤立页 `ossie-converters-guide`、`ossie-launch-announcements` 无入链（仅索引收录）。遗留：`entities/apache-software-foundation` 待建断链；全部页面仍 unverified（校准期待人工确认）
* **update**: 补全 10 个来源摘要页的 footnote 定义块（wiki/sources/ossie-*.md）

## 2026-08-15
* **ingest**: 摄入 GitHub 仓库剩余文件——core-spec 规范正文（spec.md）、表达式语言提案（OSSIE_SQL_2026）、ontology 规范正文、GitHub README、CONTRIBUTING.md、ROADMAP.md、docs/working_groups.md、converters/README.md、官网四页（首页/社区/生态/Updates 索引）、官网三更新帖（更名/2026-04/FSI）、Snowflake 三篇与 Salesforce 一篇公告
* **update**: 创建 wiki/sources/ossie-core-spec.md（核心规范正文 + 表达式语言提案 + ontology 规范正文）
* **update**: 创建 wiki/sources/ossie-repository-and-governance.md（GitHub README + CONTRIBUTING.md）
* **update**: 创建 wiki/sources/ossie-roadmap-and-working-groups.md（ROADMAP.md + working_groups.md）
* **update**: 创建 wiki/sources/ossie-converters-guide.md（converters/README.md）
* **update**: 创建 wiki/sources/ossie-website.md（官网首页/社区/生态/Updates 索引）
* **update**: 创建 wiki/sources/ossie-community-updates.md（更名公告 + 2026-04 更新 + FSI 工作组）
* **update**: 创建 wiki/sources/ossie-launch-announcements.md（Snowflake×3 + Salesforce）
* **update**: 增量更新 wiki/entities/apache-ossie.md（治理/仓库指标 1.9k/工作组与路线图/生态 60+）
* **update**: 更新 wiki/index.md 与 wiki/log.md
* **lint**: 全部 12 个来源摘要页覆盖 raw/（raw 全部 34 文件均有对应摘要页）

## 2026-08-15（前一批）
* **lint**: 全库健康检查（9 页）。发现：①修复 25 处 `../raw/`→`../../raw/` 路径（含既往 juejin 页存量 bug）与 2 处缺失 `generated`；②frontmatter/okf_version/孤立页/过期项均通过；③全部 9 页无 `verified`（unverified，校准期内待人工确认）；④1 处待建断链 `entities/apache-software-foundation.md`（页内已标注"待建"）；⑤raw/ 有 19 个来源尚未建摘要页（ roadmap/core-spec/官网页面/官网更新帖/snowflake/salesforce 公告），知识已部分进入实体页但缺 Source Summary 页 —— 待执行批量摄入
* **ingest**: 摄入 GitHub 仓库剩余文件——11 个厂商转换器 README（databricks/dbt/gooddata/gsf/honeydew/omni/orionbelt/polaris/salesforce/snowflake/wisdom）、OrionBelt 两份映射分析、Go CLI 源码、validation/validate.py、core-spec 机器可读工件（osi-schema.json/spec.yaml）、ontology.json、示例模型（flights.yaml/tpcds_semantic_model.yaml）、python 参考实现 README
* **update**: 创建 wiki/sources/ossie-converter-ecosystem.md（11 厂商 + OrionBelt 映射分析）
* **update**: 创建 wiki/sources/ossie-tooling.md（CLI/validate.py/schema/examples/python 包）
* **update**: 创建 wiki/sources/ossie-ontology-tooling.md（ontology schema + flights 示例 + 导出边界）
* **update**: 增量更新 wiki/entities/apache-ossie.md（转换器全景扩至 11 个、工具链、本体小节）
* **update**: 增量更新 wiki/concepts/hub-and-spoke.md（round-trip fidelity 逐厂商实证）
* **update**: 增量更新 wiki/concepts/semantic-metadata-interchange.md（两层互操作 + 本体层）
* **update**: 更新 wiki/index.md 与 wiki/log.md

## 2026-08-14
* **init**: 创建 wiki 目录结构与索引框架
* **ingest**: 摄入掘金文章《每天一个开源项目#40 Apache Ossie》，原始资料移至 raw/apache-ossie-juejin-overview.md
* **update**: 创建 wiki/sources/apache-ossie-juejin-overview.md
* **update**: 创建 wiki/entities/apache-ossie.md
* **update**: 创建 wiki/concepts/semantic-layer.md
* **update**: 创建 wiki/concepts/hub-and-spoke.md
* **update**: 创建 wiki/concepts/ai-context.md
* **update**: 创建 wiki/concepts/semantic-metadata-interchange.md
* **update**: 创建 AGENTS.md schema 配置文件
* **update**: 更新 wiki/index.md