# 代码质量与工具边界

## 编码规范

- 使用 `const`；确需重新赋值时使用 `let`，不使用 `var`。
- 优先使用清晰的字面量、属性简写、解构、模板字符串、默认参数和 rest 参数。
- 团队约定具名顶层函数和可复用普通函数使用 `function` 声明；箭头函数用于匿名内联回调、高阶函数的回调参数和明确需要词法 `this` 的闭包。
- 箭头函数的参数始终使用 `()` 包裹，包括只有一个参数时；复杂回调提取为具名函数。
- 不修改调用方传入对象；把复杂逻辑拆成低副作用、可测试的函数。
- 使用严格相等和控制语句花括号；模块语法遵循项目运行时与发布契约。
- 命名表达职责，避免无意义缩写、类型种类前缀和与实际语义不符的名称。
- 不直接调用任意对象上的 `hasOwnProperty`。
- 不机械禁止 `for...of`、函数声明、自增、命名导出或默认导出；以项目约定和可读性为准。

## 工具职责

- TypeScript 编译器负责类型、模块解析和声明一致性。
- typescript-eslint 负责 TypeScript AST 和需要类型信息的代码规则；类型信息来源沿用脚手架选择，不在普通源码任务中切换 parser 方案或共享 preset。
- ESLint 负责代码质量和团队约定，不与 TypeScript 核心检查或格式化规则重复竞争。
- Prettier 负责纯格式，不用格式差异表达类型或业务语义。
- 优先运行仓库已有的统一质量门禁；没有统一入口时，只组合实际存在且与变更相关的检查。

## 反模式

- 不只为匹配某个规范名称安装依赖、替换共享配置或迁移现有工具链。
- 不为满足 JavaScript 风格规则而破坏正确的 TypeScript 类型、运行时或公共契约。
- 不用 warning、批量 disable、关闭类型感知检查或扩大 ignore 范围掩盖错误。
- 不把编译器错误、类型感知 Lint、普通代码规范和纯格式问题混为一谈。
- 不因为依赖升级改变了共享规则集，就在无评估的情况下批量改写业务代码。
- 不用 `const fn = () => {}` 代替具名普通函数，也不把复杂业务逻辑长期留在匿名回调中。

评审时先判断问题属于类型、运行时、代码规范还是格式，再按项目现有规则处理；必要禁用限制在最小范围并说明原因。

参考依据：[Typed Linting](https://typescript-eslint.io/getting-started/typed-linting/)、[Shared Configs](https://typescript-eslint.io/users/configs/)、[Airbnb JavaScript Style Guide - Arrow Functions](https://github.com/airbnb/javascript#arrow-functions)。
