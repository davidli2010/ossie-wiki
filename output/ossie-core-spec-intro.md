# Apache Ossie 核心规范完全指南

> 面向完全不了解 Ossie 的读者，只讲「规范本身」：它为什么存在、怎么组织、每一条字段怎么用、表达式怎么写、本体是什么，最后落到验证与转换器实践。
>
> 本文基于本仓库 wiki（Apache Ossie 知识库）整理，核心规范快照为 **0.2.0.dev0（Draft）**，数据截至 2026-08-15；文末附资料来源。

---

## 第一部分 · 宏观：规范在解决什么

### 1.1 一个你不陌生的痛

「收入」这个词，在公司里至少有三种算法：

- dbt 里：`SUM(amount) WHERE status='paid'`
- 销售看板里：`SUM(amount)`（含未付款）
- 财务 Excel 里：`SUM(amount) - refunds`

同一家公司、同一份数据，三套定义。仪表盘互相打架，AI 问数助手更是直接懵掉。这个现象叫**语义漂移（Semantic Drift）**，本质是：**数据层早就标准了，业务含义层从来没有标准。**

打个比方：

| 层级 | 已有标准 | 作用 |
| --- | --- | --- |
| 数据格式 | Parquet / Arrow | 统一「数据怎么存」 |
| 访问接口 | ODBC / JDBC | 统一「怎么连接取数」 |
| **业务含义** | **Ossie 要填的空白** | 统一「指标是什么、怎么算、什么场景能用」 |

Ossie 不碰存储、不执行查询。它只做三件事：**定义、交换、验证**一份可版本化的「业务语义」，让 dbt、Snowflake、Tableau、Salesforce 和 AI Agent 都能读懂同一份定义。

### 1.2 规范与项目的关系

- 项目前身叫 **Open Semantic Interchange（OSI）**，2026-07-10 进入 Apache 孵化器后更名 **Apache Ossie (incubating)**，由 ASF 治理。
- 规范本体是 **YAML / JSON / Markdown**，代码（Python 参考实现、Go CLI、转换器）只是配套工具。
- 目前版本 `0.2.0.dev0`（Draft，schema 可能在发布前变更）；历史上 `0.1.1`（2025-12-11）是 Initial release。

### 1.3 规范由哪几份文档组成

| 文档 | 位置 | 状态 | 解决什么 |
| --- | --- | --- | --- |
| **核心元数据规范 spec.md** | `core-spec/` | 0.2.0.dev0 | 语义模型的结构（datasets/fields/relationships/metrics/custom_extensions/ai_context） |
| **表达式语言提案 OSSIE_SQL_2026** | `core-spec/` | Proposed Final | 指标/字段表达式用什么 SQL 子集，跨引擎可移植 |
| **本体规范 ontology.md** | `ontology/` | 0.2.0.dev0 | 在结构互操作之上做**概念互操作**（EntityType/ValueType/映射） |
| **机器可读 schema** | `core-spec/osi-schema.json`、`spec.yaml`、`ontology/ontology.json` | Draft | 给校验器和转换器吃的权威定义 |

一句话：**spec.md 定义「数据长什么样」，OSSIE_SQL_2026 定义「表达式怎么写」，ontology.md 定义「业务概念是什么」。**

---

## 第二部分 · 微观（一）：核心元数据规范 spec.md

### 2.1 三个目标

- **Standardization**：为语义模型建立统一语言与结构；
- **Extensibility**：支持领域扩展而不破坏核心兼容；
- **Interoperability**：跨 AI 与 BI 应用交换复用。

### 2.2 两个枚举（整个规范的原子）

**Dialects（方言，7 个）**：`ANSI_SQL` / `SNOWFLAKE` / `MDX` / `TABLEAU` / `DATABRICKS` / `MAQL` / `BIGQUERY`。

**DataType（数据类型，10 个）**：`String` / `Integer` / `Decimal` / `Float` / `Boolean` / `Date` / `Time` / `DateTime` / `DateTimeTz` / `Opaque`。

注意几点：

