# OpenAPI 服务接入模式

## 先选择模式

- 按服务或端点确认契约交付方式：已发布 TypeScript SDK 包时使用 SDK；只有供开发阅读的 OpenAPI 文档时手写业务 Service。
- 同一端点只保留一个调用真源。不要同时调用生成函数和手写 URL，也不要把生成类型复制为另一套本地类型。
- 一个应用可以按不同服务采用不同模式，但必须记录各自契约来源、版本或确认时间及更新责任。
- 不因拿到文档就自行生成、提交或发布 SDK；SDK 物料生产必须是任务明确授权的独立边界。

## 模式一：安装生成 SDK 包

### SDK 生产边界

- 把源 OpenAPI 作为只读事实来源；兼容转换写入独立处理区并保持确定性。
- 生成源码和构建产物必须可复现，不手工修补；形态变更落到源契约、生成配置、模板或确定性脚本。
- 团队 API 物料按 `@lrd/dy-sec-api-<service-name>` 命名，`service-name` 使用稳定的 `kebab-case`；版本和实际包名仍以目标生成仓库为准。
- 公开包从根入口导出函数、模型、请求类型和版本，发布 ESM/CJS 与声明产物；消费方不得深导入源码或构建目录。
- 公开函数名、`payload`、请求体 `data`、序列化和可选 `HttpClient` 参数以实际生成包为准，不根据源 `operationId` 或常见生成器形态猜测。
- 生成包保持传输无关，不内置应用的 baseURL、Token、语言、错误提示或框架状态。

### 应用消费边界

- SDK、`@lrd/dy-sec-bizlib-request` 及其 peer dependency 按实际直接导入关系声明，包清单与锁文件同步更新。
- 先从安装包声明确认实际签名；团队函数式 SDK 通常为 `operation(payload, options?, httpClient?)`，不要自行改成 service/client Class。
- 先初始化并取得应用拥有的 live `HttpRequest`，再把 SDK `HttpClient.request(config)` 薄适配为 `httpRequest(config)`；不在 SDK 旁复制第二套 Axios、鉴权或错误配置。
- 只有单一运行时所有者时才设置 SDK 全局 client。多个可独立挂载应用可能共享同一 SDK 模块、或 SDK 没有配套清理 API 时，使用单次 client 参数或应用内 Service 适配，避免全局 client 被相邻应用覆盖。
- 业务代码从 SDK 根入口导入生成函数和类型。仅在需要业务命名、多个 operation 编排、wire DTO 到视图模型映射或稳定 UI 边界时增加 Service；不要为每个生成函数机械写一层同签名转发。
- 不修改生成 DTO 的字段和可选性；页面需要 `camelCase` 或聚合结构时在 Service 边界显式映射。

### SDK 更新验证

- 比较根导出、函数签名、模型、必填性、枚举、错误与序列化差异，先识别破坏性变更再升级消费者。
- SDK 生产侧执行 OpenAPI 校验、生成 surface、类型、Lint、测试、构建、可复现和敏感信息检查。
- 发布前检查打包清单，并用安装 tarball 或目标源版本的最小消费者验证根入口、运行时函数和声明解析。
- 应用侧验证 SDK 依赖和锁文件、请求初始化顺序、client 所有权、真实业务调用、错误传播、typecheck 和构建。

## 模式二：只依据 OpenAPI 文档手写 Service

- 只实现当前用到的 operation，不在应用仓库仿造完整 SDK、生成目录或第二套请求基础设施。
- 按文档逐项确认 method、path、path/query/header、请求体、响应体、错误结构、必填性、枚举、空值和分页；组合 `$dy-sec-data-contract-first`，禁止凭经验补字段。
- 传输 DTO 保留 OpenAPI 的 wire name 和结构；业务视图需要不同命名或结构时，在 Service 返回前显式映射并保持单向可追踪。
- Service 直接使用应用 live `HttpRequest` 的 `get/post/...` 或调用签名，保留实例的 baseURL、鉴权、语言、拦截器和错误传播。
- 不使用 `any`、宽泛索引签名、无来源 fallback 或组件内临时响应类型；契约无法确认时停止猜测并指出缺口。
- 文档不进入源码时，不复制整份文档；在任务交付和测试中记录所依据的文档版本或确认时间，并明确后续同步责任。
- SDK 后续可用时优先替换 Service 内部调用，尽量保持已经稳定的业务 Service 返回契约，删除手写 wire DTO 和 URL 的旧真源。

## 共享职责

| 层级 | 负责 |
| --- | --- |
| 请求入口 | 初始化和导出 live 请求实例，拥有鉴权、语言、超时、拦截器与清理 |
| SDK 适配或手写 Service | 调用契约、必要编排和 wire DTO 映射 |
| Query/状态库 | 缓存、并发、刷新和 UI 数据状态 |
| 页面/组件 | 触发用户流程和展示，不拼 URL、不定义临时服务响应契约 |

- 同源静态配置或运行时资产可沿用项目已有 `fetch` 边界；不要为了形式统一把所有网络读取强制改成业务 API 请求实例。
- 单体应用在启动前初始化一次；有独立挂载生命周期的应用按所有权 setup/cleanup，请求和 SDK client 冲突再组合 `$dy-sec-microfrontend` 验证。
