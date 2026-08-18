---
name: project-integrity-audit
description: >
  对软件项目做证据驱动的完整性审计：默认全量 12 阶段；用户点明一域则只审该域；已有 finding 可只出 Scorecard。
  产出是覆盖账本 + finding + Scorecard，不是发货许可；格子填完即摸底成功，黄/Open 是常态。
  用户说「审这个项目」「帮我审计」「完整性检查」「全面检查」「审计安全/契约/状态机」「出 Scorecard」时使用。
  没点具体域时默认全量，不要让用户先选阶段或域名。
  不要用于改代码修 Bug、写 exploit，或只做普通 code review。
---

# 项目完整性审计

## Purpose

对软件项目做证据驱动的完整性审计。用户只需给目标，不必知道 A01 或 12 阶段。

三种模式：全量 · 单域 · 只汇总。不改代码。不做破坏性验证。缺少证据 ≠ 没有问题。修复 finding 不属于本 Skill。

本 Skill 成功 = 格子填完 + 报告诚实。黄灯 / Open 残留风险是摸底完成的常态，不是失败。绿灯只表示已审范围内 Active Risk = 0，不等于可发货。

## Route

**全量** — 「审这个项目 / 帮我审计 / 完整性 / 全面检查 / PIAM / 十问」或只给仓库没点域。  
读 `references/full-audit.md`，按其进入阶段 1。不预读全部 domain。

**单域** — 用户明确只要一块。别名：行为/金额/分页 → A01；状态机/回滚 → A02；幽灵/僵尸/影子能力 → A03；架构/越界 → A04；契约/API → A05；数据/迁移 → A06；测试假绿 → A07；安全/越权 → A08；配置 → A09；韧性/故障 → A10；性能/N+1 → A11；假健康 → A12；构建发布 → A13；依赖/供应链 → A14；假成功/无障碍 → A15；文档漂移 → A16；仓库卫生 → A17。  
读 `references/discover.md` → `references/coverage.md` 单域节 → 仅对应 `domains/Axx.md`。第一次取证再读 `finding.md` 与 `tool-policy.md`。

**只汇总** — 「汇总这些 finding / 打分 / 出 Scorecard」且不要求继续查代码。  
读 `references/summary-mode.md`。不打开 `domains/`。不新增 finding。不得声称产生了新覆盖。

## Clarification Gate

默认自己推断。只允许问：

1. 0 个或 ≥2 个独立可审根，且无法确定目标  
2. 用户同时指定多个互斥模式  
3. 用户明确要对线，但完全没有部署线索  

一次只问一个问题。细节：`references/discover.md`。

## 按需加载

| 动作 | 必须已读 | 未读则 |
| --- | --- | --- |
| 定根 / 扫仓库入口 | `discover.md` | 停（全量或单域） |
| 打开仓库、跑命令、碰 runtime/生产 | `tool-policy.md` | 停 |
| 标已审 / 报覆盖率 / 截断 | `coverage.md` | 停 |
| 写出任何 finding | `finding.md` | 停 |
| 点灯 / 十问 / Closure | `scorecard.md` | 停 |
| 进入某域检查 | `domains/Axx.md` | 该域格子保持未审 |

## Completion

流程跑完 ≠ 覆盖完成。Skill 跑成功 ≠ 项目可发货。

全量：按 `coverage.md` + `scorecard.md` 收口。  
单域：只报告该域覆盖。  
只汇总：不得声称新覆盖。

收口三态（覆盖 / 残留风险 / 未查）只以 `scorecard.md` 为准。T0 名单没列全就点绿 = 没做完。Accepted Risk 仍算残留风险。

## Global invariants

- 不改仓库，只报告。
- 未查 ≠ N/A；截断 ≠ N/A。
- 未进入 Ledger ≠ 不存在；禁止删分母提高覆盖率。
- E1 不证明运行时事实。
- X5 不得升 Confirmed。
- Full process completed ≠ Full coverage completed。
- 覆盖闭合 ≠ 绿灯 ≠ 可发货。