- `Decimal` 是精确十进制但**精度/标度未指定**，`Float` 是近似值——下游自己定。
- `Time`（无日期的时刻）、`DateTimeTz`（带时区/偏移的瞬间）、`Opaque`（可移植词汇之外的类型）是核心额外类型。
- `Opaque` 类型要靠 `custom_extensions` 做厂商细化；**类型未知时干脆省略 `datatype`**，比猜一个更规范。

### 2.3 Semantic Model：顶层容器

```yaml
semantic_model:
  - name: sales_analytics          # 必填，唯一标识
    description: ...                # 可选
    ai_context: ...                 # 可选
    datasets: [...]                 # 必填
    relationships: [...]            # 可选
    metrics: [...]                  # 可选
    custom_extensions: [...]        # 可选
```

六个成员里只有 `name` 和 `datasets` 是必填。整个规范围绕数据集展开，关系、指标都是「数据集之上」的抽象。

### 2.4 Datasets：逻辑数据集 = 业务实体

> 一个 dataset 对应一个业务概念（订单、客户、产品），绑定一张物理表或一个查询。

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `name` | ✅ | 唯一标识 |
| `source` | ✅ | `database.schema.table` 或查询 |
| `primary_key` | | 主键列数组（单列或复合） |
| `unique_keys` | | 唯一键数组，每条可单列或复合 |
| `description` | | 描述 |
| `ai_context` | | 同义词、常用术语 |
| `fields` | | 行级属性（分组/过滤/指标表达式用） |
| `custom_extensions` | | 厂商属性 |

键的三种写法：

```yaml
primary_key: [customer_id]              # 简单主键
primary_key: [order_id, line_number]    # 复合主键
unique_keys:
  - [email]                             # 简单唯一键
  - [first_name, last_name]             # 复合唯一键
```

`primary_key` 与 `unique_keys` 的关系：primary_key 是「首选的唯一标识」，unique_keys 是补充约束，二者都用于推断关系是 many-to-one 还是 one-to-one。

### 2.5 Relationships：外键连接

> 关系是**全局定义、按名引用**的（不像某些工具内联在数据集里）。

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `name` | ✅ | 唯一标识 |
| `from` | ✅ | 多侧数据集 |
| `to` | ✅ | 一侧数据集 |
| `from_columns` | ✅ | from 侧外键列 |
| `to_columns` | ✅ | to 侧主键/唯一键列 |
| `ai_context` / `custom_extensions` | | 同前 |

```yaml
# 简单关系
- name: orders_to_customers
  from: orders
  to: customers
  from_columns: [customer_id]
  to_columns: [id]

# 复合关系（order_lines.product_id = products.id AND order_lines.variant_id = products.variant_id）
- name: order_lines_to_products
  from: order_lines
  to: products
  from_columns: [product_id, variant_id]
  to_columns: [id, variant_id]
```

**两条硬规则**：`from_columns` 与 `to_columns` 列数必须相同；列按位置一一对应。复合键靠「列顺序」表达对应关系。

### 2.6 Fields：行级属性

> 字段 = 单列引用或标量计算表达式，用于分组、过滤与指标表达式中。

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `name` | ✅ | 数据集内唯一 |
| `expression` | ✅ | 带方言的表达式对象 |
| `dimension` | | 维度元数据（目前只有 `is_time`） |
| `label` | | 分类标签 |
| `description` | | 描述 |
| `datatype` | | 逻辑类型（10 枚举之一） |
| `ai_context` / `custom_extensions` | | 同前 |

**expression 是多方言对象**——同一字段可同时给出多套 SQL：

```yaml
expression:
  dialects:
    - dialect: ANSI_SQL
      expression: "customer_id"
    - dialect: SNOWFLAKE
      expression: "customer_id"
```

`expression` 必须是**标量** SQL（无聚合）；可以是简单列引用或计算表达式（如 `first_name || ' ' || last_name`、`UPPER(email)`）。

#### 2.6.1 最重要的设计：`datatype` vs `is_time`

这是 Ossie 区别于很多工具的关键分离：

- **`datatype`** = 这字段装什么**类型的值**（Date、Integer、String…），回答类型问题；
- **`dimension.is_time`** = 这字段是否承担**时间角色**，回答角色问题。二者正交。

