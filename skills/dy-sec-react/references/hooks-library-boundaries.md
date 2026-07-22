# React Hooks library 选择与边界

## 团队选择

- 新建或尚未形成约定的 React 应用统一选择 `ahooks` 作为通用 Hooks library。
- 选型综合持续维护、成熟度、React 兼容、类型与 SSR、文档、社区采用和迁移成本，不仅比较下载量、Stars 或发版次数。
- 不在同一应用中并存 `ahooks` 与 `react-use`、`usehooks-ts`、`rooks`、`@react-hookz/web` 等第二套综合型 Hooks library。
- 已有应用使用其他综合型 Hooks library 时，先沿用仓库约定；只有存在明确能力、维护或兼容问题时，才在独立迁移任务中评估归一，不在普通需求中顺带替换。
- 简单局部状态和清晰的单个 Effect 优先使用 React 原生 Hooks；不要为了统一而机械包装或替换。
- 不在 Skill 中固定版本。引入或升级前核对目标仓库 React 版本、包清单、锁文件、当前 peer dependency、类型、SSR、tree-shaking 和维护状态。
- 只有标准库停止维护、不能支持目标 React、存在长期未解决的关键缺陷或反复缺少必要能力时，才重新开展候选调研和迁移评估。
- 没有真实调用或明确模板基线时不预装依赖；源码直接 import 的应用必须直接声明依赖。

## 能力归属

| 场景 | 方案 |
| --- | --- |
| 局部状态、Reducer、Ref、Context 和 Effect | React 原生 Hooks |
| DOM、浏览器事件、观察器、计时器、节流防抖和轻量状态辅助 | `ahooks` |
| 服务端查询、缓存、并发、失效、轮询、重试、分页和 mutation | `@tanstack/react-query` |
| path、search params、location state 和导航 | React Router |
| 跨组件或应用内客户端状态 | 目标仓库已有 Context 或状态库 |
| 表单值、校验、提交和字段联动 | 目标仓库已有表单方案 |
| 具有业务语义的复用逻辑 | 应用内部自定义 Hook，组合上述能力 |

服务端状态只在这里声明归属；请求实例、业务 Service、错误传播和 Query 状态职责使用 `$dy-sec-frontend-request`，不要在本文件复制实现规则。

## `ahooks` 使用范围

适合优先评估的能力包括：

- `useEventListener`、`useClickAway`、`useKeyPress`、`useHover`、`useInViewport` 和 `useSize` 等浏览器交互。
- `useDebounceFn`、`useThrottleFn`、`useInterval` 和 `useTimeout` 等需要稳定清理的调度逻辑。
- `useBoolean`、`useToggle`、`useMap` 和 `useSet` 等能明显简化状态操作的辅助能力。

不要使用以下方式制造重叠职责：

- 不使用 `useRequest`、`usePagination`、`useAntdTable` 或请求型 `useInfiniteScroll` 管理远程数据；改用 TanStack Query。
- 不使用 `useUrlState` 建立第二套 URL 状态规则；优先使用 React Router。
- 不用 `useMount`、`useUnmount` 或 `useAsyncEffect` 绕过 Effect 依赖、竞态、幂等和清理要求。
- `useLocalStorageState` 只保存非敏感 UI 偏好，不保存 Token、身份、租户、权限或其他安全上下文。
- WebSocket、轮询、全局事件和模块级状态必须先确认所有者、重连或重试策略以及卸载行为。

## 客户端状态

- 组件内部或少量父子共享状态优先使用 React 原生 Hooks 和明确的 props/callback，不先创建全局 Store。
- `zustand` 只用于应用内部确有跨组件、跨视图共享需求的客户端状态，例如稳定筛选条件、局部工作区状态或轻量业务上下文。
- 服务端查询结果、缓存、刷新和 mutation 归 `@tanstack/react-query`；URL 可恢复状态归 React Router；表单值和校验归项目表单方案。
- `immer` 只在深层嵌套更新明显复杂时与 React 或 Zustand 组合，不因模板已安装或使用 Zustand 而默认启用。
- Store 按业务职责拆分并暴露窄 selector，不创建囊括请求、路由、表单和 UI 的万能 Store。
- 微应用 Store 默认只属于当前微应用；跨应用状态使用明确的平台上下文、URL 或通信契约，不直接共享 Zustand 单例。

## 单体与微前端

- 单体应用只在实际消费或仓库明确规定模板基线时声明 `ahooks`；未使用的依赖不会进入正常 tree-shaken bundle，但仍增加安装、锁文件、SBOM 和审计面。
- 微前端中的主应用和每个微应用分别声明自己直接 import 的依赖，不依赖宿主的偶然安装，不通过 Hooks library 隐式共享跨应用状态。
- 微应用卸载时释放事件监听、计时器、观察器、订阅和连接；重复挂载不得累计副作用。
- 只有任务明确触及共享依赖装配、运行时单例、生命周期或隔离时才组合 `$dy-sec-microfrontend`；普通应用内部 Hook 不因仓库采用微前端而改变实现。

## 实现与评审

- 搜索现有 imports、自定义 Hooks 和依赖声明，先判断是原生能力、通用工具、服务端状态、路由状态还是业务语义。
- 新增依赖前证明现有能力不能清晰覆盖；发现两套综合型 Hooks library 时，报告重复 API、实际调用面和迁移风险。
- 自定义业务 Hook 应表达领域意图并隐藏组合细节，不复制通用库实现，也不把请求、路由和全局状态混成无边界入口。
- 验证 Strict Mode、重复渲染、卸载清理、SSR 或无 DOM 环境、类型推断和目标构建；依赖变化同时验证锁文件和生产构建。
