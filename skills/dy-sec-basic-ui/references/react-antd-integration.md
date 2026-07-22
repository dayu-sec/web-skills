# DySec React、Ant Design 与主题接入

## 目录

- [契约优先级](#契约优先级)
- [React 与 Ant Design 根配置](#react-与-ant-design-根配置)
- [部署形态与主题作用域](#部署形态与主题作用域)
- [Ant Design 适配](#ant-design-适配)
- [业务 CSS 变量](#业务-css-变量)
- [Emotion 样式](#emotion-样式)
- [应用作用域边界](#应用作用域边界)
- [实现检查](#实现检查)

## 契约优先级

按以下顺序确定实现：

1. 当前项目的 `AGENTS.md`、主题入口、依赖版本和已有组件约定。
2. 当前项目实际安装的 `@lrd/dy-sec-bizcom-theme` 公共导出和类型。
3. 本 reference 记录的通用接入模式。

项目使用主题包时只依赖其公共导出，不读取内部文件。项目未安装主题包时沿用现有主题契约；只有任务范围和目标仓库允许时才建议接入依赖，不虚构同名 API，也不使用散落的硬编码值模拟主题包。

## React 与 Ant Design 根配置

同一个 `mode` 同时驱动 Ant Design algorithm、团队 Ant Design Token 和业务 CSS 变量。默认模式为 `light`。

```tsx
import { useMemo } from 'react';
import { ConfigProvider, theme as antdTheme } from 'antd';
import {
  getDySecAntdTheme,
  type DySecThemeMode,
  useDySecCssVariableScope,
} from '@lrd/dy-sec-bizcom-theme';

/** 应用主题入口的 Props。 */
interface AppThemeProps {
  mode: DySecThemeMode;
}

/** 使用同一个模式装配 Ant Design Token 和当前文档的业务变量作用域。 */
export function AppTheme({ mode }: AppThemeProps) {
  const theme = useMemo(() => getDySecAntdTheme(mode), [mode]);

  useDySecCssVariableScope({
    selector: 'body',
    theme: mode,
  });

  return (
    <ConfigProvider
      theme={{
        algorithm: mode === 'dark' ? antdTheme.darkAlgorithm : antdTheme.defaultAlgorithm,
        ...theme,
      }}
    >
      <main>...</main>
    </ConfigProvider>
  );
}
```

上例适用于独占当前文档的单体主应用。实际选择器必须根据文档所有权、Portal 挂载位置和目标仓库约定确定，不能无条件复制 `body` 或应用根选择器。

`useDySecCssVariableScope` 通过受控 stylesheet 写入变量，并在模式切换或应用根卸载时替换、释放。不要改成在 React `style` 属性中批量写入变量：

```tsx
<main style={getDySecOperationalCssVariables(mode)}>...</main>
```

## 部署形态与主题作用域

先确认谁拥有当前 `document`、业务页面根节点，以及 Modal、Drawer、Popover、Tooltip、Notification 等 Portal 的实际容器，再选择变量作用域：

### 独占文档的单体主应用

- 应用独占当前页面和 `body` 时，默认把 `--dy-sec-*` 语义变量作用于 `body`，使挂载到 `body` 的 Ant Design Portal 与页面内容自然继承同一主题。
- `body` 作用域只用于主题变量和必要的主题继承，不表示业务模块可以把布局、重置、滚动或组件覆盖扩散到整个文档。
- 主题切换与应用卸载仍由主题入口更新或释放受控 stylesheet，不在组件内重复维护变量。

### 微前端宿主与子应用

- 子应用默认把主题变量限制在唯一、可定位的应用根节点，不能用 `body` 覆盖宿主或其他子应用的主题。
- 子应用创建的 Portal 应挂载到其主题根节点或专属浮层容器；若必须使用宿主共享容器，由宿主提供明确的主题、层级、清理和冲突处理契约。
- 宿主若独占当前文档，可以在 `body` 提供平台级语义变量；子应用仍需按自身根节点和跨应用主题契约决定是否继承或覆盖。
- 独立运行与集成运行复用同一主题装配契约，并按 `$dy-sec-microfrontend` 验证挂载、卸载、主题同步和 Portal 清理。

## Ant Design 适配

使用 `getDySecAntdTheme(modeOrTheme)` 获取主题包提供的 Ant Design 配置，并保持窄范围适配：

- 全局 Token 只统一字体、字号、控件高度、圆角、文本、背景、边框和语义状态色。
- 组件 Token 只修正需要统一的 Button、Menu、Tabs、Table、Card、Input、Select、Alert、Drawer 和 Typography 等组件。
- 局部导航需要独立范围时，使用 `getDySecAntdMenuTheme` 配合嵌套 `ConfigProvider`，不让导航决策污染所有业务组件。
- 优先使用 Token、组件 `classNames`、`styles` 和 Semantic DOM，避免基于 `.ant-*` 内部结构的大范围全局覆盖。
- 组件 API、状态或 Semantic DOM 不确定时，使用 `$dy-sec-antd-ui-components` 查阅组件级资料。

品牌色与 `colorPrimary` 承担不同职责。只有产品约定明确改变日常主操作色时才覆盖主色 Token，不能因为品牌定制自动替换全部主操作色。

## 业务 CSS 变量

使用主题包时，业务 CSS、CSS Modules 和非 React 子树消费默认的 `--dy-sec-*` 语义变量：

```css
.panel {
  color: var(--dy-sec-text-primary);
  background: var(--dy-sec-bg-panel);
  border: 1px solid var(--dy-sec-border-subtle);
  border-radius: var(--dy-sec-radius-sm);
}

.panelHeader {
  display: flex;
  gap: var(--dy-sec-space-2);
  align-items: center;
}
```

- 优先使用文本、背景、边框、状态和间距语义变量，不把 Light/Dark 色值散落在选择器中。
- 不在业务代码中创建第二套主题变量命名空间。
- 只有迁移旧项目时才使用 `getDySecOperationalCssVariables` 的 `prefix` 选项；兼容旧前缀时记录迁移范围和删除条件。

## Emotion 样式

自研组件使用模块级样式工厂，通过 `@lrd/dy-sec-bizcom-theme/emotion` 消费稳定主题对象：

```tsx
import { css, getDySecStyles } from '@lrd/dy-sec-bizcom-theme/emotion';
import type { DySecTheme2 } from '@lrd/dy-sec-bizcom-theme';

/** 根据稳定主题对象生成组件级 Emotion 样式。 */
function getStyles(theme: DySecTheme2) {
  return {
    root: css({
      display: 'flex',
      gap: theme.spacing(1),
      color: theme.colors.text.primary,
      background: theme.colors.background.primary,
      border: `1px solid ${theme.colors.border.weak}`,
    }),
  };
}

const styles = getDySecStyles(theme, getStyles);
```

- 样式工厂定义在模块级，只接收稳定主题对象和真正影响样式的原始值。
- 不在组件函数内部重复创建 factory，也不把任意业务对象作为缓存参数。
- 主题创建和颜色派生留在主题入口；不在列表行、渲染循环或 CSS getter 中重复计算。

## 应用作用域边界

- 应用主题入口负责模式、Ant Design 根配置，以及根据文档与 Portal 所有权确定的主题作用域；页面与业务组件只消费该上下文。
- 单体主应用可以在自己独占的 `body` 写入主题语义变量；业务样式仍不得无边界覆盖 `html`、`body` 或无关页面选择器。
- 微前端子应用不得覆盖 `html`、`body`、其他应用根节点或宿主容器。
- 主题切换时更新当前应用的 stylesheet；应用根卸载时释放 stylesheet、事件监听和其他全局副作用。
- CSS 变量只传递主题值，不替代运行时上下文、路由边界或业务状态管理。
- 不同运行入口复用同一主题装配，避免形成两套颜色或间距。
- 微前端宿主与子应用之间的主题同步、选择器隔离和卸载验证由 `$dy-sec-microfrontend` 约束。

## 实现检查

- `mode` 是否同时驱动 Ant Design 和业务变量。
- 是否先确认文档、应用根节点和 Portal 容器的所有权，再选择主题作用域。
- 独占文档的单体主应用是否让 `body` 范围内的页面与 Portal 继承同一套语义变量。
- 微前端子应用的主题和 Portal 是否保持在应用根节点或宿主明确约定的共享容器内。
- 是否把 `body` 主题变量作用域误用成全局布局、重置、滚动或业务组件覆盖。
- 是否出现 `style={cssVariables}`、无明确所有权的全局 `:root` 或新的变量命名空间。
- 是否从语义 Token 取值，而不是复制 Light/Dark 字面量。
- Emotion factory 和主题对象是否稳定复用，颜色计算是否离开渲染热点。
- Ant Design 覆盖是否限定在必要 Token 或组件范围内。
- 不同运行入口、主题切换和应用根卸载后是否无残留 stylesheet。