`is_time` 的默认规则：

- 未显式设置时，若 `datatype` 是 Date/Time/DateTime/DateTimeTz → 默认 `true`；否则默认 `false`。
- **显式 `is_time` 永远优先于默认**。

四种组合的真实用法：

| 列示例 | datatype | is_time | 有效角色 | 原因 |
| --- | --- | --- | --- | --- |
| `d_date` | Date | 省略 | 时间维度 | 时间类型，默认 true |
| `created_at`（审计时间戳） | DateTime | **false** | 常规维度 | 显式退出时间默认 |
| `d_year`（整型年份） | Integer | **true** | 时间维度 | 非时间类型，显式标角色 |
| `customer_id` | Integer | 省略 | 常规维度 | 非时间类型，默认 false |

设计先例：镜像 Snowflake Semantic Views 的 `time_dimensions:` 与 LookML 的 `dimension_group`。

**给消费者的指引**：数据类型问题（cast、序列化、类型推断）看 `datatype`；角色问题（时间序列分析、时间过滤、时间感知聚合）看 `is_time` 解析结果。**只有 `is_time: true` 时不要推断具体标量类型。**

### 2.7 Metrics：模型级量化度量

> 指标可以**跨多个数据集**，是字段的聚合表达式。

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `name` | ✅ | 唯一标识 |
| `expression` | ✅ | 带方言表达式（**含聚合**） |
| `description` | | 描述 |
| `datatype` | | 逻辑类型 |
| `ai_context` / `custom_extensions` | | 同前 |

```yaml
# 简单聚合
- name: total_revenue
  expression:
    dialects:
      - dialect: ANSI_SQL
        expression: SUM(orders.amount)
  datatype: Decimal
  ai_context:
    synonyms: ["total sales", "revenue"]

# 跨数据集指标
- name: avg_orders
  expression:
    dialects:
      - dialect: ANSI_SQL
        expression: SUM(orders.amount) / COUNT(DISTINCT customers.id)
  datatype: Decimal
```

注意：`SUM(orders.amount) / COUNT(DISTINCT customers.id)` 这种跨数据集引用，要求数据集间存在已定义的关系（转换器据此生成 join）。字段与指标的区别在于——**字段是标量，指标是聚合**。

### 2.8 Custom Extensions：扩展机制的正式出口

```yaml
custom_extensions:
  - vendor_name: string   # 自由格式字符串
    data: string          # JSON 字符串
```

核心规范必然覆盖不了所有厂商的特性。`vendor_name` 是自由格式，任何厂商可定义扩展而**无需修改核心规范**。已知 vendor：`COMMON`、`SNOWFLAKE`、`SALESFORCE`、`DBT`、`DATABRICKS`、`GOODDATA`、`HONEYDEW`、`WISDOM`（生态中还出现 `DATABRICKS`/`OMNI`/`NVIDIA_GSF`/`POLARIS`/`ORIONBELT` 等）。OrionBelt 最极端，同时用 `ORIONBELT`/`OSI`/`COMMON` 三个 vendor 打包自己的私有信息。

**这是 round-trip 的命脉**：厂商私有信息放这里，导入导出时能原样拿回，而不是被静默丢弃。

### 2.9 AI Context：给 AI 的语义治理接口

`ai_context` 可以挂在模型、数据集、字段、关系、指标的任意层级。两种形态：

```yaml
# 简单字符串
ai_context: "orders, purchases, sales"

# 结构化对象
ai_context:
  instructions: "Use this for sales analysis"
  synonyms: ["orders", "purchases", "sales"]
  examples: ["Show total sales last month", "What's the revenue by region?"]
```

三个推荐字段的三层价值：

- **instructions**（约束）：告诉模型什么场景能用、什么场景不能用；
- **synonyms**（消歧）：把「GMV / 成交额 / 销售额」映射到认证指标；
- **examples**（示范）：已审核的问题/查询，给 Agent 当生成锚点。

**边界要讲清楚**：这是**可传递的上下文元数据**，不是权限控制、也不自动防幻觉。硬约束仍需落在查询网关与数据权限层。

---

