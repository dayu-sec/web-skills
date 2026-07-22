# 语言演进与废弃能力

## 版本原则

- 普通源码任务以目标仓库锁定的 TypeScript 版本和现有配置为准，不自动升级编译器。
- 新脚手架或显式升级使用整个工具链已正式支持的最新稳定版；不把 beta、RC、nightly 或尚未被构建器、框架、编辑器插件、typescript-eslint 正式支持的版本作为团队基线。
- 版本敏感任务以目标版本的编译器诊断、官方发布说明和依赖兼容范围为准，不凭记忆复用旧版本结论。
- 不把 `ignoreDeprecations` 作为长期方案；迁移期临时使用时说明阻塞来源、影响范围和移除条件。
- 全局 Skill 不硬编码“当前最新版”版本号，避免稳定版本发布后规则立即失真。

## 可擦除语法

- 新代码只使用删除类型语法后仍是有效 JavaScript 的 TypeScript 写法。
- 禁止新增 `enum` 和 `const enum`；这是团队为保持可擦除语法作出的约定，不表述为 TypeScript 官方已废弃 `enum`。
- 仅需要类型集合时使用字符串或数字字面量联合；同时需要运行时值时使用 `as const` 对象并从值推导联合类型。
- 禁止带运行时代码的 `namespace` 或旧式 `module`、构造器参数属性、`import =`、`export =` 和尖括号类型断言。
- 使用普通对象或标准模块、显式类字段、与运行时匹配的导入导出，以及必要时的 `as` 断言替代上述语法。

```ts
const Status = {
  Ready: 'ready',
  Running: 'running',
  Failed: 'failed',
} as const;

type Status = (typeof Status)[keyof typeof Status];
```

## 已淘汰能力

新代码和新脚手架不使用当前稳定编译器已经移除或判错的旧能力，包括：

- `target: es5`、`downlevelIteration`、`outFile` 和 `baseUrl`。
- `moduleResolution: node`、`node10` 或 `classic`。
- `module: amd`、`umd`、`systemjs` 或 `none`。
- 将 `esModuleInterop`、`allowSyntheticDefaultImports` 或 `alwaysStrict` 显式设为 `false`。
- 使用 `module Name {}` 声明 namespace，或在 import attributes 中使用 `asserts` 而不是 `with`。
- `/// <reference no-default-lib="true" />` 等已不再受支持的旧指令。

以上清单只记录稳定且已经淘汰的基线，不替代目标版本的官方发布说明。升级任务必须重新核对从当前版本到目标版本之间的全部 breaking changes 和 deprecations。

官方依据：[erasableSyntaxOnly](https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html)、[Enums](https://www.typescriptlang.org/docs/handbook/enums.html)、[TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)、[TypeScript Release Notes](https://www.typescriptlang.org/docs/handbook/release-notes/overview.html)。
