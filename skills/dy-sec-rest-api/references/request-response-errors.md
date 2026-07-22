# 请求、响应与错误

## 请求

- `GET`、`DELETE` 和 `HEAD` 使用路径、Query 或请求头传参，不依赖请求体。
- `POST`、`PUT` 和 `PATCH` 默认接收 `application/json`；文件或表单场景明确声明 `multipart/form-data` 或 `application/x-www-form-urlencoded`。
- 请求体、嵌套对象和数组字段统一使用 `snake_case`。
- 使用结构化 DTO 和 Bean Validation/Schema 校验基础约束；跨字段、状态相关和权限相关规则放在应用或领域边界。
- 区分缺失、显式 `null`、空字符串和空数组，不用默认值掩盖必填契约。
- 对未知字段采用严格还是兼容策略要由契约明确；安全敏感或写入接口优先拒绝未声明字段，防止批量赋值。

## 成功响应

成功时直接返回资源本身：

```json
{
  "order_id": "1001",
  "status": "pending",
  "created_at": "2026-01-15T08:30:00Z"
}
```

集合按选定分页模型返回 `items` 及必要元数据。不要增加通用成功包装：

```jsonc
{
  "code": 0,
  "message": "success",
  "data": {} // 不要这样包装
}
```

- `204 No Content` 没有响应体。
- 空集合返回 `items: []`；契约声明为数组的字段不返回 `null`。
- 响应只暴露契约字段，不直接序列化 JPA 实体、持久化对象、内部异常或外部服务 DTO。
- 内容协商默认使用 `application/json`；支持其它媒体类型时声明 `Accept` 行为和 `406` 处理。

## 错误信封

所有失败使用标准 HTTP 错误码，并返回统一错误对象：

```json
{
  "error": {
    "status": "INVALID_ARGUMENT",
    "details": [
      {
        "field": "page_size",
        "description": "must be between 1 and 100"
      }
    ]
  }
}
```

- `error.status` 是稳定、可机器判断的枚举，不使用本地化文案。
- `error.details` 必须是数组；每一项只包含契约允许的字段。
- `field` 使用线上字段路径，例如 `items[0].quantity`；非字段错误使用约定的业务目标名或省略字段。
- `description` 面向调用方说明如何修正，不包含堆栈、类名、SQL、表名、内部地址或敏感数据。
- 多语言错误由请求语言协商；客户端逻辑依赖 `status`，不依赖描述文本。

## 错误状态映射

| HTTP | `error.status` | 典型语义 |
| --- | --- | --- |
| `400` | `INVALID_ARGUMENT` | 格式、必填、枚举或校验错误 |
| `400` | `FAILED_PRECONDITION` | 当前资源状态不允许操作 |
| `400` | `OUT_OF_RANGE` | 页大小、数值或时间范围非法 |
| `401` | `UNAUTHENTICATED` | 凭据缺失、无效或过期 |
| `403` | `PERMISSION_DENIED` | 调用方无操作或资源权限 |
| `404` | `NOT_FOUND` | 资源不存在或按安全策略隐藏 |
| `409` | `ABORTED` | 并发、版本或事务冲突 |
| `409` | `ALREADY_EXISTS` | 唯一资源已经存在 |
| `429` | `RESOURCE_EXHAUSTED` | 限流或配额耗尽 |
| `499` | `CANCELLED` | 能可靠识别的客户端取消 |
| `500` | `INTERNAL` / `UNKNOWN` | 未预期的服务端错误 |
| `500` | `DATA_LOSS` | 不可恢复的数据损坏或丢失 |
| `501` | `NOT_IMPLEMENTED` | 契约能力尚未实现 |
| `503` | `UNAVAILABLE` | 服务或关键依赖暂时不可用 |
| `504` | `DEADLINE_EXCEEDED` | 超出请求时限 |

- 同一错误原因在所有端点保持相同映射。
- 业务异常显式携带允许的 HTTP 状态和错误状态；未知异常由全局处理器统一转换为泛化 `500`。
- 不把外部依赖的原始错误体直接透传给调用方；先映射为本服务契约，再保留可追踪的内部原因。
- 日志中使用请求或追踪标识关联内部诊断信息，错误响应不要返回堆栈。

## 校验与部分失败

- 结构校验一次性返回可修正的字段问题；不要把同一请求的校验错误随机截断为第一个。
- 业务状态冲突与字段格式错误分开映射，不把所有异常都标为 `INVALID_ARGUMENT`。
- 批量接口若允许部分成功，响应要为每个输入提供稳定标识和结果，并在契约中明确整体 HTTP 状态；不允许部分成功时保证原子性。
- 异步任务的执行失败记录在任务资源中，同时为查询接口提供稳定错误状态；初始 `202` 不代表最终成功。
