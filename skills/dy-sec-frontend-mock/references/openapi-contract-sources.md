# OpenAPI 契约来源与 Mock 生成

## 先识别用户提供的模式

团队前端服务端点可能通过两种方式交付契约：

1. 应用安装由 OpenAPI 生成的 TypeScript SDK/API 物料，Agent 可以检查安装包、生成源码或对应构建声明。
2. 服务端只提供供开发阅读的 OpenAPI 文档，文档不进入应用源码，前端按文档手写业务 Service。

一个应用可以按服务混用两种模式，但同一端点只能有一个当前真源。用户提供信息后，先记录：

```text
服务或业务域：...
契约模式：SDK/API 物料 | OpenAPI 文档
契约标识：包名与实际版本 | 文档标题、版本或确认时间
目标 operation：...
需要的 Mock 场景：...
```

若用户同时提供 SDK 和文档，先确认它们是否对应同一契约版本。OpenAPI 描述接口语义，目标应用实际安装的 SDK 声明和序列化代码描述当前消费者行为；两者冲突时列出 method、path、参数、必填性、枚举或响应类型差异，不把两套结构合并成兼容 Mock。为保证当前应用能够命中，可以记录 SDK 实际发出的 wire 请求，同时报告契约漂移并等待版本对齐。

## 模式一：根据 SDK 或 API 物料生成

先检查目标应用的包清单和锁文件，再从包根公开入口、声明产物或可复现生成源码确认：

- 实际解析的包版本、根 `exports` 和类型入口。
- operation 的公开函数、参数签名、必填性和抛错条件。
- HTTP method、path、path/query/header 参数、请求体位置和序列化规则。
- 成功响应 DTO、枚举、nullable、分页结构以及已声明错误响应。
- SDK 如何接收 `HttpClient`，业务 Service 是否还有编排或 wire DTO 映射。

团队函数式 API 物料通常暴露 `operation(payload, options?, httpClient?)`，请求体通常位于 `payload.data`，但必须以目标应用安装版本的声明和实现为准。不要根据函数名、源 `operationId` 或常见生成器形态猜 method、path 或字段。

生成 Mock 时遵守：

- 先比较 SDK operation 的公开 `Promise<T>`、生成请求实现的响应泛型、`HttpClient` 适配器和响应拦截器，确认 `T` 是否就是网络 wire body。相同时从 SDK 根入口 `import type` DTO 和枚举并用 `satisfies` 校验 fixture；存在拆包或映射时按真实网络 schema 生成 Mock，并单独验证 wire body 到公开返回值的转换。
- Mock 匹配 SDK 最终发出的 method、path、query 和 body。OpenAPI 的 `{id}` 等 path 模板按 Mock 插件实际语法转换，例如插件使用 path-to-regexp 时转换为 `:id`，参数名称保持不变。
- 按三阶段计算 URL：先由 SDK 渲染 operation path 并用请求实例 `baseURL`、Vite base 和拦截器得到浏览器请求 URL；再由同源与代理规则判断请求是否进入当前 Vite；最后按 Mock matcher 和项目级 `defineMock` 转换注册路径。插件 `prefix` 可能只限定拦截范围，不一定负责拼接路径，禁止把所有前缀机械相加。
- 不在 Mock 中调用 SDK operation、初始化 SDK `HttpClient` 或初始化应用请求实例。Mock 位于开发服务器侧，只负责匹配并返回 wire-level 响应。
- 不手工修改生成 SDK，也不因生成 Mock 而自行生成、提交或发布 API 物料；这些动作需要独立任务授权。

## 模式二：根据只读 OpenAPI 文档生成

文档不进入源码时，只读取用户当前提供的文档和本次 operation 直接引用的 schemas，不复制整份文档到应用或全局 Skill。逐项提取：

```text
operationId 或文档定位：...
method/path：...
path/query/header：名称、位置、类型、必填性、格式、枚举
requestBody：content-type、schema、必填性
success responses：状态码、content-type、schema
error responses：状态码、错误 schema
```

