# 官方文档来源

## 本地缓存优先

本 Skill 的官方缓存只保留组件级中文文档，避免聚合全文、变更记录、博客、设计规范和 React 指南在日常组件任务中制造噪音。

- 组件级文档清单：`references/official/component-manifest.md`
- 单组件文档：`references/official/components__<component>-cn.md`
- 单组件语义文档：`references/official/components__<component>-cn__semantic*.md`

处理组件细节、API、示例、Design Token、Semantic DOM、`classNames` 或 `styles` 时，先读取本地组件级缓存。只有在本地缓存缺失、内容明显过期，或用户明确要求核对最新官方状态时，才访问网络。

## 权威入口

- 组件总览：`https://ant.design/components/overview-cn/`
- LLMs 文档说明：`https://ant.design/docs/react/llms-cn/`
- LLMs 导航文件：`https://ant.design/llms.txt`

## 使用方式

- 先从 `references/official/component-manifest.md` 查找组件级本地文件。
- 需要单组件细节时，优先读取 `references/official/components__<component>-cn.md`。
- 需要语义结构、`classNames` 或 `styles` 相关信息时，优先读取 `references/official/components__<component>-cn__semantic.md`；若该组件只有子结构 semantic 文件，在 manifest 中查找相邻文件。
- 需要覆盖多个组件时，优先在 `component-manifest.md` 中定位多个组件级文件，不读取聚合全文缓存。
- 需要设计规范、React 指南、博客、变更记录或 LLMs 导航时，直接访问对应官方 URL；这些资料不作为日常本地缓存保留。
- 需要确认官方入口是否变化时，访问 `https://ant.design/llms.txt`，并只把组件级中文链接落到本地缓存。

## 来源优先级

1. Ant Design 官方文档。
2. 本 Skill 中缓存的 Ant Design 中文 LLMs 文档。
3. 当前 Skill 的分类 reference。
4. 仓库内已实现的相邻 UI 模式。
5. 第三方资料仅能作为背景信息，不能覆盖官方文档。

## 刷新规则

- 刷新本地缓存时，先临时读取或下载 `https://ant.design/llms.txt`，再按其中中文组件链接刷新 `references/official/components__*-cn*.md` 和 semantic 文件。
- 刷新后更新 `references/official/component-manifest.md`，只记录保留的组件级中文链接、文件名和下载状态。
- 不在本地官方缓存中保留 `llms.txt`、`llms-full-cn.txt`、`llms-semantic-cn.md`、`changelog-cn.md`、`manifest.md`、博客、设计规范或 React 指南；确实需要时按任务访问官方 URL。
- 如果本地缓存的官方文档和分类 reference 不一致，以本地官方缓存为准，更新对应 reference，并在 `component-index.md` 确认路由仍正确。
- 只修改受影响分类；不要因为一个组件变化重写全部 reference。
