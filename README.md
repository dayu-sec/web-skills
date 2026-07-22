# DySec Agent Skills

DySec 团队共享的 Agent 协作规则与 Skills。

- [`AGENTS.md`](AGENTS.md)：全局规则、Skill 分层与组合边界。
- `skills/<skill-name>/SKILL.md`：Skill 入口。
- `skills/<skill-name>/references/`：按需读取的详细规范。

项目仓库的 `AGENTS.md`、规格和源码始终优先于这里的全局约定。

## Skills

| 层级 | Skills |
| --- | --- |
| 基础通用 | `$dy-sec-code-commenting`、`$dy-sec-data-contract-first`、`$dy-sec-typescript`、`$dy-sec-rest-api` |
| Web 共享 | `$dy-sec-frontend-shared`、`$dy-sec-frontend-request`、`$dy-sec-frontend-mock`、`$dy-sec-react`、`$dy-sec-antd-ui-components`、`$dy-sec-basic-ui` |
| Web 架构扩展 | `$dy-sec-microfrontend` |
| 后端技术 | `$dy-sec-java-spring` |

Skills 位于 `$HOME/.agents/skills`。Agent 会按任务选择最小组合，也可以显式调用：

```text
使用 $dy-sec-basic-ui 设计并评审这个 React 页面。
```

## 维护

修改 Skill 时遵循 [`AGENTS.md`](AGENTS.md)，并运行 Skill Creator 校验。
