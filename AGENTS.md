# AGENTS.md

本文件只记录 `web-skills` 源码仓库的长期维护规则。团队 Skill 的规范来源是本仓库 `skills/`；用户级或项目级安装位置由安装器和使用者决定。

## 上下文边界

- 项目事实、业务规则和运行时约束以目标项目的 `AGENTS.md`、规格和源码为准；不要把某个项目的实现快照沉淀到共享 Skill。
- 跨项目稳定成立的工作流程和领域规则放入对应 `SKILL.md` 或一级 `references/`，不要在本文件复制 Skill 正文。
- 修改 Skill 前读取 Skill Creator 规则和目标 Skill 的现有结构，只做职责内的必要变更。

## Agent 资料脱敏

- 共享 `SKILL.md`、`references/` 和 `agents/openai.yaml` 只记录跨项目稳定成立的技术约束、领域方法和工作流程。
- 不从目标项目复制项目名、仓库路径、真实页面或模块名、服务名、路由、API 路径、环境变量、数据字段、业务文案、Mock 数据、客户或人员信息及实现快照。
- 需要说明形态时使用 `<project-name>`、`<service-name>`、`<ComponentName>` 等中性占位符，并明确以目标仓库约定和源码为准。
- 交付前检查 diff，并定向搜索来源项目标识、业务名和敏感配置；不可跨项目复用的内容留在项目规格、源码或项目 `AGENTS.md`。

## Skill 编写

- 使用 Skill Creator 创建或更新 Skill，并在完成后运行 Skill Creator 校验。
- 团队标识为 `dayu-sec`；目录名、`SKILL.md` frontmatter `name` 和其他 Agent 配置中的机器名称统一使用 `dayu-sec-` 前缀。
- Web 项目、架构和框架中立的应用内部工程机制使用 `dayu-sec-web-` 机器命名空间；React、Ant Design、TypeScript 等框架、组件库或语言 Skill 保留直接技术名称。正文和展示文案可以继续使用准确的“前端”和“微前端”术语。
- Skill 目录名必须与 frontmatter `name` 一致；`agents/openai.yaml` 的 `display_name` 以 `DayuSec` 开头，`default_prompt` 必须引用对应的 `$dayu-sec-*` 名称。
- OpenAI 系统 Skill、插件 Skill、第三方 Skill 和其他外部维护资料不适用团队前缀规则。
- `SKILL.md` 只保留触发范围、工作顺序、引用入口和输出要求；详细材料放入按需读取的一级 `references/`。
- Skill 正文不新增 `## 维护规则` 章节；共享仓库维护约束统一保留在本文件。
- 中文 Skill 的 frontmatter `description` 使用中文语境自然表达能力、触发场景和边界，不夹杂模板式英文触发句。
- 不创建无直接运行价值的辅助文档，例如 `README.md`、`INSTALLATION_GUIDE.md`、`QUICK_REFERENCE.md` 或 `CHANGELOG.md`。
- 下载官方资料时只保留能够减少重复请求的最小本地缓存；官方原文不改写，自写索引和说明遵循本仓库表达规则。

## 安全与交付

- 不写入真实域名、内网地址、账号、Token、私有证书、客户或人员信息。
- 不把本地代理、临时环境、端口分配、未确认版本或正在变化的项目状态写入共享 Skill。
- 校验目录名、frontmatter、reference 链接和 `agents/openai.yaml`，并定向检查已删除或重命名 Skill 的悬空引用。
- 未经用户明确要求，不提交、推送、发布或安装到用户目录。
