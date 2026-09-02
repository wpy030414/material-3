# MD3 布局与响应式设计

Material Design 3 布局系统参考：断点、标准布局与响应式实现。

## Jetpack Compose 与 Android

下方的**断点表**是**设计**参考。在 **Jetpack Compose** 中，优先使用 **`calculateWindowSizeClass`**（`androidx.compose.material3:material3-window-size-class`）和/或 **`androidx.compose.material3.adaptive`** API（例如 `currentWindowAdaptiveInfo`、列表-详情脚手架），而不是到处手写 `BoxWithConstraints` 的宽度检查。

**全屏延伸（Edge-to-edge）：** 当你在系统栏后方绘制内容时，在 `Activity`（AndroidX）上使用 **`enableEdgeToEdge()`**。应用 **`WindowInsets`**（`Modifier.statusBarsPadding()`、`navigationBarsPadding()`、**`imePadding()`**、`displayCutoutPadding()` 等）以及 **`Scaffold`** 的 `contentWindowInsets`，以确保内容和 **IME** 正确表现。

**折叠屏：** 使用 **`WindowInfoTracker`**、**`FoldingFeature`** 或 Jetpack WindowManager API——参见下方折叠屏章节；请根据你的依赖版本核实 API。

---

## Google I/O 2026：表现力布局

