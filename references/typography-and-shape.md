# MD3 字体排版、形状、层阶与动效

Material Design 3 中除色彩以外的视觉 Token 体系参考。

## 字体排版

### 字号缩放

MD3 使用 15 种基线样式 + 15 种强调样式，分为 5 个类别（Display、Headline、Title、Body、Label），每个类别各有 3 种尺寸（Large、Medium、Small）。

#### 基线字号缩放（默认值）

| Style | Font | Weight | Size (sp) | Size (rem) | Line Height | Tracking |
|-------|------|--------|-----------|------------|-------------|----------|
| Display Large | Roboto | 400 | 57 | 3.5625 | 64sp / 4rem | -0.25px |
| Display Medium | Roboto | 400 | 45 | 2.8125 | 52sp / 3.25rem | 0 |
| Display Small | Roboto | 400 | 36 | 2.25 | 44sp / 2.75rem | 0 |
| Headline Large | Roboto | 400 | 32 | 2 | 40sp / 2.5rem | 0 |
| Headline Medium | Roboto | 400 | 28 | 1.75 | 36sp / 2.25rem | 0 |
| Headline Small | Roboto | 400 | 24 | 1.5 | 32sp / 2rem | 0 |
| Title Large | Roboto | 400 | 22 | 1.375 | 28sp / 1.75rem | 0 |
| Title Medium | Roboto | 500 | 16 | 1 | 24sp / 1.5rem | 0.15px |
| Title Small | Roboto | 500 | 14 | 0.875 | 20sp / 1.25rem | 0.1px |
| Body Large | Roboto | 400 | 16 | 1 | 24sp / 1.5rem | 0.5px |
| Body Medium | Roboto | 400 | 14 | 0.875 | 20sp / 1.25rem | 0.25px |
| Body Small | Roboto | 400 | 12 | 0.75 | 16sp / 1rem | 0.4px |
| Label Large | Roboto | 500 | 14 | 0.875 | 20sp / 1.25rem | 0.1px |
| Label Medium | Roboto | 500 | 12 | 0.75 | 16sp / 1rem | 0.5px |
| Label Small | Roboto | 500 | 11 | 0.6875 | 16sp / 1rem | 0.5px |

#### 强调样式（Expressive 更新）

15 种强调样式与基线缩放一一对应，但具有**更高的字重**并附带微调。适用于：
- 组件中的选中/激活状态
- 主要操作按钮
- 需要突出显示的标题
- 未读/重要内容

使用方式：将基线 Token 替换为强调版本：
- 基线：`md.sys.typescale.display-large`
- 强调：`md.sys.typescale.emphasized.display-large`

### CSS 自定义属性

每种字体样式映射到独立的轴向 Token：

```css
/* 示例：Body Large */
--md-sys-typescale-body-large-font: 'Roboto', sans-serif;
--md-sys-typescale-body-large-weight: 400;
--md-sys-typescale-body-large-size: 1rem;        /* 16sp */
--md-sys-typescale-body-large-line-height: 1.5rem; /* 24sp */
--md-sys-typescale-body-large-tracking: 0.03125rem; /* 0.5px */

/* 示例：Title Medium */
--md-sys-typescale-title-medium-font: 'Roboto', sans-serif;
--md-sys-typescale-title-medium-weight: 500;
--md-sys-typescale-title-medium-size: 1rem;
--md-sys-typescale-title-medium-line-height: 1.5rem;
--md-sys-typescale-title-medium-tracking: 0.009375rem;

/* 示例：Label Large（用于按钮） */
--md-sys-typescale-label-large-font: 'Roboto', sans-serif;
--md-sys-typescale-label-large-weight: 500;
--md-sys-typescale-label-large-size: 0.875rem;
--md-sys-typescale-label-large-line-height: 1.25rem;
--md-sys-typescale-label-large-tracking: 0.00625rem;
```

### 在 CSS 中使用字体样式

使用独立属性逐一应用：

```css
.headline {
  font-family: var(--md-sys-typescale-headline-large-font);
  font-weight: var(--md-sys-typescale-headline-large-weight);
  font-size: var(--md-sys-typescale-headline-large-size);
  line-height: var(--md-sys-typescale-headline-large-line-height);
  letter-spacing: var(--md-sys-typescale-headline-large-tracking);
}
```

