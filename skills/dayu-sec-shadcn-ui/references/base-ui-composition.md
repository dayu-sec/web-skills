# Base UI 与组合规则

## 先识别底层原语

不要根据组件名称猜 API。先检查 `components.json` 的样式预设、组件源码 import 和已安装依赖，确认当前组件基于 Base UI、Radix UI、React Aria 或其他 primitive。以下组合规则以 Base UI 为主；其他实现使用其官方 API 与项目既有封装。

## Base UI 组合

- 通过 `render` 组合自定义触发器或链接，不套多余可点击元素。渲染目标不是原生 `button` 时，按 primitive 契约设置 `nativeButton={false}`。
- Trigger、Popup、Positioner、Portal 和 Backdrop 保持各自职责；不要仅为减少 DOM 层级删除定位、焦点或可访问性节点。
- Select、Menu、Combobox 等集合组件使用真实 Item，并为 value、label、disabled 与分组建立稳定契约；不在显示文案中反推业务值。
- Dialog、AlertDialog、Drawer 等必须提供可访问标题；视觉上隐藏标题时仍保留屏幕阅读器可读内容。
- Card、Field、Empty 等结构组件按完整结构组合，避免把 Header、Body、Footer 的间距和语义复制到每个业务页面。

## 样式与变体

- 组件默认样式只消费 `background`、`foreground`、`primary`、`muted`、`border`、`input`、`ring` 等语义 Token。
- 使用 `cn()` 合并调用侧 `className`，使用有限变体描述稳定的尺寸、强调和状态；一次性页面差异留在调用侧。
- 组件内部用 `gap` 组织图标与文字，避免依赖子元素外边距；图标可使用稳定的 `data-icon` 或等价选择器统一尺寸。
- 保留 `data-slot`、`data-*` 状态、`aria-*`、disabled 和 focus-visible 样式，避免依赖底层库未承诺的 DOM 顺序或内部类名。

## 可访问性检查

- 纯图标按钮有 `aria-label` 或等价可访问名称；Tooltip 只补充说明，不是唯一名称来源。
- 折叠、选择、当前页面和忙碌状态分别使用 `aria-expanded`、`aria-selected`、`aria-current`、`aria-busy` 或 primitive 提供的等价状态。
- 键盘可到达全部交互，Escape、方向键、Tab 顺序、焦点回收和禁用状态符合组件语义。
