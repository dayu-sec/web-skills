---
name: dy-sec-frontend-request
description: 设计、接入、迁移或评审 Web 前端数据请求层，覆盖 `@dayu-sec/bizlib-request` 的依赖与实例边界、鉴权和语言请求头、错误传播、业务 Service，以及从 OpenAPI 生成和接入 TypeScript SDK API 物料或仅依据文档手写 Service 两种模式；适用于 React、Vue、单体 Web 和微前端内部请求实现。
---

# DySec 前端数据请求

## 工作顺序

1. 读取目标仓库的 `AGENTS.md`、包管理器、`package.json`、锁文件、请求配置、业务 Service、应用入口和生命周期，确认请求层的真实边界。
2. 检查请求包的根 `exports`、声明产物、`dependencies`、`peerDependencies` 和源码导入，再决定消费方需要直接声明哪些依赖。
3. 处理依赖安装、公开入口、底层包或发布迁移时，读取 [请求包与依赖边界](references/package-boundaries.md)。
4. 处理请求实例、初始化、Token、语言请求头、重复挂载或清理时，读取 [请求运行时与实例归属](references/request-runtime.md)。
5. 处理生成 SDK 包、SDK `HttpClient` 适配、只看 OpenAPI 文档手写 Service 或两种模式迁移时，读取 [OpenAPI 服务接入模式](references/openapi-service-consumption.md)。
6. 处理业务 Service、Query 状态、错误传播或验证时，读取 [服务、错误与验证](references/services-errors-and-verification.md)。
7. 数据结构和业务错误字段使用 `$dy-sec-data-contract-first`，API 设计使用 `$dy-sec-rest-api`，包导出与类型解析使用 `$dy-sec-typescript`。
8. 任务触及微前端宿主、子应用生命周期、装配、隔离或跨应用契约时，同时使用 `$dy-sec-microfrontend`；本 Skill 只定义应用内部请求层。
9. 修改后运行目标单元已有的依赖安装、类型、Lint、格式、测试和构建命令，并验证真实消费者只从公开入口解析。

## 依赖原则

- 源码直接导入的包必须由当前消费方直接声明；类型导入同样需要可解析的直接依赖。
- 普通传递依赖未被消费方直接导入时不重复声明；peer dependency 由消费方直接满足。
- 团队请求能力统一从 `@dayu-sec/bizlib-request` 根入口导入，不读取 `@seed-fe/request` 或构建目录的深层路径。
- 依赖版本以目标仓库、已发布包和私有源的当前可解析结果为准，不在 Skill 中写死版本。

## 引用入口

- [请求包与依赖边界](references/package-boundaries.md)：判断直接依赖、传递依赖、peer dependency、公开导出和发布迁移。
- [请求运行时与实例归属](references/request-runtime.md)：处理实例配置、初始化顺序、会话请求头、幂等和清理。
- [OpenAPI 服务接入模式](references/openapi-service-consumption.md)：在生成 SDK 包和只依据文档手写 Service 之间选择，并处理 SDK 请求适配与迁移。
- [服务、错误与验证](references/services-errors-and-verification.md)：划分请求配置、业务 Service、SDK、Query 状态与错误职责并完成验证。

## 输出要求

- 说明请求包公开入口、直接依赖、peer dependency 和保留例外；不能把传递依赖误写成消费方必装依赖。
- 说明实例所有者、初始化时机、清理时机、错误归属和未改变的业务契约。
- 说明每个服务或端点采用生成 SDK 还是文档手写模式、契约来源、请求适配方式和迁移边界；同一端点不得保留两个实现真源。
- 区分请求基础设施错误、已约定业务错误和 UI 状态回收，不制造重复异常链或重复提示。
- 报告实际运行的安装、类型、Lint、格式、测试、构建和最小消费者验证证据。
