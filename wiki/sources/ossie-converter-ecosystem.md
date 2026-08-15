---
type: Source Summary
title: "Apache Ossie 转换器生态（11 厂商 README + OrionBelt 映射分析）"
description: "apache/ossie 仓库 converters/ 下 11 个厂商转换器的 README 与 OrionBelt 两份映射分析文档的摘要：双向映射、round-trip fidelity 策略、各方向信息丢失点。"
resource: https://github.com/apache/ossie/tree/main/converters
tags: [ossie, converters, hub-and-spoke, roundtrip, ecosystem]
sources:
  - { id: conv-databricks, resource: ../../raw/ossie-converter-databricks-readme.md, title: "Databricks Converter README", last_modified: 2026-08-15 }
  - { id: conv-dbt, resource: ../../raw/ossie-converter-dbt-readme.md, title: "dbt Converter README", last_modified: 2026-08-15 }
  - { id: conv-gooddata, resource: ../../raw/ossie-converter-gooddata-readme.md, title: "GoodData Converter README", last_modified: 2026-08-15 }
  - { id: conv-gsf, resource: ../../raw/ossie-converter-gsf-readme.md, title: "NVIDIA GSF Converter README", last_modified: 2026-08-15 }
  - { id: conv-honeydew, resource: ../../raw/ossie-converter-honeydew-readme.md, title: "Honeydew Converter README", last_modified: 2026-08-15 }
  - { id: conv-omni, resource: ../../raw/ossie-converter-omni-readme.md, title: "Omni Converter README", last_modified: 2026-08-15 }
  - { id: conv-orionbelt, resource: ../../raw/ossie-converter-orionbelt-readme.md, title: "OrionBelt Converter README", last_modified: 2026-08-15 }
  - { id: conv-polaris, resource: ../../raw/ossie-converter-polaris-readme.md, title: "Polaris Converter README", last_modified: 2026-08-15 }
  - { id: conv-salesforce, resource: ../../raw/ossie-converter-salesforce-readme.md, title: "Salesforce Converter README", last_modified: 2026-08-15 }
  - { id: conv-snowflake, resource: ../../raw/ossie-converter-snowflake-readme.md, title: "Snowflake Converter README", last_modified: 2026-08-15 }
  - { id: conv-wisdom, resource: ../../raw/ossie-converter-wisdom-readme.md, title: "WisdomAI Converter README", last_modified: 2026-08-15 }
  - { id: orionbelt-mapping, resource: ../../raw/ossie-orionbelt-mapping-analysis.md, title: "OSI ↔ OBML Mapping Analysis", last_modified: 2026-08-15 }
  - { id: orionbelt-ontomapping, resource: ../../raw/ossie-orionbelt-ontology-mapping-analysis.md, title: "OBML → OSI Ontology Mapping Analysis", last_modified: 2026-08-15 }
usage_window: { from: 2026-08-15, to: 2026-08-15 }
generated: { by: "llm-wiki/deepseek-v4-flash-free", at: 2026-08-15T00:00:00Z }
status: stable
stale_after: 2027-02-01
---

# Apache Ossie 转换器生态

> 快照 2026-08-15（GitHub main 分支）。11 个厂商的独立 README + 2 份 OrionBelt 映射分析，是 converters/README.md 总指南的厂商级展开。

## 转换器概览

