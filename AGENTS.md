# AGENTS.md

本文件记录本机 Codex 全局协作规则，适用于全局 Skills 和通用 Agent 资料。不要在这里写入项目名称、仓库地址、真实域名、内网 IP、账号、Token、人员信息、客户信息、端口分配或正在变化的项目状态。

## 上下文边界

- 项目事实、业务规则和运行时约束以当前项目仓库的 `AGENTS.md`、规格文档和源码为准；不要把某个项目的临时状态沉淀到全局上下文。
- 全局规则只写跨项目、跨 Skill 可复用的工作约束；单个 Skill 的领域知识放在该 Skill 的 `SKILL.md` 或 `references/` 中。
- 处理本机全局 Skill 时，优先读取 Skill Creator 规则和目标 Skill 的现有结构，再做最小必要修改。

## Agent 资料脱敏

- 创建或更新共享、全局、模板级 `AGENTS.md`，以及团队 `SKILL.md`、`references/`、`agents/openai.yaml` 时，只写跨项目稳定成立的技术约束、领域方法和工作流程。
- 不从当前项目复制项目名、仓库路径、真实页面或模块名、服务名、路由、API 路径、项目专用环境变量、数据字段、业务文案、Mock 数据、客户或人员信息、内部环境信息及实现快照。需要说明形态时使用 `<project-name>`、`<service-name>`、`<ComponentName>` 等中性占位符。
- 项目级 `AGENTS.md` 可以记录完成协作所必需的稳定架构、技术选型、职责边界和源码入口，但不得把敏感业务内容、真实业务数据或短期实现状态当作示例；详细业务事实留在项目规格、契约和源码中。
- 交付 Agent 资料前检查变更 diff，并定向搜索当前项目标识、业务名和敏感配置；发现不可跨项目复用的内容时，将其移回项目规格或源码，不得同步到用户级、共享仓库或脚手架模板。

## Skill 编写

- 使用 Skill Creator 创建或更新 Skill，并在完成后运行 Skill Creator 校验。
- 团队标识为 `dy-sec`。团队创建或维护的 Skill 目录名、`SKILL.md` frontmatter `name` 和其他 Agent 配置中的机器名称统一使用 `dy-sec-` 前缀。
- 团队 Skill 的目录名必须与 frontmatter `name` 一致；`agents/openai.yaml` 的 `display_name` 以 `DySec` 开头，`default_prompt` 必须引用对应的 `$dy-sec-*` 名称。
- OpenAI 系统 Skill、插件 Skill、第三方 Skill 和其他外部维护的 Agent 资料不适用团队前缀规则，不得为满足本规则直接重命名。
- 团队全局 Skill 统一维护在 `$HOME/.agents/skills`，不要在 `$HOME/.codex/skills` 保存重复副本。
- 全局 Skill 只沉淀跨项目可复用的技术约束、领域方法和工作流程；不得写入某个项目的名称、目录现状、页面名称、路由、数据字段、业务文案或实现快照。示例使用中性占位命名，并明确以目标仓库约定和源码为准。
- `SKILL.md` 只保留触发范围、工作顺序、引用入口和输出要求；详细材料放入按需读取的 `references/`。
- Skill 正文不新增 `## 维护规则` 章节；跨 Skill 的维护约束统一写在本文件或对应项目级 Agent 规则中。
- 中文团队使用的 Skill，frontmatter `description` 使用中文语境自然表达能力、触发场景和边界，不夹杂 `Use when`、`or when the user mentions`、`Use only when` 这类模板英文触发句式。
- 必要的专有名词、包名、组件名、URL、API、frontmatter 字段和代码标识可以保留英文；不要为了中文化牺牲准确性。
- 不创建无直接运行价值的辅助文档，例如 `README.md`、`INSTALLATION_GUIDE.md`、`QUICK_REFERENCE.md`、`CHANGELOG.md`。
- 参考资料保持一层渐进式披露：`SKILL.md` 直接链接一级 reference；一级 reference 不再要求 Agent 连环读取多层资料，除非任务确实需要。
- 下载官方资料时只保留能减少后续重复请求的最小本地缓存；优先保留单主题、单组件或单契约文件，不默认保留聚合全文、变更记录、博客、教程和导航索引。
- 下载的官方原文是来源材料，不按本文件的中文表达规则改写；自写的 Skill 正文、元数据和索引说明需要符合中文表达规则。

## Skill 分层与组合

