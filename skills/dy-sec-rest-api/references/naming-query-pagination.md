# 命名、查询与分页

## 线上的命名边界

| 位置 | 规则 | 示例 |
| --- | --- | --- |
| 静态资源路径 | 小写复数名词、`kebab-case` | `/security-events` |
| 路径变量 | `snake_case` | `/{event_id}` |
| Query 参数 | `snake_case` | `event_type` |
| JSON 请求和响应字段 | `snake_case` | `created_at` |
| 自定义动作 | 小写动词，复合词用连字符 | `:import-cancel` |

- 代码内部继续遵循语言和仓库约定，例如 Java 使用 `camelCase`，在序列化和参数绑定边界显式映射线上名称。
- 缩写按一个完整单词处理，避免 `sourceIP`、`userID` 等风格漂移；线上分别表达为 `source_ip`、`user_id`。
- 参数名使用英文或英文与数字组合，避免无意义介词和后置形容词。
- 枚举值选择一种稳定风格并写入契约；不要把本地化展示文案作为可持久化的枚举值。

## 过滤

简单过滤直接使用 Query 参数：

```http
GET /orders?status=active&owner_id=42
```

复杂组合查询统一放入 `q`：

```http
GET /orders?q=status:active AND amount:[10 TO 100]
```

- 同一请求不得混用基本过滤和 `q` 高级查询，除非仓库契约明确规定组合语义。
- 只有服务端真正实现并测试后，才在契约中声明范围、布尔、分组、通配符或正则能力。
- 对字段、操作符和排序键建立允许列表；参数化查询，禁止把客户端表达式直接拼接为 SQL、JPQL 或脚本。
- 明确未知过滤字段、重复参数、空值、大小写和非法语法的处理方式，默认返回 `400 INVALID_ARGUMENT`。

## 排序

使用 `order_by` 表达一个或多个排序键，前缀 `-` 表示降序：

```http
GET /orders?order_by=created_at,-priority
```

- 从左到右应用排序优先级。
- 分页查询必须包含稳定且唯一的最终排序键，例如在 `created_at` 后追加 `id`。
- 只允许按契约声明的字段排序；不要把任意输入直接映射为数据库列名。

## 范围式分页

请求：

```http
GET /orders?offset=0&limit=20
```

响应：

```json
{
  "total": 120,
  "items": []
}
```

- `offset` 从 `0` 开始，`limit` 设置默认值、最小值和最大值。
- 仅在总量计算成本可接受且调用方确实需要 `total` 时使用。
- 数据频繁变化时说明翻页漂移风险，必要时改用令牌分页。

## 迭代式分页

请求：

```http
GET /orders?page_size=20&page_token=opaque-token
```

响应：

```json
{
  "next_page_token": "opaque-next-token",
  "items": []
}
```

- `page_token` 必须是不透明值，服务端校验签名、版本、查询条件和有效期；客户端不得解析其内部结构。
- 最后一页省略或返回空的 `next_page_token`，行为要在 OpenAPI 中固定。
- 相同令牌和相同查询条件应产生可预测结果；条件改变后拒绝旧令牌。

同一端点只选择一种分页模型。不要混入 `page`、`size`、`data`、`total_pages` 等另一套信封，除非目标仓库已有明确且需兼容的契约。

## 时间、时区与语言

- 日期使用 `YYYY-MM-DD`；时间点使用 ISO 8601/RFC 3339，例如 `2026-01-15T08:30:00Z` 或带明确偏移的时间。
- 服务端内部优先使用 UTC 保存和比较绝对时间，响应按契约保留 `Z` 或偏移。
- 需要调用方时区时使用仓库约定的标准请求头；不要从服务器本地时区或无偏移字符串猜测。
- 语言区域使用 BCP 47，并通过 `Accept-Language` 协商，例如 `zh-CN`。
- 时间和语言请求头都要在 OpenAPI 中声明默认行为、允许值和不支持值的处理。
