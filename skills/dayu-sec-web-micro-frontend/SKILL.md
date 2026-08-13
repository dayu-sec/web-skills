---
name: dayu-sec-web-micro-frontend
description: 处理 Web 微前端架构的设计、实现、迁移与评审，覆盖工作区与独立仓库、宿主和子应用职责、运行时生命周期、路由与资源基址、样式和主题隔离、跨应用通信、独立运行、装配失败与集成验证；适用于 Garfish、qiankun、single-spa、Module Federation 等微前端场景。
---

# DayuSec Micro Frontend

## 工作顺序

1. 同时使用 `$dayu-sec-web-project-conventions`，读取目标仓库的 `AGENTS.md`、工作区结构、清单、运行时入口和构建配置，确认实际 Git 根及微前端角色。
2. 需要定位工作区、装配配置、宿主、子应用、Git 边界或关键入口时，读取 [项目结构与发现顺序](references/project-structure-and-discovery.md)。
3. 识别宿主、子应用、共享包、项目装配配置和微前端运行时的所有权；先确定目标单元，再修改代码。
4. 设计或评审职责、配置、依赖和通信时读取 [架构与所有权边界](references/architecture-boundaries.md)。
5. 处理生命周期、路由、资源基址、独立运行或装配错误时读取 [运行时、路由与制品](references/runtime-routing-and-artifacts.md)。
6. 处理布局、滚动、主题、样式、Portal 或集成验收时读取 [UI、主题隔离与验证](references/ui-theme-and-verification.md)。
7. 按实际代码组合任务 Skill：React 使用 `$dayu-sec-react`，shadcn/ui 与 Tailwind CSS 使用 `$dayu-sec-shadcn-ui`，基础视觉与主题使用 `$dayu-sec-basic-ui`，数据请求使用 `$dayu-sec-web-request`，TypeScript 使用 `$dayu-sec-typescript`。
8. 同时验证子应用独立运行和宿主集成；修改生命周期或全局副作用时必须验证重复挂载与卸载清理。

## 分层边界

- `$dayu-sec-web-project-conventions` 统一源码职责、路径命名、文件路由和业务视图协议；本 Skill 不复制这些公共规则。
- 本 Skill 只负责微前端架构扩展，不重复 React Hooks、组件选型、请求拦截、数据契约或主题 Token 的应用内部规则。
- 宿主和子应用内部仍遵循对应任务 Skill；只有涉及跨应用边界、装配、生命周期和隔离时才由本 Skill 补充约束。
- 单体 Web 任务使用 `$dayu-sec-web-monolith`，不加载本 Skill，也不保留微前端生命周期、通信或兼容分支。

## 引用入口

- [项目结构与发现顺序](references/project-structure-and-discovery.md)：工作区、配置仓库、宿主和子应用的目录角色、Git 边界、关键入口与定位顺序。
- [架构与所有权边界](references/architecture-boundaries.md)：宿主、子应用、共享包、配置、依赖和通信契约。
- [运行时、路由与制品](references/runtime-routing-and-artifacts.md)：生命周期、路由协同、资源路径、独立运行、错误和部署制品。
- [UI、主题隔离与验证](references/ui-theme-and-verification.md)：壳层与页面职责、作用域、滚动、Portal、主题同步和验收矩阵。

## 输出要求

- 明确指出目标单元、实际 Git 根、所有权、跨应用契约和不应修改的相邻单元。
- 区分独立运行行为、宿主集成行为和部署制品行为，不用其中一种验证替代另外两种。
- 说明新增或修改的全局副作用、清理时机、失败边界和恢复路径。
- 使用目标仓库的真实框架和配置作为依据；不要把其他项目的运行时、路径、字段或业务页面约定带入当前实现。
