# MD3 色彩系统

Material Design 3 色彩系统的完整参考：色彩角色、色调色板、动态配色和配色方案生成。

## 色彩角色

MD3 定义了 29 个以上的色彩角色，按组分类。**Jetpack Compose：** 映射到 `MaterialTheme.colorScheme`（例如 `primary`、`onPrimary`）。**Web：** 每个角色以 CSS 自定义属性 `--md-sys-color-{role-name}` 的形式存在。

### 强调色

三个强调色组（primary、secondary、tertiary）各有 4 个角色：

| 角色 | CSS Token | 用途 |
|------|-----------|------|
| Primary | `--md-sys-color-primary` | 高强调度的填充、文本、相对于表面的图标 |
| On Primary | `--md-sys-color-on-primary` | primary 上的文本和图标 |
| Primary Container | `--md-sys-color-primary-container` | 关键组件的突出填充（如 FAB） |
| On Primary Container | `--md-sys-color-on-primary-container` | primary-container 上的文本和图标 |
| Secondary | `--md-sys-color-secondary` | 较低强调度的填充、文本、图标 |
| On Secondary | `--md-sys-color-on-secondary` | secondary 上的文本和图标 |
| Secondary Container | `--md-sys-color-secondary-container` | 退让性组件（色调按钮） |
| On Secondary Container | `--md-sys-color-on-secondary-container` | secondary-container 上的文本和图标 |
| Tertiary | `--md-sys-color-tertiary` | 互补性填充、文本、图标 |
| On Tertiary | `--md-sys-color-on-tertiary` | tertiary 上的文本和图标 |
| Tertiary Container | `--md-sys-color-tertiary-container` | 互补性容器填充 |
| On Tertiary Container | `--md-sys-color-on-tertiary-container` | tertiary-container 上的文本和图标 |

**使用指南：**
- **Primary**：最突出的组件 —— FAB、高强调度按钮、激活状态
- **Secondary**：较不突出的组件 —— 筛选标签、色调按钮、选择状态
- **Tertiary**：与 primary/secondary 形成平衡的对比强调 —— 输入框、徽标

### 错误色

不随动态配色方案变化的静态颜色：

| 角色 | CSS Token | 用途 |
|------|-----------|------|
| Error | `--md-sys-color-error` | 用于紧急元素的醒目颜色 |
| On Error | `--md-sys-color-on-error` | error 上的文本和图标 |
| Error Container | `--md-sys-color-error-container` | 错误容器填充 |
| On Error Container | `--md-sys-color-on-error-container` | error-container 上的文本和图标 |

### 表面色

| 角色 | CSS Token | 用途 |
|------|-----------|------|
| Surface | `--md-sys-color-surface` | 默认背景色 |
| On Surface | `--md-sys-color-on-surface` | 任意表面上的文本和图标 |
| On Surface Variant | `--md-sys-color-on-surface-variant` | 表面上较低强调度的文本/图标 |
| Surface Container Lowest | `--md-sys-color-surface-container-lowest` | 最低强调度容器 |
| Surface Container Low | `--md-sys-color-surface-container-low` | 低强调度容器 |
| Surface Container | `--md-sys-color-surface-container` | 默认容器（导航区域） |
| Surface Container High | `--md-sys-color-surface-container-high` | 高强调度容器 |
| Surface Container Highest | `--md-sys-color-surface-container-highest` | 最高强调度容器 |
| Surface Dim | `--md-sys-color-surface-dim` | 两种主题中最暗的表面 |
| Surface Bright | `--md-sys-color-surface-bright` | 两种主题中最亮的表面 |

**表面容器层级**：使用 `surface` 作为背景，`surface-container` 用于导航。5 个容器层级创造视觉层级和嵌套深度，特别适用于包含多个面板的扩展布局。

**Surface dim/bright**：与普通 surface（在亮色和暗色之间翻转）不同，dim 和 bright 在两种主题中保持其相对亮度。使用 bright 用于需要始终最亮的区域，dim 用于需要始终最暗的区域。

### 反色

用于与周围 UI 形成对比的元素（例如 snackbars）：

| 角色 | CSS Token | 用途 |
|------|-----------|------|
| Inverse Surface | `--md-sys-color-inverse-surface` | 对比元素的背景 |
| Inverse On Surface | `--md-sys-color-inverse-on-surface` | inverse-surface 上的文本 |
| Inverse Primary | `--md-sys-color-inverse-primary` | inverse-surface 上的可操作文本 |

### 轮廓色

