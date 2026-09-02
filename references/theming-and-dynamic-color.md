# MD3 主题与动态配色

创建、应用和管理 Material Design 3 主题的完整指南。

## 主题架构

相同的**语义角色**（primary、onSurface、surface container 等）出现在每个平台上：

| 平台 | 主题接口 |
|----------|----------------|
| **Jetpack Compose** | `MaterialTheme(colorScheme, typography, shapes, …)` |
| **Flutter** | `ThemeData` + `ColorScheme`, `useMaterial3: true` |
| **Web** | CSS 自定义属性 `--md-sys-*`，作用于 `:root` 或子树 |

**Web 推荐：** 使用 `@m3e/react`（`<M3eTheme>` 组件）实现完整的 Material 3 Expressive 主题。`@material/web` 仅处于维护模式，M3 Expressive 尚未在其上实现。

---

## @m3e/react 主题系统（推荐）

`@m3e/react` 提供了 `<M3eTheme>` 组件，统一管理动态配色、弹性动效、密度和焦点指示器。

### React 用法（推荐）

```tsx
import { M3eTheme } from "@m3e/react/theme";
import { M3eButton } from "@m3e/react/button";
import { M3eCard } from "@m3e/react/card";

<M3eTheme
  colorScheme="light"
  contrast="medium"
  motion="expressive"
  density="comfortable"
>
  {/* 所有子组件自动继承主题 */}
  <M3eButton variant="filled">主题化按钮</M3eButton>
  <M3eCard variant="elevated">主题化卡片</M3eCard>
</M3eTheme>
```

### `<M3eTheme>` 属性

| 属性 | 类型 | 描述 |
|------|------|------|
| `colorScheme` | string | `light`、`dark`、`system`（跟随系统） |
| `contrast` | string | `default`、`medium`、`high` |
| `motion` | string | `standard`（标准缓动）、`expressive`（弹性动效） |
| `density` | string | `compact`、`comfortable`、`spacious` |
| `direction` | string | `ltr`、`rtl` |
| `textSize` | string | `100%`、`150%`、`200%` |

### 动态切换主题

```tsx
// @m3e/react（推荐）
import { M3eTheme } from "@m3e/react/theme";
import { useRef } from "react";

const themeRef = useRef<HTMLElement>(null);

// 切换深色模式
if (themeRef.current) {
  themeRef.current.colorScheme = themeRef.current.colorScheme === 'dark' ? 'light' : 'dark';
}

// 切换对比度
if (themeRef.current) {
  themeRef.current.contrast = 'high';
}

// 切换动效模式
if (themeRef.current) {
  themeRef.current.motion = 'expressive';
}
```

### @m3e/react 与 @material/web 令牌兼容性

`@m3e/react` 使用与 `@material/web` 相同的 `--md-sys-color-*` CSS 自定义属性体系，因此为 `@material/web` 编写的主题 CSS 可以直接用于 `@m3e/react`。迁移时只需更换组件导入和元素名。

---

## Web：Theme Builder 和 CSS（@material/web / @m3e/react 通用）

