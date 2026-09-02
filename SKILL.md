---
name: material-3
description: >
  实现 Google Material Design 3 (Material You) Web UI 系统。
  涵盖 MD3 设计令牌（CSS 自定义属性）、推荐实现库 @m3e/react（React）（真正支持
  Material 3 Expressive），以及主题、深色模式、布局等。当涉及：
  "material design"、"MD3"、"material you"、"material component"、
  "md3 button"、"@m3e/react"、"--md-sys-color" 时使用本技能。
---

## 概览

本技能指导在 **Web** 上实现 Google Material Design 3 (MD3) — 一个个性化、自适应、表现力强的设计系统。MD3 使用动态配色、色调表面、圆角形状和弹簧物理动效，让 UI 充满活力且富有个性。React 项目推荐使用 **`@m3e/react`**，通过 **CSS 自定义属性**（`--md-sys-*`）实现完整 M3E 支持。`@m3e/web` 是 `@m3e/react` 的底层 Web Components 层，非 React 框架（Vue/Svelte/Vanilla）可直接使用。

**与 MD2 的关键区别：**
- 色调表面取代层阶阴影，成为主要的深度提示
- 动态配色可从单一主色生成完整配色方案
- 默认采用全圆角
- 弹簧物理动效（Expressive）— @m3e/react 原生支持
- 3 级用户可控对比度（标准/中等/高）

**实现库选择指南：**

| 库 | 推荐度 | 说明 |
|---|---|---|
| **@m3e/react** | ⭐ 强烈推荐 | React 绑定，完整 TypeScript 支持，真正支持 Material 3 Expressive |
| **@m3e/web** | — | @m3e/react 的底层实现，Vue/Svelte/Vanilla 可直接使用 |
| **@material/web** | ⚠️ 维护模式 | Google 官方但已进入维护模式，不支持 Expressive 特性 |
| **MUI / MUI X** | ❌ 不推荐 | 未实现 M3E（Material 3 Expressive），仅部分 MD3 令牌支持 |

**与 frontend-design 技能的关系：**
当两个技能同时激活时，MD3 提供设计系统（令牌、组件、布局规则），frontend-design 在这些约束内提供创意指导。MD3 规则在组件结构和令牌使用上优先。注意：**Roboto / Roboto Flex 是 MD3 Web 的正确默认字体** — frontend-design 中避免使用 Roboto 的建议在实现 MD3 时不适用。

## 决策树

**你要构建什么？**
```
完整应用脚手架     → 参见"通用模式：应用外壳" + references/layout-and-responsive.md
单个组件           → 参见"组件速查表" → references/component-catalog.md
自定义主题         → 参见 references/theming-and-dynamic-color.md
表单/输入布局      → 参见 references/component-catalog.md § 输入组件
导航结构           → 参见 references/navigation-patterns.md
数据展示           → 参见 references/component-catalog.md § 数据展示
```

**选择什么实现方案？**
```
React + @m3e/react（推荐）  → React 绑定 + 完整 TypeScript 支持 + Expressive 完整支持
@material/web              → 单独导入 + CSS 自定义属性（维护模式，无 Expressive）
```

## 设计令牌系统