| 角色 | CSS Token | 用途 |
|------|-----------|------|
| Outline | `--md-sys-color-outline` | 重要边界（文本字段边框，3:1 对比度） |
| Outline Variant | `--md-sys-color-outline-variant` | 装饰性元素（分隔线、卡片边框） |

**重要提示**：不要将 `outline` 用于分隔线 —— 请使用 `outline-variant`。不要将 `outline-variant` 用于需要 3:1 对比度的交互边界 —— 请使用 `outline`。

### 固定强调色（附加）

这些颜色在亮色和暗色主题中保持不变（与普通容器颜色会随主题变化不同）：

| 角色 | CSS Token |
|------|-----------|
| Primary Fixed | `--md-sys-color-primary-fixed` |
| Primary Fixed Dim | `--md-sys-color-primary-fixed-dim` |
| On Primary Fixed | `--md-sys-color-on-primary-fixed` |
| On Primary Fixed Variant | `--md-sys-color-on-primary-fixed-variant` |
| Secondary Fixed | `--md-sys-color-secondary-fixed` |
| Secondary Fixed Dim | `--md-sys-color-secondary-fixed-dim` |
| On Secondary Fixed | `--md-sys-color-on-secondary-fixed` |
| On Secondary Fixed Variant | `--md-sys-color-on-secondary-fixed-variant` |
| Tertiary Fixed | `--md-sys-color-tertiary-fixed` |
| Tertiary Fixed Dim | `--md-sys-color-tertiary-fixed-dim` |
| On Tertiary Fixed | `--md-sys-color-on-tertiary-fixed` |
| On Tertiary Fixed Variant | `--md-sys-color-on-tertiary-fixed-variant` |

**注意**：固定颜色不会随主题适配，因此可能导致对比度问题。对于对比度至关重要的元素，请使用常规强调色角色。

## 色调色板系统

MD3 通过**种子色**和色调色板系统生成颜色：

### 工作原理

1. 选择一个**种子色**（十六进制值）
2. 种子色生成 **5 个色调色板**：Primary、Secondary、Tertiary、Neutral、Neutral-Variant
3. 每个色板使用 **0–100** 之间的色调停止点（常用 16 个关键停止点：0、10、20、25、30、35、40、50、60、70、80、90、95、98、99、100）
4. 色彩角色根据亮色或暗色方案映射到特定的色调值

### 色调值映射（亮色方案）

| 角色 | 色调色板 | 色调 |
|------|----------|------|
| Primary | Primary | 40 |
| On Primary | Primary | 100 |
| Primary Container | Primary | 90 |
| On Primary Container | Primary | 10 |
| Surface | Neutral | 98 |
| On Surface | Neutral | 10 |
| Surface Container | Neutral | 94 |
| Surface Container Low | Neutral | 96 |
| Surface Container Lowest | Neutral | 100 |
| Surface Container High | Neutral | 92 |
| Surface Container Highest | Neutral | 90 |
| Outline | Neutral-Variant | 50 |
| Outline Variant | Neutral-Variant | 80 |

### 色调值映射（暗色方案）

| 角色 | 色调色板 | 色调 |
|------|----------|------|
| Primary | Primary | 80 |
| On Primary | Primary | 20 |
| Primary Container | Primary | 30 |
| On Primary Container | Primary | 90 |
| Surface | Neutral | 6 |
| On Surface | Neutral | 90 |
| Surface Container | Neutral | 12 |
| Surface Container Low | Neutral | 10 |
| Surface Container Lowest | Neutral | 4 |
| Surface Container High | Neutral | 17 |
| Surface Container Highest | Neutral | 22 |
| Outline | Neutral-Variant | 60 |
| Outline Variant | Neutral-Variant | 30 |

## 配色规则

颜色只能以其预期的配对方式使用，以确保可访问的对比度：

| 容器/填充 | 文本/图标颜色 |
|-----------|---------------|
| `primary` | `on-primary` |
| `primary-container` | `on-primary-container` |
| `secondary` | `on-secondary` |
| `secondary-container` | `on-secondary-container` |
| `tertiary` | `on-tertiary` |
| `tertiary-container` | `on-tertiary-container` |
| `error` | `on-error` |
| `error-container` | `on-error-container` |
| `surface` | `on-surface` 或 `on-surface-variant` |
| `surface-container-*` | `on-surface` 或 `on-surface-variant` |
| `inverse-surface` | `inverse-on-surface` 或 `inverse-primary` |

**切勿将颜色用于非预期的配对** —— 这会破坏对比度保证，特别是在动态配色和高对比度模式下。

