---
name: dayu-sec-web-project-conventions
description: 统一单体与微前端 Web 项目的发现顺序、源码职责、目录和文件命名、文件式路由与业务视图分层及团队 Skill 组合；适用于 React、Vue 等 Web 项目的创建、实现、迁移、重构与评审。
---

# DayuSec Web 项目约定

## 工作顺序

1. 读取目标项目及上级适用的 `AGENTS.md`，再检查清单、构建配置、源码目录和实际 Git 根；项目事实和已有明确约定优先。
2. 判断当前目标是单体应用，还是微前端的工作区、装配配置、宿主、子应用或共享包。
3. 单体应用同时使用 `$dayu-sec-web-monolith`；微前端目标同时使用 `$dayu-sec-web-micro-frontend`。只有明确的架构迁移任务才同时加载两个架构 Skill。
4. 设计、创建、迁移或评审源码职责与路径命名时，读取 [源码结构与命名](references/source-structure-and-naming.md)。
5. 涉及文件式路由、页面入口或业务视图时，读取 [文件路由与业务视图](references/file-routing-and-business-views.md)。
6. 根据实际文件和职责读取 [Skill 组合](references/skill-composition.md)，只追加完成任务需要的框架、请求、Mock、UI、语言或契约 Skill。
7. 修改后运行目标仓库已有的针对性检查；路由变更同时验证直接 URL、刷新和历史导航，结构变更不得顺带迁移范围外的存量路径。

## 架构边界

- 本 Skill 只定义单体与微前端共用的项目协议，不复制单体 Shell、微前端生命周期、React API、请求实例、Mock 或 UI 组件规则。
- 已有项目使用等价目录或命名时先建立职责映射；除非任务明确要求迁移，不为统一形式发起无关重命名。
- 项目 `AGENTS.md`、清单、源码和运行时与本 Skill 不一致时，以项目事实为准，并在交付中说明差异。

## 引用入口

- [源码结构与命名](references/source-structure-and-naming.md)：通用源码职责以及 React、Vue 和一般模块的路径命名。
- [文件路由与业务视图](references/file-routing-and-business-views.md)：`pages` 路由入口与业务视图的统一职责协议。
- [Skill 组合](references/skill-composition.md)：公共约定、架构扩展和任务 Skill 的最小组合。

## 输出要求

- 说明当前项目的实际 Git 根、架构类型、目标单元和选用的最小 Skill 组合。
- 指出保留的项目约定、迁移范围、必要例外和验证证据。
- 发现路由入口承载业务逻辑、扫描排除规则掩盖目录失范、业务视图依赖路由文件名或跨 Git 根误操作时，明确报告或修复。
