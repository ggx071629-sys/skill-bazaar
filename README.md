# skill-bazaar

Agent Skill 集市。每个子目录是一个可安装技能。

目前收录：

| 技能 | 做什么 |
| --- | --- |
| [project-integrity-audit](./project-integrity-audit) | 对软件项目做证据驱动的完整性摸底，产出覆盖账本、finding 与 Scorecard |

## 安装 project-integrity-audit

装好后，对代理说「审这个项目」「帮我审计」「完整性检查」，或运行 `/project-integrity-audit`。

**不是发货许可。** 格子填完即摸底成功；黄灯 / Open 是常态；绿灯 ≠ 可发货。不改代码。

### Skills CLI

```bash
npx skills add ggx071629-sys/skill-bazaar --skill project-integrity-audit
```

全局、跳过确认：

```bash
npx skills add ggx071629-sys/skill-bazaar --skill project-integrity-audit -g -y
```

### 手工放到 Grok

用户级（所有项目）— 把 `project-integrity-audit/` 整个目录复制到：

```text
~/.grok/skills/project-integrity-audit/
```

仓库级（仅当前项目）：

```text
<repo>/.grok/skills/project-integrity-audit/
```

目录里必须有 `SKILL.md`。新开会话后可用 `grok inspect` 确认来源。

### 其他兼容路径

同一目录也可放到 `~/.claude/skills/`、`~/.cursor/skills/`，或项目下的 `.claude/skills/`、`.agents/skills/`。

## 不做什么

- 不改仓库、不修 Bug
- 不写 exploit、不做破坏性验证
- 不是普通 code review
- 不宣布项目可发货

## 仓库布局

```text
project-integrity-audit/
  SKILL.md
  references/
```

新技能：在仓库根新增 `<skill-name>/SKILL.md`，不要塞进已有技能目录。
