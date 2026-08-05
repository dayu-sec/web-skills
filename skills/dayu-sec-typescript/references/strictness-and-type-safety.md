# 严格模式与类型安全

## 核心规则

- 沿用项目已启用的严格检查，不在普通源码任务中降低或扩大编译器配置范围。
- 外部输入从 `unknown` 开始，按既有 Schema 或契约验证后再使用。
- 使用类型守卫、判别联合和 `never` 收窄并穷尽互斥状态。
- 区分 `null`、`undefined`、属性缺失和空集合，不为消除错误把必填字段改成可选。
- 配置对象优先使用 `satisfies` 保留推断；公共 API 和复杂返回边界显式标注，局部代码使用可靠推断。
- 泛型必须表达参数、返回值或多个成员之间的真实类型关系；没有关系时使用具体类型。
- Promise 必须被等待、返回、显式忽略或处理；`catch` 错误按 `unknown` 收窄。
- 使用带原因的 `@ts-expect-error` 表达已知例外，不使用 `@ts-ignore` 永久压制错误。
- `skipLibCheck` 不能用来掩盖本项目源码或自产声明错误。

## 反模式

- 不用 `any`、双重断言或宽泛环境声明伪造已经验证的类型。
- 不用类型断言代替运行时验证，也不为通过检查虚构缺失字段或兜底值。
- 不用非空断言隐藏初始化顺序、生命周期或空态问题。
- 不用多个互相矛盾的布尔字段表达互斥状态，优先使用判别联合。
- 不用 `Partial<T>` 或大面积可选属性模糊创建态、更新态和完整实体之间的差异。
- 不吞掉 Promise rejection，也不把未知错误直接当作固定错误结构。

评审时优先检查外部输入、公共类型、空值语义、状态穷尽、断言和异步错误是否与运行时事实一致。

官方依据：[strict](https://www.typescriptlang.org/tsconfig/strict.html)、[Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)、[Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)。
