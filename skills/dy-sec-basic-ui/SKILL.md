---
name: dy-sec-basic-ui
description: 为 DySec React 与 Ant Design Web 项目设计、实现或评审基础 UI 和操作型业务界面，覆盖信息层级、页面结构、交互状态、可访问性、Light/Dark、主题 Token、Emotion 和主题包接入；适用于微前端与单体应用内部的筛选、图表、列表、表格、详情、导航和自定义组件。
---

# DySec 基础 UI

## 工作方式

1. 读取目标仓库的 `AGENTS.md`、设计系统依赖、主题入口和已有界面，确认应用是否独占当前文档、Portal 实际挂载位置和微前端边界，沿用项目真实契约。
2. 确认目标用户、核心任务、主次操作、信息层级、数据密度，以及空态、加载态、异常态和权限态。
3. 设计或评审视觉层级、页面结构、操作区、列表、详情、自适应和可访问性时，读取 [基础 UI 视觉语言](references/design-language.md)。
4. 处理 Ant Design Token、Emotion、CSS 变量、主题包或应用作用域时，先按部署形态确定主题与 Portal 作用域，再读取 [React、Ant Design 与主题接入](references/react-antd-integration.md)。
5. 优先组合 Ant Design 基础组件；需要核对组件选型、API 或 Semantic DOM 时，同时使用 `$dy-sec-antd-ui-components`。
6. 实现后同时检查 Light/Dark、键盘焦点、文本溢出、窄视口、内容滚动、浮层遮挡和状态辨识，不只检查静态截图。

## 组合入口

- 本 Skill 约束微前端与单体应用内部共用的基础 UI，不定义宿主、子应用、生命周期、装配或跨应用隔离。
- 任务涉及微前端边界、主题同步或跨应用样式隔离时，同时使用 `$dy-sec-microfrontend`。
- React 组件职责使用 `$dy-sec-react`；TypeScript、注释和数据契约分别使用对应基础 Skill，本 Skill 不复制其规则。

## 输出要求

- 说明目标用户流程、页面区域、主次操作、状态覆盖和滚动归属。
- 实现或评审时指出实际使用的主题入口、语义 Token、文档与 Portal 所有权、应用作用域和布局边界。
- 项目约定与本 Skill 不一致时遵循更具体的项目约定，并明确说明差异。
- 说明完成的 Light/Dark、自适应、可访问性和交互状态验证。
