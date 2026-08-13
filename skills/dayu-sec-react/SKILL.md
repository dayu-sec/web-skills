---
name: dayu-sec-react
description: 处理 React 前端代码的实现、重构与评审，覆盖依赖基线与选型、组件职责、Hooks library、状态管理、表单状态和 React Router 适配；适用于微前端与单体 Web 中的 `.tsx`、`.jsx`、React 路由和视图实现。
---

# DayuSec React

## 工作顺序

1. 先读取目标仓库的 `AGENTS.md`、现有目录和依赖版本，确认本次修改所属的 React 应用和模块。
2. 识别路由适配、业务视图、公共组件、请求和状态的职责边界，再修改 React 代码。
3. 涉及模板依赖基线、包选型、重复能力、未使用依赖或 bundle 影响时，读取 [React 前端依赖选择](references/dependency-selection.md)。
4. 涉及通用 Hooks library 的选择、引入、迁移、使用或评审时，读取 [React Hooks library 选择与边界](references/hooks-library-boundaries.md)。
5. 新建或重命名 React 组件、Hook、路由、页面、业务视图及其附属文件，或调整文件路由与视图分层时，同时使用 `$dayu-sec-web-project-conventions`；本 Skill 不复制 Web 项目共享的源码路径和视图分层规则。
6. React 组件或 Hook 任务涉及数据请求时，同时使用 `$dayu-sec-web-request`；本 Skill 不复制请求实例、错误传播或业务 Service 规则。
7. 沿用项目已有组件库、图标库、主题、请求和状态方案；新增依赖前确认现有能力不能覆盖。
8. 修改后运行目标单元已有的类型、Lint、格式、测试和构建命令；涉及路由时补充直接 URL、刷新和历史导航验证。
9. 目标仓库采用微前端且任务涉及宿主、子应用、生命周期、装配、跨应用通信或隔离时，同时使用 `$dayu-sec-web-micro-frontend`；本 Skill 不定义这些架构规则。

## 组件声明

- 团队约定函数组件使用具名 `function` 声明，不使用顶层箭头函数声明组件。
- 高阶组件或框架 API 必须接收函数表达式时，使用具名函数表达式保留组件名称；内联回调和箭头函数参数格式遵循 `$dayu-sec-typescript`。

## 引用入口

- [React 前端依赖选择](references/dependency-selection.md)：维护较大模板依赖基线，并按请求、契约、状态、UI、工具和重型场景选择已有依赖。
- [React Hooks library 选择与边界](references/hooks-library-boundaries.md)：选择通用 Hooks library，划分 React 原生 Hooks、`ahooks`、TanStack Query、React Router、状态库、表单和业务 Hooks 的职责。

## 组合入口

- 源码路径命名、文件路由与业务视图分层使用 `$dayu-sec-web-project-conventions`，不在 React Skill 中复制团队协议。
- 数据请求使用 `$dayu-sec-web-request`，不在 React Skill 中复制请求基础设施规则。
- TypeScript 函数声明、箭头函数和可擦除语法使用 `$dayu-sec-typescript`，不在 React Skill 中复制语言层规范。

## 输出要求

- 说明本次修改所属的 React 应用与模块，以及 React 路由适配和业务视图各自承担的职责。
- 涉及依赖时，说明使用场景、能力所有者、直接依赖、未选择相邻依赖的原因，以及 tree-shaking 或懒加载验证。
- 涉及 Hooks library 时，说明沿用或新增的方案、能力归属、直接依赖和未引入第二套综合库的证据。
- 指出保留的项目约定、必要例外和验证证据。
- 发现 React 路由适配承载业务逻辑、全局状态泄漏或组件职责混杂时，作为可维护性问题明确报告或修复。
