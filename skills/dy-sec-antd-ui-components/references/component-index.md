# 组件分类索引

## 先判断目标范围

- 只影响某个业务页面或功能模块：读取对应组件分类和 `scaffold-ui-rules.md`。
- 影响应用壳层、菜单、面包屑或侧边栏：读取 `component-layout-navigation.md` 和 `scaffold-ui-rules.md`。
- 影响项目配置、菜单配置或路由前缀：这不是普通 UI 组件任务，先读取目标仓库的配置和路由契约。
- 影响微前端挂载、宿主与子应用职责或跨应用隔离：同时使用 `$dy-sec-microfrontend`，不要在本 Skill 内推导架构规则。

## 任务到 reference 的映射

| 任务类型 | 首选 reference |
| --- | --- |
| 按钮、lucide 图标、Typography 文字排版、Space 间距、Flex 弹性布局、基础操作 | `component-common.md` |
| Layout、Menu、Breadcrumb、Tabs、Dropdown、侧边栏、页头 | `component-layout-navigation.md` |
| Form、Input、Select、DatePicker、Upload、校验 | `component-data-entry.md` |
| Table、List、Descriptions、Statistic、Tag、Badge、Empty | `component-data-display.md` |
| Modal、Drawer、Message、Notification、Alert、Result、Spin、Skeleton | `component-feedback.md` |
| Tree、Transfer、Calendar、Tour、ColorPicker、QRCode 等重型或低频组件 | `component-advanced.md` |
| 响应式、文本溢出、应用内样式作用域、评审检查 | `scaffold-ui-rules.md` |

## 渐进式读取规则

- 初始计划阶段只读 `SKILL.md` 加一个最相关 reference。
- 组件行为不确定时，先打开 `official-docs.md`，再从 `official/component-manifest.md` 定位本地组件文档。
- 单组件 API、示例和 Token 优先读取 `official/components__<component>-cn.md`。
- Semantic DOM、`classNames` 和 `styles` 优先读取 `official/components__<component>-cn__semantic.md` 或 manifest 中相邻 semantic 文件。
- 文字、间距、弹性布局优先路由到 `component-common.md`，并检查是否应使用 `Typography`、`Space`、`Flex`，不要直接让 Agent 用裸 `div` / `span` 拼 UI。
- 图标任务优先路由到 `component-common.md` 的 `lucide-react` 规则；只有项目已明确采用 `@ant-design/icons` 时才读取 Ant Design `Icon` 相关官方文档。
- 任务跨分类时，先确定主流程组件，再补充一个辅助 reference。
- 任务跨应用或模块时，先解释跨边界原因，再加载对应架构 Skill。

## 代表性路由

- 新增表单页：`component-data-entry.md`。
- 优化列表页：`component-data-display.md`。
- 设计页面操作按钮或工具图标：`component-common.md`。
- 调整侧边菜单或面包屑：`component-layout-navigation.md`。
- 处理加载失败、空状态或保存结果：`component-feedback.md`。