## 动态配色

动态配色从外部来源创建个性化的配色方案：

### 用户生成（壁纸）

操作系统从用户壁纸中提取种子色并生成配色方案。**Android：** 在 **Android 12+（API 31+）** 上使用 `dynamicLightColorScheme` / `dynamicDarkColorScheme`。**Web：** 浏览器**没有**等效的壁纸动态配色 API；你可以通过库从**内容**（如图片）中派生种子色，但这是应用特定的行为，而非系统级壁纸主题。

### 基于内容

从应用内内容（专辑封面、书籍封面等）中提取种子色，以创建上下文相关的配色方案。

### 使用 JavaScript 生成配色方案

```javascript
import {
  argbFromHex,
  themeFromSourceColor,
  applyTheme,
} from '@material/material-color-utilities';

// Generate a theme from a seed color
const theme = themeFromSourceColor(argbFromHex('#6750A4'));

// Apply to the document
applyTheme(theme, { target: document.body, dark: false });
```

### 从种子色手动生成 CSS

```javascript
import {
  argbFromHex,
  hexFromArgb,
  SchemeContent,
  Hct,
} from '@material/material-color-utilities';

function generateScheme(seedHex, isDark = false) {
  const hct = Hct.fromInt(argbFromHex(seedHex));
  const scheme = new SchemeContent(hct, isDark, 0.0); // 0.0 = standard contrast

  return {
    '--md-sys-color-primary': hexFromArgb(scheme.primary),
    '--md-sys-color-on-primary': hexFromArgb(scheme.onPrimary),
    '--md-sys-color-primary-container': hexFromArgb(scheme.primaryContainer),
    '--md-sys-color-on-primary-container': hexFromArgb(scheme.onPrimaryContainer),
    '--md-sys-color-secondary': hexFromArgb(scheme.secondary),
    '--md-sys-color-on-secondary': hexFromArgb(scheme.onSecondary),
    '--md-sys-color-secondary-container': hexFromArgb(scheme.secondaryContainer),
    '--md-sys-color-on-secondary-container': hexFromArgb(scheme.onSecondaryContainer),
    '--md-sys-color-tertiary': hexFromArgb(scheme.tertiary),
    '--md-sys-color-on-tertiary': hexFromArgb(scheme.onTertiary),
    '--md-sys-color-tertiary-container': hexFromArgb(scheme.tertiaryContainer),
    '--md-sys-color-on-tertiary-container': hexFromArgb(scheme.onTertiaryContainer),
    '--md-sys-color-error': hexFromArgb(scheme.error),
    '--md-sys-color-on-error': hexFromArgb(scheme.onError),
    '--md-sys-color-error-container': hexFromArgb(scheme.errorContainer),
    '--md-sys-color-on-error-container': hexFromArgb(scheme.onErrorContainer),
    '--md-sys-color-surface': hexFromArgb(scheme.surface),
    '--md-sys-color-on-surface': hexFromArgb(scheme.onSurface),
    '--md-sys-color-on-surface-variant': hexFromArgb(scheme.onSurfaceVariant),
    '--md-sys-color-surface-container': hexFromArgb(scheme.surfaceContainer),
    '--md-sys-color-surface-container-low': hexFromArgb(scheme.surfaceContainerLow),
    '--md-sys-color-surface-container-lowest': hexFromArgb(scheme.surfaceContainerLowest),
    '--md-sys-color-surface-container-high': hexFromArgb(scheme.surfaceContainerHigh),
    '--md-sys-color-surface-container-highest': hexFromArgb(scheme.surfaceContainerHighest),
    '--md-sys-color-outline': hexFromArgb(scheme.outline),
    '--md-sys-color-outline-variant': hexFromArgb(scheme.outlineVariant),
    '--md-sys-color-inverse-surface': hexFromArgb(scheme.inverseSurface),
    '--md-sys-color-inverse-on-surface': hexFromArgb(scheme.inverseOnSurface),
    '--md-sys-color-inverse-primary': hexFromArgb(scheme.inversePrimary),
  };
}

// Apply to document
function applyScheme(seedHex, isDark = false) {
  const tokens = generateScheme(seedHex, isDark);
  const root = document.documentElement;
  for (const [property, value] of Object.entries(tokens)) {
    root.style.setProperty(property, value);
  }
}
```

## 色彩协调

当集成不来自种子色的自定义品牌色时，使用色彩协调将其融入色调系统：

