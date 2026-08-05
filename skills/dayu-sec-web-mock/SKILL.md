---
name: dayu-sec-web-mock
description: 规划、实现、迁移或评审 Web 前端本地 Mock，覆盖插件扫描配置、目录与入口边界、OpenAPI SDK 或文档契约、场景分支、共享数据、热更新和运行验证；适用于 React、Vue、微前端与单体 Web 的开发期 Mock 任务。
---

# DayuSec 前端 Mock

## 工作顺序

1. 读取目标仓库的 `AGENTS.md`、包管理文件、锁文件、构建配置、请求配置、业务 Service、现有 Mock、接口规格和生成类型，确认 Mock 服务的实际入口与契约来源。
2. 识别用户提供的是已安装或可检查的 OpenAPI TypeScript SDK/API 物料，还是仅供开发阅读的 OpenAPI 文档；读取 [OpenAPI 契约来源与 Mock 生成](references/openapi-contract-sources.md)，同一端点只保留一个契约真源。
3. 规划或迁移 Mock 目录、入口、fixture 和辅助文件时，读取 [Mock 目录与入口边界](references/directory-and-entries.md)。
4. 核对仓库实际解析的 Mock 插件版本、类型声明、官方文档以及 `cwd`、根目录、`include`、`exclude`、请求前缀和代理配置；不把其他版本的默认值直接套入当前项目。
5. 涉及响应字段、错误体或已有接口结构时，同时使用 `$dayu-sec-data-contract-first`；涉及 URL、HTTP 方法、状态码、分页或 REST 错误语义时同时使用 `$dayu-sec-rest-api`；涉及 TypeScript 类型和模块解析时同时使用 `$dayu-sec-typescript`。
6. 涉及 SDK 接入、手写业务 Service、请求实例、Base URL、拦截器或错误传播时同时使用 `$dayu-sec-web-request`。只有任务触及宿主、子应用、生命周期、隔离或跨应用 Mock 契约时才额外使用 `$dayu-sec-web-micro-frontend`。
7. 按目标仓库的服务或稳定业务域实现最小变更；目录迁移不顺带修改接口 URL、响应契约、概率规则或场景行为，契约实现不猜测无来源字段。
8. 运行仓库已有的类型、Lint、格式和测试命令，并启动真实开发服务器验证入口发现、请求匹配、响应、条件分支和热更新；已有业务 Service 时至少验证一条实际 Service 调用链。

## 引用入口

- [OpenAPI 契约来源与 Mock 生成](references/openapi-contract-sources.md)：根据已安装 SDK/API 物料或仅供阅读的 OpenAPI 文档生成 Mock，并核对业务 Service、请求实例和最终 wire-level 请求时读取。
- [Mock 目录与入口边界](references/directory-and-entries.md)：规划、迁移或评审 Mock 根目录、业务子目录、可扫描入口、辅助文件、插件递归扫描和运行验证时读取。

## 输出要求

- 说明 Mock 插件与实际版本、扫描根目录、入口模式、请求拦截范围和代理关系；区分已由源码或文档确认的行为与推断。
- 按端点说明契约输入模式、真源版本或确认时间、SDK operation 或文档 operation、method/path、参数、请求体、成功响应、错误响应、有效请求 URL 和 Mock 场景；发现 SDK 与文档冲突时停止合并并报告差异。
- 说明项目级辅助文件和可扫描业务入口的职责边界、目录划分依据、业务 Service 与请求实例边界、场景表达方式和必要例外。
- 实施后报告静态检查、至少一个嵌套入口的实际请求、插件来源日志、热更新验证，以及已有业务 Service 时的最小调用链验证；无法运行时明确缺失的验证证据。
- 发现入口后缀误用于辅助文件、目录层级擅自改变 URL、Mock 响应偏离契约、重复路由或扫描配置掩盖目录问题时，明确报告或修复。