Material 的 [I/O 2026 更新](https://m3.material.io/blog/whats-new-at-io26)引入了更广泛的表现力/自适应布局指南：

- **表现力布局脚手架**：设计屏幕以适应手机、桌面、折叠屏、手表、XR 及其他空间形态。在 Compose 中，优先使用 Material3 自适应脚手架和窗口尺寸类别，而非硬编码的手机/平板分支。
- **8dp 间距系统**：将间距视为令牌。使用 8dp 刻度来设定边距、内边距、间隔和组件间距，以便一致地应用密度和设备类别变化。
- **手表指南**：使用基于物理的运动、弧形文本样式和贴边容器。避免将手机布局缩小到圆形或紧凑的可穿戴屏幕上。
- **XR 指南**：使用空间面板和基于深度的抬高。不要将 XR 仅仅视为更大的桌面画布；需考虑深度、舒适性和面板放置。

### 间距令牌模式

一次定义间距，然后将其映射到上下文：

```kotlin
object MdSpacing {
    val xxs = 4.dp
    val xs = 8.dp
    val sm = 16.dp
    val md = 24.dp
    val lg = 32.dp
    val xl = 48.dp
}
```

将间距令牌用于：
- 屏幕边距
- 面板间距
- 列表项间距
- 卡片内边距
- 组件组和工具栏

不要在可复用的 UI 中散布原始 `Dp` 字面量。仅当数值确实是组件特有的时，才将一次性数值保留在局部。

---

## 窗口尺寸类别

MD3 定义了 5 个断点类别：

| 类别 | 宽度范围 | 典型设备 | 列数 |
|------|----------|----------|------|
| 紧凑 | < 600dp | 手机竖屏 | 4 |
| 中等 | 600–839dp | 平板竖屏、折叠屏 | 8 |
| 展开 | 840–1199dp | 平板横屏、小型桌面 | 12 |
| 大 | 1200–1599dp | 桌面 | 12 |
| 超大 | 1600dp+ | 超宽屏、大型桌面 | 12 |

### CSS 媒体查询（Web）

将这些用于 **CSS 布局**。**Compose** 应用应使用窗口尺寸类别 / 自适应 API，而非仅在 CSS 中复制此逻辑。

```css
/* 紧凑（默认——移动优先） */
/* 无需媒体查询，这是基础样式 */

/* 中等 */
@media (min-width: 600px) { }

/* 展开 */
@media (min-width: 840px) { }

/* 大 */
@media (min-width: 1200px) { }

/* 超大 */
@media (min-width: 1600px) { }
```

### dp 到 px 的转换
在 Web 上，标准密度下 1dp ≈ 1px。断点值直接对应 CSS 像素。

## 布局结构

### 关键术语

- **窗口（Window）**：应用的可见区域
- **面板（Pane）**：窗口内的布局容器。面板可以是固定的、灵活的、浮动的或半永久的
- **列（Column）**：面板内的垂直内容块
- **边距（Margin）**：屏幕边缘与内容之间的空间
- **间距（Gutter）**：列之间的空间
- **间隔（Spacer）**：面板之间的空间（在多面板布局中）

### 边距和间距值

| 窗口尺寸 | 边距 | 间距 |
|----------|------|------|
| 紧凑 | 16dp | 8dp |
| 中等 | 24dp | 16dp |
| 展开 | 24dp | 16dp |
| 大 | 24dp | 24dp |
| 超大 | 24dp | 24dp |

## 标准布局

MD3 定义了 3 种标准布局作为起点。始终从其中之一开始，而非从原始网格开始。

### 信息流布局

**使用场景**：展示大量可浏览项目的集合（社交信息流、新闻、产品网格）。

```
Compact:    Single column of cards
Medium:     2-column grid
Expanded:   3-column grid
Large:      4-column grid + optional side panel
```

```html
<div class="md3-feed">
  <div class="md3-feed__item">
    <!-- Card content -->
  </div>
  <div class="md3-feed__item">
    <!-- Card content -->
  </div>
  <!-- More items -->
</div>
```

```css
.md3-feed {
  display: grid;
  gap: 8px;
  padding: 16px;
  grid-template-columns: 1fr; /* Compact: 1 column */
}

@media (min-width: 600px) {
  .md3-feed {
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
    padding: 24px;
  }
}

@media (min-width: 840px) {
  .md3-feed {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (min-width: 1200px) {
  .md3-feed {
    grid-template-columns: repeat(4, 1fr);
    gap: 24px;
  }
}
```

### 列表-详情布局

**使用场景**：浏览项目列表，其中每个项目都有详细内容（邮件、文件浏览器、通讯录）。

```
Compact:    List view OR detail view (navigate between them)
Medium:     Side-by-side list (1/3) + detail (2/3)
Expanded:   Side-by-side with wider detail pane
```

**基础 CSS 模式（静态比例）：**

```html
<div class="md3-list-detail">
  <aside class="md3-list-detail__list">
    <md-list>
      <md-list-item type="button" class="active">
        <div slot="headline">Item 1</div>
        <div slot="supporting-text">Description</div>
      </md-list-item>
      <md-list-item type="button">
        <div slot="headline">Item 2</div>
        <div slot="supporting-text">Description</div>
      </md-list-item>
    </md-list>
  </aside>
  <main class="md3-list-detail__detail">
    <h2>Item 1 Detail</h2>
    <p>Full content here...</p>
  </main>
</div>
```

```css
.md3-list-detail {
  display: flex;
  flex-direction: column;
  min-height: 100%;
}

.md3-list-detail__list {
  background: var(--md-sys-color-surface-container);
  border-radius: var(--md-sys-shape-corner-large);
  overflow: auto;
}

.md3-list-detail__detail {
  flex: 1;
  padding: 24px;
}

/* Compact: show one at a time */
@media (max-width: 599px) {
  .md3-list-detail__detail { display: none; }
  .md3-list-detail--detail-active .md3-list-detail__list { display: none; }
  .md3-list-detail--detail-active .md3-list-detail__detail { display: block; }
}

/* Medium+: side by side */
@media (min-width: 600px) {
  .md3-list-detail {
    flex-direction: row;
    gap: 24px;
    padding: 24px;
  }
  .md3-list-detail__list {
    width: 360px;
    flex-shrink: 0;
  }
}

/* Expanded: wider detail */
@media (min-width: 840px) {
  .md3-list-detail__list {
    width: 400px;
  }
}
```

**可调整大小的变体：** 如果需要用户可拖拽调整面板比例，使用 `M3eSplitPane`（参见下方"可调整大面板的拖拽手柄"章节）代替手写 flex 布局。

### 可调整大面板的拖拽手柄

在列表-详情和辅助面板布局中，用户可以使用拖拽手柄调整面板大小。

**使用 `M3eSplitPane`（推荐）：** `@m3e/react/split-pane` 提供了内置拖拽手柄的可调整大小双面板布局，无需手写拖拽逻辑：

```tsx
import { M3eSplitPane } from "@m3e/react/split-pane";
import { M3eList, M3eListItem } from "@m3e/react/list";

<M3eSplitPane orientation="horizontal" initialRatio={0.4}>
  <div slot="start">
    {/* 列表/导航面板 */}
    <M3eList>
      <M3eListItem>项目 1</M3eListItem>
      <M3eListItem>项目 2</M3eListItem>
    </M3eList>
  </div>
  <div slot="end">
    {/* 详情面板 */}
    <h2>详情</h2>
    <p>选中项的完整内容...</p>
  </div>
</M3eSplitPane>
```

`M3eSplitPane` 自动处理拖拽交互、键盘可访问性和面板最小尺寸约束。支持 `orientation="horizontal"`（左右分割）和 `orientation="vertical"`（上下分割）。

> **注意：** 如果你需要完全自定义的面板调整行为（例如多面板嵌套），可以手写拖拽手柄。但在大多数场景下，`M3eSplitPane` 已经足够。

### 辅助面板布局

**使用场景**：主要内容需要补充信息（文档 + 属性面板、视频 + 评论）。

```
Compact:    Stacked — primary on top, supporting below (or bottom sheet)
Medium:     Side-by-side (2/3 primary + 1/3 supporting)
Expanded:   Same but with more space
```

**基础 CSS 模式：**

```html
<div class="md3-supporting-pane">
  <main class="md3-supporting-pane__primary">
    <!-- Primary content (2/3) -->
  </main>
  <aside class="md3-supporting-pane__secondary">
    <!-- Supporting content (1/3) -->
  </aside>
</div>
```

```css
.md3-supporting-pane {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 16px;
}

/* Medium+: side by side */
@media (min-width: 600px) {
  .md3-supporting-pane {
    flex-direction: row;
    gap: 24px;
    padding: 24px;
  }
  .md3-supporting-pane__primary { flex: 2; }
  .md3-supporting-pane__secondary { flex: 1; }
}
```

**进阶方案：** 对于需要用户可拖拽调整比例的场景，使用 `M3eSplitPane`（`@m3e/react/split-pane`）。对于辅助面板以抽屉形式滑入/滑出的场景（如属性面板、工具面板），使用 `M3eDrawerContainer`（`@m3e/react/drawer-container`）并配置 `slot="end"` 作为右侧抽屉。

## CSS 容器查询

对于组件级的响应式行为（独立于视口），使用容器查询：

```css
/* 定义容器 */
.md3-card-container {
  container-type: inline-size;
  container-name: card;
}

/* 响应容器宽度 */
@container card (min-width: 400px) {
  .md3-card {
    flex-direction: row; /* 容器较宽时使用水平布局 */
  }
}

@container card (max-width: 399px) {
  .md3-card {
    flex-direction: column; /* 容器较窄时使用垂直布局 */
  }
}
```

## 自适应组件行为

组件在不同断点间变换：

| 组件 | 紧凑 | 中等（含折叠屏展开） | 展开+ / 大屏幕 |
|------|------|---------------------|----------------|
| 导航 | 底部栏 | 侧边栏 | 侧边抽屉 |
| 应用栏 | 小（64dp） | 小（64dp） | 小或中（112dp） |
| 对话框 | 全屏 | 居中对话框 | 居中对话框（最大宽度 560dp） |
| 底部工作表 | 全高 | 部分高度 | 侧边工作表 |
| 搜索 | 全屏搜索视图 | 持久搜索栏 | 持久搜索栏 |
| 卡片 | 全宽单列 | 多列网格 | 多列网格（最多 4 列） |
| 内容面板 | 单面板 | 可选第二面板 | 两到三个面板 |
| 输入方式 | 仅触摸 | 触摸 + 手写笔 | 触摸 + 鼠标/触控板 + 键盘 |

## 完整应用布局示例

使用 `@m3e/react` 组件构建的完整应用骨架。导航组件根据断点切换，应用栏使用 `M3eAppBar`。

```tsx
import { M3eDrawerContainer, M3eNavMenu, M3eNavMenuItem, M3eIcon, M3eIconButton } from "@m3e/react";
import { M3eAppBar } from "@m3e/react/app-bar";

<M3eDrawerContainer>
  {/* 导航（展开+断点时持久显示） */}
  <M3eNavMenu slot="start">
    <M3eNavMenuItem label="Dashboard" active>
      <M3eIcon slot="start">dashboard</M3eIcon>
    </M3eNavMenuItem>
    <M3eNavMenuItem label="Settings">
      <M3eIcon slot="start">settings</M3eIcon>
    </M3eNavMenuItem>
  </M3eNavMenu>

  {/* 主区域 */}
  <main slot="content">
    {/* 顶部应用栏 */}
    <M3eAppBar variant="small">
      <M3eIconButton slot="navigation" icon="menu" />
      <span slot="title">Dashboard</span>
      <M3eIconButton slot="action" icon="search" />
    </M3eAppBar>

    {/* 使用标准布局的内容区域 */}
    <div className="md3-content-area">
      {/* 在此处使用信息流、列表-详情或辅助面板布局 */}
    </div>
  </main>
</M3eDrawerContainer>
```

```css
/* 主内容区域——保留响应式间距和最大宽度约束 */
.md3-content-area {
  flex: 1;
  overflow-y: auto;
}

/* 紧凑：垂直堆叠 */
@media (max-width: 599px) {
  .md3-content-area { padding: 16px; }
}

/* 中等 */
@media (min-width: 600px) and (max-width: 839px) {
  .md3-content-area { padding: 24px; }
}

/* 展开+ */
@media (min-width: 840px) {
  .md3-content-area { padding: 24px; }
}

/* 大+：限制最大内容宽度 */
@media (min-width: 1200px) {
  .md3-content-area {
    max-width: 1040px;
    margin: 0 auto;
    padding: 24px;
  }
}
```

### 导航跨断点切换策略

导航组件根据窗口尺寸类别切换（详见 `navigation-patterns.md`）：

```css
/* 紧凑（<600dp）：底部导航栏 */
.md3-nav-rail, .md3-drawer-container > [slot="start"] { display: none; }
.md3-nav-bar { display: flex; }

/* 中等（600–839dp）：侧边导航栏 */
@media (min-width: 600px) {
  .md3-nav-bar { display: none; }
  .md3-nav-rail { display: flex; }
}

/* 展开+（840dp+）：持久抽屉 */
@media (min-width: 840px) {
  .md3-nav-rail { display: none; }
  /* M3eDrawerContainer 自动显示 slot="start" 抽屉 */
}
```

> **提示：** `M3eAppBar` 支持 4 种变体：`small`（64dp）、`center-aligned`（64dp）、`medium`（112dp）、`large`（152dp）。中等/大变体在滚动时自动折叠为小变体。在大屏上可以根据场景选择 `medium` 或 `large` 变体以提供更醒目的标题区域。

## 折叠屏与大屏

MD3 为折叠屏设备、平板和大屏形态提供了具体指南。这些是 Material Design 3 中的一等公民——而非事后补充。

### 折叠屏姿态

折叠屏设备引入了传统手机不存在的姿态：

| 姿态 | 描述 | 布局行为 |
|------|------|----------|
| **平展（展开）** | 设备完全打开，单个大屏幕 | 根据宽度视为中等或展开窗口类别 |
| **半开（桌面模式）** | 水平折叠约 90 度，下半部分放在桌上 | 在铰链处分割内容——上半部分显示视频/图片，下半部分显示控件/信息 |
| **半开（书本模式）** | 垂直折叠约 90 度，像书一样持握 | 在铰链处分割内容——一侧显示列表，另一侧显示详情 |
| **折叠** | 设备合拢，使用外屏/封面屏 | 视为紧凑——仅显示核心内容 |

### 铰链感知布局

折痕/铰链是一个物理分隔线。永远不要在铰链区域放置交互内容或关键信息。

**Web——CSS 视口分段 API：**

```css
/* 检测具有两个水平分段的双屏/折叠屏设备 */
@media (horizontal-viewport-segments: 2) {
  .md3-list-detail {
    flex-direction: row;
  }
  .md3-list-detail__list {
    /* 占据左侧分段 */
    width: env(viewport-segment-width 0 0);
    margin-right: env(viewport-segment-left 1 0, 0px) - env(viewport-segment-right 0 0, 0px);
  }
  .md3-list-detail__detail {
    flex: 1;
  }
}

/* 检测桌面模式姿态（两个垂直分段） */
@media (vertical-viewport-segments: 2) {
  .md3-media-player {
    display: flex;
    flex-direction: column;
  }
  .md3-media-player__video {
    height: env(viewport-segment-height 0 0);
  }
  .md3-media-player__controls {
    flex: 1;
  }
}
```

**Flutter——`MediaQuery` 与显示特性：**

```dart
Widget build(BuildContext context) {
  final displayFeatures = MediaQuery.of(context).displayFeatures;
  final hinge = displayFeatures.whereType<DisplayFeature>().where(
    (f) => f.type == DisplayFeatureType.hinge || f.type == DisplayFeatureType.fold,
  ).firstOrNull;

  if (hinge != null) {
    // 折叠屏设备——在铰链处分割
    return TwoPane(
      startPane: ListPane(),
      endPane: DetailPane(),
      paneProportion: 0.5,
      panePriority: isPortrait ? TwoPanePriority.start : TwoPanePriority.both,
    );
  }

  // 单屏——使用窗口尺寸类别
  final width = MediaQuery.sizeOf(context).width;
  if (width < 600) return CompactLayout();
  if (width < 840) return MediumLayout();
  return ExpandedLayout();
}
```

**Jetpack Compose——`WindowInfoTracker` 与 `FoldingFeature`：**

```kotlin
@Composable
fun AdaptiveLayout() {
    val windowInfo = WindowInfoTracker.getOrCreate(LocalContext.current)
        .windowLayoutInfo(LocalContext.current as Activity)
        .collectAsState(initial = WindowLayoutInfo(emptyList()))

    val foldingFeature = windowInfo.value.displayFeatures
        .filterIsInstance<FoldingFeature>()
        .firstOrNull()

    when {
        foldingFeature != null && foldingFeature.state == FoldingFeature.State.HALF_OPENED -> {
            // 桌面模式或书本模式姿态
            if (foldingFeature.orientation == FoldingFeature.Orientation.HORIZONTAL) {
                TabletopLayout(foldingFeature)  // 上方：内容，下方：控件
            } else {
                BookLayout(foldingFeature)  // 左侧：列表，右侧：详情
            }
        }
        else -> {
            // 基于窗口尺寸类别的标准自适应布局
            val windowSizeClass = calculateWindowSizeClass(LocalContext.current as Activity)
            StandardAdaptiveLayout(windowSizeClass)
        }
    }
}
```

### 桌面模式姿态模式

当设备处于桌面模式姿态（水平折叠，下半部分放置在平面上）时，内容自然分为两半：

```
┌─────────────────────┐
│                     │  ← 上半部分：视觉内容
│   视频 / 图片 /     │     （相机取景器、视频播放器、
│   主要内容          │      图片画廊、地图）
│                     │
├─ ─ ─ 铰链 ─ ─ ─ ─ ─┤
│                     │  ← 下半部分：控件与信息
│  控件 / 文本 /      │     （播放控件、聊天输入、
│  辅助信息           │      产品详情、工具栏）
│                     │
└─────────────────────┘
```

### 书本模式姿态模式

当设备处于书本模式姿态（垂直折叠，像书一样持握）时，自然映射为列表-详情：

```
┌──────────┬──────────┐
│          │          │
│  列表 /  │  详情    │
│  导航 /  │  内容    │
│  浏览    │  / 编辑  │
│          │          │
└──────────┴──────────┘
         铰链
```

### 大屏布局指南

适用于平板、Chromebook、桌面和大型折叠屏（展开、大、超大）：

**内容宽度约束：**
- 不要将内容拉伸以填满超宽屏幕——超过 ~80 个字符的阅读行会变得难以扫视
- 将正文内容限制为最大宽度（通常 840–1040dp）并居中
- 将多余空间用于多面板布局，而非更宽的单列

```css
/* 在大屏幕上限制内容宽度 */
@media (min-width: 1200px) {
  .md3-content-area {
    max-width: 1040px;
    margin-inline: auto;
  }
}
```

**各窗口类别的多面板策略：**

| 窗口类别 | 列数 | 推荐布局 |
|----------|------|----------|
| 紧凑（<600dp） | 4 | 单面板。视图间全屏导航。 |
| 中等（600–839dp） | 8 | 可选第二面板。窄列表的列表-详情。侧栏导航。 |
| 展开（840–1199dp） | 12 | 标准双面板。列表-详情或辅助面板。抽屉导航。 |
| 大（1200–1599dp） | 12 | 两到三个面板。带侧面板的信息流。持久辅助面板。 |
| 超大（1600dp+） | 12 | 三面板或带宽敞边距的约束双面板。 |

**输入与交互差异：**
- 大屏幕通常有鼠标/触控板输入——悬停状态和右键菜单很重要
- 触摸目标仍保持最小 48dp，但可以用悬停工具提示补充
- 键盘快捷键在桌面级设备上成为预期功能
- 拖放操作在大屏幕上更自然

```css
/* 为指针设备添加悬停状态 */
@media (hover: hover) {
  .md3-card:hover {
    background: color-mix(
      in srgb,
      var(--md-sys-color-on-surface) 8%,
      var(--md-sys-color-surface)
    );
  }

  .md3-list-item:hover {
    background: color-mix(
      in srgb,
      var(--md-sys-color-on-surface) 8%,
      transparent
    );
  }
}

/* 确保指针设备特有的可用性 */
@media (pointer: fine) {
  /* 滚动条、调整大小手柄、更紧凑的间距是可以接受的 */
  .md3-drag-handle { cursor: col-resize; }
}
```

**Flutter——自适应输入：**

```dart
Widget build(BuildContext context) {
  final width = MediaQuery.sizeOf(context).width;
  final isLargeScreen = width >= 840;

  return Scaffold(
    body: Row(
      children: [
        // 导航自适应
        if (isLargeScreen)
          NavigationRail(
            destinations: destinations,
            selectedIndex: selectedIndex,
            onDestinationSelected: onSelected,
            labelType: NavigationRailLabelType.all,
            leading: FloatingActionButton(
              onPressed: onCompose,
              child: const Icon(Icons.edit),
            ),
          ),
        // 内容填充剩余空间
        Expanded(
          child: isLargeScreen
              ? Row(
                  children: [
                    SizedBox(width: 360, child: ListPane()),
                    const VerticalDivider(width: 1),
                    Expanded(child: DetailPane()),
                  ],
                )
              : selectedItem == null
                  ? ListPane()
                  : DetailPane(),
        ),
      ],
    ),
    bottomNavigationBar: isLargeScreen
        ? null
        : NavigationBar(
            destinations: destinations.map((d) =>
              NavigationDestination(icon: d.icon, label: d.label)).toList(),
            selectedIndex: selectedIndex,
            onDestinationSelected: onSelected,
          ),
  );
}
```

### 折叠屏感知的标准布局

三种标准布局自然地适应折叠屏：

| 布局 | 折叠屏行为 |
|------|------------|
| **信息流** | 展开时：多列网格填满两半。桌面模式：上方网格，下方选中项预览。 |
| **列表-详情** | 书本模式：左半列表，右半详情——完美自然契合。桌面模式：上方列表，下方详情。 |
| **辅助面板** | 书本模式：左侧主要内容，右侧辅助内容。桌面模式：上方主要内容，下方辅助控件。 |

### 大屏与折叠屏测试

**Web：**
- 使用 Chrome DevTools 响应模式在 600、840、1200 和 1600px 断点处测试
- 使用 pointer: coarse（触摸）和 pointer: fine（鼠标）媒体查询测试
- 验证内容在 1600px+ 时不会超出可读行长度

**Flutter：**
- 使用 `DevicePreview` 包模拟折叠屏和平板
- 使用 `MediaQuery` 覆盖 `displayFeatures` 进行测试
- 在 Android 模拟器上运行：Pixel Fold、7.6" 折叠屏、10" 平板、Chromebook

**Compose：**
- 使用 Android Studio 折叠屏模拟器（Pixel Fold、7.6" Foldable）
- 测试姿态变化：平展 → 半开 → 折叠
- 使用 `WindowInfoTracker` 验证折叠感知布局切换

### 折叠屏/大屏支持审查清单

审查时，检查以下具体项目：

- [ ] 应用使用 `MediaQuery.sizeOf(context).width` 或等效方法确定窗口尺寸类别
- [ ] 布局在 600dp 处从单面板切换为多面板
- [ ] 导航跨断点变换：底部栏 → 侧栏 → 抽屉
- [ ] 内容在大屏幕上具有最大宽度约束（不拉伸填满）
- [ ] 折痕/铰链处未放置关键内容或交互元素
- [ ] 已处理折叠屏姿态（如果目标为折叠屏设备）：桌面模式和书本模式
- [ ] 指针设备存在悬停状态（`@media (hover: hover)`）
- [ ] 即使在大屏幕上触摸目标仍保持最小 48dp
- [ ] 对话框在中等+屏幕上居中（非全屏）
- [ ] 底部工作表在展开+屏幕上转换为侧边工作表

## 间距系统

MD3 使用 4dp 基础网格进行间距控制：

| 用途 | 数值 |
|------|------|
| 组件内边距 | 4, 8, 12, 16, 24dp |
| 组件间距 | 8, 12, 16, 24dp |
| 区块间距 | 24, 32, 48dp |
| 布局边距 | 16dp（紧凑），24dp（中等+） |
| 网格间距 | 8dp（紧凑），16dp（中等），24dp（大+） |

始终使用 4dp 的倍数以保持一致的空间节奏。

## @m3e/react 布局组件

`@m3e/react` 提供了多个布局专用组件，帮助构建 M3E 自适应界面。这些组件替代了手写的 CSS 模式，提供开箱即用的交互和可访问性支持。

### 应用栏（App Bar）

顶部应用栏，支持 4 种变体：

```tsx
import { M3eAppBar, M3eIconButton } from "@m3e/react/app-bar";

<M3eAppBar variant="small">
  <M3eIconButton slot="navigation" icon="menu" />
  <span slot="title">应用标题</span>
  <M3eIconButton slot="action" icon="search" />
  <M3eIconButton slot="action" icon="more_vert" />
</M3eAppBar>
```

**变体：**
- `small`（64dp）：标准小应用栏
- `center-aligned`（64dp）：标题居中
- `medium`（112dp）：中等高度，滚动时折叠为 small
- `large`（152dp）：大标题区域，滚动时折叠为 small

**特性：**
- 自动处理滚动行为（elevate on scroll）
- medium/large 变体在滚动时平滑折叠为 small
- 支持 leading/trailing 图标按钮
- 自动应用正确的颜色和排版令牌

> **提示：** 在"完整应用布局示例"中已展示了 `M3eAppBar` 的完整用法。

### 分割面板（Split Pane）

可拖拽调整大小的双视图布局，替代手写的拖拽手柄 CSS：

```tsx
import { M3eSplitPane } from "@m3e/react/split-pane";
import { M3eList, M3eListItem } from "@m3e/react/list";

<M3eSplitPane orientation="horizontal" initialRatio={0.4}>
  <div slot="start">
    {/* 左侧/上方内容 */}
    <M3eList>
      <M3eListItem>项目 1</M3eListItem>
      <M3eListItem>项目 2</M3eListItem>
    </M3eList>
  </div>
  <div slot="end">
    {/* 右侧/下方内容（详情） */}
    <h2>详情</h2>
    <p>选中项的完整内容...</p>
  </div>
</M3eSplitPane>
```

**特性：**
- 内置拖拽手柄，自动处理键盘可访问性
- 支持 `horizontal`（左右）和 `vertical`（上下）分割
- 自动约束面板最小尺寸
- 替代手写的 `.md3-drag-handle` CSS 模式

### 抽屉容器（Drawer Container）

响应式布局容器，管理一个或两个滑动抽屉。用于替代手写的导航容器 CSS：

```tsx
import { M3eDrawerContainer, M3eNavMenu, M3eNavMenuItem, M3eIcon } from "@m3e/react";

<M3eDrawerContainer>
  <M3eNavMenu slot="start">
    <M3eNavMenuItem label="收件箱" active>
      <M3eIcon slot="start">inbox</M3eIcon>
    </M3eNavMenuItem>
    <M3eNavMenuItem label="已发送">
      <M3eIcon slot="start">send</M3eIcon>
    </M3eNavMenuItem>
  </M3eNavMenu>
  <main slot="content">
    {/* 主内容 */}
  </main>
</M3eDrawerContainer>
```

**特性：**
- 支持 `slot="start"`（左侧抽屉）和 `slot="end"`（右侧抽屉）
- 标准模式（持久显示，360dp 宽）和模态模式（覆盖 + scrim）
- 自动响应断点变化
- 替代手写的 `.md3-app-layout` 和 `.md3-nav` CSS 模式

### 内容面板（Content Pane）

带形状的可垂直滚动内容表面：

```tsx
import { M3eContentPane } from "@m3e/react/content-pane";
import { M3eHeading } from "@m3e/react/heading";

<M3eContentPane>
  <div slot="header">
    <M3eHeading level="headline-medium">标题</M3eHeading>
  </div>
  <div slot="content">
    {/* 可滚动内容 */}
  </div>
</M3eContentPane>
```

**特性：**
- `slot="header"`：粘性顶部（可选）
- `slot="content"`：可滚动主体
- 自动应用形状和颜色令牌
- 适合卡片、列表、表单等内容容器

### 导航组件（跨断点切换）

`@m3e/react` 提供三种导航组件，根据窗口尺寸类别切换：

**底部导航栏（紧凑屏幕）：**
```tsx
<M3eNavBar>
  <M3eNavBarItem label="首页" active>
    <M3eIcon slot="active-icon">home</M3eIcon>
    <M3eIcon slot="inactive-icon">home</M3eIcon>
  </M3eNavBarItem>
  {/* 更多项目（3-5 个） */}
</M3eNavBar>
```

**侧边导航栏（中等屏幕）：**
```tsx
<M3eNavRail>
  <M3eFab slot="fab" icon="edit" />
  <M3eNavRailItem label="首页" active>
    <M3eIcon slot="active-icon">home</M3eIcon>
    <M3eIcon slot="inactive-icon">home</M3eIcon>
  </M3eNavRailItem>
  {/* 更多项目 */}
</M3eNavRail>
```

**导航菜单（展开+屏幕，配合抽屉容器）：**
```tsx
<M3eNavMenu>
  <M3eNavMenuItem label="首页" active>
    <M3eIcon slot="start">home</M3eIcon>
  </M3eNavMenuItem>
  {/* 更多项目（支持层级） */}
</M3eNavMenu>
```

### 8dp 间距系统与 @m3e/react

`@m3e/react` 通过 `<M3eTheme>` 的 `density` 属性支持密度调整：

```tsx
import { M3eTheme } from "@m3e/react/theme";
import { M3eCard } from "@m3e/react/card";

{/* 紧凑密度 */}
<M3eTheme density="compact">
  <M3eCard>紧凑卡片</M3eCard>
</M3eTheme>

{/* 舒适密度（默认） */}
<M3eTheme density="comfortable">
  <M3eCard>舒适卡片</M3eCard>
</M3eTheme>

{/* 宽松密度 */}
<M3eTheme density="spacious">
  <M3eCard>宽松卡片</M3eCard>
</M3eTheme>
```

密度调整影响组件内边距、间距和最小尺寸，保持 8dp 间距系统的一致性。

### @m3e/react 自适应布局建议

`@m3e/react` 组件自动适配不同屏幕尺寸。以下是推荐的自适应策略：

| 窗口尺寸 | 导航 | 布局 | 建议 |
|----------|------|------|------|
| 紧凑 (<600dp) | `M3eNavBar` | 单列 | 全屏导航切换 |
| 中等 (600-839dp) | `M3eNavRail` | 双列或列表-详情 | 使用 `M3eSplitPane` |
| 展开 (840-1199dp) | `M3eDrawerContainer` | 多面板 | 持久导航抽屉 |
| 大 (1200-1599dp) | `M3eDrawerContainer` | 多面板+侧面板 | 限制内容最大宽度 |
| 超大 (1600dp+) | `M3eDrawerContainer` | 三面板 | 宽裕边距 |

**何时使用 @m3e/react 组件 vs 手写 CSS：**

| 场景 | 推荐方案 |
|------|----------|
| 应用栏 | `M3eAppBar`（替代 `.md3-top-app-bar`） |
| 可调整面板 | `M3eSplitPane`（替代 `.md3-drag-handle`） |
| 导航容器 | `M3eDrawerContainer`（替代 `.md3-app-layout`） |
| 导航菜单 | `M3eNavMenu` / `M3eNavRail` / `M3eNavBar` |
| 内容容器 | `M3eContentPane`（可选） |
| 信息流网格 | 手写 CSS grid（布局模式，非组件） |
| 列表-详情（静态） | 手写 CSS flex（布局模式）或 `M3eSplitPane`（可调整） |
| 辅助面板（静态） | 手写 CSS flex（布局模式） |
| 辅助面板（抽屉） | `M3eDrawerContainer` + `slot="end"` |

> **原则：** 布局级别的响应式模式（网格、flex 布局、断点媒体查询）保留为 CSS。组件级别的交互（拖拽调整、抽屉滑动、导航切换）使用 `@m3e/react` 组件。
