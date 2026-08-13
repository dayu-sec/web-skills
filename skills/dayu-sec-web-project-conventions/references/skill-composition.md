# Skill 组合

## 组合顺序

1. 先使用 `$dayu-sec-web-project-conventions` 确认项目、Git 根、源码职责和公共约定。
2. 单体应用追加 `$dayu-sec-web-monolith`；微前端工作区、装配配置、宿主、子应用或跨应用任务追加 `$dayu-sec-web-micro-frontend`。
3. 再按实际文件和任务追加最小专用 Skill，不因仓库包含某项技术就加载全部能力。

## 任务映射

| 任务职责 | Skill |
| --- | --- |
| 单体应用所有权、Shell、运行与验证 | `$dayu-sec-web-monolith` |
| 微前端装配、生命周期、通信、隔离与集成 | `$dayu-sec-web-micro-frontend` |
| React 组件、Hooks 和 Router 适配 | `$dayu-sec-react` |
| 请求实例、SDK、Service 和错误传播 | `$dayu-sec-web-request` |
| 本地 Mock、场景和契约来源 | `$dayu-sec-web-mock` |
| shadcn/ui、Tailwind CSS、组件原语与表单 UI | `$dayu-sec-shadcn-ui` |
| 基础 UI、主题、响应式与可访问性 | `$dayu-sec-basic-ui` |
| TypeScript 类型、模块和声明 | `$dayu-sec-typescript` |
| 运行时数据结构和 Schema | `$dayu-sec-data-contract-first` |
| REST 资源、HTTP 和 OpenAPI | `$dayu-sec-rest-api` |
| 源码注释 | `$dayu-sec-code-commenting` |

## 常用组合

- 单体 React 页面：公共约定 + 单体架构 + React；请求、UI 和契约按实际任务追加。
- 单体 Vue 页面：公共约定 + 单体架构；使用项目已有 Vue Skill 或框架约定，不加载 React。
- 微前端宿主或子应用：公共约定 + 微前端；再按目标仓库的框架和职责追加。
- 纯类型、纯组件、纯请求或纯数据契约任务：保留项目与架构边界，只加载直接相关的专用 Skill。
- 单体迁移到微前端：公共约定 + 单体架构 + 微前端，分别说明迁移前后所有权和最小变更。

项目 `AGENTS.md`、清单和源码始终优先。发现不同 Skill 对同一职责给出冲突规则时，先回到职责所有者，不通过同时保留两套实现制造兼容分支。
