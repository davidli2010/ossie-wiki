---
title: "Apache Ossie 本体规范 ontology.md"
source: "https://github.com/apache/ossie/tree/main/ontology/ontology.md"
author:
  - "[[Apache Software Foundation]]"
published: 2026-08-14
created: 2026-08-14
description: "Ossie 本体规范：概念类型（EntityType/ValueType）、内置概念、关系与多重性、identify_by/derived_by/requires、概念映射与对象/链接映射（0.2.0.dev0）。"
tags:
  - "ossie"
  - "ontology"
  - "spec"
  - "clippings"
---
> 抓取时间：2026-08-14（main 分支）。原文含 ASF License header。

# Apache Ossie - Ontology Specification（0.2.0.dev0）

## Enumerations

### Concept types（区分两类概念）
| ConceptType | Description |
|---------|-------------|
| `EntityType` | 必须用其它信息引用的现实世界概念 |
| `ValueType` | 带附加语义的数据类型 |

实体类型：代表必须借助其它信息引用的真实对象（如人可用社保号或私人邮箱引用）；在部分建模语言中称 entities 或 object types。值类型：代表某数据类型（SQL 的 Integer/String 类）实例并附加语义，如社保号是恰好九位数字的字符串或正整数；部分语言称 data types 或 domains。

### Built-in concepts（内置概念，可直接按名引用）
`Any`（最一般实体类型）、`Boolean`、`Date`、`DateTime`、`Decimal`、`Float`、`Integer`、`String`（各为最一般的值类型）。

### Multiplicities
| Multiplicity | Description |
|---------|-------------|
| `ManyToOne` | 关系的最后一个 role 由其它 roles 唯一确定 |
| `OneToOne` | 两个方向都是 ManyToOne（仅限二元关系） |

## Ontologies
本体是企业数据的概念模型，用概念、关系与业务规则描述企业。规范以层级结构表示本体，把每个关系归到扮演其第一个 role 的概念下。

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 本规范唯一名 |
| `description` | string | No | 人类可读描述 |
| `ai_context` | string/object | No | AI 工具附加上下文 |
| `ontology` | list | Yes | 构成此本体的概念及其分组关系 |

### Concepts（概念）
概念代表业务中有意义的事物的类型，如 person、company、salary。每个本体隐式包含全部内置概念。字段：`concept`（唯一名）、`type`（EntityType|ValueType）、`description`、`extends`（父类型）、`derived_by`（派生群体表达式）、`identify_by`（唯一引用该概念对象的关系）、`requires`（约束群体表达式）、`relationships`（该概念play第一个role的关系）。

### Extends
每个用户声明概念延伸一个或多个概念，成为其子类型。任何值类型必须直接或间接延伸内置值类型（如 Integer、String）。实体类型只能延伸实体类型，且每个实体类型隐式延伸内置概念 `Any`。

### Relationships（关系）
关系把多个概念的对象关联起来，并声明如何表达这些链接。关系具有 set 语义（非 bag），链接不含 null。

字段：`name`（标识一部分）、`description`、`multiplicity`（多重性约束）、`roles`（附加 roles）、`derived_by`、`requires`、`verbalizes`（表达链接的 pattern，必填）。

每个关系用"所属概念名.关系名"唯一标识（如 `Person.earns`），支持表达式中类似 OOP 的 "dot-join" 导航。

#### Roles
对象在关系链接中扮演 roles。把关系看作窄表，链接是行，roles 是列。每个 role 由概念扮演；角色名缺省即扮演它的概念名；同名概念扮演多个 role 时必须给附加 role 声明区分名。

#### Multiplicities
多 role 关系中，最后 role 的对象可由其它 roles 对象元组函数式确定 → `ManyToOne`。三元+ 关系中作用于第 n 个 role。二元关系特殊情况下可声明 `OneToOne`（两个方向都 many-to-one）。

### Identifying relationships（标识关系）
实体对象不能直接引用，必须借助一个或多个关系。`identify_by` 数组允许建模者列出构成概念首选标识符的关系名（如 Person.nr 用社保号引用人；License.acct + License.seat_nr 引用许可证）。

### Derivation expressions（派生表达式）
概念与关系可用表达式派生。派生关系/概念可视为视图，对象或链接由其它概念/关系派生。示例：
- `ancestor_of` 派生关系：基础情形"Person 是其 child 的 parent"，递归情形 "Person 是 descendant 的 parent 的 ancestor"；
- `taxed_at` 派生关系：按 filing status 与收入决定税率；
- `Employee` 派生概念：`EXISTS ( Person.earns )` 分类出有薪水的人。

派生关系的表达式被解释为构造链接的规则（类似 SQL 查询构造新表行为）。

### Requires
requires 列出给概念或关系附加语义的表达式（对群体必须成立的条件）。概念级表达式必须引用该概念（如 `0 < SocialSecurityNr`）；关系级必须引用一个或多个 roles（如 `Amount > 0.0`、`Item.offers_in(Store)`）。

## Ontology mappings（本体映射）
本体映射声明如何把逻辑层的字段值映射到本体中的对象与链接。与本体按概念划分一致，映射分区到 concept mappings。

### Concept mappings
每个概念映射声明如何用对象填充概念、以及如何用链接填充该概念下分组的关系。声明由引用逻辑模型字段的表达式 pattern 构成（逻辑模型用 Ossie 核心语义模型规范声明）。字段：`concept`（必填）、`object_mappings` 或 `link_mappings`（二选一）。

### Object mappings（对象映射）
用模式 SQL 表达式把多个数据集字段映射到某概念的对象。
- 值类型或简单标识实体类型：对象映射就是一个 SQL 表达式（如 `PERSONS.SSN` 映射 Person 对象）。
- 无简单标识的实体类型：对象映射用 referent mappings 数组，每个声明如何用该概念的标识关系从其它对象映射到对象。

Referent mapping 字段：`relationship`（标识关系名）、`expression` 或 `referent_mappings`（二选一）。可嵌套（如 OrderLineItem 复合标识：`nr` 用表达式、`order` 用嵌套 referent mapping 映射 CustOrder 对象）。

### Link mappings（链接映射）
描述如何把逻辑字段 schema 映射到概念下分组的关系。组织成树结构避免重复并澄清映射意图。字段：`object_mapping`（必填，映射元组最后位置的对象）、`relationship`（其链接包含此映射元组的关系）、`children`（子树）。映射层级必须与所命名关系的 arity 一致（顶层可命名一元关系、二级二元、以此类推）。

语义上：每个链接映射用 SQL 表达式 pattern 映射到对象元组，元组可形成某关系链接或构成子映射映射的更长元组前缀。

## Version History
- **0.2.0.dev0**（2026-05-29）：Basic support for ontologies and logical schema mappings
  - 核心本体结构：Concept、relationships、business rules（requires 与 derived_by）
  - 从逻辑模型映射进本体的 schema mappings