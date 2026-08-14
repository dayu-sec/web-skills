---
name: dayu-sec-ui-shadcn
description: 设计、选择、接入、实现、迁移或评审基于 shadcn/ui、Tailwind CSS 与底层无样式组件原语的项目自有 UI 基础层，覆盖单包或 pnpm Monorepo 中的 components.json 发现、CLI 工作流、组件选择与组合、主题 Token、表单校验、无障碍、存量组件库迁移和微前端样式隔离；适用于 React Web 项目中的基础组件、应用 Shell、主题样式和表单控件，不定义具体业务信息架构或品牌视觉。
---

# DayuSec shadcn/ui

## 工作顺序

1. 读取目标仓库 `AGENTS.md`、workspace 清单、依赖、样式入口和路径别名，定位全部 `components.json` 与实际组件源码，确认应用 workspace、包管理器、Tailwind 版本、底层原语、主题与源码所有权。
2. 初始化、查询、添加或更新组件时阅读 [项目与 CLI 工作流](references/project-and-cli-workflow.md)，先检查真实 CLI 能力与本地定制，再执行最小变更。
3. 把交互需求映射到现有组件或最小组合时阅读 [组件选择](references/component-selection.md)，先检查本地源码与导出，再决定使用、组合或新增。
4. 实现或评审 Button、Select、Dropdown、Dialog、Tooltip、Collapsible 等基础组件时阅读 [Base UI 与组合规则](references/base-ui-composition.md)。底层不是 Base UI 时，以目标仓库实际 primitive API 为准。
5. 处理字段、校验、提交、动态表单或表单依赖选型时阅读 [表单与校验](references/forms-and-validation.md)。
6. 接入 Tailwind v4、主题变量、深浅色或微前端样式边界时阅读 [Tailwind、主题与隔离](references/tailwind-theme-and-isolation.md)。
7. 从 Ant Design 等存量组件库迁移或需要阶段性共存时阅读 [存量组件库迁移](references/migration-from-antd.md)，按所有权边界逐段替换。
8. 运行目标项目已有的类型、Lint、格式、测试和生产构建；交互或样式变更补充真实浏览器中的键盘、焦点、主题、窄屏、浮层和滚动验证。

## 职责边界

- packages/ui 中引入的社区组件视为只读物料，不改源码、不补兼容 Props、不包装成另一套本地组件库；生成与引入后的社区源码作为标准只读资产维护。
- Monorepo 中的私有 UI workspace 仍属于当前项目源码，不自动成为跨仓库发布组件库；以目标项目的部署和所有权契约判断架构边界。
- 品牌色、密度、排版和页面骨架使用目标项目已确认的规范或对应产品 UI/UX Skill；内部运营和管理后台使用 `$dayu-sec-ui-internal-ops`。React 组件职责与 Hooks 使用 `$dayu-sec-react`；微前端生命周期与跨应用契约使用 `$dayu-sec-web-micro-frontend`。
- 路由、权限、领域 Schema、业务文案和服务调用始终由目标项目定义。本 Skill 只负责 UI 基础设施及其组合契约。
- 不固定依赖版本、base 预设或目录名；以目标仓库、当前 CLI 输出和已安装源码为准。

## 输出要求

- 说明组件源码、主题 Token、底层原语与业务调用层各自的所有权，以及新增依赖的直接使用场景。
- 列出新增、复用和迁移的组件；若保留存量组件库，明确共存边界和后续删除条件。
- 说明组件选择依据；已预装但未使用的能力不算业务实现，不因组件存在就初始化 Provider 或引入页面。
- 表单任务说明状态、Schema、校验适配器和 UI 字段的职责分工。
- 报告静态、单元、构建和浏览器验证；未验证的 Portal、主题、响应式、独立运行或宿主集成边界必须明确标注。
