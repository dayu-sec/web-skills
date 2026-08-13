# 项目与 CLI 工作流

## 发现顺序

1. 读取项目约束、`package.json`、锁文件、TypeScript 别名、构建配置和入口 CSS。
2. 检查 `components.json`、现有 `components/ui`、统一 `cn()` 以及组件的本地修改。
3. 使用当前 shadcn CLI 的 `info`、`search`、`docs` 或 `view` 能力核对项目状态和组件来源；命令参数以 `--help` 和当前版本输出为准。
4. 只有在现有源码不能覆盖需求时，才初始化或添加组件。

## 初始化

- React + Vite + Tailwind v4 项目可优先采用 Tailwind 的 Vite 插件，并让 `components.json` 指向真实入口 CSS 和别名。
- 选择 Base UI、Radix UI 或其他底层原语前，确认组件注册源、既有组件和团队维护成本。已有项目不为追逐预设而混入第二套 primitive。
- 初始化前先让 TypeScript 别名能被 CLI 解析；不要接受生成到字面量 `@/` 目录等错误结果。
- `components.json` 应准确声明 TSX、RSC、CSS 变量、图标库、组件目录和工具目录；Tailwind v4 没有配置文件时不要虚构路径。

## 添加与更新组件

- 添加前查看组件内容和依赖，只选择实际需要的组件；不要一次生成完整目录作为未来储备。
- 先用 dry-run、diff 或等价只读能力识别将新增、修改和覆盖的文件。存在本地修改时逐文件合并，不直接使用强制覆盖。
- 保留生成组件的 `data-slot`、ARIA、ref、Portal、焦点管理与卸载行为；项目定制只围绕语义 Token、明确变体和已验证缺陷。
- 组件源码、primitive 包、图标包和 `cn()` 必须各有单一来源。删除组件库前先搜索最后一个 import、Provider、样式和类型引用。

## 验证

- 检查 `components.json`、别名和入口 CSS 能被 CLI 与构建器共同解析。
- 运行类型、Lint、格式、测试和生产构建，并检查最终 chunk 没有意外带入整套旧组件库。
- 在浏览器验证触发器、浮层、键盘导航、焦点回收、深浅主题和窄屏布局。
