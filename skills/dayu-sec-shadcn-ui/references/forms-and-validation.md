# 表单与校验

## 职责分层

- shadcn/ui 的 Field、Label、Input、Select 等只负责结构、交互和无障碍，不拥有表单状态或领域校验。
- Zod 等 Schema 是提交数据和不可信输入的运行时契约；不要在 JSX、Schema 与 Service 中复制三套近似规则。
- React Hook Form 负责字段注册、脏状态、触碰状态、提交和性能；`@hookform/resolvers` 负责把 Schema 错误转换为表单库错误。

## 何时引入 resolver

- 模板明确以 React Hook Form + Zod 作为业务表单基线，并提供真实可运行的表单入口时，直接声明 `react-hook-form`、`zod` 和 `@hookform/resolvers`，用 `zodResolver(schema)` 建立唯一校验链路。
- 只有简单受控配置、搜索筛选或组件库自带表单已经满足需求时，不为了“未来可能使用”额外装配第二套状态引擎。
- 存量 UI 库 Form 可以继续负责布局和字段呈现，但 Schema 校验只能有一个权威入口。不要同时维护 UI 库 rules 与 Zod 的重复业务规则。

## 实现方式

- 原生输入优先使用 `register`；Select、Switch、日期等非原生受控组件使用 `Controller`，显式映射 `value`、`onChange`、`onBlur` 和 ref 能力。
- 用 FieldGroup、Field、FieldLabel、FieldDescription、FieldError 或目标项目的等价结构组织字段；相关字段集合使用 FieldSet 与 Legend。
- Field 在错误时暴露 `data-invalid`，输入控件设置 `aria-invalid`，错误文本与控件通过稳定 id 或组件约定关联。
- 提交按钮反映 submitting 状态并防止重复提交；reset 明确回到默认值还是服务端快照。提交失败保留用户输入，成功反馈不能假装已持久化。
- Schema 与表单值类型从同一来源推导；需要输入/输出转换时显式区分 `z.input` 与 `z.output`。

## 验证

- 单测 Schema 的边界值与错误路径；组件测试或浏览器验证空提交、修正错误、受控组件、提交中、失败、成功和重置。
- 检查错误不只依赖颜色、首个错误可被定位、键盘顺序稳定，异步错误不会覆盖用户更新后的值。
