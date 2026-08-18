# Finding 契约

未读本文件不得写出 finding。

问题写成：`{域} / {失效}`，例如 `A09 配置与环境完整性 / F05 Ineffective`。

## 失效

F01 Missing · F02 Broken · F03 Drifted · F04 Bypassed · F05 Ineffective · F06 False Positive · F07 False Negative · F08 Unreachable · F09 Zombie · F10 Shadow · F11 Unsafe Default · F12 Stale · F13 Inconsistent · F14 Incomplete

## 证据

E0 嫌疑（Suspected，只能 Candidate）· E1 静态 · E2 已复现（Reproduced）· E3 运行时 · E4 生产。

禁止 exploit；禁止破坏性复现（停生产库、打非己系统、真删数据）。

E1 **不能**无条件等于 Confirmed。E1 只确认纯静态事实（硬编码 secret、不可达分支、双方字段类型直接冲突、源码写明的禁层被越过）。Actual 依赖运行时、环境、部署、线上版本、外部系统或真实耗时 → E1 默认只能 Candidate。禁止把「可能线上不生效」写成「线上配置确认无效」。

## 风险（各 0–4，和 0–20）

Impact · Reachability · Detectability · Blast Radius · Persistence。  
0–4 Informational · 5–8 Low · 9–12 Medium · 13–16 High · 17–20 Critical。禁止跳过五维拍 High。

只根据**当前证据**打分。禁止因「理论上最坏很严重」抬分。未知写在 Reachability/Evidence 里；证据升级后可重评。E1 Candidate 不得默认 Critical。

| 维 | 0 | 1 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- | --- |
| Impact | 无行为影响，仅信息/维护 | 轻微体验/运维，无持久错误状态 | 有限功能错误或降级，可恢复 | 重要业务错误、安全边界/数据错误、显著不可用 | 严重数据损失、越权、大额财务/合规、核心能力失效 |
| Reachability（越高越容易触发） | 当前不可达 / 仅理论 | 管理员、罕见环境、非默认或多重前置 | 特定用户/状态/非主路径 | 常见用户流程或常见故障可触发 | 默认路径、公开入口、自动或高频 |
| Detectability（越高越难发现） | 系统立即阻止或可靠检测 | 明确错误 + 监控/告警易发现 | 日常日志或常规排查可发现 | 无直接告警，需专项排查 | 静默失败、False Success、长期看起来正常 |
| Blast Radius | 无实际影响对象 | 单请求 / 单记录 / 单用户 | 一组用户 / 一租户 / 一组件 | 多租户 / 大量用户 / 关键子系统 | 全局 / 跨租户 / 整平台 |
| Persistence | 无持久影响，立即自动恢复 | 临时；retry/restart 可恢复 | 状态持续，局部人工可修 | 跨 restart/deploy；需 backfill/迁移 | 不可逆、外部永久副作用、永久损坏或长期安全后果 |

## Expected Source（每条必填）

| 码 | 含义 | 最高 Status |
| --- | --- | --- |
| X1 Explicit Contract | PRD / ADR / API spec / schema / 状态表 | Confirmed |
| X2 Executable Contract | 类型、invariant、契约测试 | Confirmed |
| X3 Established System Invariant | 多处**独立**实现共同维持的明确规则（不是「多数文件碰巧这样写」） | Confirmed |
| X4 Domain Necessity | 数据/安全/事务不可违反条件 | Confirmed |
| X5 Inferred Expectation | 仅根据代码模式或「没写允许」推断 | 只能 Candidate |

没有写允许 ≠ 明确禁止。X5 不得 Confirmed。

## Schema

Finding ID：`{域}-{三位}`，如 `A02-001`。

**两个状态，不要混：**

| 字段 | 取值 | 含义 |
| --- | --- | --- |
| Evidence Status | Candidate / Confirmed / Reproduced | 有多确定 |
| Disposition | Open / Resolved / Accepted Risk / Rejected / Duplicate | 处理到哪。本 Skill **默认 Open**。不得因写了修复建议写成 Resolved。Resolved 仅当用户出示修复+回归证据 |

写出时 Evidence Status 只能是上三者。Scorecard 红/黄规则只以 `scorecard.md` 为准（Active Risk = Open 或 Accepted Risk）。

Component **必填**：写所属 `OBJ-xxx Canonical Name`。不要为单个 stale 文件新建 Object；挂到所属应用/包，Locator 写具体路径。

Capability **必填**，允许 `N/A — <理由>`（如 `N/A — no product capability relation`）。禁止裸 `N/A`。禁止为了填字段编造 `CAP-??? Repository Maintenance` 并污染账本 B。`CAP-xxx` 只引用账本 B 已有行；创建权只在 A03（`coverage.md`）。

必填：Title，域，失效，Component，Capability，Execution Path，Expected / Actual，Expected Source，Trigger，Reachability 文字，**Evidence 块**，Evidence Level，Evidence Status，Disposition，五维 + Total + Severity，Root Cause，Contributing Factors，Immediate Mitigation，Recommended Root-Cause Fix，Regression Protection，Related Findings。缺字段不得计入 Confirmed。密钥打码。

## Evidence 块

禁止只写「代码中可以看出」。至少含 Evidence Level、Source、Locator、Observation、Context、**Revision**。

**Revision：** 优先 commit SHA。无 `.git` / 仅有 zip 或 snapshot 时写 `snapshot / archive / workspace revision unavailable`，**不得因此降为 E0，不得编造 commit，不得为此问用户**。

E1：repository + 路径:行 + Revision。E2：Command、Result、Environment、Revision。E3：Environment、Observed At、Path。E4：production + 只读来源 + Observed At。

Root Cause 允许：`Unknown — not established by current evidence.`

- 根因未知 → Recommended Root-Cause Fix = `blocked on root cause`，禁止编因果。
- Immediate Mitigation 仍可给，且必须被 Actual/Evidence 直接支撑（堵住已看到的洞）。禁止把重写架构当 mitigation。
- Regression Protection 可以针对 Immediate Mitigation。

跨域只写 Related，不扩审。单域发现别的域：记编号，主结论留本域。

## 合并（按 Locator）

同一条 Evidence Locator（去空白、统一路径分隔后相同）只保留一条 finding。并行切片与同一切片重复写出都适用。

保留：Evidence Status 更高者（Reproduced > Confirmed > Candidate）；同级留字段更全者；再同级留先写出的 ID。其余 Disposition=`Duplicate`，Related 指向保留条。禁止用删除重复条来减少 Open。Locator 不同则不是同一条，不要因「看起来像同一类」合并。

## 校验（只汇总必读）

点灯前先按「合并」处理同一 Locator。缺字段的条目单独列出，不计入红灯。无 Evidence Status 时把旧 `Status` 当作 Evidence Status，Disposition 视为 Open。不重算五维：原文 Severity 沿用并标注「未复核五维」。不新增 finding。不改代码。