- 基础通用层保存与框架和部署形态无关的能力，例如 TypeScript、注释、数据契约和 REST API；不得引入 React、微前端或具体 UI 组件库的实现约束。
- Web 共享层保存微前端与单体 Web 都需要的能力，例如前端请求、前端 Mock、React、Ant Design 和业务 UI；只描述应用内部实现，不承载宿主、子应用、生命周期或跨应用装配规则。
- 架构扩展层保存只在特定架构启用的能力。微前端任务使用 `$dy-sec-microfrontend`；单体 Web 任务不加载该 Skill，也不为了兼容微前端增加生命周期、通信或隔离分支。
- 专用架构 Skill 可以要求组合使用通用 Skill；通用 Skill 只保留组合入口，不复制专用架构细节，避免两处规则分叉。
- 一个任务同时涉及多个层级时，先确定架构边界，再按实际文件和职责加载最小 Skill 组合；不得仅因仓库中存在某种架构就让所有任务都加载其专用 Skill。
- 源码路径命名、文件式路由入口与业务视图分层属于框架无关的 Web 共享机制，由 `$dy-sec-frontend-shared` 统一定义；React、Vue 等框架 Skill 只补充各自的框架适配并组合使用该 Skill，不复制共享协议。
- 前端数据请求属于独立的 Web 共享机制，由 `$dy-sec-frontend-request` 统一定义依赖边界、实例归属、初始化、错误传播和业务 Service 分层；单体与微前端内部共同使用，只有任务触及微前端生命周期、隔离或跨应用契约时才额外组合 `$dy-sec-microfrontend`。
- 前端本地 Mock 属于独立的 Web 共享机制，由 `$dy-sec-frontend-mock` 统一定义插件扫描、目录与入口、OpenAPI SDK 或文档契约来源、场景实现和运行验证；响应契约按需组合 `$dy-sec-data-contract-first` 与 `$dy-sec-rest-api`，请求与 Service 边界组合 `$dy-sec-frontend-request`，只有触及跨应用 Mock 契约时才额外组合 `$dy-sec-microfrontend`。

| 层级 | Skill | 组合规则 |
| --- | --- | --- |
| 基础通用层 | `$dy-sec-code-commenting`、`$dy-sec-data-contract-first`、`$dy-sec-typescript`、`$dy-sec-rest-api` | 按代码与契约任务独立启用，不感知 Web 部署形态 |
| Web 共享层 | `$dy-sec-frontend-shared`、`$dy-sec-frontend-request`、`$dy-sec-frontend-mock`、`$dy-sec-react`、`$dy-sec-antd-ui-components`、`$dy-sec-basic-ui` | 微前端与单体 Web 共用，按框架无关工程机制、数据请求、本地 Mock、React、组件或基础 UI 职责选择 |
| Web 架构扩展层 | `$dy-sec-microfrontend` | 仅在任务触及宿主、子应用、生命周期、装配、隔离或跨应用契约时与 Web 共享层组合 |
| 后端技术层 | `$dy-sec-java-spring` | 处理 Java/Spring 后端实现；需要 API 或数据契约规则时再组合基础通用层 |

单体 React UI 默认从 Web 共享层选择所需 Skill；数据请求使用 `$dy-sec-frontend-request`，本地 Mock 使用 `$dy-sec-frontend-mock`。微前端 React UI 使用相同共享 Skill，并只在任务触及架构边界时额外加载 `$dy-sec-microfrontend`。纯组件、纯类型或纯数据契约任务不因所在仓库采用微前端而自动加载架构扩展层。

## UI Skill 约束

- Ant Design UI Skill 的本地官方缓存只保留组件级中文文档和组件级 `Semantic DOM` 文档，以及用于定位这些文件的 `component-manifest.md`。
- Ant Design UI Skill 不把 `llms.txt`、`llms-full-cn.txt`、`llms-semantic-cn.md`、`changelog-cn.md`、博客、设计规范或 React 指南作为日常本地缓存保留；需要刷新或核对时访问官方 URL。
- 文字排版、说明文本、省略和可复制文本优先使用 Ant Design `Typography`。
- 组件间距、横纵排列、工具栏、表单操作区和局部弹性布局优先使用 Ant Design `Space` 或 `Flex`。
- React 图标优先使用项目已有图标库或 `lucide-react`；未安装时按项目包管理器提示安装或先确认是否允许新增依赖，不默认转向 `@ant-design/icons`。

## 安全

- 不写入内网地址、真实域名、账号、Token、私有证书、客户或人员信息。
- 不把本地代理、临时环境、端口分配、未确认版本号或正在增长的项目状态写入全局规则。