## 第三部分 · 微观（二）：表达式语言 OSSIE_SQL_2026

> 指标和字段的 `expression` 需要一个「可移植、可验证」的语言。提案新增方言 `Ossie_SQL_2026`，并建议设为**默认方言**。

### 3.1 定位与范围

- 基础是 **ANSI SQL:2003 Core**（ISO/IEC 9075-2:2003），看重跨库采用（Snowflake/Databricks/PostgreSQL/BigQuery）、语义明确、现代分析特性（window/CTE）。
- **范围限定在 Logical Layer**（metrics / fields / filters）；ontology 层表达式另有提案。
- 工作组：Will Pugh（Snowflake）牵头，参与含 Malloy、AtScale、Salesforce、dbt Labs、Relational AI、Databricks、Cube、ThoughtSpot、Lightdash、Starburst、Denodo。

### 3.2 支持什么 / 刻意不支持什么

**支持**：算术/比较/逻辑运算符、`BETWEEN`、`IN`（仅值列表，非子查询）、`LIKE`/`ILIKE`、`IS NULL`、`CASE WHEN`、聚合、窗口、标量函数、括号。

**刻意不支持**（这是设计，不是缺陷）：

| 构造 | 为什么交给语义层 |
| --- | --- |
| `SELECT` / `FROM` / `JOIN` | 数据集与关系负责 |
| `GROUP BY` | grain（粒度）由消费者控制 |
| `WHERE` | 用 filter 表达 |
| 子查询 / CTE | 用字段引用或 `EXISTS_IN` |
| `UNION` / `INTERSECT` / `EXCEPT`、`DDL` / `DML` | 超出表达式边界 |

### 3.3 标识符

- 正常标识符**大小写不敏感**；`"quoted"` 精确匹配；标准 ANSI 标识符，最长 128 字符。
- 命名空间支持多点 `dataset.field` 引用。

### 3.4 函数族（REQUIRED / RECOMMENDED / MAY 三级合规）

| 类别 | 级别 | 关键函数 | 备注 |
| --- | --- | --- | --- |
| 聚合 | REQUIRED | SUM/COUNT/COUNT(*)/COUNT(DISTINCT)/AVG/MIN/MAX | 附可分解性标注（Distributive/Algebraic/Holistic） |
| 统计 | REQUIRED | STDDEV/STDDEV_POP/VARIANCE/VAR_POP 等 | |
| 百分位 | REQUIRED | MEDIAN/PERCENTILE_CONT/DISC | Holistic |
| 近似 | RECOMMENDED | APPROX_COUNT_DISTINCT（~2% 误差）/APPROX_PERCENTILE（~1%） | sketch-based；PostgreSQL 不支持 |
| 条件聚合 | REQUIRED | DISTINCT 修饰 + CASE 过滤聚合 | |
| 日期/时间 | REQUIRED | CURRENT_*、YEAR/MONTH/DAY/HOUR、EXTRACT/DATE_PART、DATE_TRUNC/DATEADD/DATEDIFF、TO_DATE/TO_TIMESTAMP | format-string 解析为 EXPERIMENTAL（跨引擎 token 差异大） |
| 日期格式化 | EXPERIMENTAL | TO_CHAR + 可移植 token 核心（YYYY/MM/MON…） | 名称 token 依赖 locale |
| 字符串 | REQUIRED | CONCAT/LENGTH/LOWER/UPPER/TRIM/LEFT/RIGHT/SUBSTRING/REPLACE/SPLIT_PART、POSITION/CONTAINS/STARTSWITH、LIKE/ILIKE/REGEXP_LIKE | regex 系列为 RECOMMENDED |
| 数学 | REQUIRED | ABS/ROUND/FLOOR/CEIL/TRUNC/MOD/SIGN/POWER/SQRT/EXP/LN/LOG、GREATEST/LEAST | 三角为 RECOMMENDED |
| 条件 | REQUIRED | CASE/IF/IFF/NULLIF/COALESCE/IFNULL/NVL/NVL2/ZEROIFNULL | |
| 窗口 | REQUIRED | ROW_NUMBER/RANK/DENSE_RANK/NTILE/LAG/LEAD/FIRST_VALUE/LAST_VALUE/NTH_VALUE + 聚合窗口 | |
| 类型转换 | REQUIRED | CAST（VARCHAR/INTEGER/DECIMAL/FLOAT/BOOLEAN/DATE/TIMESTAMP/TIME） | TRY_CAST RECOMMENDED |

