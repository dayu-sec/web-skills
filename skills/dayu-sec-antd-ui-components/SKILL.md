---
name: dayu-sec-antd-ui-components
description: 协助处理以 Ant Design 6.x 为基础的 UI 组件选型、页面搭建、组件用法核对和评审任务；适用于微前端与单体 Web 中需要读取组件级中文官方缓存、拆分组件类别、保持模块边界和渐进式参考资料的场景。
---

# Ant Design UI 基础组件

## 工作顺序

1. 先判断目标应用、页面或组件模块，避免把局部 UI 改动扩大到应用壳层或全局配置。
2. 再判断 UI 任务类型：通用控件、布局导航、数据录入、数据展示、反馈状态、重型组件，或脚手架 UI 规则核对。
3. 只读取当前任务需要的最小资料：通常是本文件加一个 `references/` 分类文件。
4. 当组件行为、语义结构、版本差异或示例写法不确定时，先读取组件级本地官方缓存，再考虑访问 Ant Design 网络文档。
5. 在最终建议或代码评审里说明目标单元边界、组件选择理由、状态覆盖和验证路径。

## 参考资料入口

- 官方资料来源：`references/official-docs.md`
- 组件级本地官方缓存索引：`references/official/component-manifest.md`
- 组件分类索引：`references/component-index.md`
- 通用控件：`references/component-common.md`
- 布局与导航：`references/component-layout-navigation.md`
- 数据录入：`references/component-data-entry.md`
- 数据展示：`references/component-data-display.md`
- 反馈与状态：`references/component-feedback.md`
- 重型或高级组件：`references/component-advanced.md`
- 脚手架 UI 规则：`references/scaffold-ui-rules.md`

## 输出要求

- 对普通 UI 任务，先说明应该读取哪个分类 reference；不要默认加载完整组件目录或全文官方缓存。
- 对组件选择，给出业务意图、用户操作、状态覆盖和替代组件取舍。
- 对官方组件细节，优先从 `references/official/components__<component>-cn.md` 读取；需要 `classNames`、`styles` 或 Semantic DOM 时读取相邻 `references/official/components__<component>-cn__semantic.md`。
- 对文字排版、组件间距和弹性对齐，优先使用 Ant Design 的 `Typography`、`Space`、`Flex`；不要默认用裸 `div`、`span`、零散 margin 或临时 CSS 拼出组件库已有能力。
- 对应用布局、菜单、面包屑和侧边栏相关任务，必须检查路由、菜单、刷新和局部加载失败行为。
- 保持页面、表单、列表、局部状态和业务组件位于对应业务模块，不把业务逻辑提升到应用壳层。
- 对样式任务，优先使用设计系统、主题变量和 Ant Design 组件能力；避免覆写 `body`、`html`、应用根节点等全局选择器。
- 任务涉及微前端挂载区、宿主与子应用职责或跨应用隔离时，同时使用 `$dayu-sec-web-micro-frontend`；本 Skill 只负责组件选型和应用内部 UI 实现。
- 对需要图标的按钮或工具入口，优先使用项目已有图标库；React 应用中优先使用 `lucide-react`，未安装时提示用户按项目包管理器安装或确认是否允许添加依赖。不要默认改用 Ant Design 基础组件中的 `Icon` / `@ant-design/icons`，除非项目既有规范已经明确采用它们。