也可以使用 `font` 简写属性来简化（注意：需要定义简写 Token）：

```css
.headline {
  font: var(--md-sys-typescale-headline-large-weight)
        var(--md-sys-typescale-headline-large-size) /
        var(--md-sys-typescale-headline-large-line-height)
        var(--md-sys-typescale-headline-large-font);
  letter-spacing: var(--md-sys-typescale-headline-large-tracking);
}
```

### 字体定制

MD3 使用两种字体角色：
- **品牌**：用于 Display 和 Headline 样式（侧重表现力）
- **正文**：用于 Title、Body 和 Label 样式（侧重可读性）

两者默认均为 Roboto。自定义方式如下：

```css
:root {
  /* 品牌字体，用于 display/headline */
  --md-ref-typeface-brand: 'Your Display Font', sans-serif;
  /* 正文字体，用于 body/label/title */
  --md-ref-typeface-plain: 'Your Body Font', sans-serif;
}
```

### Roboto Flex

Roboto Flex 是一款可变字体，支持多个轴向：
- **Weight** (wght)：100–1000
- **Width** (wdth)：25–151
- **Optical size** (opsz)：8–144

```css
@font-face {
  font-family: 'Roboto Flex';
  src: url('RobotoFlex-VariableFont.woff2') format('woff2');
  font-weight: 100 1000;
  font-stretch: 25% 151%;
}
```

### 字号单位

| 平台 | 单位 | 换算 |
|------|------|------|
| Android | sp | 1:1 |
| Web | rem | sp / 16 = rem（假设根字号为 16px） |

示例：10sp = 0.625rem, 12sp = 0.75rem, 14sp = 0.875rem, 16sp = 1rem, 24sp = 1.5rem

### 组件字体用法

| 组件 | 字体样式 |
|------|----------|
| Button label | Label Large |
| Card title | Title Medium |
| Card body | Body Medium |
| Top app bar title | Title Large |
| Navigation label | Label Medium |
| Dialog headline | Headline Small |
| Dialog body | Body Medium |
| Chip label | Label Large |
| Text field input | Body Large |
| Text field label | Body Small（浮动）/ Body Large（静止） |
| List headline | Body Large |
| List supporting text | Body Medium |
| Snackbar text | Body Medium |
| Tooltip text | Body Small |
| Tab label | Title Small |
| Badge count | Label Small |

## 形状

### 圆角半径缩放

| Token | Value (dp) | Value (px/CSS) | 默认组件 |
|-------|-----------|----------------|----------|
| `none` | 0 | 0px | — |
| `extra-small` | 4 | 4px | Snackbar |
| `small` | 8 | 8px | Text fields、menus、chips |
| `medium` | 12 | 12px | Cards |
| `large` | 16 | 16px | FAB、extended FAB、nav drawer |
| `large-increased` | 20 | 20px | （Expressive 更新） |
| `extra-large` | 28 | 28px | Dialogs、bottom sheets、side sheets |
| `extra-large-increased` | 32 | 32px | （Expressive 更新） |
| `extra-extra-large` | 48 | 48px | （Expressive 更新） |
| `full` | — | 9999px | Buttons、badges、pills、sliders |

### CSS 自定义属性

```css
:root {
  --md-sys-shape-corner-none: 0px;
  --md-sys-shape-corner-extra-small: 4px;
  --md-sys-shape-corner-small: 8px;
  --md-sys-shape-corner-medium: 12px;
  --md-sys-shape-corner-large: 16px;
  --md-sys-shape-corner-large-increased: 20px;
  --md-sys-shape-corner-extra-large: 28px;
  --md-sys-shape-corner-extra-large-increased: 32px;
  --md-sys-shape-corner-extra-extra-large: 48px;
  --md-sys-shape-corner-full: 9999px;
}
```

### 组件形状映射

| 组件 | 默认形状 Token |
|------|----------------|
| Buttons（所有类型） | `full` |
| FAB | `large` |
| Extended FAB | `large` |
| Icon button | `full` |
| Chips | `small` |
| Cards | `medium` |
| Dialogs | `extra-large` |
| Text fields | `small`（顶部圆角） |
| Menus | `small` |
| Navigation drawer | `large`（末端圆角） |
| Bottom sheets | `extra-large`（顶部圆角） |
| Snackbar | `extra-small` |
| Badges | `full` |
| Sliders (handle) | `full` |
| Switch (track) | `full` |
| Tabs (indicator) | `full`（顶部圆角） |
| Search bar | `full` |

