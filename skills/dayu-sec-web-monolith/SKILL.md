---
name: dayu-sec-web-monolith
description: 处理单体 Web 应用的设计、实现、迁移、重构与评审，覆盖应用所有权、Shell 与业务视图边界、路由和 Provider 装配、运行入口、SPA 制品及真实运行验证；适用于一个部署和运行单元内承载完整用户体验的 React、Vue 等 Web 项目。
---

# DayuSec Web 单体架构

## 工作顺序

1. 同时使用 `$dayu-sec-web-project-conventions`，读取项目 `AGENTS.md`、清单、构建配置、源码入口和实际 Git 根，确认当前目标确为单体 Web 应用。
2. 设计或评审应用、Shell、Router、Provider 和业务职责时，读取 [架构与所有权](references/architecture-and-ownership.md)。
3. 定位应用入口、路由入口、布局、业务视图、请求、主题和本地开发配置；以实际职责搜索，不假设固定文件名。
4. 处理启动、路由、构建、预览、SPA fallback 或可见行为时，读取 [运行与验证](references/runtime-and-verification.md)。
5. 按任务追加 React、Vue、请求、Mock、UI、TypeScript 或契约 Skill；不得为了潜在拆分预留微前端生命周期、通信和隔离分支。
6. 只有任务明确包含单体向微前端迁移或跨应用协作时，才同时使用 `$dayu-sec-web-micro-frontend` 并分别说明迁移前后的所有权。

## 架构边界

- 单体应用在一个部署和运行单元内拥有页面路由、用户体验、业务功能、数据访问和局部状态。
- 常规任务收敛在当前应用和 Git 根内；外部系统、部署变更或跨应用协作超出当前边界时先确认契约。
- 本 Skill 不复制通用路径命名、文件路由协议、框架 API、请求实例或 UI 规则。

## 引用入口

- [架构与所有权](references/architecture-and-ownership.md)：应用、Shell、路由、Provider、业务视图和基础设施职责。
- [运行与验证](references/runtime-and-verification.md)：入口发现、导航、构建制品、SPA fallback 和运行验证。

## 输出要求

- 说明应用实际入口、路由、Shell/布局、业务视图和基础设施所有者。
- 指出任务是否保持在单体边界内，以及是否引入新的全局副作用或部署影响。
- 区分静态检查、构建通过和真实运行验证，不用其中一种替代另一种。