### 3.5 合规等级与跨工具对照

- **MUST** = REQUIRED 全达标；**SHOULD** = 尽量达成 RECOMMENDED；**MAY** = 方言扩展。
- 提案附**跨工具映射表**：Ossie 标准函数 ↔ Tableau / Looker Studio / DAX 逐函数对照，例如 `COUNT(DISTINCT x)` ↔ `COUNTD` / `COUNT_DISTINCT` / `DISTINCTCOUNT`。转换器靠这张表跨平台落地。

---

## 第四部分 · 微观（三）：本体规范 ontology.md

> 核心 spec 解决**结构性互操作**（任何工具都能读写统一格式）。但 A 家叫 `customer`、B 家叫 `client`，格式读得懂不等于**说的同一件事**。ontology 解决**概念互操作**。

本体是企业数据的概念模型——用概念、关系与业务规则描述企业。注意：ontology 是**独立文档类型**（0.2.0.dev0），不并入 core-spec。

### 4.1 两类概念（ConceptType）

| 类型 | 含义 | 例子 |
| --- | --- | --- |
| `EntityType` | 必须用其它信息引用的现实世界概念 | 人（用社保号/邮箱引用）、公司 |
| `ValueType` | 带附加语义的数据类型 | 「社保号 = 恰好九位数字的字符串」 |

**内置概念**：`Any`（最一般实体类型）+ `Boolean`/`Date`/`DateTime`/`Decimal`/`Float`/`Integer`/`String`（各为最一般的值类型）。

**Multiplicity（多重性）**：`ManyToOne`（最后一个 role 由其它 roles 唯一确定）；`OneToOne`（二元关系的两个方向都 many-to-one）。

### 4.2 Ontologies 结构

```yaml
name: ...
description: ...
ai_context: ...
ontology: [...]        # 必填：概念及其分组关系
```

每个关系归到「扮演其第一个 role 的概念」下。概念字段：`concept`（唯一名）、`type`、`description`、`extends`、`derived_by`、`identify_by`、`requires`、`relationships`。

### 4.3 四个关键机制

**extends（继承）**：用户概念延伸一个或多个概念成子类型。值类型必须（直接/间接）延伸内建值类型；实体类型只能延伸实体类型且隐式延伸 `Any`。

**identify_by（标识关系）**：实体对象不能直接引用，必须借助一个或多个关系（如 `Person.nr` 用社保号引用人；`License.acct + License.seat_nr` 引用许可证）。

**derived_by（派生表达式）**：派生关系/概念可视为**视图**，由其它概念/关系派生：
- `ancestor_of`：递归派生关系；
- `Employee` 派生概念：`EXISTS ( Person.earns )` 分类出有薪水的人；
- 派生关系的表达式被解释为「构造链接的规则」（类似 SQL 构造新表）。

**requires（业务不变量）**：对群体必须成立的条件：
- 概念级：`0 < SocialSecurityNr`、`DegreesLatitude <= 90`；
- 关系级：`Amount > 0.0`、`Item.offers_in(Store)`。

官方示例 `examples/flights.yaml` 把「海拔以英尺计、纬度不超过 90、取消码只能 ABCD」这类跨模型语义固化下来，是「概念互操作」的落点：

```yaml
NrFeet extends [Decimal]        # ValueType：带单元语义的十进制
requires:
  - "DegreesLatitude <= 90"
  - "CancelationCode == 'A' OR 'B' OR 'C' OR 'D'"
```

### 4.4 Relationships 与 dot-join

关系字段：`name`、`description`、`multiplicity`、`roles`、`derived_by`、`requires`、`verbalizes`（必填）。关系是 **set 语义（非 bag）**，链接不含 null。

