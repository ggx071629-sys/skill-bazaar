# skill-bazaar

这个仓库用来发现、分享、安装和管理可复用的 Agent Skills。

根目录里每个文件夹是一支技能：目录名就是安装名，自带 `SKILL.md`，和其他技能互不依赖。打开这个仓库时，当前公开的是：

| 目录 | 做什么 |
| --- | --- |
| [project-integrity-audit](./project-integrity-audit) | 对软件项目做证据驱动的完整性摸底，给出覆盖账本、finding 与 Scorecard。 |
| [tech-stack-selection](./tech-stack-selection) | 收敛主要技术路线，解释取舍，并维护已选、建议、待定与备选决策记录。 |

从本仓库按名安装：

```bash
npx skills add ggx071629-sys/skill-bazaar --skill <skill-name>
```

`<skill-name>` 换成上表目录名。装下来之后，怎么触发、怎么跑、有什么限制，以该目录里的 `SKILL.md` 为准。

仓库现在是这样放的：

```text
skill-bazaar/
  README.md
  tech-stack-selection/
    SKILL.md
    agents/openai.yaml
  project-integrity-audit/
    SKILL.md
    references/
```

以后新技能同样落在根目录，一个目录一支。必有 `SKILL.md`；细则放 `references/`，辅助命令放 `scripts/`，都按需要再加。

进这个仓库的技能要能单独安装、交给 Agent 就能执行完整任务，并且结果可核对、结论可追溯到证据。

往这里加技能：在根目录建文件夹，补一行到上面的表，开 Pull Request。