- 只实现当前业务场景需要的 operation，不仿造完整 SDK、生成目录或第二套请求基础设施。
- 手写 Service 使用应用已经初始化的 live `HttpRequest`；使用 `@lrd/dy-sec-bizlib-request` 的项目继续从其公开入口取得实例，保留 Base URL、鉴权、语言、拦截器和错误传播。
- 手写 wire DTO 严格保留 OpenAPI 字段名、必填性、枚举、nullable 和嵌套结构。页面视图模型或 Service 映射后的 `camelCase` 结构不能作为 Mock 响应契约。
- 文档缺少响应 schema、错误结构、示例含义或版本信息时，先指出缺口；不要用 UI 字段、常见命名、历史印象或 `a || b || c` fallback 补齐。
- 文档已经声明错误 schema 时，该 schema 优先于 `$dy-sec-rest-api` 的通用错误示例；REST Skill 用于核对语义，不覆盖当前端点已有契约。
- 在任务交付中记录所依据的文档版本或确认时间及未确认项，但不把变化中的业务文档内容沉淀到全局 Skill。

## 请求层与 Mock 的职责

`@lrd/dy-sec-bizlib-request` 负责应用内请求实例、Base URL、鉴权、语言、超时、拦截器、错误标准化及其所有权；业务 Service 负责调用 SDK 或按文档手写请求并执行必要映射；Mock 负责开发服务器侧的 wire-level 请求匹配和响应。

- Mock 不替换业务 Service，不在页面中拼 URL，也不创建第二个 Axios 或 `HttpRequest`。
- SDK 模式下，业务 Service 或薄适配器把 live `HttpRequest` 适配给 SDK `HttpClient`；Mock 仍只匹配适配后发出的 HTTP 请求。
- 文档模式下，业务 Service 直接使用 live `HttpRequest` 的调用签名；Mock 返回文档声明的 wire DTO，不返回页面摘要或 Query 缓存结构。
- 命名请求实例访问不同服务源时，分别计算有效 URL。浏览器直接访问外部绝对地址且未经过当前 Vite 代理或前缀时，本地 Mock 插件通常无法拦截；先确认请求是否进入当前开发服务器。
- 微应用只在自身运行时 setup/cleanup 请求实例；Mock 插件属于开发服务器基础设施，不为 Mock 增加应用生命周期或跨应用共享状态。

## Fixture 与场景生成

- 默认生成确定、可复现、脱敏的合成数据；不写入真实账号、人员、客户、域名、Token 或原始服务响应。
- 成功场景覆盖契约必填字段；可选字段只在目标 UI 流程或指定场景需要时加入，不为“看起来完整”虚构内容。
- 枚举、格式、nullable、数组与分页严格按契约。Mock 列表与详情使用能维持关联的稳定 ID，不默认使用无种子的随机数。
- 正常、空集合、校验失败、未认证、无权限、未找到和服务错误只在用户要求、UI 流程需要或契约声明相应响应时生成；错误状态和错误体必须来自契约。
- 同一 URL 的分支使用契约声明的 path/query/header/body 条件配置互斥的 `validator` 或等价机制；不同业务演示状态使用插件 `scene` 或仓库现有方案，不注册多个无法区分的默认路由，也不通过猜测字段切换。
- Mock 只复现传输契约和指定场景，不复制请求库的重试、错误标准化、Query 缓存或 Service 映射逻辑。

## 实施与验证

每个端点在实现前建立最小对照：

| 契约模式 | operation | method/path | 参数与请求体 | 成功与错误响应 | 有效 URL | Mock 场景 |
| --- | --- | --- | --- | --- | --- | --- |
| SDK/API 物料或 OpenAPI 文档 | 以真源为准 | 以真源为准 | 以真源为准 | 以真源为准 | 结合请求配置计算 | 仅列已要求或有契约依据的场景 |

完成后验证：

1. TypeScript 能从 SDK 根入口解析类型，或手写 DTO 与文档逐项一致。
2. 实际业务 Service 发出的 method、URL、query、headers 和 body 能命中 Mock，不只用手工请求绕过 Service 验证。
3. 成功响应和每个已实现错误场景的状态码、content-type、wire DTO 与契约一致。
4. SDK 模式下至少覆盖一个 path/query/body 序列化特征；文档模式下至少覆盖一个必填参数和响应结构。
5. 插件日志显示命中新 Mock 入口，入口与共享依赖热更新有效。
6. 请求实例初始化、错误传播、Service 映射和 UI 状态职责未因 Mock 改动而改变。
