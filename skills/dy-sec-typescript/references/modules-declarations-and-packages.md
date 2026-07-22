# 模块、声明与包边界

## 判断顺序

处理模块、声明或包边界前，确认目标运行时、`package.json` 模块与导出字段、构建产物，以及与当前任务相关的 TypeScript 配置。源码使用 `import` 不代表运行时一定是 ESM。

## 核心规则

- 模块语法、文件扩展名、声明和导出必须描述真实运行时；不为统一风格强行迁移合法的 ESM 或 CJS 边界。
- 浏览器 Bundler、Node.js、测试和构建脚本保持清晰的运行时边界，不在同一模块无约定地混用 ESM 与 CJS。
- 只用于类型的符号使用 `import type`；启用保留导入语义时同步检查实际输出。
- 不从其他包的 `src/`、构建目录或未导出的深层路径获取类型。
- 路径别名必须被类型检查、构建、测试和运行时共同理解，不能只让编辑器解析成功。
- `.d.ts` 只描述真实存在的 API；ambient module、`declare global` 和资源声明限制在最小作用域。
- 包的 `exports`、JavaScript 产物、source map 和声明路径必须对应，不引用未发布源码或消费者无法解析的别名。
- 不同运行时需要独立的类型环境时，沿用脚手架已有边界，不在普通源码任务中重组配置。

## 反模式

- 不用 `.mts`、`.cts`、动态 `import()` 或扩展名改写绕过错误的模块配置。
- 不新增宽泛的 `declare module '*'`、全局类型或手写第三方声明来隐藏真实解析错误。
- 不手工修改生成的声明文件，也不让源码类型与发布声明分叉。
- 不仅凭本地源码 typecheck 判断包可用；发布边界必须验证打包清单、导入路径和消费者解析。

修改库、声明或包结构后，按项目已有方式检查产物，并用最小真实消费者验证安装、导入和 typecheck；双模块包分别验证 ESM、CJS 和声明解析。

官方依据：[Modules Reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html)、[Declaration Files](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)、[Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)。
