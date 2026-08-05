# React 前端依赖选择

## 模板与应用基线

- 团队成员以后端开发为主时，模板和应用可以维护经过评审的较完整依赖基线，降低业务启动和重复选型成本。
- 已安装只表示能力可用，不表示每个应用都要初始化 Provider、Store、Schema、编辑器或示例代码；没有真实场景时不要添加演示接线。
- 依赖属于明确模板基线时，不因当前零 import 自动删除；调整基线应作为独立维护任务评估生成模板、现有应用、锁文件、许可证和供应链影响。
- 源码直接 import 的包由当前应用直接声明。微前端主应用和各微应用分别满足自己的直接依赖，不依赖宿主或相邻应用的偶然安装。
- 不在 Skill 中固定版本；以目标仓库 `package.json`、锁文件、当前注册源、React peer dependency 和发布产物为准。
- 高频小工具依赖 tree-shaking；编辑器、图表、图模型、Markdown、高亮等重型能力优先按页面或功能懒加载，并用生产构建确认 chunk。

## 基础设施与状态

| 场景 | 选择 | 边界 |
| --- | --- | --- |
| React 组件、路由和表单 | React、React Router、项目已有表单方案 | 不用全局状态替代路由或表单状态 |
| UI、主题和图标 | Ant Design、ProComponents、主题包、`lucide-react` | 组件选型使用 `$dayu-sec-antd-ui-components`，界面使用 `$dayu-sec-basic-ui` |
| 请求实例与业务 Service | `@dayu-sec/bizlib-request` | 不直接使用 `axios` 发起业务请求，使用 `$dayu-sec-web-request` |
| 服务端状态 | `@tanstack/react-query` | 不存入 Zustand，不使用 `ahooks/useRequest` |
| 运行时契约校验 | `zod` | 只校验不可信边界，使用 `$dayu-sec-data-contract-first` |
| React 通用 Hooks | `ahooks` | 只处理浏览器、事件、调度和轻量状态辅助，使用 `$dayu-sec-react` |
| 跨组件客户端状态 | `zustand`，必要时组合 `immer` | 不承载服务端缓存、URL 或普通表单状态 |
| 国际化 | `i18next`、`react-i18next` 和项目语言资源 | 语言、Ant Design 与日期 locale 保持同一来源 |
| 日志 | 项目统一 logger | 不保留 `console.log`，不记录 Token 或敏感载荷 |
| 可合并批请求 | `@seed-fe/batch-request` | 只用于接口明确支持且确有批量合并收益的场景 |

## 高频工具

| 依赖 | 合适场景 | 不要用于 |
| --- | --- | --- |
| `clsx` | 条件 class、CSS Modules 和外部 `className` 合并 | 替代 Design Token、Emotion 或组件状态建模 |
| `dayjs` | 日期解析、格式化、比较、区间和 locale | 猜测接口时区或改变服务端时间契约 |
| `change-case` | 展示名、非契约标识和显式命名转换 | 静默改写 API 字段、路由、权限键或持久化标识 |
| `lodash-es` | 原生实现明显更难读的集合、对象和函数工具 | `Date.now()`、普通 `map`、简单空值判断等平台已有能力 |
| `qs` | 接口契约要求嵌套对象、数组或特殊格式的 query 序列化 | 简单 URL 参数或 React Router 已负责的 search params |
| `filesize` | 把字节数格式化为用户可读文本 | 修改底层数值、上传契约或限额判断单位 |
| `dompurify` | 在渲染不可信 HTML 前执行净化 | 校验普通 JSON、纯文本或业务字段契约 |
| `copy-to-clipboard` | 需要兼容降级且现有复制能力不足 | Ant Design `Typography` 可复制文本或原生 Clipboard API 已足够的场景 |
| `immer` | 深层嵌套状态更新能显著降低不可变更新复杂度 | 简单对象、数组或为了与 Zustand 成套而默认启用 |

使用工具前先确认输入和输出契约。工具只简化实现，不得发明字段、多结构 fallback、时区、单位、编码或安全语义。

## 重型场景能力

- 图表使用项目已有 React ECharts 封装；业务页面不直接管理底层 ECharts 实例，除非封装无法覆盖并有验证依据。
- G6、X6、查询构建器、拖拽、缩放和富文本编辑器只在对应交互真实存在时加载，不把多个同类库用于同一功能。
- Monaco、CodeMirror、wangEditor 按代码编辑、轻量文本编辑和富文本职责选择，不因模板已安装而同时初始化。
- Markdown 使用项目已有 `react-markdown` 与 remark/rehype 组合；语法高亮沿用项目既定方案，不同时引入多套渲染管线。
- KaTeX、Shiki、Highlight.js、图编辑器和大型 worker 资源应验证懒加载、静态资源路径、卸载和微前端集成行为。

## 选择步骤

1. 搜索 `package.json`、锁文件、imports、现有封装和项目 `AGENTS.md`，区分“模板已提供”和“业务已使用”。
2. 按能力所有者选择依赖；同类库并存时先沿用当前模块已验证方案，不在普通需求中顺带迁移。
3. 能用平台或现有组件清晰完成时不新增抽象；已有依赖能明显降低错误或复杂度时直接使用，不重复手写。
4. 增加运行时代码后验证类型、Lint、测试和生产构建；重型能力同时检查 chunk、首屏、静态资源和卸载清理。
