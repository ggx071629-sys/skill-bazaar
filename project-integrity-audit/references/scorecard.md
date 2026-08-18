# Scorecard 与收口

未读本文件不得点灯、答十问或宣布 Closure。

## 灯

**Active Risk** = Disposition 为 `Open` **或** `Accepted Risk`。接受风险 ≠ 风险消失。Resolved / Rejected / Duplicate 才退出风险灯。

| 灯 | 条件 |
| --- | --- |
| 红 | 存在 Active Risk 且 High/Critical 的 Confirmed 或 Reproduced |
| 黄 | 无红，但存在 Active Risk 的 Low/Medium Confirmed/Reproduced，**或** Open 的 Candidate |
| 绿 | 该域覆盖已完成，且 Active Risk = 0 |
| 灰 | 未审、覆盖不足，或缺必要访问 |

Critical + Accepted Risk 仍红；可同时标注 `Action Required: no`、`Residual Risk: Critical`。仅有 3 条 Open Low Confirmed → 黄。有 Active Risk 但对象未审完 → 灰。A15：UX / a11y / Trust 任一项未查 → 灰。A03：只看 Capability Ledger 是否每条都有五种 Result 之一（`未判定` 不算覆盖完成）。

## 计数

Severity 分级计数；Active Risk 的 Confirmed vs Candidate；Disposition 分项（含 Accepted Risk 残留）；专题（无则 0）：Zombie、Shadow、契约漂移、数据风险、False Success、架构越界、生产漂移、死/旧资产。

## 十问（全量必答）

每问挂 finding ID、「已查 0」，或「未查」+ Ledger 行。禁止靠印象。

1. 核心业务行为是否真正正确？
2. 状态和流程是否有非法或缺失转换？
3. 声称的能力是否真实存在且可达？
4. 代码是否仍遵守架构？
5. 边界与数据契约是否一致？
6. 测试通过是否意味着关键行为正确？
7. 数据、安全、配置、故障恢复是否可靠？
8. 仓库代码是否成为当前运行系统？
9. 用户看到的状态是否等于真实状态？
10. 是否还有失去价值但仍在维护的资产？

## 交付核对

三态分开写，禁止揉成「能不能信」或「审完了所以能发」：

1. **覆盖** — Closure Open|Closed + `coverage.md` 全部百分比。Closed = 格子填完，不是绿灯。
2. **残留风险** — 灯 + 最严重 Active Risk（含 Accepted Risk）。黄 / Open 是摸底完成的常态。绿 = 已审范围 Active Risk = 0，**不等于可发货**。
3. **未查** — 明确没查什么（含截断、缺权限、tier-cut）。

然后再给：模式 + 已查范围；两本 Ledger；finding 列表（Verified 不进；Duplicate 按 `finding.md` 处理后不进 Active Risk）；Scorecard。截断对象必须仍在表里。点灯条件只以本文件为准。
