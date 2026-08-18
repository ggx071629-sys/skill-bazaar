# Coverage Ledger

未读本文件不得标「已审」、报覆盖率或做截断。两本账禁止混分母：「创建订单」和「PostgreSQL」不得进同一个分母。

跑完 12 阶段 ≠ 项目已全量覆盖。

## A. System Object Ledger

阶段 1 填对象，后续阶段填域。**同一物理对象一行**，用稳定 ID，禁止靠显示名当主键。**A03 不进本账本。**

```text
Object ID            # OBJ-001 起，本跑内递增
Canonical Name
Primary Type         # 见下方优先级；决定默认相关域
Roles                # 同时承担的职责，如 Service, API；不另建行
Tier                 # T0 | T1 | T2（阶段 2 后按能力引用回写）
Discovery Source
Scope State          # in-scope | out-of-agreed-scope | tier-cut
Relevant Domains
每域 Status          # 已审 / 未审 / N/A
Evidence
Finding IDs
Not Audited Reason
Notes
```

`Scope State` ≠ 域 Status ≠ N/A。`tier-cut` / `out-of-agreed-scope` 不得写成 N/A。

Finding 与能力必须引用 `OBJ-xxx`，不要只写 `payment-service`。

### Primary Type 判定（按职责语义，按序命中即停）

**能独立 build/publish 不足以成为 Application。** Application 是运行时产品单元，不是构建单元。

1. 独立数据存储 → Database  
2. 外部系统提供的接口/依赖 → External API  
3. 独立部署环境/运行目标 → Deployment Target  
4. 主要职责是后台消费/异步执行 → Worker  
5. 主要职责是定时触发 → Cron  
6. 主要职责是队列/消息通道 → Queue  
7. 用户或系统可直接执行的命令入口 → CLI  
8. 可被其他制品消费、但不能独立承担运行时业务 → Package / Library  
9. 独立运行并承担产品/系统运行时职责，且不属于上述更具体类型 → Application  
10. 其余 → Service / API / Webhook  

`Primary Type` 决定 Domain Applicability。`Roles` 只描述兼职。同一对象不要一次 Application、另一次 Service。

允许的 Primary Type：Application、Service、Package、Library、Worker、Queue、Cron、Webhook、CLI、API、Database、External API、Deployment Target。禁止每个函数/文件一行。

## B. Capability Ledger（A03 专用）

一行 = 一条产品能力。**只有 A03 路径可以创建 `CAP-xxx` 行并写 Result。** 阶段 1 只列能力候选名，不分配 ID。其他域引用已有 `CAP-xxx`，或写 `N/A — <理由>`。禁止非 A03 切片扩账本 B 或编造 `CAP-???`。

```text
Capability ID        # CAP-001 起
Capability
Declared Source
User Entry
App Entry
Implementation
Dependency
Related Object IDs   # 链上 OBJ-xxx
Result               # Verified | Phantom | Hidden | Zombie | Shadow；未完成写未判定
Evidence
Finding IDs
```

Verified 只登记于此，不出 finding。一条能力只有一个 Result。Hidden / Shadow 可无正式声明，仍占一行。禁止把 User Entry / Implementation 再拆成覆盖行。

### 能力名单

并集去重，**先列全名，再给 Result**：

1. 产品声明（README / 文档 / IA / 营销）
2. 用户可到达入口（路由、菜单、CLI 子命令）
3. 已暴露未声明入口（Hidden / Shadow 候选）

只列已审能力 = 账本 B 造假。Result 可暂 `未判定`（计入未审）。

## Domain Applicability Matrix

Relevant Domains **禁止从零勾选**。从默认域出发，用书面 N/A 排除。

| Object Type | 默认相关域 |
| --- | --- |
| Application | A01 A02 A04 A05 A07 A08 A09 A10 A11 A12 A13 A14 A15 A16 A17 |
| Service | A01 A02 A04 A05 A06 A08 A09 A10 A11 A12 |
| Package / Library | A01 A04 A05 A07 A11 A16 A17 |
| Worker | A01 A02 A05 A06 A08 A09 A10 A11 A12 |
| Queue | A02 A05 A06 A10 A11 A12 |
| Cron | A01 A02 A05 A06 A08 A09 A10 A12 |
| Webhook | A01 A02 A05 A08 A09 A10 A12 |
| CLI | A01 A05 A08 A09 A15 A16 |
| API | A01 A02 A05 A06 A08 A09 A10 A11 A12 A15 |
| Database | A05 A06 A08 A09 A10 A11 A12 A13 |
| External API | A05 A08 A09 A10 A11 A12 A14 |
| Deployment Target | A09 A12 A13 A14 |

N/A 必须写**这一实例**为何没有该关切。禁止「不相关」「跳过」。用 N/A 把默认域删到只剩两格再宣布审完 = 覆盖造假。

## 已审门槛

对象×域标已审 ⇔ 有 finding ID，或「已查 0」且写明查了哪条路径/文件/契约。禁止只写对象名就打勾。每阶段结束回写本阶段格子。

## 规模与截断

截断只改变本跑审到哪，不改变 Mapped。**截断 ≠ N/A。** Reason 只能是 `tier-cut` 或 `out of agreed scope`。Closure 仍 Open。

阶段 1 先按 Primary Type 预标：Application / Worker / Database / Deployment Target → T0，其余待定。阶段 2 填完 Related Object IDs 后回写：

| Tier | 规则 | Scope |
| --- | --- | --- |
| T0 | 全部 Application、Worker、Database、Deployment Target；账本 B **完整能力名单**；每条能力链上的 User Entry / App Entry / 直接 Implementation 对象 | 必须留下；Result 来不及给 → `未判定` |
| T1 | 被至少一条已登记能力引用、且不是 T0 的对象 | 尽量审完 |
| T2 | 未被能力引用的其余对象 | 可 `tier-cut`；Package 可只抽 A17 |

丢掉 T0、只留「3 个核心服务」= 覆盖造假。用户收窄范围：范围外仍在 Mapped，Reason=`out of agreed scope`。

## 覆盖率（分开报，禁止平均）

```text
T0 Object Complete         = T0 中无未审格对象 / T0 对象数
T1 Object Complete         = 同上（T1）
System Cell Coverage       = 账本 A 已审格 /（默认域格 − 合法 N/A）
System Object Complete     = 账本 A 零未审格对象 / 账本 A 对象数
Capability Result Coverage = 账本 B 已有五种 Result 之一 / 账本 B 行数
Overall Audit Closure      = 两本账未审格都为 0 才 Closed，否则 Open 并同时报上列全部百分比
```

任一账本仍有未审格，禁止说「100% 全量完成」。只能说：全量流程执行完成 + 上列百分比 + Closure=Open|Closed。

## 单域

不升级成 12 阶段。范围内无该域对象：已查 + 0 finding。A03 只建账本 B；其余只建账本 A（本域列，仍从默认矩阵出发）。范围内入口做轻量盘点即可。
