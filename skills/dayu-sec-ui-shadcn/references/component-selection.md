# 组件选择

## 选择顺序

1. 先描述交互语义、数据规模、触发方式、是否阻塞当前任务以及键盘行为，不从视觉外形反推组件。
2. 检查项目上下文与本地 UI 源码，读取候选组件的导出、Props、变体和底层 primitive；已安装组件与项目修改优先于模型记忆。
3. 优先使用一个现有组件及其内置变体；单个组件不足时组合最少的现有组件；只有语义能力确实缺失时才新增源码。
4. 把布局、领域文案、数据请求和业务状态留在应用侧，不塞入基础组件或创建覆盖全部组件的二次封装。

## 需求映射

| 需求 | 优先检查 |
| --- | --- |
| 操作与链接式操作 | Button、ButtonGroup |
| 文本、长文本与带附加内容输入 | Input、Textarea、InputGroup |
| 有限选项与布尔状态 | Checkbox、RadioGroup、Switch、Toggle、ToggleGroup |
| 固定列表选择与可搜索选择 | NativeSelect、Select、Combobox、Command |
| 字段结构、说明与错误 | Field、Label；表单状态按项目表单方案处理 |
| 文件与附件展示 | Attachment；文件选择、上传和持久化由业务层负责 |
| 数据与摘要展示 | Table、Card、Item、Badge、Avatar；图表先检查项目既有图表栈 |
| 页面与层级导航 | Breadcrumb、Tabs、Pagination、NavigationMenu、Sidebar |
| 模态、侧边和确认流程 | Dialog、Sheet、Drawer、AlertDialog |
| 轻量补充信息 | Tooltip、HoverCard、Popover |
| 加载、进度、空和错误反馈 | Spinner、Skeleton、Progress、Empty、Alert、Toast |
| 内容分组与可调整布局 | Accordion、Collapsible、Separator、ScrollArea、Resizable |
| 菜单与命令入口 | DropdownMenu、ContextMenu、Menubar、Command |
| 会话与消息 | MessageScroller、Message、Bubble、Marker |

## 易混淆边界

- 用 AlertDialog 承载必须明确确认的破坏性或高风险动作；普通阻塞式编辑用 Dialog，保留页面上下文的侧向任务用 Sheet，移动端主导的底部任务才考虑 Drawer。
- 选项少且需直接比较时使用 RadioGroup 或 ToggleGroup；固定长列表使用 Select；需要搜索、过滤或自由触发时使用 Combobox，Command 只负责命令式检索与选择表面。
- Tabs 切换同一页面内并列内容；ToggleGroup 改变一个值或显示模式，不用 Tabs 模拟表单选项。
- Tooltip 只补充短说明且不能成为唯一可访问名称；HoverCard 预览关联对象，Popover 承载可交互的轻量内容。
- Skeleton 表示结构尚未加载，Spinner 表示活动进行中，Progress 表示可量化进度；Empty 与 Alert 分别表达无数据和需要关注的状态。
- Chart、Questionnaire、Message、MessageScroller、Bubble、Marker 等场景化组件不应作为脚手架基础快照的默认成员；只有真实业务交互成立时才按需添加，不为展示组件创建示例路由或 Provider。
- Attachment 可作为通用附件展示基础组件，但不拥有文件选择、校验、上传、进度、重试、服务端文件标识、下载权限或持久化状态。
- 添加 Chart 前先检查项目是否已有 ECharts、Recharts 或其他图表栈，不为统一外观引入第二套图表运行时；Questionnaire 不替代通用表单状态、Schema 和提交契约。
