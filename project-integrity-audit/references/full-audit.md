# 全量 12 阶段

先读本文件再开跑。阶段 1：先 `discover.md` 再 `coverage.md`。打开仓库或跑命令前读 `tool-policy.md`。每阶段**只读**表中域文件。第一次写 finding 前读 `finding.md`。收口读 `scorecard.md`。禁止改成 17 域循环扫，禁止开场读完全部 domains。

阶段 1 可以没有 finding。某域没证据：该域灰 + 原因，不卡死后续。用户中途改口「只做完安全」：已完成域出分，其余灰，不要假装全量完成。每阶段结束按 `coverage.md` 回写格子。允许按阶段表并行取证：账本 B 只由阶段 2 创建（`coverage.md`）；写 Scorecard 前按 `finding.md` 合并同一 Locator。

| 阶段 | 做什么 | 读 |
| --- | --- | --- |
| 1 Reality Mapping | 按 `discover.md` 打开仓库入口，填 System Object Ledger；列入口。禁止把能力写入账本 A | `discover.md` `coverage.md` `tool-policy.md` |
| 2 Capability | 唯有本阶段可创建 `CAP-xxx` 并写 Result；五种结果（含 Verified）。规则 `coverage.md` | `domains/A03.md` |
| 3 Execution Path | 核心能力真实调用链；bypass / 重复 / 不可达 / 越界；关键路径行为 | `domains/A04.md` `domains/A01.md` |
| 4 Contract | Frontend↔Backend、服务间、事件、API↔DB、App↔Env、SDK↔API | `domains/A05.md` |
| 5 State & Data | 核心状态机 + 数据生命周期 | `domains/A02.md` `domains/A06.md` |
| 6 Verification | 测试有无证明价值，不看框架 | `domains/A07.md` |
| 7 Failure | 对真实依赖做故障推演；同一路径看资源无界 | `domains/A10.md` `domains/A11.md` |
| 8 Security & Config | 身份权限租户密钥；配置是否真生效 | `domains/A08.md` `domains/A09.md` |
| 9 Delivery | Source→Runtime 等式；交付门禁是否真执行；依赖与供应链 | `domains/A13.md` `domains/A14.md` |
| 10 Observability | 每条关键旅程 Detect / Diagnose / Attribute | `domains/A12.md` |
| 11 Experience & Trust | 有界面的关键旅程：UX + Accessibility + Trust，不得只做 Trust | `domains/A15.md` |
| 12 Drift & Lifecycle | 声明漂移 + 仓库卫生 | `domains/A16.md` `domains/A17.md` |