| 转换器 | 语言 | 目标格式 | 方向 | round-trip 策略 |
| --- | --- | --- | --- | --- |
| **Databricks** | Python | Unity Catalog Metric View YAML v1.1 | 双向离线 | Import 保留 MV-only 特性至 `custom_extensions[DATABRICKS]`，MV→OSI→MV 无损；Export 丢 OSI 无 MV 槽位的字段（带警告）[^conv-databricks] |
| **dbt** | Python | MetricFlow Semantic Interface `semantic_manifest.json` | 双向 | MSI→OSI 有损（4 类 issue 逐一记录）；OSI→MSI 尽力重建、不丢任何东西[^conv-dbt] |
| **GoodData** | Python | 声明式 Logical Data Model (LDM) JSON | 双向 | 自定义扩展保留 labels/date granularity/geo；MAQL 双方言表达式；**metrics 不转换**（MAQL 上下文无关模型 ≠ SQL 表达式模型）[^conv-gooddata] |
| **NVIDIA GSF** | Python | GSF `GsfModelDocument` YAML | 双向离线 | `NVIDIA_GSF` 扩展内嵌整个原生文档换取 full-cycle 复用 live identifier；datatype↔物理类型互逆[^conv-gsf] |
| **Honeydew** | Python | workspace YAML 目录 | 双向 | `HONEYDEW` 扩展保存 connection_expr；unique_keys↔primary_key 语义归一化[^conv-honeydew] |
| **Omni** | Python | 多文件模型（model/relationships/views/topics） | 双向离线 | `custom_extensions[OMNI]` 保存所有 Omni-only 特性，Omni→OSI→Omni 无损；Export 丢 `unique_keys`、部分 ai_context[^conv-omni] |
| **OrionBelt** | Python | OBML v1.0 | 双向 + ontology 导出 | 混淆度最高：measures/metrics 分解、global↔inline joins 重组、`ORIONBELT`/`OSI` 双 vendor 扩展[^conv-orionbelt] |
| **Polaris** | Java | Iceberg REST Catalog（Apache Polaris） | 双向（在线） | field 级 `POLARIS` 扩展保留精确 Iceberg 类型；Table properties 入 `COMMON` 扩展[^conv-polaris] |
| **Salesforce** | Java | Salesforce Semantic Model JSON | 双向 | `SALESFORCE` 扩展保留未映射属性（含 Email/Text、Currency/Number 细粒度类型）；metrics 当前不导出[^conv-salesforce] |
| **Snowflake** | Python | Cortex Analyst semantic model YAML | 单向（OSI→SF） | 纯离线；OSI 无对应槽位的概念（如关系 ai_context）丢弃并警告[^conv-snowflake] |
| **WisdomAI** | Python | Domain export JSON (format 1.0) | 双向离线 | 基数折叠进关系方向；关系名称/ai_context 用于恢复 ONE_TO_ONE/MANY_TO_MANY；`METRIC_TABLE_UNRESOLVED` 等警告[^conv-wisdom] |

## 跨转换器的共性经验

1. **round-trip 保真度设计是显式工程决策**：Databricks/Omni/GSF 均声明「Import 到 `custom_extensions`，Export 恢复」从而实现某一方向无损；而 Export 方向的"OSI 无厂商槽位"特性普遍带警告丢弃。[^conv-databricks][^conv-omni][^conv-gsf]
2. **`vendor_name` 标记体系**：各厂用自己的 vendor tag（DATABRICKS/OMNI/NVIDIA_GSF/HONEYDEW/SALESFORCE/POLARIS/GOODDATA/ORIONBELT），OrionBelt 还额外用 `OSI` vendor 存 OBML 无法表达的 OSI 原生字段，用 `COMMON` 存 OBML 私有类型信息。[^conv-orionbelt][^conv-polaris]
3. **Errors 而非静默猜测**：Databricks/Omni 对违反 requirement 的输入抛 `ConversionError`（如 diamond join fan-out、非 equi-join、无 schema 的 source），绝不出无效结果。[^conv-databricks][^conv-omni]
4. **方言回退**：一致采用「厂商方言 > ANSI_SQL > 警告」，`MDX`/`TABLEAU`/`MAQL` 等非 SQL 方言不入 sqlglot。[^conv-dbt][^conv-gooddata][^conv-orionbelt]
5. **datatype 独立性**：`datatype`（逻辑类型）与 `dimension.is_time`（时间角色）正交；Salesforce/Polaris 均强调「显式 String 时间维度仍是 String」。[^conv-salesforce][^conv-polaris]

## 成熟度差异

- **最成熟**（Hypothesis property-based round-trip + TPC-DS 基线测试）：Databricks、Omni、OrionBelt。[^conv-databricks][^conv-omni][^conv-orionbelt]
- **在线转换**：仅 Polaris（通过 Iceberg REST Catalog 读写真实 catalog）。[^conv-polaris]
- **标注意开发中**：Snowflake（"under active development… avoid in production"）。[^conv-snowflake]
- **已知缺口**：GoodData 与 Salesforce 的 metrics 不转换（对 SAQL/MAQL 的上下文相关语义无 OSI 等价）；salesforce 复杂关系（Formula/SemanticField）存模型级扩展而非转换。[^conv-gooddata][^conv-salesforce]

