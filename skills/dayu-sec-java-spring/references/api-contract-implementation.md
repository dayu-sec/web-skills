# API 契约实现

## 契约先行

- 先确定 OpenAPI 是设计真源还是由代码确定性生成；不要同时手工维护并允许漂移。
- 按 `$dayu-sec-rest-api` 确定资源路径、方法、状态码、分页、错误和版本。
- 将 OpenAPI 的必填、可空、枚举、格式、长度和响应状态落实到 Java 类型、校验和异常映射。
- 生成类型存在时优先复用或适配生成类型，不另写字段相似但语义漂移的 DTO。

## Controller

- 使用 `@RestController` 和稳定的资源级 `@RequestMapping`；版本前缀由仓库统一装配。
- 显式声明 `@PathVariable("resource_id")`、`@RequestParam(name = "page_size")` 等线上名称，避免只依赖编译参数名。
- 请求体使用 `@Valid @RequestBody`；方法或 Query/Path 校验按仓库约定启用 `@Validated`。
- 使用 `ResponseEntity` 或仓库统一机制准确表达 `201`、`202`、`204` 和响应头，不把所有结果固定为 `200`。
- Controller 只调用应用用例并转换结果；不写事务、复杂分支、数据库查询或外部调用。

## DTO 与校验

- Java 字段使用 `camelCase`，通过全局 Jackson `SNAKE_CASE` 或显式 `@JsonProperty` 生成 `snake_case` JSON。
- Query 和 Path 参数不受 Jackson 命名策略影响，必须单独绑定线上名称。
- 使用最具体的 Java 类型：日期用 `LocalDate`，绝对时间用 `Instant`/`OffsetDateTime`，标识按契约选择字符串、UUID 或数值。
- Bean Validation 负责非空、长度、格式、范围和集合大小；业务状态和跨资源规则由应用/领域层验证。
- 对条件必填和跨字段约束使用类级校验或领域行为，不在 Controller 堆叠临时 `if`。
- 默认值只在契约允许时设置，并在 OpenAPI、DTO 和运行时保持一致。

## 序列化

- 统一 ObjectMapper 的命名、时间和枚举策略；外部服务协议不同时使用独立客户端 ObjectMapper，避免污染全局行为。
- 禁用时间戳序列化，输出 ISO 8601；明确时区和精度。
- 不把 `FAIL_ON_UNKNOWN_PROPERTIES=false` 当成无条件最佳实践：写接口和安全敏感契约优先严格校验，兼容策略须有来源。
- 避免双向实体关系、Lazy Proxy 和内部字段被自动序列化。

## 异常处理

- 使用 `@RestControllerAdvice` 或平台统一处理器把校验、认证、授权、业务冲突、资源不存在和未知异常映射到统一错误契约。
- 业务异常携带稳定错误状态和允许的调用方描述；HTTP 映射集中维护。
- 未知异常记录一次完整诊断上下文，对外返回泛化 `500 INTERNAL`，不暴露堆栈和底层异常消息。
- 外部依赖异常先转换为本服务语义；不要直接透传上游状态、字段或错误体。
- 不在 Controller 捕获 `Exception` 后返回 `200`、空对象或自定义布尔值。

## OpenAPI 注解与文档

- 为操作提供稳定 `operationId`、summary、description 和 tags。
- schema 注明必需字段、枚举、格式、约束和中性示例；注解不能代替运行时校验。
- 声明所有实际响应码和统一错误 schema；异步接口描述任务状态查询方式。
- 生成并 lint OpenAPI，比较契约差异；对破坏性变化阻断交付或按版本迁移。

## 认证与授权

- 认证由过滤器/安全链完成，Controller 从受信上下文读取身份，不从普通 Query 参数接受操作者身份。
- 在方法或应用用例边界执行授权，并验证资源归属；URL 隐藏不是授权。
- 使用最小权限和默认拒绝策略；Actuator、文档和管理端点单独保护。
- CORS、CSRF、会话和 Token 策略按真实客户端形态配置，不复制宽松示例。

## API 测试重点

- 验证线上字段是 `snake_case`，Query/Path 名称与 OpenAPI 相同。
- 验证每个主要成功状态和错误状态、媒体类型、响应头及错误信封。
- 验证未知字段、空值、边界值、非法枚举和时间格式。
- 验证 `401`、`403`、资源归属和不泄露策略。
