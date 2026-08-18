# 发现：去哪找、何时问

阶段 1 必读。用户不必知道本文件。用仓库里的文件把账本 A 的对象和账本 B 的能力**名**填出来；填格子规则仍以 `coverage.md` 为准。

模式由 `SKILL.md` 路由，本文件不改模式。

## 默认推断（先做，不要先问）

| 缺口 | 做法 |
| --- | --- |
| 用户没说审哪个目录 | 工作区里只有一个 git 根 → 用它 |
| 没给生产/线上权限 | Runtime、E3/E4、Rendered UI 标未查，**不要问账号密码** |
| 没有 OpenAPI / ADR / 状态表 | 从路由、类型、代码反推；期望标 X5，不要让用户补架构课 |
| 能力声明很少 | 用入口反推名单（路由、菜单、CLI），不要让用户先列功能清单 |

## 必须问（只问这一类）

- 0 个或 ≥2 个可审根（多 repo / 多独立产品），且用户没指是哪一个
- 用户说「审计一下」又同时点了两个不同域
- 用户目标就是对线核对，但本环境没有任何部署/版本线索

一次只问一件事。能推断的不要问。

## 按这个顺序打开（有就读，没有就跳过并记来源缺失）

1. `README*`、`docs/`、产品/IA 文档 → 声称能力  
2. `package.json` / `pnpm-workspace` / `go.mod` / `pyproject.toml` / `Cargo.toml` / `*.sln` → Application、Package  
3. `docker-compose*`、`Dockerfile*`、`k8s/`、`helm/`、`terraform/` → Deployment Target、Database、Worker  
4. `.github/workflows/`、`.gitlab-ci*`、`Jenkinsfile`、类似 CI → 以后 A13 门禁  
5. `openapi*.{yml,yaml,json}`、`*.graphql`、`*.proto`、路由目录（`app/`、`pages/`、`src/routes`、`cmd/`）→ API、CLI、入口  
6. 队列/定时：`*cron*`、worker 目录、queue 配置 → Queue、Cron、Worker  
7. 锁文件与镜像 tag → 以后 A14  

找不到的来源写「未找到」，不要编对象。

## 能力名怎么长出来

来源并集与入账规则只以 `coverage.md` 为准。本文件只负责打开那些文件，不另定一份名单规则。

## 找不到怎么办

| 缺 | 做 |
| --- | --- |
| 无 README | 只用入口与清单，能力声明侧记缺失 |
| 无 CI | A13 门禁未查，不编流水线 |
| 无 UI | 账本 A 里 A15 按实例 N/A（理由：无用户面） |
| 纯库无服务 | Primary Type = Package/Library（见 `coverage.md`），不要标成 Application；Deployment 可无 |
