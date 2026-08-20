# Apache Ossie 知识库 Schema

## 项目背景

本 wiki 追踪 **Apache Ossie (incubating)** —— 一个行业级规范努力，旨在标准化在分析、AI 与 BI 平台之间交换语义元数据的方式，提供厂商中立、单一可信源的语义数据。Ossie 前身为 Open Semantic Interchange (OSI)。

主要研究方向：
- 语义层规范（datasets / fields / relationships / metrics / ai_context / custom_extensions）
- Hub-and-Spoke 转换器架构与 round-trip fidelity
- AI Context 与 Text-to-SQL / 数据 Agent 治理
- 生态采用（dbt、GoodData、Snowflake、Salesforce、Omni、OrionBelt、Polaris、Honeydew）
- Apache 孵化流程、版本与成熟度演进

## 页面类型

| type 值 | 目录 | 命名格式 | 说明 |
|---------|------|---------|------|
| `Entity` | `wiki/entities/` | `{实体名}.md` | 项目、组织、工具、人物，用 `entity_type` 细分 |
| `Concept` | `wiki/concepts/` | `{概念名}.md` | 抽象概念、架构、方法论 |
| `Source Summary` | `wiki/sources/` | `{来源关键词}.md` | 单篇原始资料的摘要页 |
| `Comparison` | `wiki/comparisons/` | `{a}-vs-{b}.md` | 对比分析，用 `items_compared` 标注 |
| `Playbook` | `wiki/playbooks/` | `{场景}.md` | 工作流与操作指南 |

## Frontmatter（OKF v0.2 五大家族）

每页必填 `type`；推荐维护 `title` / `description` / `tags`；概念与摘要页登记 `sources[]`；写入时补 `generated: { by, at }`；经人工确认后补 `verified: { by: "human:<id>", at: ... }`；有过期风险的内容设 `stale_after`。

actor 约定：agent 用 `llm-wiki/deepseek-v4-flash-free`，人用 `human:<id>`，进程用 `process:<id>`。信任层级由 `verified` 推导（无 = unverified / 仅非 human: = machine-confirmed / 含 human: = human-reviewed），不使用 confidence 字段。

## 命名与链接约定

- 文件名全部小写、连字符分隔、去掉类型前缀。
- 站内互链用相对于当前页面的 Markdown 路径：来源页/概念页/实体页分别使用 `../sources/...`、`../concepts/...`、`../entities/...`；`wiki/index.md` 使用 `entities/...`。禁止以 `/` 开头，确保普通 Markdown 渲染器可直接打开。
- 引用 bundle 外文件（raw/output）用相对路径：`../raw/xxx.md`。
- 正文事实性论断用 footnote，label 必须等于某个 `sources[].id`。

## 操作规则

1. **查询规则**：回答任何关于知识库的问题时，第一步必须读取 `wiki/index.md`，根据索引中的摘要和标签定位相关页面，然后再深入阅读具体页面。禁止跳过索引直接搜索文件内容。
2. **产出物路由**：持久化知识（实体、概念、来源摘要、对比分析、工作流）写入 `wiki/`；一次性交付物（报告、幻灯片、图表、导出文件）写入 `output/`。
3. 每次查询产出交付物后，评估是否有值得持久化的知识，如有则同步更新 wiki（页面 + index.md + log.md）。
4. **摄入工作流**：新资料进入 `raw/` 后，阅读全文 → 写来源摘要页（`wiki/sources/`）→ 扫描并增量更新相关页面 → 更新 `wiki/index.md` → 追加 `wiki/log.md`。
5. 记录身份使用 actor 约定（见上）。
6. 每次写操作后执行「结构守护者」轻量检查：索引新鲜度、孤立文件、缺失目录、日志存在性、frontmatter 合规。
7. Apache Ossie 状态相关数据（版本、stars、commit、路线图）以来源日期为准，跨时间点追踪时注明日期并给出 `stale_after`。
