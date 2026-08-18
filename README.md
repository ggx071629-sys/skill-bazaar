# Skill Bazaar

Skill Bazaar 用于发现、分享、安装和管理可复用的 Agent Skills。

每个技能是独立目录，可按名安装，互不耦合。本仓库是目录与分发入口，不是某一个 Skill 的说明书。

## Skills

| Skill | 简介 |
| --- | --- |
| [project-integrity-audit](./project-integrity-audit) | 证据驱动的项目完整性摸底，产出覆盖账本、finding 与 Scorecard |

新增技能时在此表追加一行，并在仓库根增加同名目录。

## Install

```bash
npx skills add ggx071629-sys/skill-bazaar --skill <skill-name>
```

将 `<skill-name>` 换成上表中的目录名，例如 `project-integrity-audit`。

## Layout

```text
<skill-name>/
  SKILL.md          # 必填：name、description、执行说明
  references/       # 可选：按需加载的细则
  scripts/          # 可选：可执行辅助脚本
```

一个目录只放一个 Skill。`SKILL.md` 是唯一入口。

## Catalog rules

收录需同时满足：

- **独立** — 单独安装即可使用，不依赖本仓库里的其他 Skill
- **可安装** — 目录名即安装名，结构符合上表
- **可执行** — 代理读完 `SKILL.md` 能完成一项完整任务
- **可验证** — 有明确成功/失败标准，不把流程跑完当成结果正确
- **可追溯** — 结论能回到证据、路径或版本，禁止无出处断言

## Contribute

1. 在仓库根新建 `<skill-name>/`，写入 `SKILL.md`。
2. 按需补充 `references/` 或 `scripts/`，不要改其他 Skill 的目录。
3. 在上方 Skills 表追加一行（链接 + 一句话简介）。
4. 打开 Pull Request。

用法、限制、域细则写在该 Skill 自己的 `SKILL.md` 里，不要写进本页。