### 形状变形（Expressive）

在 M3 Expressive 更新中，组件可以在交互时进行形状变形：
- 按钮在按下时形状发生变形
- 选中状态可以改变形状
- 加载指示器通过形状变形来展示进度

**平台说明**：形状变形**不包含**在 `@material/web` 中（[Material Web 已进入维护模式；Expressive 不适用于 Web](https://m3.material.io/develop/web)）。**Jetpack Compose** 是 Google 针对 Android 记录 expressive 形状/动效行为的平台。**Flutter：** 请查阅当前 SDK 版本对应的 Material 3 / expressive 文档。**Web：** 可通过 CSS 对 `border-radius` 的过渡动画来近似模拟。

## 层阶

### 层阶级别

| Level | DP Height | 用途 |
|-------|-----------|------|
| 0 | 0dp | 大多数静止状态的组件 |
| 1 | 1dp | 抬升变体（cards、sheets） |
| 2 | 3dp | Menus、nav bar、滚动后的 app bar |
| 3 | 6dp | FAB、dialogs、search、date/time pickers |
| 4 | 8dp | 仅用于悬停/聚焦时的提升 |
| 5 | 12dp | 仅用于悬停/聚焦时的提升 |

### 色调层阶（非阴影）

MD3 使用**色调表面颜色**来表达层阶，而非阴影。层阶越高 = 表面色调越浅（浅色主题中）或表面色调越浅（深色主题中）。

surface container 角色与这一概念的对应关系：
- Level 0：`surface`（最扁平）
- Level 1：`surface-container-low`
- Level 2：`surface-container`
- Level 3：`surface-container-high`
- Level 4-5：`surface-container-highest`

### 何时使用阴影

阴影仅在以下情况下适用：
- 组件悬浮于可能视觉繁杂的内容之上（例如，FAB 悬浮在图片上方）
- 仅靠颜色不足以提供额外的深度提示（例如，重叠的元素）
- 平台约定需要阴影（部分 Android 组件）

### CSS 阴影值

当需要阴影时：

```css
/* Level 1 */
box-shadow: 0 1px 2px rgba(0,0,0,0.3), 0 1px 3px 1px rgba(0,0,0,0.15);

/* Level 2 */
box-shadow: 0 1px 2px rgba(0,0,0,0.3), 0 2px 6px 2px rgba(0,0,0,0.15);

/* Level 3 */
box-shadow: 0 4px 8px 3px rgba(0,0,0,0.15), 0 1px 3px rgba(0,0,0,0.3);

/* Level 4 */
box-shadow: 0 6px 10px 4px rgba(0,0,0,0.15), 0 2px 3px rgba(0,0,0,0.3);

/* Level 5 */
box-shadow: 0 8px 12px 6px rgba(0,0,0,0.15), 0 4px 4px rgba(0,0,0,0.3);
```

### 组件层阶映射

| Level | 静止状态下的组件 |
|-------|------------------|
| 0 | App bar（扁平）、filled/tonal/outlined buttons、button groups、filled/outlined cards、carousel、chips、full-screen dialogs、icon buttons、lists、nav rail、segmented buttons、sliders、split buttons、tabs |
| 1 | Banners、modal bottom sheet、elevated button、elevated card、elevated chips、modal nav drawer、modal side sheet |
| 2 | App bar（滚动后）、menus、nav bar、rich tooltips、toolbar |
| 3 | Date pickers、modal dialogs、extended FAB、FAB、FAB menu close button、search、time pickers |

**悬停/聚焦**：大多数可交互组件在悬停/聚焦时提升 1 个级别（例如，FAB 从 Level 3 升至 Level 4）。

## 动效

### 弹簧物理动效（Expressive 更新）

MD3 Expressive（2025 年 5 月）为组件动画引入了弹簧物理动效。弹簧让动效更加自然、响应更加灵敏：

- 弹簧没有固定的持续时间——它们根据输入动态响应
- 两种方案：**standard**（实用型）和 **expressive**（弹性型）
- **Jetpack Compose** 在当前 Material3 中提供了 motion scheme / 面向弹簧的 API（参见 `MotionScheme` 及对应 BOM）。**MDC-Android** 可能因版本而异。**Web：** Material Web 未实现 Expressive 动效物理——请使用缓动/持续时间或自定义 CSS/JS。**Flutter：** 请查阅对应 Flutter/Material 版本以确认对等支持。

### 缓动与持续时间（过渡动画）

缓动/持续时间系统用于**过渡动画**（进入、退出、共享轴），同时也作为 Web 端回退方案：

#### 缓动集合

**Emphasized**（推荐用于大多数过渡——体现 MD3 风格）：
| Type | CSS Cubic-bezier | 用途 |
|------|-----------------|------|
| Emphasized | `cubic-bezier(0.2, 0, 0, 1)` | 在屏幕上开始并结束 |
| Emphasized Decelerate | `cubic-bezier(0.05, 0.7, 0.1, 1)` | 进入屏幕 |
| Emphasized Accelerate | `cubic-bezier(0.3, 0, 0.8, 0.15)` | 退出屏幕 |

**Standard**（用于功能性过渡、Web 端回退）：
| Type | CSS Cubic-bezier | 用途 |
|------|-----------------|------|
| Standard | `cubic-bezier(0.2, 0, 0, 1)` | 在屏幕上开始并结束 |
| Standard Decelerate | `cubic-bezier(0, 0, 0, 1)` | 进入屏幕 |
| Standard Accelerate | `cubic-bezier(0.3, 0, 1, 1)` | 退出屏幕 |

#### 持续时间缩放

| Token | Value | 用途 |
|-------|-------|------|
| Short 1 | 50ms | 微交互 |
| Short 2 | 100ms | 小型过渡 |
| Short 3 | 150ms | 小型过渡 |
| Short 4 | 200ms | 退出过渡 |
| Medium 1 | 250ms | 中型过渡 |
| Medium 2 | 300ms | 标准过渡 |
| Medium 3 | 350ms | 中型过渡 |
| Medium 4 | 400ms | 进入过渡 |
| Long 1 | 450ms | 大型过渡 |
| Long 2 | 500ms | 大型强调过渡 |
| Long 3 | 550ms | 复杂过渡 |
| Long 4 | 600ms | 复杂过渡 |
| Extra Long 1 | 700ms | 页面过渡 |
| Extra Long 2 | 800ms | 页面过渡 |
| Extra Long 3 | 900ms | 复杂页面过渡 |
| Extra Long 4 | 1000ms | 复杂页面过渡 |

#### 推荐搭配

| 过渡类型 | 缓动 | 持续时间 |
|----------|------|----------|
| 元素停留在屏幕上 | Emphasized | 500ms |
| 元素进入屏幕 | Emphasized Decelerate | 400ms |
| 元素永久退出 | Emphasized Accelerate | 200ms |
| 元素暂时退出 | Emphasized | 300ms |
| 小型功能性过渡 | Standard | 300ms |

### CSS 实现

```css
:root {
  /* 缓动 */
  --md-sys-motion-easing-emphasized: cubic-bezier(0.2, 0, 0, 1);
  --md-sys-motion-easing-emphasized-decelerate: cubic-bezier(0.05, 0.7, 0.1, 1);
  --md-sys-motion-easing-emphasized-accelerate: cubic-bezier(0.3, 0, 0.8, 0.15);
  --md-sys-motion-easing-standard: cubic-bezier(0.2, 0, 0, 1);
  --md-sys-motion-easing-standard-decelerate: cubic-bezier(0, 0, 0, 1);
  --md-sys-motion-easing-standard-accelerate: cubic-bezier(0.3, 0, 1, 1);

  /* 持续时间 */
  --md-sys-motion-duration-short1: 50ms;
  --md-sys-motion-duration-short2: 100ms;
  --md-sys-motion-duration-short3: 150ms;
  --md-sys-motion-duration-short4: 200ms;
  --md-sys-motion-duration-medium1: 250ms;
  --md-sys-motion-duration-medium2: 300ms;
  --md-sys-motion-duration-medium3: 350ms;
  --md-sys-motion-duration-medium4: 400ms;
  --md-sys-motion-duration-long1: 450ms;
  --md-sys-motion-duration-long2: 500ms;
  --md-sys-motion-duration-long3: 550ms;
  --md-sys-motion-duration-long4: 600ms;
  --md-sys-motion-duration-extra-long1: 700ms;
  --md-sys-motion-duration-extra-long2: 800ms;
  --md-sys-motion-duration-extra-long3: 900ms;
  --md-sys-motion-duration-extra-long4: 1000ms;
}

/* 示例：对话框进入 */
.md3-dialog-enter {
  animation: dialog-enter var(--md-sys-motion-duration-medium4)
             var(--md-sys-motion-easing-emphasized-decelerate);
}

@keyframes dialog-enter {
  from { opacity: 0; transform: scale(0.8); }
  to { opacity: 1; transform: scale(1); }
}

/* 示例：淡出 */
.md3-fade-out {
  animation: fade-out var(--md-sys-motion-duration-short4)
             var(--md-sys-motion-easing-emphasized-accelerate);
}

@keyframes fade-out {
  from { opacity: 1; }
  to { opacity: 0; }
}
```

## M3E Expressive 在 Web 上的实现

### @m3e/react 的 Expressive 支持

`@m3e/react` 是唯一真正在 Web 上实现 Material 3 Expressive 的组件库。与 `@material/web`（维护模式、无 Expressive）不同，`@m3e/react` 原生支持：

- **弹性动效 (Spring Motion)**：组件动画使用弹簧物理模型，无需固定时长，动态响应输入
- **形状变换 (Shape Morphing)**：组件在交互时可在形状间平滑过渡
- **Expressive 组件变体**：按钮、FAB、卡片等组件提供更丰富的视觉风格和配置选项

### 形状变换

在 M3 Expressive 更新中，组件可在交互时变换形状：
- 按钮在按下时形状变换
- 选中状态可改变形状
- 加载指示器使用形状变换展示进度

`@m3e/react` 提供了 `<M3eShape>` 组件，内建形状变换支持：

```tsx
// @m3e/react（推荐）
import { M3eShape } from "@m3e/react/shape";
import { M3eIcon } from "@m3e/react/icon";

<M3eShape shape="circle" morphTo="rounded-square" on="click">
  <M3eIcon>play_arrow</M3eIcon>
</M3eShape>
```

```css
m3e-shape {
  --m3e-shape-transition-duration: 300ms;
  --m3e-shape-transition-easing: var(--md-sys-motion-easing-emphasized);
}
```

### 弹性动效

`@m3e/react` 通过 `<M3eTheme>` 组件启用弹性动效：

```tsx
// @m3e/react（推荐）
import { M3eTheme } from "@m3e/react/theme";
import { M3eButton } from "@m3e/react/button";
import { M3eFab } from "@m3e/react/fab";
import { M3eCard } from "@m3e/react/card";
import { M3eIcon } from "@m3e/react/icon";

<M3eTheme motion="expressive">
  {/* 所有子组件使用弹性动效 */}
  <M3eButton variant="filled">弹性按钮</M3eButton>
  <M3eFab ariaLabel="创建">
    <M3eIcon slot="icon">add</M3eIcon>
  </M3eFab>
  <M3eCard variant="elevated">弹性卡片</M3eCard>
</M3eTheme>
```

弹性动效模式下，组件的过渡和动画将使用弹簧物理模型，产生更自然、更有弹性的运动效果。

### CSS 近似实现

如果不使用 `@m3e/react`，可以用 CSS 近似实现 Expressive 效果：

```css
/* 形状变换（近似） */
.md3-button {
  border-radius: var(--md-sys-shape-corner-full);
  transition: border-radius var(--md-sys-motion-duration-short4)
              var(--md-sys-motion-easing-emphasized);
}

.md3-button:active {
  border-radius: var(--md-sys-shape-corner-medium);
}

/* 弹性动效（近似 — 使用 CSS 缓动模拟弹簧） */
.md3-expressive-enter {
  animation: expressive-enter 500ms cubic-bezier(0.34, 1.56, 0.64, 1);
}

@keyframes expressive-enter {
  from { opacity: 0; transform: scale(0.8); }
  to { opacity: 1; transform: scale(1); }
}
```

> **注意：** CSS 近似实现无法完全复现 `@m3e/react` 的弹簧物理动效。建议在生产环境中使用 `@m3e/react` 获得完整的 Expressive 体验。