## OrionBelt 映射分析细节

### OBML ↔ OSI 结构差异（核心）
- OSI 是**单层模型数组**（`semantic_model[]`），OBML 是单模型 + 命名 dict（dataObjects/dimensions/measures/metrics）。[^orionbelt-mapping]
- **Measures vs Metrics**：OSI 只有一个"metrics"含完整 SQL；OBML 分离 measures（简单聚合）与 metrics（引用 `{[Name]}` 的派生计算）。转换器自动分解：`SUM(store_sales.ss_ext_sales_price) / COUNT(DISTINCT customer.c_customer_sk)` → 2 个 auto-measure + 1 个 metric formula。[^orionbelt-mapping]
- **Relationships**：OSI 全局定义、字符串引用；OBML inline 在 from 侧 dataObject 上。转换器在两种表示间重组。[^orionbelt-mapping]
- **命名**：OSI 全 snake_case code；OBML 支持 display name + code 双命名，code 成为 OSI field name。[^orionbelt-mapping]

### OBML-only / OSI-only 特性处理
- OBML-only（secondary joins/pathName、allowFanOut、dynamicDate、timeGrain、format、filters、withinGroup、locale 等）用时 `COMMON` vendor + `obml_` 前缀键保留在 custom_extensions；dynamicDate 与 locale 尚未保留。[^orionbelt-mapping]
- OSI-only：`unique_keys` 经 `OSI` vendor 扩展 round-trip（`obml_unique_keys`）；多方言表达式首读 `ANSI_SQL`→`SNOWFLAKE`→`DATABRICKS`，非 SQL 方言不解析；无法分解的 metric 逐字存 `obml_unconverted_metrics`（`LOSSY:` 警告，不丢弃）。[^orionbelt-mapping]
- 双向双验证：JSON Schema（Draft 7 / Draft 2020-12）+ 语义校验（OrionBelt ReferenceResolver/SemanticValidator；OSI 侧 unique names + ref 检查）。[^orionbelt-mapping]

### OBML → OSI Ontology 导出（独立文档）
- 产出**单独** OSI ontology 文档（`$id` 与核心 spec 不同、独立 root），绝不与 core-spec 合并——validate.py 只读第一个 YAML 文档。[^orionbelt-ontomapping]
- `dataObject`→`EntityType` concept，join→Relationship（multiplicity 由 joinType 映射），PK 列→object_mapping 表达式，FK 列→link_mapping。[^orionbelt-ontomapping]
- 已知缝隙：many-to-many 跳过（OSI Multiplicity 枚举仅 ManyToOne/OneToOne）、复合键取首列、measures/metrics 不在 ontology 层、反向 importer（OSI ontology→OBML）推迟到 OSI 去掉 dev 后缀。[^orionbelt-ontomapping]

## 相关实体
- [Apache Ossie](/entities/apache-ossie.md)（转换器全景见该页）

## 引入/强化的概念
- [Hub-and-Spoke 转换架构](/concepts/hub-and-spoke.md)——本页是「round-trip fidelity 成立前提」的逐厂商实证

## 与已有知识的关联
- 掘金概览（2026-07-17）评 8 个转换器；本批快照（2026-08-15）增至 11 个（新增 Databricks、NVIDIA GSF、WisdomAI），并给出每方向的精确丢/留点。[^conv-databricks][^conv-gsf][^conv-wisdom]

[^conv-databricks]: Databricks Converter README
[^conv-dbt]: dbt Converter README
[^conv-gooddata]: GoodData Converter README
[^conv-gsf]: NVIDIA GSF Converter README
[^conv-honeydew]: Honeydew Converter README
[^conv-omni]: Omni Converter README
[^conv-orionbelt]: OrionBelt Converter README
[^conv-polaris]: Polaris Converter README
[^conv-salesforce]: Salesforce Converter README
[^conv-snowflake]: Snowflake Converter README
[^conv-wisdom]: WisdomAI Converter README
[^orionbelt-mapping]: OSI ↔ OBML Mapping Analysis
[^orionbelt-ontomapping]: OBML → OSI Ontology Mapping Analysis