每个关系用「所属概念名.关系名」唯一标识（如 `Person.earns`），支持表达式中类似 OOP 的 **"dot-join" 导航**。roles 的隐喻：把关系看作窄表，链接是行、roles 是列；role 名缺省即扮演它的概念名。

### 4.5 Ontology mappings：把逻辑模型映射回概念

- **concept_mappings**：每个概念映射声明如何用对象填充概念、用链接填充该概念下分组的关系。`object_mappings` 与 `link_mappings` 二选一。
- **object_mappings**：值类型/简单标识实体用单个 SQL 表达式（如 `PERSONS.SSN` → Person 对象）；复合标识用可嵌套的 referent mappings（如 OrderLineItem：`nr` 用表达式 + `order` 嵌套映射 CustOrder 对象）。
- **link_mappings**：树形组织避免重复；`object_mapping`（必填）+ `relationship` + `children`；映射层级必须与关系 arity 一致（顶层一元、二级二元、以此类推）。

### 4.6 版本与边界

- 0.2.0.dev0（2026-05-29）：Basic support for ontologies and logical schema mappings。
- 实证缺口（来自 OrionBelt ontology 导出分析）：many-to-many 跳过（Multiplicity 枚举仅 ManyToOne/OneToOne）、复合键取首列、measures/metrics 不进入 ontology 层、反向 importer 推迟到 OSI 去掉 dev 后缀。ValueType 建模在 OBML 导出中被 deferred。

---

## 第五部分 · 落地：验证链与转换器

### 5.1 官方校验器 validation/validate.py

四层校验，逐层收紧：

1. **JSON Schema**（Draft 2020-12）：结构合规；
2. **唯一性**：dataset / field / metric / relationship 重名检查；
3. **关系引用**：关系端点（from/to）必须存在；
4. **SQL 语法**（sqlglot）：方言映射 `ANSI_SQL`→默认、`SNOWFLAKE`/`DATABRICKS`/`BIGQUERY`→对应方言；**`MDX`/`TABLEAU`/`MAQL` 无 sqlglot 方言，跳过 SQL 校验**。

`--schema` 参数可指向 ontology JSON，核心 spec 与 ontology 文档共用同一脚本。注意：**这是结构 + 引用 + 语法校验，不是完整语义编译器**。

### 5.2 方言回退链

转换器统一采用「**目标方言 > ANSI_SQL > 警告**」的回退顺序。字段/指标缺厂商方言时回退 ANSI_SQL 并记录警告；厂商与 ANSI 皆无则报错。这是多方言设计的运行规则。

### 5.3 转换器与 round-trip 三法则

Hub-and-Spoke 架构下，每个厂商一个 import、一个 export（`2×N` vs 点对点 `N×(N-1)`）。converter 指南要求的 **Round-Trip Fidelity 三法则**：

1. **绝不静默丢弃信息**——厂商特定属性无核心等价物时存 `custom_extensions`；
2. 尽量保留字段顺序（部分厂商对声明顺序敏感）；
3. 为所有厂商保留 `custom_extensions`——单一 Ossie 模型可同时携带多厂商元数据。

对 core-spec 用户而言，最相关的观察（2026-08-15，11 个转换器快照）：

- **「无损」是转换器级的显式决策，不是默认状态**：Databricks 把 Metric View 独有特性存进 `DATABRICKS` 扩展实现 MV→OSI→MV 无损；Omni 同理；NVIDIA GSF 甚至把整份原生文档嵌入扩展。
- 反向（OSI→厂商）常见有损，策略是「带警告丢弃」而非静默丢失。
- 映射不动的就**抛 `ConversionError`**（diamond join fan-out、非 equi-join、无 schema 的 source），绝不出无效结果。
- 结构性差异靠转换器内建逻辑桥接：OrionBelt 把全局 relationships 重组为 OBML 内联 joins、自动把 metric 分解为 measures + 公式。

### 5.4 参考实现与 CLI

- **PyPI 包 `apache-ossie`**（Python 3.11+，Pydantic v2）：所有 Python 转换器的解析/构造/校验/序列化共享基础。
- **Go CLI `ossie`**（cobra）：`convert`/`validate`/`plugin` 子命令；转换器以 subprocess 插件运行（plugin.yaml 声明 invoke 命令，JSON 信封 stdin/stdout 交换）。**诚实提示**：`convert`、`validate`、`plugin install|remove` 主体目前仍是 "not yet implemented"，已实现的是 `plugin list` 与插件发现/注入。规范先行、工具跟进。

