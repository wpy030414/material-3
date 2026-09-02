# MD3 导航模式

用于选择和实现 Material Design 3 导航组件的指南。

## Jetpack Compose（主要）

使用 **`androidx.compose.material3`**：`NavigationBar`、`NavigationRail`、`NavigationDrawerItem`、`ModalNavigationDrawer`、`DismissibleNavigationDrawer`、`PermanentNavigationDrawer`、`NavigationBarItem`、`NavigationRailItem`、顶部应用栏（`TopAppBar`、`CenterAlignedTopAppBar`、`LargeTopAppBar`，以及按 BOM 提供的表现力变体），以及 **`Scaffold`**（`bottomBar`、`floatingActionButton`、`snackbarHost`）。

使用 **Navigation Compose**（`NavHost`、`composable`、`rememberNavController`）连接目标页面。对于**自适应** UI，使用 **`calculateWindowSizeClass`**、**`androidx.compose.material3.adaptive`** 或 **`currentWindowAdaptiveInfo`** / **`NavigableListDetailPaneScaffold`**（名称和包取决于所用的 BOM — 请查阅 [Android Developers](https://developer.android.com/jetpack/androidx/releases/compose-material3)）。

Material 的 [I/O 2026 更新](https://m3.material.io/blog/whats-new-at-io26)增加了表现力/自适应的重点：

- 优先为手机、桌面端、折叠屏、手表和 XR 使用表现力/自适应脚手架，而不是将单一手机导航模式向上扩展。
- 表现力搜索和搜索应用栏具有刷新的视觉风格、动效和更灵活的尾随图标行为。在可用的情况下使用当前的 Compose Material3 API；仅在面向 Web 时使用 web/CSS 近似方案。
- 保持导航间距使用 8dp 间距系统，以便侧栏/抽屉/应用栏间距可以根据设备类别和密度自适应。

```kotlin
// Conceptual — adapt routes and selection to your app
Scaffold(
    bottomBar = {
        NavigationBar {
            destinations.forEach { dest ->
                NavigationBarItem(
                    selected = currentRoute == dest.route,
                    onClick = { navController.navigate(dest.route) },
                    icon = { Icon(dest.icon, contentDescription = dest.label) },
                    label = { Text(dest.label) }
                )
            }
        }
    }
) { innerPadding ->
    NavHost(
        navController = navController,
        startDestination = "home",
        modifier = Modifier.padding(innerPadding)
    ) { /* composable routes */ }
}
```

**Web（有限）：** 下方的 HTML/`@material/web` 部分对于基于 token 的网站仍然有用；[Material Web 仅处于维护状态](https://m3.material.io/develop/web)。

---

## 导航组件选择

### 决策树

```
How many primary destinations?
├── 2 destinations → Tabs (primary)
├── 3–5 destinations
│   ├── Compact screen (<600dp) → Navigation Bar (bottom)
│   ├── Medium screen (600–839dp) → Navigation Rail (side)
│   └── Expanded+ screen (840dp+) → Navigation Drawer (side) or Rail
├── 6+ destinations
│   ├── Compact → Navigation Drawer (modal)
│   ├── Medium → Navigation Drawer (standard) or Rail + overflow menu
│   └── Expanded+ → Navigation Drawer (standard)
└── Hierarchical (nested sections)
    └── Navigation Drawer with sections
```

### 速查表

| 组件 | 目标页面数 | 屏幕尺寸 | 持久性 | 位置 |
|-----------|-------------|-------------|-------------|----------|
| 底部导航栏 | 3–5 | 紧凑 | 持久 | 底部 |
| 导航侧栏 | 3–7 | 中等 | 持久 | 侧边（起始） |
| 导航抽屉 | 无限制 | 展开及以上 | 标准或模态 | 侧边（起始） |
| 标签页 | 2+ 个相关视图 | 任意 | 持久 | 顶部（应用栏下方） |
| 底部应用栏 | —（上下文操作） | 紧凑 | 持久 | 底部 |

## 底部导航栏

**使用场景**：紧凑（手机）屏幕上的 3–5 个主要目标页面。
**位置**：屏幕底部，始终可见。

> **@m3e/react 等价元素：** `M3eNavBar`。推荐优先使用 @m3e/react。

```tsx
// @m3e/react（推荐）
import { M3eNavBar, M3eNavBarItem, M3eIcon } from "@m3e/react/nav-bar";

<M3eNavBar activeIndex={0}>
  <M3eNavBarItem label="Home" activeIcon="home" inactiveIcon="home" />
  <M3eNavBarItem label="Search" activeIcon="search" inactiveIcon="search" />
  <M3eNavBarItem label="Notifications" activeIcon="notifications" inactiveIcon="notifications" />
  <M3eNavBarItem label="Profile" activeIcon="person" inactiveIcon="person" />
</M3eNavBar>
```


### 样式

```css
md-navigation-bar {
  --md-navigation-bar-container-color: var(--md-sys-color-surface-container);
}
```

### 指南
- 始终显示标签（不要仅使用图标）
- 激活状态使用填充图标，未激活状态使用轮廓图标
- 不要在少于 3 个或多于 5 个目标页面时使用
- 在内容密集的屏幕中向下滚动时隐藏（可选）
- 海拔层级 2（3dp）

## 导航侧栏

**使用场景**：中等屏幕（平板）上的 3–7 个主要目标页面。
**位置**：起始边缘（LTR 中为左侧），始终可见。

### 结构
- 宽度：80dp
- 顶部可选 FAB
- 导航项垂直堆叠
- 激活项显示指示器药丸

### 实现

```tsx
// @m3e/react（推荐）
import { M3eNavRail, M3eNavRailItem, M3eFab, M3eIcon } from "@m3e/react/nav-rail";

<M3eNavRail>
  <M3eFab slot="fab" size="small" variant="tertiary" ariaLabel="新建">
    <M3eIcon slot="icon">edit</M3eIcon>
  </M3eFab>
  <M3eNavRailItem label="首页" active activeIcon="home" inactiveIcon="home" />
  <M3eNavRailItem label="搜索" activeIcon="search" inactiveIcon="search" />
  <M3eNavRailItem label="设置" activeIcon="settings" inactiveIcon="settings" />
</M3eNavRail>
```


> **@material/web 回退：** `@material/web` 没有提供导航侧栏组件。对于不使用 `@m3e/react` 的项目，需要自行实现或使用其他库。

### 指南
- 将项目对齐到顶部（在可选 FAB 下方）
- 始终显示标签（可以隐藏，但建议显示）
- 顶部 FAB 可选但常见
- 海拔层级 0

## 导航抽屉

**使用场景**：目标页面众多、展开屏幕或深层级结构。
**位置**：起始边缘，标准（持久）或模态（覆盖层）。

> **@m3e/react 推荐：** 使用 `M3eDrawerContainer`（`import { M3eDrawerContainer } from "@m3e/react/drawer-container"`）。支持响应式滑动抽屉，在不同断点自动切换标准/模态模式。

### 标准抽屉（持久）

与内容并排始终可见。宽度：360dp。

```html
<div class="md3-layout">
  <md-navigation-drawer opened>
    <div slot="headline">App Name</div>
    <md-list>
      <md-list-item type="button" active>
        <md-icon slot="start">inbox</md-icon>
        <div slot="headline">Inbox</div>
        <div slot="trailing-supporting-text">24</div>
      </md-list-item>
      <md-list-item type="button">
        <md-icon slot="start">send</md-icon>
        <div slot="headline">Sent</div>
      </md-list-item>
      <md-divider></md-divider>
      <md-list-item type="button">
        <md-icon slot="start">drafts</md-icon>
        <div slot="headline">Drafts</div>
      </md-list-item>
    </md-list>
  </md-navigation-drawer>
  <main class="md3-content">
    <!-- Page content -->
  </main>
</div>
```

### 模态抽屉（覆盖层）

带有遮罩层覆盖内容。用于较小屏幕或内容空间有限时。

```html
<md-navigation-drawer type="modal" id="nav-drawer">
  <!-- Same content as standard -->
</md-navigation-drawer>

<script>
  // Toggle drawer
  document.getElementById('menu-btn').addEventListener('click', () => {
    const drawer = document.getElementById('nav-drawer');
    drawer.opened = !drawer.opened;
  });
</script>
```

### 指南
- 标准抽屉使用 `surface-container` 背景
- 模态抽屉具有海拔层级 1 和遮罩层覆盖
- 使用分隔线和分区标题对目标页面分组
- 激活项使用 `secondary-container` 背景
- 形状：末端圆角使用 `large`（LTR 中为右边缘）

## 顶部应用栏

**使用场景**：每个页面都需要标题和可选操作。

**I/O 2026 说明：** 搜索应用栏是当前表现力搜索指南的一部分。在 Compose 中，在自行实现之前请检查 Material3 BOM 中的表现力应用栏和搜索 API。对于 Web，将基于 token 的顶部应用栏与自定义搜索字段/视图结合使用，因为 Material Web 未提供完整的表现力搜索对等功能。

### 变体

| 变体 | 标题 | 高度 | 滚动行为 |
|---------|-------|--------|----------------|
| 居中对齐 | 居中 | 64dp | 滚动时提升至层级 2 |
| 小型 | 起始对齐 | 64dp | 滚动时提升至层级 2 |
| 中型 | 底部，起始对齐 | 112dp | 滚动时收缩至 64dp |
| 大型 | 底部，起始对齐 | 152dp | 滚动时收缩至 64dp |

### 实现

```tsx
// @m3e/react（推荐）
import { M3eAppBar, M3eIconButton, M3eIcon } from "@m3e/react/app-bar";

{/* 小型应用栏 */}
<M3eAppBar variant="small">
  <M3eIconButton slot="navigation" ariaLabel="打开菜单">
    <M3eIcon>menu</M3eIcon>
  </M3eIconButton>
  <span slot="title">页面标题</span>
  <M3eIconButton slot="action" ariaLabel="搜索">
    <M3eIcon>search</M3eIcon>
  </M3eIconButton>
  <M3eIconButton slot="action" ariaLabel="更多选项">
    <M3eIcon>more_vert</M3eIcon>
  </M3eIconButton>
</M3eAppBar>

{/* 中型应用栏（可折叠） */}
<M3eAppBar variant="medium">
  <M3eIconButton slot="navigation" ariaLabel="返回">
    <M3eIcon>arrow_back</M3eIcon>
  </M3eIconButton>
  <span slot="title">页面标题</span>
  <M3eIconButton slot="action" ariaLabel="更多">
    <M3eIcon>more_vert</M3eIcon>
  </M3eIconButton>
</M3eAppBar>
```


> **@material/web 回退：** `@material/web` 没有提供应用栏组件。对于不使用 `@m3e/react` 的项目，需要自行实现或使用其他库。

### 滚动行为

```javascript
// 滚动时提升应用栏层级
const appBar = document.querySelector('.md3-app-bar');
window.addEventListener('scroll', () => {
  // M3eAppBar 内部会自动处理滚动状态
  // 如需自定义，可监听 scroll 事件
  if (window.scrollY > 0) {
    appBar.setAttribute('elevated', '');
  } else {
    appBar.removeAttribute('elevated');
  }
});
```

## 标签页

**使用场景**：在同一层级的相关页面之间切换。

> **@m3e/react 等价元素：** `M3eTabs`（`import { M3eTabs } from "@m3e/react/tabs"`）。

### 主要与次要

- **主要标签页**：顶级内容切换（航班 / 酒店 / 探索）
- **次要标签页**：主要内容内的子分区

```html
<!-- Primary tabs -->
<md-tabs>
  <md-primary-tab active>
    <md-icon slot="icon">flight</md-icon>
    Flights
  </md-primary-tab>
  <md-primary-tab>Hotels</md-primary-tab>
  <md-primary-tab>Car Rental</md-primary-tab>
</md-tabs>

<!-- Secondary tabs (nested under primary) -->
<md-tabs>
  <md-secondary-tab active>Overview</md-secondary-tab>
  <md-secondary-tab>Reviews</md-secondary-tab>
  <md-secondary-tab>Photos</md-secondary-tab>
</md-tabs>
```

### 标签页 + 面板关联

```html
<md-tabs id="my-tabs">
  <md-primary-tab id="tab-1" aria-controls="panel-1" active>Tab 1</md-primary-tab>
  <md-primary-tab id="tab-2" aria-controls="panel-2">Tab 2</md-primary-tab>
</md-tabs>

<div id="panel-1" role="tabpanel" aria-labelledby="tab-1">
  Panel 1 content
</div>
<div id="panel-2" role="tabpanel" aria-labelledby="tab-2" hidden>
  Panel 2 content
</div>

<script>
  document.getElementById('my-tabs').addEventListener('change', (e) => {
    // Hide all panels
    document.querySelectorAll('[role="tabpanel"]').forEach(p => p.hidden = true);
    // Show selected panel
    const activeTab = e.target.querySelector('[active]');
    const panelId = activeTab.getAttribute('aria-controls');
    document.getElementById(panelId).hidden = false;
  });
</script>
```

## 响应式导航模式

关键的 MD3 模式：导航组件在不同断点之间转换。

### 手机 → 平板 → 桌面

```
Compact (<600dp):   Navigation Bar (bottom)
Medium (600–839dp): Navigation Rail (side)
Expanded (840dp+):  Navigation Drawer (side, standard)
```

### CSS 实现

```css
/* 使用 @m3e/react 组件的响应式切换：
   M3eNavBar（紧凑）、M3eNavRail（中等）、M3eDrawerContainer（展开+）
   组件自身处理样式，此处仅控制布局可见性 */

.md3-nav-drawer-wrapper,
.md3-nav-rail,
.md3-nav-bar { display: none; }

/* Compact: show bottom navigation bar */
@media (max-width: 599px) {
  .md3-nav-bar { display: flex; }
  .md3-app { flex-direction: column; }
}

/* Medium: show navigation rail */
@media (min-width: 600px) and (max-width: 839px) {
  .md3-nav-rail { display: flex; }
  .md3-app { flex-direction: row; }
}

/* Expanded+: show navigation drawer */
@media (min-width: 840px) {
  .md3-nav-drawer-wrapper { display: flex; }
  .md3-app { flex-direction: row; }
}
```

### 完整响应式外壳（@m3e/react 版）

使用 `@m3e/react` 组件构建完整的响应式应用外壳：

```css
/* 应用外壳基础布局 */
.md3-app {
  display: flex;
  min-height: 100vh;
  background: var(--md-sys-color-surface);
  color: var(--md-sys-color-on-surface);
}

.md3-main {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.md3-body {
  flex: 1;
  padding: 16px;
}

/* 默认隐藏所有导航变体 */
.md3-nav-drawer-wrapper,
.md3-nav-rail-m3e,
.md3-nav-bar-m3e {
  display: none;
}

/* 紧凑屏幕：显示底部导航栏 */
@media (max-width: 599px) {
  .md3-app {
    flex-direction: column;
  }
  .md3-nav-bar-m3e {
    display: flex;
    order: 1;
  }
}

/* 中等屏幕：显示导航侧栏 */
@media (min-width: 600px) and (max-width: 839px) {
  .md3-nav-rail-m3e {
    display: flex;
  }
}

/* 展开+屏幕：显示导航抽屉 */
@media (min-width: 840px) {
  .md3-nav-drawer-wrapper {
    display: flex;
  }
}
```

## @m3e/react 导航组件（推荐）

`@m3e/react` 提供了比 `@material/web` 更完整的导航组件集合，真正支持 Material 3 Expressive。

### 导航组件对照

| 组件 | @m3e/react（推荐） | @material/web | 说明 |
|------|----------|---------------|------|
| 底部导航栏 | `M3eNavBar` | `md-navigation-bar` | 两者都有 |
| 导航侧栏 | `M3eNavRail` | ❌ 无 | **@m3e 独有** |
| 导航抽屉 | `M3eDrawerContainer` | `md-navigation-drawer` | @m3e 支持响应式滑动抽屉 |
| 导航菜单 | `M3eNavMenu` | ❌ 无 | **@m3e 独有**，层级导航 |
| 应用栏 | `M3eAppBar` | ❌ 无 | **@m3e 独有** |
| 工具栏 | `M3eToolbar` | ❌ 无 | **@m3e 独有** |
| 面包屑 | `M3eBreadcrumb` | ❌ 无 | **@m3e 独有** |

### 导航侧栏（@m3e/react 独有）

```tsx
// @m3e/react（推荐）
import { M3eNavRail, M3eNavRailItem, M3eIcon } from "@m3e/react/nav-rail";

<M3eNavRail>
  <M3eNavRailItem label="首页" active activeIcon="home" inactiveIcon="home" />
  <M3eNavRailItem label="搜索" activeIcon="search" inactiveIcon="search" />
  <M3eNavRailItem label="设置" activeIcon="settings" inactiveIcon="settings" />
</M3eNavRail>
```

### 应用栏（@m3e/react 独有）

```tsx
// @m3e/react（推荐）
import { M3eAppBar, M3eIconButton, M3eIcon } from "@m3e/react/app-bar";

<M3eAppBar variant="small">
  <M3eIconButton slot="navigation" ariaLabel="菜单">
    <M3eIcon>menu</M3eIcon>
  </M3eIconButton>
  <span slot="title">页面标题</span>
  <M3eIconButton slot="action" ariaLabel="搜索">
    <M3eIcon>search</M3eIcon>
  </M3eIconButton>
</M3eAppBar>
```

### 响应式导航（@m3e/react 版）

使用 `@m3e/react` 组件实现跨断点的自适应导航：

```tsx
// @m3e/react（推荐）
import { M3eDrawerContainer, M3eNavMenu, M3eNavMenuItem, M3eIcon } from "@m3e/react";
import { M3eNavRail, M3eNavRailItem } from "@m3e/react/nav-rail";
import { M3eNavBar, M3eNavBarItem } from "@m3e/react/nav-bar";
import { M3eAppBar, M3eIconButton } from "@m3e/react/app-bar";

<div className="md3-app">
  {/* 导航抽屉（展开+） */}
  <aside className="md3-nav-drawer-wrapper">
    <M3eDrawerContainer>
      <M3eNavMenu>
        <M3eNavMenuItem label="首页" active>
          <M3eIcon slot="start">home</M3eIcon>
        </M3eNavMenuItem>
        <M3eNavMenuItem label="搜索">
          <M3eIcon slot="start">search</M3eIcon>
        </M3eNavMenuItem>
        <M3eNavMenuItem label="设置">
          <M3eIcon slot="start">settings</M3eIcon>
        </M3eNavMenuItem>
      </M3eNavMenu>
    </M3eDrawerContainer>
  </aside>

  {/* 导航侧栏（中等） */}
  <M3eNavRail className="md3-nav-rail-m3e" />

  {/* 主内容 */}
  <main className="md3-main">
    <M3eAppBar>
      <span slot="title">首页</span>
    </M3eAppBar>
    <div className="md3-body">{/* 页面内容 */}</div>
  </main>

  {/* 底部导航栏（紧凑） */}
  <M3eNavBar className="md3-nav-bar-m3e" />
</div>
```

```css
/* 使用 @m3e/react 的响应式导航切换 */
.md3-nav-drawer-wrapper,
.md3-nav-rail,
.md3-nav-bar { display: none; }

@media (max-width: 599px) {
  .md3-nav-bar { display: flex; }
}
@media (min-width: 600px) and (max-width: 839px) {
  .md3-nav-rail { display: flex; }
}
@media (min-width: 840px) {
  .md3-nav-drawer-wrapper { display: flex; }
}
```
