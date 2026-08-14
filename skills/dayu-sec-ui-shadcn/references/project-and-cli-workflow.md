# 项目与 CLI 工作流

## 发现顺序

1. 读取项目约束、根与 workspace `package.json`、锁文件、TypeScript 别名、构建配置和入口 CSS。
2. 定位全部 `components.json`，根据源码、构建入口和 workspace 依赖确定承载任务的应用配置；再检查 UI 源码、统一 `cn()`、package exports 和本地修改。
3. 优先使用项目已安装的 CLI，并从应用目录运行 `info --json`，或在仓库根通过 `-c <app-workspace>` 指定上下文；命令与参数以当前版本 `--help` 为准。
4. 只有在现有源码不能覆盖需求时，才初始化或添加组件。

CLI 无法运行时，从 `components.json`、依赖、源码 import 和 resolved alias 恢复 framework、Tailwind、base、图标库与组件清单；不要因缺少在线文档而猜 API。

## 初始化

- React + Vite + Tailwind v4 项目可优先采用 Tailwind 的 Vite 插件，并让 `components.json` 指向真实入口 CSS 和别名。
- 选择 Base UI、Radix UI 或其他底层原语前，确认组件注册源、既有组件和团队维护成本。已有项目不为追逐预设而混入第二套 primitive。
- 初始化前先让 TypeScript 别名能被 CLI 解析；不要接受生成到字面量 `@/` 目录等错误结果。
- `components.json` 应准确声明 TSX、RSC、CSS 变量、图标库、组件目录和工具目录；Tailwind v4 没有配置文件时不要虚构路径。
- Monorepo 中每个参与 shadcn 文件路由的 workspace 都保留自己的 `components.json`。应用配置把共享 `ui`、`utils` 或 hooks 指向 UI workspace，UI 配置把内部 alias 指向自身源码；两端保持 style、base color、图标库和 RSC 选项一致。
- UI workspace 是项目自有源码包时使用显式 package exports 和逐文件 import；不要为方便导入创建聚合全部组件的 barrel。

## 添加与更新组件

- 添加前查看组件内容、依赖与目标路径。普通应用和脚手架都应按真实通用能力维护经过评审的组件清单，不因 registry 提供条目就预装场景化组件。`add --all` 只用于观察 registry 全量变化，不作为组件完整性验收；只有目标项目明确要求完整离线镜像时才批量生成，且不把 Blocks、Examples 或业务演示混入基础层。
- `packages/ui` 中引入的社区组件视为只读物料，不改源码、不补兼容 Props、不包装成另一套本地组件库。添加与更新组件时先用 dry-run、diff 或等价只读能力识别改动范围。
- 在线时使用 `search`、`docs`、`view` 核对尚未安装的 registry 条目；不要通过 GitHub raw 绕过 CLI 的 base、alias、依赖与文件路由。
- 离线时先使用本地源码和导出类型。缺失能力只有能由现有组件保持正确语义、可访问性与交互契约时才组合，否则报告 registry 访问前提，不从模型记忆伪造上游实现。
- 保留生成组件的 `data-slot`、ARIA、ref、Portal、焦点管理与卸载行为；项目定制只围绕语义 Token、明确变体和已验证缺陷。
- 组件源码、primitive 包、图标包和 `cn()` 必须各有单一来源。删除组件库前先搜索最后一个 import、Provider、样式和类型引用。

## 验证

- 检查 `components.json`、别名和入口 CSS 能被 CLI 与构建器共同解析。
- Monorepo 中对待添加的单个组件验证 dry-run 的组件、Hook、依赖和 CSS 目标落入预期 workspace，应用 import 能通过 package exports 与 TypeScript 共同解析；不要用 `add --all` 的零差异作为精选快照验收条件。
- 运行类型、Lint、格式、测试和生产构建，并检查最终 chunk 没有意外带入整套旧组件库。
- 在浏览器验证触发器、浮层、键盘导航、焦点回收、深浅主题和窄屏布局。
