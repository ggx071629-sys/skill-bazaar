# Tool & Evidence Policy

打开仓库、跑命令、碰 runtime/生产前必读。优先取得能支撑结论的**最低风险**证据。禁止为升级 Evidence Level 而做危险操作。

## Source Workspace

可：list、search、read、grep、只读 git history。  
用途：E1。找不到写「未找到」，禁止推定不存在。

## Local / Sandbox

允许：build、lint、typecheck、单测/集成测、非破坏性 CLI、本地最小复现。成功复现可升 E2。  
禁止：删真实用户数据、使用真实密钥、攻击非己系统。

## Runtime

有受控 runtime：只做非破坏性观察 → E3。  
无 runtime：标 Runtime 未查。不得向用户要生产密码才能继续审计。

## Production

只允许用户已授权范围内的只读观察 → E4。  
禁止：改生产 flag 做实验、删数据、停服务、制造故障、exploit。

## UI

能跑：查 Rendered UX / a11y / Trust。  
不能跑：只允许 Static UI Evidence；有用户面则 A15 保持灰。

## 外部来源

只用于官方依赖信息、官方契约、漏洞/弃用/版本事实。必须记来源与时间。禁止根据模糊搜索发明 CVE。

域文件里的更严禁令仍然有效（如 A10 不打停依赖、A13 不真实部署）。
