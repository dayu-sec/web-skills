# DayuSec Agent Skills

DayuSec 团队共享的 Agent Skills。

- [`AGENTS.md`](AGENTS.md)：本仓库的 Skill 维护规则。
- `skills/<skill-name>/SKILL.md`：Skill 入口。
- `skills/<skill-name>/references/`：按需读取的详细规范。

目标项目的 `AGENTS.md`、规格和源码始终优先于共享 Skill。

## Skills

| 层级 | Skills |
| --- | --- |
| Web 公共约定 | `$dayu-sec-web-project-conventions` |
| Web 架构扩展 | 单体 `$dayu-sec-web-monolith`；微前端 `$dayu-sec-web-micro-frontend` |
| Web 任务能力 | `$dayu-sec-web-request`、`$dayu-sec-web-mock`、`$dayu-sec-react` |
| UI 产品与工程 | `$dayu-sec-ui-foundations`、`$dayu-sec-ui-product-client-facing`、`$dayu-sec-ui-product-internal-ops`、`$dayu-sec-ui-shadcn` |
| 基础通用 | `$dayu-sec-code-commenting`、`$dayu-sec-data-contract-first`、`$dayu-sec-typescript`、`$dayu-sec-rest-api` |
| 后端技术 | `$dayu-sec-java-spring` |

Web 项目先使用 `$dayu-sec-web-project-conventions` 确认边界，再选择一个架构扩展并按任务追加最小能力组合：

```text
使用 $dayu-sec-web-project-conventions 和 $dayu-sec-web-monolith 评审这个单体 React 页面。
```

## 维护

修改 Skill 时遵循 [`AGENTS.md`](AGENTS.md)，并运行 Skill Creator 校验。