```javascript
import { Blend } from '@material/material-color-utilities';

// Harmonize a custom color with the primary color
const harmonized = Blend.harmonize(customColorArgb, primaryColorArgb);
```

这会将自定义颜色的色相略微偏向配色方案的主色，使其在保持自身特征的同时融入整体。

## 用户可控对比度（2025 年 5 月）

MD3 现支持 3 个对比度级别：
- **标准**（0.0）：默认对比度
- **中等**（0.5）：增加角色间的色调距离
- **高**（1.0）：最大色调距离，用于视觉无障碍

```javascript
// Generate high contrast scheme
const scheme = new SchemeContent(hct, isDark, 1.0); // 1.0 = high contrast
```

对比度参数调整配对角色之间的色调距离，在不改变整体色彩感觉的前提下提高可读性。

## 基准配色方案（默认值）

未使用动态配色的产品的静态基准方案：

### 亮色主题

```css
:root {
  --md-sys-color-primary: #6750A4;
  --md-sys-color-on-primary: #FFFFFF;
  --md-sys-color-primary-container: #EADDFF;
  --md-sys-color-on-primary-container: #21005D;
  --md-sys-color-secondary: #625B71;
  --md-sys-color-on-secondary: #FFFFFF;
  --md-sys-color-secondary-container: #E8DEF8;
  --md-sys-color-on-secondary-container: #1D192B;
  --md-sys-color-tertiary: #7D5260;
  --md-sys-color-on-tertiary: #FFFFFF;
  --md-sys-color-tertiary-container: #FFD8E4;
  --md-sys-color-on-tertiary-container: #31111D;
  --md-sys-color-error: #B3261E;
  --md-sys-color-on-error: #FFFFFF;
  --md-sys-color-error-container: #F9DEDC;
  --md-sys-color-on-error-container: #410E0B;
  --md-sys-color-surface: #FEF7FF;
  --md-sys-color-on-surface: #1D1B20;
  --md-sys-color-on-surface-variant: #49454F;
  --md-sys-color-surface-container-lowest: #FFFFFF;
  --md-sys-color-surface-container-low: #F7F2FA;
  --md-sys-color-surface-container: #F3EDF7;
  --md-sys-color-surface-container-high: #ECE6F0;
  --md-sys-color-surface-container-highest: #E6E0E9;
  --md-sys-color-surface-dim: #DED8E1;
  --md-sys-color-surface-bright: #FEF7FF;
  --md-sys-color-outline: #79747E;
  --md-sys-color-outline-variant: #CAC4D0;
  --md-sys-color-inverse-surface: #322F35;
  --md-sys-color-inverse-on-surface: #F5EFF7;
  --md-sys-color-inverse-primary: #D0BCFF;
}
```

### 暗色主题

```css
@media (prefers-color-scheme: dark) {
  :root {
    --md-sys-color-primary: #D0BCFF;
    --md-sys-color-on-primary: #381E72;
    --md-sys-color-primary-container: #4F378B;
    --md-sys-color-on-primary-container: #EADDFF;
    --md-sys-color-secondary: #CCC2DC;
    --md-sys-color-on-secondary: #332D41;
    --md-sys-color-secondary-container: #4A4458;
    --md-sys-color-on-secondary-container: #E8DEF8;
    --md-sys-color-tertiary: #EFB8C8;
    --md-sys-color-on-tertiary: #492532;
    --md-sys-color-tertiary-container: #633B48;
    --md-sys-color-on-tertiary-container: #FFD8E4;
    --md-sys-color-error: #F2B8B5;
    --md-sys-color-on-error: #601410;
    --md-sys-color-error-container: #8C1D18;
    --md-sys-color-on-error-container: #F9DEDC;
    --md-sys-color-surface: #141218;
    --md-sys-color-on-surface: #E6E0E9;
    --md-sys-color-on-surface-variant: #CAC4D0;
    --md-sys-color-surface-container-lowest: #0F0D13;
    --md-sys-color-surface-container-low: #1D1B20;
    --md-sys-color-surface-container: #211F26;
    --md-sys-color-surface-container-high: #2B2930;
    --md-sys-color-surface-container-highest: #36343B;
    --md-sys-color-surface-dim: #141218;
    --md-sys-color-surface-bright: #3B383E;
    --md-sys-color-outline: #938F99;
    --md-sys-color-outline-variant: #49454F;
    --md-sys-color-inverse-surface: #E6E0E9;
    --md-sys-color-inverse-on-surface: #322F35;
    --md-sys-color-inverse-primary: #6750A4;
  }
}
```

## @m3e/react 与动态配色