使用 `androidx.compose.material3.MaterialTheme`。在 **Android 12+（API 31+）** 上启用时优先使用**动态配色**；否则使用 `lightColorScheme` / `darkColorScheme` 或由 [Material Theme Builder](https://material-foundation.github.io/material-theme-builder/) 生成的 Kotlin 代码。

```kotlin
import androidx.compose.material3.*
import android.os.Build
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.ui.platform.LocalContext

@Composable
fun MyAppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> darkColorScheme(
            primary = Color(0xFFD0BCFF),
            onPrimary = Color(0xFF381E72),
            // … 添加其余角色或使用生成的 Theme.kt
        )
        else -> lightColorScheme(
            primary = Color(0xFF6750A4),
            onPrimary = Color(0xFFFFFFFF),
            // … 添加其余角色或使用生成的 Theme.kt
        )
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography(),
        shapes = Shapes(),
        content = content
    )
}
```

---

## Flutter 主题

```dart
import 'package:flutter/material.dart';

// 基于种子色的基础 MD3 主题
MaterialApp(
  theme: ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.deepPurple,
      brightness: Brightness.light,
    ),
  ),
  darkTheme: ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.deepPurple,
      brightness: Brightness.dark,
    ),
  ),
);

// 动态配色（Android 12+）
// 需要 package:dynamic_color
DynamicColorBuilder(
  builder: (lightDynamic, darkDynamic) {
    return MaterialApp(
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: lightDynamic ?? ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: darkDynamic ?? ColorScheme.fromSeed(
          seedColor: Colors.deepPurple,
          brightness: Brightness.dark,
        ),
      ),
    );
  },
);
```

---

## Web：主题构建器与 CSS

### 1. 选择种子色
从一个代表品牌的十六进制颜色开始。整个配色方案将基于此种子色生成。

### 2. 生成配色方案
使用 `@material/material-color-utilities` 生成亮色和暗色配色方案：

```bash
npm install @material/material-color-utilities
```

```javascript
import {
  argbFromHex,
  hexFromArgb,
  SchemeContent,
  Hct,
} from '@material/material-color-utilities';

function generateTheme(seedHex, isDark = false, contrast = 0.0) {
  const hct = Hct.fromInt(argbFromHex(seedHex));
  const scheme = new SchemeContent(hct, isDark, contrast);

  return {
    // Primary
    '--md-sys-color-primary': hexFromArgb(scheme.primary),
    '--md-sys-color-on-primary': hexFromArgb(scheme.onPrimary),
    '--md-sys-color-primary-container': hexFromArgb(scheme.primaryContainer),
    '--md-sys-color-on-primary-container': hexFromArgb(scheme.onPrimaryContainer),
    // Secondary
    '--md-sys-color-secondary': hexFromArgb(scheme.secondary),
    '--md-sys-color-on-secondary': hexFromArgb(scheme.onSecondary),
    '--md-sys-color-secondary-container': hexFromArgb(scheme.secondaryContainer),
    '--md-sys-color-on-secondary-container': hexFromArgb(scheme.onSecondaryContainer),
    // Tertiary
    '--md-sys-color-tertiary': hexFromArgb(scheme.tertiary),
    '--md-sys-color-on-tertiary': hexFromArgb(scheme.onTertiary),
    '--md-sys-color-tertiary-container': hexFromArgb(scheme.tertiaryContainer),
    '--md-sys-color-on-tertiary-container': hexFromArgb(scheme.onTertiaryContainer),
    // Error
    '--md-sys-color-error': hexFromArgb(scheme.error),
    '--md-sys-color-on-error': hexFromArgb(scheme.onError),
    '--md-sys-color-error-container': hexFromArgb(scheme.errorContainer),
    '--md-sys-color-on-error-container': hexFromArgb(scheme.onErrorContainer),
    // Surface
    '--md-sys-color-surface': hexFromArgb(scheme.surface),
    '--md-sys-color-on-surface': hexFromArgb(scheme.onSurface),
    '--md-sys-color-on-surface-variant': hexFromArgb(scheme.onSurfaceVariant),
    '--md-sys-color-surface-container-lowest': hexFromArgb(scheme.surfaceContainerLowest),
    '--md-sys-color-surface-container-low': hexFromArgb(scheme.surfaceContainerLow),
    '--md-sys-color-surface-container': hexFromArgb(scheme.surfaceContainer),
    '--md-sys-color-surface-container-high': hexFromArgb(scheme.surfaceContainerHigh),
    '--md-sys-color-surface-container-highest': hexFromArgb(scheme.surfaceContainerHighest),
    '--md-sys-color-surface-dim': hexFromArgb(scheme.surfaceDim),
    '--md-sys-color-surface-bright': hexFromArgb(scheme.surfaceBright),
    // Outline
    '--md-sys-color-outline': hexFromArgb(scheme.outline),
    '--md-sys-color-outline-variant': hexFromArgb(scheme.outlineVariant),
    // Inverse
    '--md-sys-color-inverse-surface': hexFromArgb(scheme.inverseSurface),
    '--md-sys-color-inverse-on-surface': hexFromArgb(scheme.inverseOnSurface),
    '--md-sys-color-inverse-primary': hexFromArgb(scheme.inversePrimary),
  };
}
```

### 3. 应用主题

```javascript
function applyTheme(seedHex, isDark = false, contrast = 0.0) {
  const tokens = generateTheme(seedHex, isDark, contrast);
  const root = document.documentElement;
  for (const [prop, value] of Object.entries(tokens)) {
    root.style.setProperty(prop, value);
  }
}

// 应用亮色主题
applyTheme('#1A73E8');

// 应用暗色主题
applyTheme('#1A73E8', true);

// 应用高对比度
applyTheme('#1A73E8', false, 1.0);
```

### 4. 导出为 CSS

```javascript
function exportAsCSS(seedHex) {
  const light = generateTheme(seedHex, false);
  const dark = generateTheme(seedHex, true);

  let css = ':root {\n';
  for (const [prop, value] of Object.entries(light)) {
    css += `  ${prop}: ${value};\n`;
  }
  css += '}\n\n';
  css += '@media (prefers-color-scheme: dark) {\n  :root {\n';
  for (const [prop, value] of Object.entries(dark)) {
    css += `    ${prop}: ${value};\n`;
  }
  css += '  }\n}\n';
  return css;
}
```

## 品牌色集成

### 映射现有品牌色

如果已有品牌色，可将其映射到 MD3 角色：

| 品牌概念 | MD3 角色 |
|--------------|----------|
| 主品牌色 | 用作 `primary` 调色板的种子色 |
| 辅助品牌色 | 覆盖 `secondary` 或用作自定义颜色 |
| 强调色 | 映射到 `tertiary` |
| 警告/危险色 | 覆盖 `error`（或保留 MD3 默认值） |
| 背景色 | 从种子色生成（不要硬编码） |

### 将品牌色用作种子色

最简单的方法：将主品牌色用作种子色。算法会自动生成协调的辅助色和第三色。

```javascript
// 品牌主色蓝色
applyTheme('#1A73E8');
```

### 额外品牌色的色彩协调

如果需要集成一个并非从种子色生成的特定品牌色：

```javascript
import { Blend, argbFromHex, hexFromArgb } from '@material/material-color-utilities';

// 品牌的橙色强调色
const brandOrange = argbFromHex('#FF6D00');
// 生成的主色
const schemePrimary = argbFromHex('#1A73E8');

// 色彩协调：将色相稍微向主色偏移
const harmonizedOrange = hexFromArgb(Blend.harmonize(brandOrange, schemePrimary));
// 在主题中使用 harmonizedOrange 作为自定义颜色
```

## 暗色主题

### 自动生成

暗色主题基于相同的种子色自动生成。色调映射的变化如下：
- 亮色主题对表面使用较浅的色调（80-100），对强调色使用较深的色调（10-40）
- 暗色主题则反转：对表面使用较深的色调（4-22），对强调色使用较浅的色调（80-90）

### CSS 实现

```css
/* 亮色主题（默认） */
:root {
  --md-sys-color-primary: #6750A4;
  --md-sys-color-surface: #FEF7FF;
  /* ... 所有亮色令牌 ... */
}

/* 通过媒体查询应用暗色主题 */
@media (prefers-color-scheme: dark) {
  :root {
    --md-sys-color-primary: #D0BCFF;
    --md-sys-color-surface: #141218;
    /* ... 所有暗色令牌 ... */
  }
}

/* 通过类名应用暗色主题（用于手动切换） */
.dark-theme {
  --md-sys-color-primary: #D0BCFF;
  --md-sys-color-surface: #141218;
  /* ... 所有暗色令牌 ... */
}
```

### 运行时主题切换

```javascript
class ThemeManager {
  constructor(seedHex) {
    this.seedHex = seedHex;
    this.isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    this.contrast = 0.0; // 标准

    // 监听系统主题变化
    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
      this.isDark = e.matches;
      this.apply();
    });
  }

  apply() {
    const tokens = generateTheme(this.seedHex, this.isDark, this.contrast);
    const root = document.documentElement;
    for (const [prop, value] of Object.entries(tokens)) {
      root.style.setProperty(prop, value);
    }
    root.setAttribute('data-theme', this.isDark ? 'dark' : 'light');
  }

  toggleDark() {
    this.isDark = !this.isDark;
    this.apply();
  }

  setContrast(level) {
    // 0.0 = 标准, 0.5 = 中等, 1.0 = 高
    this.contrast = level;
    this.apply();
  }

  setSeed(hex) {
    this.seedHex = hex;
    this.apply();
  }
}

// 用法
const theme = new ThemeManager('#6750A4');
theme.apply();

// 切换暗色模式
document.getElementById('theme-toggle').addEventListener('click', () => theme.toggleDark());
```

## 高对比度主题

MD3 支持 3 个对比度级别，可通过 `contrast` 参数调节：

| 级别 | 值 | 效果 |
|-------|-------|--------|
| 标准 | 0.0 | 默认色调距离 |
| 中等 | 0.5 | 增大色调距离，更易阅读 |
| 高 | 1.0 | 最大色调距离，最高可读性 |

```javascript
// 标准对比度
const standard = new SchemeContent(hct, isDark, 0.0);

// 中等对比度
const medium = new SchemeContent(hct, isDark, 0.5);

// 高对比度
const high = new SchemeContent(hct, isDark, 1.0);
```

更高的对比度会增加成对颜色角色（如 `primary` 和 `on-primary`）之间的色调距离，在不根本改变色彩感觉的前提下使文本更加清晰可读。

## 组件级覆盖（Web）

使用组件专属的 CSS 自定义属性覆盖单个 **Web** 组件的外观。**Compose** 则使用 `TextFieldDefaults`、`ButtonDefaults`、`MaterialTheme` 等。

```css
/* 覆盖特定按钮颜色 */
md-filled-button {
  --md-filled-button-container-color: var(--md-sys-color-tertiary);
  --md-filled-button-label-text-color: var(--md-sys-color-on-tertiary);
}

/* 覆盖文本字段形状 */
md-outlined-text-field {
  --md-outlined-text-field-container-shape: var(--md-sys-shape-corner-medium);
}

/* 覆盖 FAB */
md-fab {
  --md-fab-container-color: var(--md-sys-color-tertiary-container);
  --md-fab-icon-color: var(--md-sys-color-on-tertiary-container);
}

/* 覆盖开关颜色 */
md-switch {
  --md-switch-selected-track-color: var(--md-sys-color-primary);
  --md-switch-selected-handle-color: var(--md-sys-color-on-primary);
}
```

### 组件令牌命名规则

组件令牌遵循：`--md-{component}-{element}-{property}`

示例：
- `--md-filled-button-container-color`
- `--md-filled-button-container-shape`
- `--md-filled-button-label-text-color`
- `--md-outlined-text-field-outline-color`
- `--md-outlined-text-field-focus-outline-color`
- `--md-fab-container-color`
- `--md-fab-container-shape`
- `--md-switch-selected-track-color`

## 基于内容的动态配色

从图像中提取颜色并将其应用为主题：

```javascript
import {
  QuantizerCelebi,
  Score,
  argbFromRgb,
} from '@material/material-color-utilities';

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

  // 量化并评分以找到最佳种子色
  const quantized = QuantizerCelebi.quantize(pixels, 128);
  const scored = Score.score(quantized);
  const seedColor = scored[0]; // 最佳颜色

  return hexFromArgb(seedColor);
}

// 用法
const seed = await themeFromImage('/album-cover.jpg');
applyTheme(seed);
```

## 作用域主题

为 UI 的不同区域应用不同的主题：

```css
/* 默认主题 */
:root {
  --md-sys-color-primary: #6750A4;
  /* ... */
}

/* 某个区域的作用域主题 */
.premium-section {
  --md-sys-color-primary: #B69DF8;
  --md-sys-color-primary-container: #3F2D7A;
  /* 仅覆盖需要变更的部分 */
}
```

```html
<main>
  <!-- 使用默认主题 -->
  <section class="regular">
    <md-filled-button>Standard</md-filled-button>
  </section>

  <!-- 使用高级主题 -->
  <section class="premium-section">
    <md-filled-button>Premium</md-filled-button>
  </section>
</main>
```

CSS 自定义属性具有级联特性，因此子元素会自动继承最近祖先元素的令牌。