所有 MD3 令牌使用 `md.sys` 命名空间，在 Web 上以 **CSS 自定义属性**（`--md-sys-*`）暴露。通过 [Material Theme Builder](https://m3.material.io/theme-builder) 或 `@material/material-color-utilities` 生成色彩值。

### 色彩令牌（`--md-sys-color-*`）

| 令牌 | 用途 |
|------|------|
| `primary` | 高强调填充、文本、图标（在表面之上） |
| `on-primary` | primary 上的文本/图标 |
| `primary-container` | 关键组件的突出填充（FAB 等） |
| `on-primary-container` | primary-container 上的文本/图标 |
| `secondary` / `on-secondary` | 较不突出的强调色 |
| `secondary-container` / `on-secondary-container` | 退让型组件（色调按钮） |
| `tertiary` / `on-tertiary` | 对比强调色 |
| `tertiary-container` / `on-tertiary-container` | 补充容器 |
| `error` / `on-error` | 错误状态（静态 — 不随动态配色改变） |
| `error-container` / `on-error-container` | 错误容器填充 |
| `surface` | 默认背景色 |
| `on-surface` | 任何表面上的文本/图标 |
| `on-surface-variant` | 表面上较低强调的文本/图标 |
| `surface-container-lowest` | 最低强调容器 |
| `surface-container-low` | 低强调容器 |
| `surface-container` | 默认容器（导航区域） |
| `surface-container-high` | 高强调容器 |
| `surface-container-highest` | 最高强调容器 |
| `surface-dim` / `surface-bright` | 在亮/暗模式下保持相对亮度 |
| `inverse-surface` / `inverse-on-surface` / `inverse-primary` | 对比元素（提示条） |
| `outline` | 重要边界（文本输入框边框） |
| `outline-variant` | 装饰元素（分隔线） |

完整详情：`references/color-system.md`

### 字体令牌（`--md-sys-typescale-*`）

| 缩放 | 尺寸 | 用途 |
|------|------|------|
| Display | L / M / S | 英雄文本、大数字 |
| Headline | L / M / S | 章节标题 |
| Title | L / M / S | 较小标题、卡片标题 |
| Body | L / M / S | 段落文本、描述 |
| Label | L / M / S | 按钮、标签、注释 |

每个样式都有以下令牌：`-font`、`-weight`、`-size`、`-line-height`、`-tracking`

另外还有 15 个 **强调** 变体（更高字重）通过 `--md-sys-typescale-emphasized-*` 提供

完整详情：`references/typography-and-shape.md`

### 形状令牌（`--md-sys-shape-corner-*`）

| 令牌 | 值 | 示例组件 |
|------|-----|---------|
| `none` | 0dp | — |
| `extra-small` | 4dp | 标签、提示条 |
| `small` | 8dp | 文本输入框、菜单 |
| `medium` | 12dp | 卡片 |
| `large` | 16dp | FAB、导航抽屉 |
| `large-increased` | 20dp | (Expressive) |
| `extra-large` | 28dp | 对话框、底部表单 |
| `extra-large-increased` | 32dp | (Expressive) |
| `extra-extra-large` | 48dp | (Expressive) |
| `full` | 9999px | 按钮、标签、徽章 |

### 层阶级别

| 级别 | DP | 色调偏移 | 用途 |
|------|-----|---------|------|
| 0 | 0dp | 无 | 平面、大多数静止组件 |
| 1 | 1dp | +5% primary | 凸起卡片、模态表单 |
| 2 | 3dp | +8% primary | 菜单、导航栏、滚动后的应用栏 |
| 3 | 6dp | +11% primary | FAB、对话框、搜索、日期/时间选择器 |
| 4 | 8dp | +12% primary | （仅悬停/聚焦增加） |
| 5 | 12dp | +14% primary | （仅悬停/聚焦增加） |

MD3 的层阶通过 **色调表面颜色** 而非阴影来传达。仅在需要针对繁忙背景提供额外保护时才使用阴影。

### 动效

MD3 Expressive（2025 年 5 月）引入了弹簧物理动效 — **`@m3e/react` 原生支持**，而 `@material/web` 不支持。如果使用 `@material/web` 或纯 CSS，继续使用缓动/持续时间系统（进入/退出/共享轴）：

| 缓动 | 持续时间 | 过渡类型 |
|------|---------|---------|
| 强调 | 500ms | 进入并停留在屏幕上 |
| 强调减速 | 400ms | 进入屏幕 |
| 强调加速 | 200ms | 退出屏幕 |
| 标准 | 300ms | 进入并停留在屏幕上（实用） |
| 标准减速 | 250ms | 进入屏幕（实用） |
| 标准加速 | 200ms | 退出屏幕（实用） |

CSS 缓动值：
- 强调：`cubic-bezier(0.2, 0, 0, 1)`
- 强调减速：`cubic-bezier(0.05, 0.7, 0.1, 1)`
- 强调加速：`cubic-bezier(0.3, 0, 0.8, 0.15)`
- 标准：`cubic-bezier(0.2, 0, 0, 1)`
- 标准减速：`cubic-bezier(0, 0, 0, 1)`
- 标准加速：`cubic-bezier(0.3, 0, 1, 1)`

## 组件速查表

| 组件 | @m3e/react 组件 | @material/web 元素 | 主要变体 | 类别 |
|------|-----------------|-------------------|---------|------|
| 按钮 | `M3eButton` | `md-filled-button` 等 | Filled, Outlined, Text, Elevated, Tonal; 5 种尺寸 (XS–XL); 切换 | 操作 |
| 按钮组 | `M3eButtonGroup` | `md-button-group` | 标准、连接 | 操作 |
| 扩展 FAB | `M3eFab` (extended) | `md-extended-fab` | Surface, Primary, Secondary, Tertiary | 操作 |
| FAB | `M3eFab` | `md-fab` | Small, Medium, Large | 操作 |
| FAB 菜单 | `M3eFabMenu` | ❌ 无 | 带菜单的 FAB | 操作 |
| 图标按钮 | `M3eIconButton` | `md-icon-button` 等 | Standard, Filled, Filled Tonal, Outlined | 操作 |
| 进度指示器 | `M3eProgressIndicator` | `md-linear-progress`, `md-circular-progress` | 线性、环形；确定/不确定 | 通信 |
| 加载指示器 | `M3eLoadingIndicator` | ❌ 无 | 包含/未包含 | 通信 |
| 分隔线 | `M3eDivider` | `md-divider` | 全宽、内缩 | 容器 |
| 对话框 | `M3eDialog` | `md-dialog` | 基础、全屏 | 容器 |
| 卡片 | `M3eCard` | ❌ 无 | Filled, Outlined, Elevated | 容器 |
| 底部表单 | `M3eBottomSheet` | ❌ 无 | 标准、模态 | 容器 |
| 侧边表单 | ❌ 无 | ❌ 无 | 标准、模态 | 容器 |
| 复选框 | `M3eCheckbox` | `md-checkbox` | — | 输入 |
| 标签 | `M3eChips` | `md-chip-set` 等 | Assist, Filter, Input, Suggestion | 输入 |
| 菜单 | `M3eMenu` | `md-menu`, `md-menu-item` | — | 输入 |
| 单选按钮 | `M3eRadioGroup` | `md-radio` | — | 输入 |
| 滑块 | `M3eSlider` | `md-slider` | 连续、离散、范围 | 输入 |
| 开关 | `M3eSwitch` | `md-switch` | 带/不带图标 | 输入 |
| 文本输入框 | `M3eTextField` | `md-filled-text-field`, `md-outlined-text-field` | Filled, Outlined | 输入 |
| 日期选择器 | `M3eDatepicker` | ❌ 无 | 停靠、模态、范围 | 输入 |
| 时间选择器 | `M3eTimepicker` | ❌ 无 | 停靠、模态 | 输入 |
| 搜索 | `M3eSearch` | ❌ 无 | 搜索栏、搜索视图 | 输入 |
| 自动完成 | `M3eAutocomplete` | ❌ 无 | — | 输入 |
| 选择器 | `M3eSelect` | ❌ 无 | 单选、多选 | 输入 |
| 分段按钮 | `M3eSegmentedButton` | ❌ 无 | 单选、多选 | 输入 |
| 表单字段 | `M3eFormField` | ❌ 无 | 容器 | 输入 |
| 日期输入 | `M3eDateInput` | ❌ 无 | 分段日期输入 | 输入 |
| 导航栏 | `M3eNavBar` | `md-navigation-bar` | — | 导航 |
| 导航侧栏 | `M3eNavRail` | ❌ 无 | 紧凑、展开 | 导航 |
| 导航抽屉 | `M3eDrawerContainer` | `md-navigation-drawer` | 标准、模态 | 导航 |
| 导航菜单 | `M3eNavMenu` | ❌ 无 | 层级导航 | 导航 |
| 标签页 | `M3eTabs` | `md-tabs`, `md-primary-tab`, `md-secondary-tab` | 主要、次要 | 导航 |
| 面包屑 | `M3eBreadcrumb` | ❌ 无 | — | 导航 |
| 列表 | `M3eList` | `md-list`, `md-list-item` | 单行、双行、三行 | 数据展示 |
| 头像 | `M3eAvatar` | ❌ 无 | 图片、图标、文本 | 数据展示 |
| 徽章 | `M3eBadge` | ❌ 无 | 点、数字 | 数据展示 |
| 应用栏 | `M3eAppBar` | ❌ 无 | 小、中、大、居中 | 导航 |
| 工具栏 | `M3eToolbar` | ❌ 无 | 上下文操作 | 导航 |
| 分页器 | `M3ePaginator` | ❌ 无 | — | 数据展示 |
| 步骤器 | `M3eStepper` | ❌ 无 | 向导式流程 | 数据展示 |
| 树形视图 | `M3eTree` | ❌ 无 | 层级数据 | 数据展示 |
| 目录 | `M3eToc` | ❌ 无 | 页内导航 | 数据展示 |
| 提示条 | `M3eSnackbar` | ❌ 无 | — | 通信 |
| 工具提示 | `M3eTooltip` | ❌ 无 | 简单、丰富 | 通信 |
| 骨架屏 | `M3eSkeleton` | ❌ 无 | 加载占位 | 通信 |
| 分割面板 | `M3eSplitPane` | ❌ 无 | 可拖拽分隔 | 容器 |
| 展开面板 | `M3eExpansionPanel` | ❌ 无 | 可折叠区块 | 容器 |
| 轮播 | `M3eSlideGroup` | ❌ 无 | 多浏览、未包含、英雄 | 容器 |
| 内容面板 | `M3eContentPane` | ❌ 无 | 可滚动表面 | 容器 |
| 形状 | `M3eShape` | ❌ 无 | 抽象形状、形状变换 | 通信 |
| 标题 | `M3eHeading` | ❌ 无 | Display, Headline, Title, Label | 数据展示 |
| 图标 | `M3eIcon` | `md-icon` | Material Symbols | 通信 |
| 主题 | `M3eTheme` | ❌ 无 | 动态配色、动效、密度 | 主题 |

> @m3e/web 提供等效的 Web Components（`m3e-button`、`m3e-card` 等），适用于 Vue/Svelte/Vanilla JS 项目。

**说明：** `@m3e/react` 组件使用 `M3e` 前缀（如 `M3eButton`、`M3eCard`），提供了 45+ 个组件，完整覆盖 Material 3 Expressive。@material/web 仅有约 20 个组件且处于维护模式，缺少 Card、Badge、Tooltip、Snackbar、Bottom Sheet、Search、Navigation Rail、Date/Time Picker、App Bar、FAB Menu、Split Button、Loading Indicator 等重要组件。

完整组件详情及代码示例：`references/component-catalog.md`

## @m3e/react（推荐）

**`@m3e/react`** 是 React 项目的**首选推荐**，提供符合习惯的、类型化的 React 绑定，真正实现 [Material Design 3 Expressive](https://m3.material.io/)，通过 CSS 自定义属性（`--md-sys-*`）实现主题定制。

**展示与文档：** <https://matraic.github.io/m3e>

### 特性

- **表现力组件：** 利用 Material 3 的设计令牌、动态配色、形状和动效系统实现高度可定制的 UI 元素
- **统一架构：** ESM 优先，模块化入口点，支持 tree-shaking 以最小化包体积
- **无障碍优先：** 内建无障碍标准支持，确保所有用户都能获得包容性体验
- **主题与个性化：** 通过 Material 3 的高级主题功能，轻松将组件适配到品牌或用户偏好
- **性能优化：** 轻量、快速加载的组件，专为现代 Web 平台设计
- **安全意识：** 采用安全默认模式，包括 XSS 安全模板、Trusted Types 兼容性，以及对强内容安全策略 (CSP) 的支持以强制安全的 DOM 交互并缓解注入风险

### 安装

```bash
pnpm install @m3e/react
```

### 基本用法

`@m3e/react` 组件使用 `M3e` 前缀（如 `M3eButton`、`M3eCard`、`M3eNavBar`）。

```tsx
"use client";

import { M3eButton } from "@m3e/react/button";
import { M3eCard } from "@m3e/react/card";
import { M3eNavBar } from "@m3e/react/nav-bar";
import { M3eTextField } from "@m3e/react/text-field";

export default function MyApp() {
  return (
    <>
      <M3eButton variant="filled" onClick={() => console.log("点击了！")}>
        点击我
      </M3eButton>
      <M3eTextField label="邮箱" type="email" variant="outlined" />
      <M3eCard variant="outlined">
        <div slot="content">React 卡片内容</div>
      </M3eCard>
    </>
  );
}
```

**注意：** React 绑定仅支持客户端。在 Next.js 中，**必须** 在 `"use client"` 边界内使用。

### 主题与 CSS 自定义属性

通过在 `:root` 或任何祖先元素上设置 CSS 自定义属性来应用自定义主题：

```css
:root {
  /* 色彩方案（使用 @material/material-color-utilities 生成） */
  --md-sys-color-primary: #6750A4;
  --md-sys-color-on-primary: #FFFFFF;
  --md-sys-color-primary-container: #EADDFF;
  --md-sys-color-on-primary-container: #21005D;
  --md-sys-color-secondary: #625B71;
  --md-sys-color-on-secondary: #FFFFFF;
  --md-sys-color-secondary-container: #E8DEF8;
  --md-sys-color-on-secondary-container: #1D192B;
  --md-sys-color-surface: #FEF7FF;
  --md-sys-color-on-surface: #1D1B20;
  --md-sys-color-surface-container: #F3EDF7;
  --md-sys-color-outline: #79747E;
  --md-sys-color-outline-variant: #CAC4D0;

  /* 字体排版 */
  --md-sys-typescale-body-large-font: 'Roboto Flex', sans-serif;
  --md-sys-typescale-body-large-size: 1rem;
  --md-sys-typescale-body-large-weight: 400;
  --md-sys-typescale-body-large-line-height: 1.5rem;

  /* 形状 */
  --md-sys-shape-corner-full: 9999px;
  --md-sys-shape-corner-medium: 12px;
}
```

### 深色主题

通过类或媒体查询覆盖色彩令牌来应用深色主题：

```css
@media (prefers-color-scheme: dark) {
  :root {
    --md-sys-color-primary: #D0BCFF;
    --md-sys-color-on-primary: #381E72;
    --md-sys-color-primary-container: #4F378B;
    --md-sys-color-on-primary-container: #EADDFF;
    --md-sys-color-surface: #141218;
    --md-sys-color-on-surface: #E6E0E9;
    --md-sys-color-surface-container: #211F26;
    --md-sys-color-outline: #938F99;
    --md-sys-color-outline-variant: #49454F;
  }
}
```

完整主题指南：`references/theming-and-dynamic-color.md`

## 通用模式

### 应用外壳

标准 MD3 应用，使用 `@m3e/react` 组件构建响应式导航 + 顶部应用栏 + 内容区域。组件自身样式由组件库处理，这里只需关注结构布局 CSS：

```tsx
"use client";

import { M3eNavRail } from "@m3e/react/nav-rail";
import { M3eFab } from "@m3e/react/fab";
import { M3eNavBar, M3eNavBarItem } from "@m3e/react/nav-bar";
import { M3eIcon } from "@m3e/react/icon";
import { M3eAppBar } from "@m3e/react/app-bar";

export default function AppShell({ children }: { children: React.ReactNode }) {
  return (
    <div className="md3-app">
      <M3eNavRail aria-label="主导航">
        <M3eFab size="small" aria-label="撰写">
          <M3eIcon slot="icon">edit</M3eIcon>
        </M3eFab>
        <M3eNavBar>
          <M3eNavBarItem label="首页">
            <M3eIcon slot="active-icon">home</M3eIcon>
            <M3eIcon slot="inactive-icon">home</M3eIcon>
          </M3eNavBarItem>
          <M3eNavBarItem label="搜索">
            <M3eIcon slot="active-icon">search</M3eIcon>
            <M3eIcon slot="inactive-icon">search</M3eIcon>
          </M3eNavBarItem>
        </M3eNavBar>
      </M3eNavRail>
      <main className="md3-content">
        <M3eAppBar headline="页面标题" />
        <div className="md3-body">{children}</div>
      </main>
    </div>
  );
}
```

```css
/* 外壳结构布局 — 组件自身样式由 @m3e/react 处理 */
.md3-app {
  display: flex;
  min-height: 100vh;
  background: var(--md-sys-color-surface);
  color: var(--md-sys-color-on-surface);
}

.md3-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.md3-body {
  padding: 24px;
  flex: 1;
}

/* 响应式：紧凑屏幕 (<600dp) 切换到底部导航栏 */
@media (max-width: 599px) {
  .md3-app { flex-direction: column; }
}
```

**响应式导航模式：** 紧凑屏幕用 `M3eNavBar`，中等屏幕用 `M3eNavRail`，扩展屏幕 (840dp+) 用 `M3eDrawerContainer`。详细实现与断点切换逻辑：`references/navigation-patterns.md`

### 卡片网格

使用 `M3eCard` 组件 — 卡片自身的样式（圆角、填充、层阶）由组件处理，布局层面只需关注网格排列：

```tsx
"use client";

import { M3eCard } from "@m3e/react/card";
import { M3eButton } from "@m3e/react/button";

export function CardGrid() {
  return (
    <div className="md3-card-grid">
      <M3eCard variant="outlined">
        <img src="image.jpg" alt="描述" slot="media" style={{width:'100%', aspectRatio:'16/9', objectFit:'cover'}} />
        <div slot="content">
          <h3 style={{font: 'var(--md-sys-typescale-title-medium)'}}>卡片标题</h3>
          <p style={{font: 'var(--md-sys-typescale-body-medium)', color: 'var(--md-sys-color-on-surface-variant)'}}>
            此卡片的支持文本。
          </p>
        </div>
        <div slot="actions">
          <M3eButton variant="text">了解更多</M3eButton>
          <M3eButton variant="tonal">操作</M3eButton>
        </div>
      </M3eCard>
    </div>
  );
}
```

```css
/* 网格布局 — 卡片样式由 M3eCard 组件处理 */
.md3-card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 16px;
}
```

**最佳实践：** 卡片网格的最小列宽建议 280–320dp，`gap` 使用 8dp 的倍数。更多卡片变体（Filled / Elevated）与媒体卡片用法：`references/component-catalog.md`

### 表单布局

使用 `M3eTextField` / `M3eButton` — 组件样式由组件库处理，布局层面关注表单结构和间距：

```tsx
"use client";

import { M3eTextField } from "@m3e/react/text-field";
import { M3eButton } from "@m3e/react/button";

export function ContactForm() {
  return (
    <form className="md3-form">
      <M3eTextField label="全名" variant="outlined" required />
      <M3eTextField label="邮箱" type="email" variant="outlined" required />
      <M3eTextField label="消息" type="textarea" variant="outlined" rows={4} />
      <div className="md3-form__actions">
        <M3eButton variant="text" type="reset">取消</M3eButton>
        <M3eButton variant="filled" type="submit">提交</M3eButton>
      </div>
    </form>
  );
}
```

```css
/* 表单布局 — 输入框样式由 M3eTextField 组件处理 */
.md3-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
  max-width: 560px;
}

.md3-form__actions {
  display: flex;
  gap: 8px;
  justify-content: flex-end;
  margin-top: 8px;
}
```

**最佳实践：** 表单 `max-width` 建议 400–600dp 以保持可读性，`gap` 使用 16dp 的倍数。更多输入组件（选择器、日期选择器、自动完成）用法：`references/component-catalog.md`

更多模式：`references/navigation-patterns.md`、`references/layout-and-responsive.md`

## 反模式

**在 Web 上实现 MD3 时绝不要做这些：**

- **混用 MD2 和 MD3 库**：不要同时使用 `@material/mdc-*` (MD2) 和 `@material/web` (MD3) 或 `@m3e/react` (MD3)。它们的 API 和样式不兼容。
- **硬编码颜色**：始终使用 `var(--md-sys-color-*)` 令牌，永远不要使用原始 hex/rgb 值。硬编码颜色会破坏动态主题、深色模式和对比度调整。
- **忽略色调配对**：仅按预期配对组合颜色（例如 `primary` + `on-primary`、`surface-container` + `on-surface`）。任意配对会破坏动态配色和高对比度模式下的对比度。
- **使用 `outline` 做分隔线**：使用 `outline-variant` 做分隔线。`outline` 用于重要边界，如文本输入框边框。
- **导入整个 @m3e/react 或 @material/web**：始终导入单独的组件模块。桶导入会包含每个组件并破坏包体积。
- **直接使用 `border-radius`**：使用形状令牌（`var(--md-sys-shape-corner-medium)`），这样形状与主题保持一致。
- **默认使用阴影做层阶**：MD3 通过色调表面颜色传达层阶，而非阴影。仅在元素需要与繁忙背景额外分离时才添加阴影。
- **应用 frontend-design 的"避免 Roboto"规则**：在 Web 上，Roboto 或 Roboto Flex 是默认的 MD3 字体。仅在有意定制字号缩放时才替换。
- **假设 SSR 兼容性**：`@m3e/react` 和 `@material/web` 底层使用 Web Components（自定义元素），需要 JavaScript 才能渲染。如果没有额外的 hydration 策略，它们在 SSR 中不会产生有意义的 HTML。
- **忽略大屏幕**：MD3 为所有屏幕尺寸设计。不要发布仅限手机的布局 — 在 600dp+ 使用多面板，并在大 (1200dp+) 和超大 (1600dp+) 窗口上将内容限制在最大宽度 (840–1040dp)。无限宽度的文本行不可读。
- **使用 MUI、MWC 或其他非 M3E 库**：MUI 未实现 Material 3 Expressive（缺少弹簧动效、形状变换、完整色彩令牌）；MWC（`@material/web`）已进入维护模式且组件覆盖不完整；`@material/mdc-*` 是过时的 MD2。**始终使用 `@m3e/react` 获得完整、最新的 M3E 支持。**

## MD3 合规性审查

当以 `audit` 作为参数调用时（例如 `/material-3 audit`），或当被要求审查/评审 MD3 合规性时，分析目标页面并生成合规性报告。

### 审查流程

1. **识别目标**：用户提供 URL（使用浏览器工具检查）、文件路径（读取源代码）或正在运行的应用。
2. **检查以下类别** 并为每个类别评分 0–10：

| 类别 | 检查内容 |
|------|---------|
| **色彩令牌** | `--md-sys-color-*` / 生成的 CSS。正确的色调配对（`onX` 在 `X` 上）。深色主题。 |
| **字体排版** | MD3 字号令牌；正确的角色（Display, Headline, Title, Body, Label）。 |
| **形状** | `var(--md-sys-shape-*)`。按钮：full；卡片：medium；避免魔法数字。 |
| **层阶** | 色调层阶；相关处的悬停/聚焦。 |
| **组件** | 优先使用 `@m3e/react`（React，推荐）；`@material/web` 仅限维护模式回退；或规范对齐的 HTML/CSS。正确的变体。 |
| **布局** | 标准布局；大宽度下的可读最大宽度。 |
| **导航** | 栏/侧栏/抽屉模式按尺寸类别。 |
| **动效** | CSS 动效令牌（缓动/持续时间）作为回退，或 @m3e/react 的弹簧动效。 |
| **无障碍** | 验证对比度：UI 组件通常需要 **3:1** 用于大文本/边框，**4.5:1** 用于正常文本 (WCAG 2.x)。ARIA、键盘导航、触摸目标 (~48dp)。 |
| **主题** | `:root` 或子树上的 CSS 自定义属性。 |

3. **生成报告**：

```
# MD3 合规性审查报告

目标：[URL 或文件路径]
日期：[日期]
总分：[X/100]

## 按类别评分
| 类别       | 分数 | 状态 |
|------------|------|------|
| 色彩令牌   | X/10 | [通过/警告/失败] |
| 字体排版   | X/10 | [通过/警告/失败] |
| 形状       | X/10 | [通过/警告/失败] |
| 层阶       | X/10 | [通过/警告/失败] |
| 组件       | X/10 | [通过/警告/失败] |
| 布局       | X/10 | [通过/警告/失败] |
| 导航       | X/10 | [通过/警告/失败] |
| 动效       | X/10 | [通过/警告/失败] |
| 无障碍     | X/10 | [通过/警告/失败] |
| 主题       | X/10 | [通过/警告/失败] |

## 关键问题
[列出评分 0-3 的项目，带具体 file:line 引用和修复方案]

## 警告
[列出评分 4-6 的项目，带建议]

## 通过
[列出评分 7-10 的项目，说明做得好的地方]

## 推荐修复（按优先级排序）
1. [最有影响力的修复优先]
2. ...
```

### 审查方法

**对于实时 URL**（浏览器或开发工具）：
- 检查计算样式和 CSS 变量（`--md-sys-*`）
- 调整视口大小或使用响应模式测试断点
- 如有帮助，在关键宽度截图

**对于源代码**（提供的文件路径）：
- HTML/JSX/Vue/Svelte；CSS/SCSS 令牌
- 检查导入是否使用 `@m3e/react` vs `@material/web` vs `@material/mdc-*` (MD2)

**快速检查**（根据栈调整路径）：
```
# 推荐：确认使用了 @m3e/react
grep -rn '@m3e/react' --include='*.js' --include='*.ts' --include='*.tsx'

# Web：硬编码颜色
grep -rn '#[0-9a-fA-F]\{3,8\}' --include='*.css' --include='*.scss'

# MD2 on web（不推荐）
grep -rn '@material/mdc-' --include='*.js' --include='*.ts'

# MUI（不推荐）
grep -rn '@mui/material' --include='*.js' --include='*.ts' --include='*.tsx'

# @material/web（维护模式，建议迁移到 @m3e/react）
grep -rn '@material/web' --include='*.js' --include='*.ts' --include='*.tsx'
```

**浏览器自动化**（如果你的环境暴露 MCP 浏览器工具）：导航、快照 DOM/CSS 变量、调整大小以测试断点 — 可选，不要求。

### 评分指南

- **9-10**：完全符合 MD3，使用正确的令牌和模式
- **7-8**：基本符合，小问题（例如几个硬编码值）
- **4-6**：部分符合，一些 MD3 模式但有显著缺口
- **1-3**：主要违规，主要是非 MD3 或 MD2 模式
- **0**：不适用或完全缺失

状态阈值：**通过** (7+)、**警告** (4-6)、**失败** (0-3)

## @material/web（维护模式）

`@material/web` 是 Google 官方的 Material 3 Web Components 库，但已进入维护模式（2025 年初）。

**现状：**
- 不再开发新特性
- 仅修复关键 bug
- 不支持 Material 3 Expressive（弹簧动效、形状变换、扩展圆角等）
- 约 20 个组件（远少于 @m3e/react 的 45+）

**基本用法（仅限维护现有项目）：**
```javascript
import '@material/web/button/filled-button.js';
import '@material/web/textfield/outlined-text-field.js';
```

**迁移建议：** 新项目应直接使用 `@m3e/react` 获得完整 Material 3 Expressive 支持。现有 @material/web 项目可参考上方 @m3e/react 章节（两者共享 `--md-sys-*` CSS 自定义属性），逐步迁移。

## 参考文档

- `references/color-system.md` — 色彩角色、色调调色板、动态配色、CSS 映射
- `references/typography-and-shape.md` — 字号缩放、形状圆角、层阶、动效、Expressive 笔记
- `references/component-catalog.md` — 组件：`@m3e/react` 和 `@material/web`（如适用），CSS 回退
- `references/navigation-patterns.md` — 导航选择、自适应模式
- `references/layout-and-responsive.md` — 断点、标准布局
- `references/theming-and-dynamic-color.md` — 主题：CSS 自定义属性和动态配色