`@m3e/react` 完全兼容上述所有 `--md-sys-color-*` CSS 自定义属性令牌。使用 `@material/material-color-utilities` 生成的配色方案可以直接应用于 `@m3e/react` 组件。色彩令牌方法是框架无关的（基于 CSS 自定义属性），因此无论你使用 React、Vue 还是原生 Web Components，都可以统一应用。

### 在 @m3e/react 中应用动态配色

```tsx
// @m3e/react（推荐）
import { M3eTheme } from "@m3e/react/theme";
import { M3eButton } from "@m3e/react/button";
import { M3eCard } from "@m3e/react/card";
import { argbFromHex, hexFromArgb, SchemeContent, Hct } from '@material/material-color-utilities';
import { useEffect } from "react";

function applyM3eTheme(seedHex: string, isDark = false, contrast = 0.0) {
  const hct = Hct.fromInt(argbFromHex(seedHex));
  const scheme = new SchemeContent(hct, isDark, contrast);
  const root = document.documentElement;

  // 设置所有色彩令牌 — @m3e/react 自动读取
  root.style.setProperty('--md-sys-color-primary', hexFromArgb(scheme.primary));
  root.style.setProperty('--md-sys-color-on-primary', hexFromArgb(scheme.onPrimary));
  root.style.setProperty('--md-sys-color-primary-container', hexFromArgb(scheme.primaryContainer));
  root.style.setProperty('--md-sys-color-on-primary-container', hexFromArgb(scheme.onPrimaryContainer));
  root.style.setProperty('--md-sys-color-secondary', hexFromArgb(scheme.secondary));
  root.style.setProperty('--md-sys-color-on-secondary', hexFromArgb(scheme.onSecondary));
  root.style.setProperty('--md-sys-color-secondary-container', hexFromArgb(scheme.secondaryContainer));
  root.style.setProperty('--md-sys-color-on-secondary-container', hexFromArgb(scheme.onSecondaryContainer));
  root.style.setProperty('--md-sys-color-tertiary', hexFromArgb(scheme.tertiary));
  root.style.setProperty('--md-sys-color-on-tertiary', hexFromArgb(scheme.onTertiary));
  root.style.setProperty('--md-sys-color-tertiary-container', hexFromArgb(scheme.tertiaryContainer));
  root.style.setProperty('--md-sys-color-on-tertiary-container', hexFromArgb(scheme.onTertiaryContainer));
  root.style.setProperty('--md-sys-color-surface', hexFromArgb(scheme.surface));
  root.style.setProperty('--md-sys-color-on-surface', hexFromArgb(scheme.onSurface));
  root.style.setProperty('--md-sys-color-surface-container', hexFromArgb(scheme.surfaceContainer));
  root.style.setProperty('--md-sys-color-outline', hexFromArgb(scheme.outline));
  root.style.setProperty('--md-sys-color-outline-variant', hexFromArgb(scheme.outlineVariant));
  // ... 其余令牌同理
}

export default function ThemedApp() {
  useEffect(() => {
    // 应用品牌色主题
    applyM3eTheme('#6750A4');
  }, []);

  return (
    <M3eTheme>
      <M3eButton variant="filled">主题化按钮</M3eButton>
      <M3eCard variant="elevated">主题化卡片</M3eCard>
    </M3eTheme>
  );
}
```

### 从图片提取动态配色

`@m3e/react` 组件会自动响应 CSS 令牌变化，因此从图片提取种子色后只需更新令牌即可：

```javascript
import { QuantizerCelebi, Score, argbFromRgb, hexFromArgb } from '@material/material-color-utilities';

async function themeFromImage(imageUrl) {
  const img = new Image();
  img.crossOrigin = 'anonymous';
  img.src = imageUrl;
  await img.decode();

  const canvas = document.createElement('canvas');
  canvas.width = img.width;
  canvas.height = img.height;
  const ctx = canvas.getContext('2d');
  ctx.drawImage(img, 0, 0);

  const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  const pixels = [];
  for (let i = 0; i < imageData.data.length; i += 4) {
    pixels.push(argbFromRgb(imageData.data[i], imageData.data[i+1], imageData.data[i+2]));
  }

  const quantized = QuantizerCelebi.quantize(pixels, 128);
  const scored = Score.score(quantized);
  return hexFromArgb(scored[0]);
}

// 用法：从专辑封面提取主题色
const seed = await themeFromImage('/album-cover.jpg');
applyM3eTheme(seed);
// @m3e/react 所有组件自动更新色彩
```