### 5.5 一份「旗舰示例」帮你串起全部概念

`examples/tpcds_semantic_model.yaml` 是 core-spec 的完整示范（TPC-DS 零售模型），也是各转换器的基线测试基准。它的骨架长这样（示意）：

```yaml
version: "0.2.0.dev0"
semantic_model:
  - name: tpcds_retail_model
    ai_context:
      instructions: "Use this semantic model for retail analytics..."
    datasets:
      - name: store_sales                # 事实表
        source: tpcds.public.store_sales
        primary_key: [ss_item_sk, ss_ticket_number]   # 复合主键
        fields:
          - name: ss_sold_date_sk
            expression:
              dialects:
                - dialect: ANSI_SQL
                  expression: ss_sold_date_sk
            datatype: Integer
            dimension: { is_time: false }   # 显式退出时间角色
          - name: ss_sales_price
            expression:
              dialects:
                - dialect: ANSI_SQL
                  expression: ss_sales_price
            datatype: Decimal
      # ... date_dim / customer / item / store 等维度数据集
    relationships: [ ... ]               # 外键关系
    metrics: [ ... ]                     # 跨数据集聚合指标
```

把这一份 YAML 从头读一遍，等于把第二部分的每一条规则都用一遍。

---

## 第六部分 · 版本、边界与判断

### 6.1 版本状态

| 版本 | 时间 | 说明 |
| --- | --- | --- |
| 0.1.1 | 2025-12-11 | Initial release：核心语义模型结构；datasets/relationships/fields/metrics；多方言指标表达式；厂商扩展框架；agent 上下文 |
| 0.2.0.dev0 | 开发中 | Schema 可变；**生产环境勿依赖此版本**；新增 ontology 基本支持 |

### 6.2 已知的规范级缺口（路线图诚实承认）

- 指标粒度（grain）、关系基数、复合键业务语义、跨数据集安全聚合；
- 语义查询语言 + 参考 SQL 编译器、Registry；
- verified queries、暴露控制、认证与治理钩子；
- 单位/货币一等注解、PII 标注、extended metadata（display_format、default_aggregation 等，Issue #100）；
- 逻辑建模（语义与物理存储解耦）。

### 6.3 给你的三句话判断

1. **它是标准不是引擎**：Ossie 负责定义、交换、验证，不连接数据库执行查询。
2. **三层可移植性**：结构层（spec.md）+ 表达式层（OSSIE_SQL_2026）+ 概念层（ontology.md），一层比一层接近「业务含义」。
3. **看成熟度看细节**：「转换器目录存在」不等于完整兼容认证；要看每家的 round-trip 测试与明示的有损点。

---

## 附：资料来源

本文事实综合自本仓库 wiki（`wiki/`），全部页面为 machine-generated（写入者 `llm-wiki/deepseek-v4-flash-free`），**尚未经人工核验（unverified）**；规范快照以各页标注日期为准（2026-08-15）。

| 主题 | 详情看 |
| --- | --- |
| 核心规范正文（spec.md 全字段） | `wiki/sources/ossie-core-spec.md` |
| 表达式语言 OSSIE_SQL_2026 | `wiki/sources/ossie-core-spec.md` |
| 本体规范 ontology.md | `wiki/sources/ossie-core-spec.md`、`wiki/sources/ossie-ontology-tooling.md` |
| 机器可读 schema / validate.py / CLI / 参考实现 | `wiki/sources/ossie-tooling.md` |
| 转换器指南（9 步 / round-trip 三法则） | `wiki/sources/ossie-converters-guide.md` |
| 11 厂商转换器实证 | `wiki/sources/ossie-converter-ecosystem.md` |
| 概念背景（语义层 / 交换 / Hub-and-Spoke / AI Context） | `wiki/concepts/*.md` |
| 项目本体与路线图 | `wiki/entities/apache-ossie.md` |