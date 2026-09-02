# MD3 组件目录

Material Design 3 组件完整参考。**主要映射：** Jetpack Compose (`androidx.compose.material3`) 是当前大多数用户发布 UI 的平台；**Web** 推荐使用 **`@m3e/react`**（React 项目首选，完整 TypeScript 支持和 React 惯用 API）；`@material/web` 已进入仅维护状态，组件较少。

**不推荐** MUI、MWC 等库 — 它们没有真正实现 M3E（Material 3 Expressive）。

## Web 实现选择

| 实现库 | 元素前缀 | 包名 | Expressive 支持 | 状态 |
|--------|---------|------|-----------------|------|
| **@m3e/react（推荐）** | `M3e*` (React 组件) | `@m3e/react/{component}` | ✅ 完整 | 活跃开发 |
| @material/web | `md-*` | `@material/web/{category}/{component}.js` | ❌ 无 | 仅维护 |

> @m3e/web 提供等效的 Web Components 元素（`m3e-button`、`m3e-card` 等），适用于非 React 框架。详见 <https://matraic.github.io/m3e>。

**@m3e 独有的组件**（@material/web 不提供的）：
- `M3eCard`、`M3eBadge`、`M3eTooltip`、`M3eSnackbar`、`M3eLoadingIndicator`
- `M3eBottomSheet`、`M3eSearch`、`M3eAutocomplete`、`M3eSelect`
- `M3eNavRail`、`M3eNavMenu`、`M3eAppBar`、`M3eToolbar`、`M3eBreadcrumb`
- `M3eFabMenu`、`M3eSplitButton`、`M3eSegmentedButton`
- `M3eDatepicker`、`M3eTimepicker`、`M3eCalendar`、`M3eDateInput`
- `M3eSkeleton`、`M3eAvatar`、`M3eHeading`、`M3eShape`、`M3eTheme`
- `M3eStepper`、`M3eTree`、`M3eToc`、`M3ePaginator`
- `M3eExpansionPanel`、`M3eSplitPane`、`M3eContentPane`、`M3eSlideGroup`
- `M3eDrawerContainer`、`M3eFormField`、`M3eTextareaAutosize`

> **文档与示例：** <https://matraic.github.io/m3e>

## Google I/O 2026 组件更新

Material 的 [I/O 2026 更新](https://m3.material.io/blog/whats-new-at-io26) 重点刷新了 **列表**、**菜单**、**搜索** 和 **搜索应用栏** 的表现力指南，以 Jetpack Compose 作为主要实现路径。在 Android 中实现时：

- 优先使用当前的 `androidx.compose.material3` 组件，并根据当前使用的 Material3 BOM 版本验证表现力 API。
- 表现力变体预计将包含更丰富的视觉风格、动效和灵活的配置选项。
- Web 实现推荐使用 `@m3e/react`，它原生支持 Expressive 特性；`@material/web` 仅为维护模式的规范对齐近似方案。

## 操作

### 按钮

MD3 有 5 种按钮类型，按强调程度排列：Filled > Filled Tonal > Elevated > Outlined > Text。

**通用属性**（所有按钮类型共享）：

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `disabled` | boolean | 禁用按钮 |
| `href` | string | 将按钮转换为链接 |
| `target` | string | 链接目标（`_blank` 等） |
| `trailingIcon` | boolean | 将图标移动到末尾位置 |
| `type` | string | 表单类型：`button`、`submit`、`reset` |

#### 填充按钮

**使用场景**：主要操作，最高强调度。

> **@m3e/react 推荐：** `M3eButton`（`@m3e/react/button`，设置 `variant="filled"`）。

```tsx
import { M3eButton } from '@m3e/react/button';

<M3eButton variant="filled">Get started</M3eButton>
<M3eButton variant="filled" href="/signup" icon="arrow_forward" trailingIcon>
  Sign up
</M3eButton>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-filled-button` | **导入**：`@material/web/button/filled-button.js`

```html
<md-filled-button>Get started</md-filled-button>
<md-filled-button href="/signup">
  <md-icon slot="icon">arrow_forward</md-icon>
  Sign up
</md-filled-button>
```

**自定义**：`--md-filled-button-container-color`、`--md-filled-button-label-text-color`、`--md-filled-button-container-shape`、`--md-filled-button-container-height`
</details>

#### 填充色调按钮

**使用场景**：中等强调度，比填充按钮更柔和。适合与填充按钮并排放置的次要操作。

> **@m3e/react 推荐：** `M3eButton`（`@m3e/react/button`，设置 `variant="tonal"`）。

```tsx
import { M3eButton } from '@m3e/react/button';

<M3eButton variant="tonal">Save draft</M3eButton>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-filled-tonal-button` | **导入**：`@material/web/button/filled-tonal-button.js`

```html
<md-filled-tonal-button>Save draft</md-filled-tonal-button>
```

**自定义**：`--md-filled-tonal-button-container-color`、`--md-filled-tonal-button-label-text-color`
</details>

#### 凸起按钮

**使用场景**：带阴影的中等强调度按钮。适用于色调按钮会与背景融合的彩色背景上。

> **@m3e/react 推荐：** `M3eButton`（`@m3e/react/button`，设置 `variant="elevated"`）。

```tsx
import { M3eButton } from '@m3e/react/button';

<M3eButton variant="elevated">Add to cart</M3eButton>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-elevated-button` | **导入**：`@material/web/button/elevated-button.js`

```html
<md-elevated-button>Add to cart</md-elevated-button>
```
</details>

#### 描边按钮

**使用场景**：中等强调度，中性。适合次要操作。

> **@m3e/react 推荐：** `M3eButton`（`@m3e/react/button`，设置 `variant="outlined"`）。

```tsx
import { M3eButton } from '@m3e/react/button';

<M3eButton variant="outlined">Cancel</M3eButton>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-outlined-button` | **导入**：`@material/web/button/outlined-button.js`

```html
<md-outlined-button>Cancel</md-outlined-button>
```

**自定义**：`--md-outlined-button-outline-color`、`--md-outlined-button-outline-width`
</details>

#### 文字按钮

**使用场景**：最低强调度。内联操作、对话框操作、不太重要的选项。

> **@m3e/react 推荐：** `M3eButton`（`@m3e/react/button`，设置 `variant="text"`）。

```tsx
import { M3eButton } from '@m3e/react/button';

<M3eButton variant="text">Learn more</M3eButton>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-text-button` | **导入**：`@material/web/button/text-button.js`

```html
<md-text-button>Learn more</md-text-button>
```
</details>

#### 按钮尺寸（表现力）

按钮现支持 5 种尺寸：extra-small、small（默认）、medium、large、extra-large。

```tsx
<M3eButton variant="filled" size="xs">Extra Small</M3eButton>
<M3eButton variant="filled" size="sm">Small (默认)</M3eButton>
<M3eButton variant="filled" size="md">Medium</M3eButton>
<M3eButton variant="filled" size="lg">Large</M3eButton>
<M3eButton variant="filled" size="xl">Extra Large</M3eButton>
```

<details>
<summary>@material/web 备选（通过 CSS 设置）</summary>

```css
md-filled-button { --md-filled-button-container-height: 32px; } /* XS */
md-filled-button { --md-filled-button-container-height: 40px; } /* S (default) */
md-filled-button { --md-filled-button-container-height: 48px; } /* M */
md-filled-button { --md-filled-button-container-height: 56px; } /* L */
md-filled-button { --md-filled-button-container-height: 64px; } /* XL */
```
</details>

**无障碍**：按钮内置 button 角色。使用纯图标按钮时请使用 `aria-label`。最小触摸目标 48x48dp。

### 按钮组

**使用场景**：将相关操作组合在一起，使用连接的视觉处理方式。

> **@m3e/react 推荐：** `M3eButtonGroup`（`@m3e/react/button-group`）。

```tsx
import { M3eButtonGroup } from '@m3e/react/button-group';
import { M3eButton } from '@m3e/react/button';

<M3eButtonGroup>
  <M3eButton variant="outlined">Day</M3eButton>
  <M3eButton variant="outlined">Week</M3eButton>
  <M3eButton variant="outlined">Month</M3eButton>
</M3eButtonGroup>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-button-group` | **导入**：`@material/web/button/button-group.js`

```html
<md-button-group>
  <md-outlined-button>Day</md-outlined-button>
  <md-outlined-button>Week</md-outlined-button>
  <md-outlined-button>Month</md-outlined-button>
</md-button-group>
```
</details>

### 浮动操作按钮 (FAB)

**使用场景**：屏幕上最重要的单个操作。

> **@m3e/react 推荐：** `M3eFab`（`@m3e/react/fab`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `size` | string | `small`、`medium`（默认）、`large` |
| `variant` | string | `surface`、`primary`、`secondary`、`tertiary` |
| `label` | string | 文本标签（用于扩展 FAB） |

```tsx
import { M3eFab } from '@m3e/react/fab';

<M3eFab aria-label="Create new" icon="add" />

<M3eFab size="small" variant="tertiary" aria-label="Edit" icon="edit" />
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-fab` | **导入**：`@material/web/fab/fab.js`

```html
<md-fab aria-label="Create new">
  <md-icon slot="icon">add</md-icon>
</md-fab>

<md-fab size="small" variant="tertiary" aria-label="Edit">
  <md-icon slot="icon">edit</md-icon>
</md-fab>
```

**自定义**：`--md-fab-container-color`、`--md-fab-container-shape`、`--md-fab-icon-color`
</details>

**无障碍**：由于 FAB 通常为纯图标，请始终提供 `aria-label`。

### 扩展 FAB

**使用场景**：带说明文字的主要操作。

> **@m3e/react 推荐：** `M3eFab`（`@m3e/react/fab`，设置 `extended` 和 `label`）。

```tsx
import { M3eFab } from '@m3e/react/fab';

<M3eFab extended label="New message" icon="edit" />
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-extended-fab` | **导入**：`@material/web/fab/extended-fab.js`

```html
<md-extended-fab label="New message">
  <md-icon slot="icon">edit</md-icon>
</md-extended-fab>
```
</details>

### 图标按钮

> **@m3e/react 推荐：** `M3eIconButton`（`@m3e/react/icon-button`）。

4 种变体：

| 变体 | @m3e/react 组件 | @material/web 元素 |
|---------|---------|--------|
| Standard | `M3eIconButton` | `md-icon-button` |
| Filled | `M3eIconButton` (`variant="filled"`) | `md-filled-icon-button` |
| Filled Tonal | `M3eIconButton` (`variant="tonal"`) | `md-filled-tonal-icon-button` |
| Outlined | `M3eIconButton` (`variant="outlined"`) | `md-outlined-icon-button` |

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `toggle` | boolean | 启用切换行为 |
| `selected` | boolean | 选中状态（当启用 toggle 时） |

```tsx
import { M3eIconButton } from '@m3e/react/icon-button';

<M3eIconButton aria-label="Settings" icon="settings" />

{/* 切换图标按钮（点赞/取消点赞） */}
<M3eIconButton toggle aria-label="Favorite" icon="favorite_border" selectedIcon="favorite" />
```

<details>
<summary>@material/web 备选</summary>

**导入**：`@material/web/iconbutton/icon-button.js`（及各变体）

```html
<md-icon-button aria-label="Settings">
  <md-icon>settings</md-icon>
</md-icon-button>

<!-- 切换图标按钮（点赞/取消点赞） -->
<md-icon-button toggle aria-label="Favorite">
  <md-icon>favorite_border</md-icon>
  <md-icon slot="selected">favorite</md-icon>
</md-icon-button>
```
</details>

**无障碍**：始终提供 `aria-label`。切换按钮应为两种状态都提供描述性标签。

### 分段按钮

**使用场景**：在 2–5 个互斥选项之间切换视图或排序方式。

> **@m3e/react 推荐：** `M3eSegmentedButton`（`@m3e/react/segmented-button`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | string | 当前选中的分段值 |
| `multiple` | boolean | 允许多选（默认 false） |
| `disabled` | boolean | 禁用整个分段按钮组 |

```tsx
import { M3eSegmentedButton, M3eSegmentedButtonItem } from '@m3e/react/segmented-button';

<M3eSegmentedButton value="list" aria-label="View options">
  <M3eSegmentedButtonItem value="list" label="List" icon="view_list" />
  <M3eSegmentedButtonItem value="grid" label="Grid" icon="grid_view" />
  <M3eSegmentedButtonItem value="map" label="Map" icon="map" />
</M3eSegmentedButton>
```

**无障碍**：内置 `group` 角色。提供 `aria-label` 描述用途。每个分段项自动具有 `aria-pressed` 状态。

## 通信

### 徽章

**使用场景**：在图标或头像上显示未读计数、状态指示或通知数量。

> **@m3e/react 推荐：** `M3eBadge`（`@m3e/react/badge`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `content` | string/number | 徽章内容（数字或文本） |
| `dot` | boolean | 仅显示圆点（无内容） |
| `color` | string | 颜色变体（`error`、`primary`、`tertiary`） |
| `max` | number | 最大值（超出显示 `max+`） |

```tsx
import { M3eBadge } from '@m3e/react/badge';

{/* 带数字的徽章 */}
<M3eBadge content={3}>
  <M3eIcon>notifications</M3eIcon>
</M3eBadge>

{/* 仅圆点 */}
<M3eBadge dot>
  <M3eIcon>mail</M3eIcon>
</M3eBadge>

{/* 带最大值 */}
<M3eBadge content={150} max={99}>
  <M3eIcon>shopping_cart</M3eIcon>
</M3eBadge>
```

**无障碍**：使用 `aria-label` 为屏幕阅读器提供完整描述（例如 "3 条未读通知"）。

### 进度指示器

**使用场景**：显示操作的进度状态。

> **@m3e/react 推荐：** `M3eLinearProgress`、`M3eCircularProgress`（`@m3e/react/progress`）。另有 `M3eLoadingIndicator`（`@m3e/react/loading-indicator`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `value` | number | 当前进度（0–1） |
| `max` | number | 最大值（默认 1） |
| `indeterminate` | boolean | 显示不确定动画 |
| `fourColor` | boolean | 四色不确定变体 |

```tsx
import { M3eLinearProgress, M3eCircularProgress } from '@m3e/react/progress';

{/* 确定进度 */}
<M3eLinearProgress value={0.6} />
<M3eCircularProgress value={0.75} />

{/* 不确定进度 */}
<M3eLinearProgress indeterminate />
<M3eCircularProgress indeterminate />
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-linear-progress`、`md-circular-progress`
**导入**：`@material/web/progress/linear-progress.js`、`@material/web/progress/circular-progress.js`

```html
<!-- 确定进度 -->
<md-linear-progress value="0.6"></md-linear-progress>
<md-circular-progress value="0.75"></md-circular-progress>

<!-- 不确定进度 -->
<md-linear-progress indeterminate></md-linear-progress>
<md-circular-progress indeterminate></md-circular-progress>
```

**自定义**：`--md-linear-progress-active-indicator-color`、`--md-circular-progress-active-indicator-color`
</details>

**无障碍**：内置 `progressbar` 角色。请添加 `aria-label` 提供上下文（例如 "Loading messages"）。

### 提示条

**使用场景**：在屏幕底部显示简短的反馈信息，可带操作按钮。

> **@m3e/react 推荐：** `M3eSnackbar`（`@m3e/react/snackbar`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `message` | string | 提示文本 |
| `actionText` | string | 操作按钮文本 |
| `open` | boolean | 显示状态 |
| `timeout` | number | 自动关闭时间（毫秒，默认 5000） |
| `dismissButton` | boolean | 显示关闭按钮 |

```tsx
import { M3eSnackbar } from '@m3e/react/snackbar';

{/* 基础提示 */}
<M3eSnackbar message="Message sent" open />

{/* 带操作按钮 */}
<M3eSnackbar
  message="Message deleted"
  actionText="Undo"
  open
  onAction={handleUndo}
/>

{/* 带关闭按钮 */}
<M3eSnackbar
  message="File uploaded"
  dismissButton
  open
/>
```

**无障碍**：内置 `role="status"` 和 `aria-live="polite"`。操作按钮应使用动词描述（如 "撤销"、"重试"）。

### 工具提示

**使用场景**：为图标按钮、截断文本或其他交互元素提供悬停/聚焦时的上下文说明。

> **@m3e/react 推荐：** `M3eTooltip`（`@m3e/react/tooltip`）。

两种类型：
- **Plain**：简短文本标签，在悬停/聚焦时显示。用于图标按钮和截断文本。
- **Rich**：多行内容，可带操作按钮。用于复杂说明。

| 属性 | 类型 | 描述 |
|------|------|------|
| `content` | string | 提示文本 |
| `position` | string | 位置（`top`、`bottom`、`start`、`end`） |
| `delay` | number | 延迟显示时间（毫秒） |

```tsx
import { M3eTooltip } from '@m3e/react/tooltip';
import { M3eIconButton } from '@m3e/react/icon-button';

{/* Plain tooltip */}
<M3eTooltip content="Delete item" position="top">
  <M3eIconButton aria-label="Delete" icon="delete" />
</M3eTooltip>

{/* Rich tooltip */}
<M3eTooltip position="bottom" content={
  <>
    <strong>File size</strong>
    <p>This document is 2.4 MB and was last edited 3 days ago.</p>
  </>
}>
  <M3eIconButton icon="info" />
</M3eTooltip>
```

**无障碍**：工具提示内容应简明扼要。对于纯图标按钮，`aria-label` 仍然是必需的，tooltip 是补充而非替代。

## 容器

### 卡片

**使用场景**：将相关内容和操作组合在一起，显示为一个独立的视觉单元。

> **@m3e/react 推荐：** `M3eCard`（`@m3e/react/card`）。

三种变体：

| 变体 | 外观 | 高度层级 |
|---------|-----------|-----------|
| Filled | Surface-container-highest 填充，无边框 | Level 0 |
| Outlined | Surface 填充，outline-variant 边框 | Level 0 |
| Elevated | Surface-container-low 填充，带阴影 | Level 1 |

| 属性 | 类型 | 描述 |
|------|------|------|
| `variant` | string | 变体类型（`filled`、`outlined`、`elevated`） |
| `interactive` | boolean | 启用悬停和点击效果 |

```tsx
import { M3eCard } from '@m3e/react/card';
import { M3eButton } from '@m3e/react/button';

{/* 描边卡片（推荐用于大多数场景） */}
<M3eCard variant="outlined">
  <M3eCard.Content>
    <h3>Project Update</h3>
    <p>The new design system is ready for review.</p>
  </M3eCard.Content>
  <M3eCard.Actions>
    <M3eButton variant="tonal">Review</M3eButton>
  </M3eCard.Actions>
</M3eCard>

{/* 填充卡片 */}
<M3eCard variant="filled" interactive>
  <M3eCard.Media src="hero.jpg" alt="Hero image" />
  <M3eCard.Content>
    <h3>Featured Article</h3>
    <p>Read the latest updates on Material Design 3.</p>
  </M3eCard.Content>
</M3eCard>

{/* 凸起卡片 */}
<M3eCard variant="elevated">
  <M3eCard.Content>
    <M3eIcon>trending_up</M3eIcon>
    <h3>Analytics</h3>
    <p>View your performance metrics.</p>
  </M3eCard.Content>
</M3eCard>
```

**无障碍**：如果卡片可点击，使用 `interactive` 属性并添加适当的 `aria-label` 或 `role`。

### 对话框

**使用场景**：显示需要用户响应的模态内容。

> **@m3e/react 推荐：** `M3eDialog`（`@m3e/react/dialog`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `open` | boolean | 显示对话框 |
| `type` | string | `alert`（默认） |

```tsx
import { M3eDialog } from '@m3e/react/dialog';
import { M3eButton } from '@m3e/react/button';

<M3eDialog open>
  <M3eDialog.Headline>Confirm action</M3eDialog.Headline>
  <M3eDialog.Content>
    Are you sure you want to proceed?
  </M3eDialog.Content>
  <M3eDialog.Actions>
    <M3eButton variant="text" onClick={handleCancel}>Cancel</M3eButton>
    <M3eButton variant="tonal" onClick={handleConfirm}>Confirm</M3eButton>
  </M3eDialog.Actions>
</M3eDialog>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-dialog` | **导入**：`@material/web/dialog/dialog.js`

```html
<md-dialog id="confirm-dialog">
  <div slot="headline">Confirm action</div>
  <form slot="content" method="dialog">
    Are you sure you want to proceed?
  </form>
  <div slot="actions">
    <md-text-button form="confirm-dialog" value="cancel">Cancel</md-text-button>
    <md-filled-tonal-button form="confirm-dialog" value="confirm">Confirm</md-filled-tonal-button>
  </div>
</md-dialog>
```
</details>

**无障碍**：对话框会自动捕获焦点。使用 `Headline` 子组件提供可访问的标题。

### 底部表单

**使用场景**：从屏幕底部滑入的面板，用于补充内容或次要操作。两种变体：Standard（持久型，与内容共存）和 Modal（模态型，阻止交互，带有遮罩层）。

> **@m3e/react 推荐：** `M3eBottomSheet`（`@m3e/react/bottom-sheet`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `open` | boolean | 显示状态 |
| `modal` | boolean | 启用模态模式（带遮罩层） |
| `peekHeight` | number | 预览高度（像素） |
| `expandable` | boolean | 允许拖动展开 |

```tsx
import { M3eBottomSheet } from '@m3e/react/bottom-sheet';

{/* Standard 底部表单 */}
<M3eBottomSheet open peekHeight={200}>
  <M3eBottomSheet.Header>
    <h3>Now Playing</h3>
  </M3eBottomSheet.Header>
  <M3eBottomSheet.Content>
    <p>Song title - Artist</p>
    <div className="controls">...</div>
  </M3eBottomSheet.Content>
</M3eBottomSheet>

{/* Modal 底部表单 */}
<M3eBottomSheet modal open>
  <M3eBottomSheet.Header>
    <h3>Filter Options</h3>
  </M3eBottomSheet.Header>
  <M3eBottomSheet.Content>
    <M3eCheckbox label="Option A" />
    <M3eCheckbox label="Option B" />
  </M3eBottomSheet.Content>
  <M3eBottomSheet.Actions>
    <M3eButton variant="filled">Apply</M3eButton>
  </M3eBottomSheet.Actions>
</M3eBottomSheet>
```

**无障碍**：模态模式下自动管理焦点捕获。提供清晰的标题和关闭方式。

### 侧边表单

**使用场景**：从屏幕侧面滑入的面板，用于显示补充内容或导航。两种变体：Standard（标准型，停靠在内容旁边）和 Modal（模态型，带有遮罩层覆盖内容）。

> **@m3e/react 推荐：** `M3eDrawerContainer`（`@m3e/react/drawer-container`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `open` | boolean | 显示状态 |
| `type` | string | `standard` 或 `modal` |
| `position` | string | `start` 或 `end`（默认 `start`） |

```tsx
import { M3eDrawerContainer } from '@m3e/react/drawer-container';
import { M3eList, M3eListItem } from '@m3e/react/list';

<M3eDrawerContainer type="standard" open>
  <M3eDrawerContainer.Drawer>
    <h3>Filters</h3>
    <M3eList>
      <M3eListItem>Category A</M3eListItem>
      <M3eListItem>Category B</M3eListItem>
    </M3eList>
  </M3eDrawerContainer.Drawer>
  <M3eDrawerContainer.Content>
    <p>Main content area</p>
  </M3eDrawerContainer.Content>
</M3eDrawerContainer>

{/* Modal 侧边表单 */}
<M3eDrawerContainer type="modal" open>
  <M3eDrawerContainer.Drawer>
    <h3>Details</h3>
    <p>Additional information here</p>
  </M3eDrawerContainer.Drawer>
  <M3eDrawerContainer.Content>
    <p>Content behind modal</p>
  </M3eDrawerContainer.Content>
</M3eDrawerContainer>
```

**无障碍**：模态模式下自动管理焦点捕获。提供清晰的标题和关闭方式。

### 分隔线

**使用场景**：在内容区域之间创建视觉分隔。

> **@m3e/react 推荐：** `M3eDivider`（`@m3e/react/divider`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `inset` | boolean | 两侧添加缩进 |
| `insetStart` | boolean | 起始侧添加缩进 |
| `insetEnd` | boolean | 结束侧添加缩进 |

```tsx
import { M3eDivider } from '@m3e/react/divider';

<M3eDivider />
<M3eDivider inset />
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-divider` | **导入**：`@material/web/divider/divider.js`

```html
<md-divider></md-divider>
<md-divider inset></md-divider>
```
</details>

### 轮播

**使用场景**：水平滚动展示一组内容卡片、图片或媒体项。

> **@m3e/react 推荐：** `M3eSlideGroup`（`@m3e/react/slide-group`）。

三种配置：
- **Multi-browse**：多个项目可见，可滚动
- **Uncontained**：项目可延伸到视口边缘之外
- **Hero**：一个大尺寸的主项目搭配较小的预览项

| 属性 | 类型 | 描述 |
|------|------|------|
| `showArrows` | boolean | 显示导航箭头 |
| `snap` | boolean | 启用吸附对齐 |
| `peek` | string | 预览宽度（如 `32px`） |

```tsx
import { M3eSlideGroup } from '@m3e/react/slide-group';
import { M3eCard } from '@m3e/react/card';

{/* Multi-browse 轮播 */}
<M3eSlideGroup showArrows snap>
  <div className="slide">
    <img src="image1.jpg" alt="Image 1" />
    <h3>Item 1</h3>
  </div>
  <div className="slide">
    <img src="image2.jpg" alt="Image 2" />
    <h3>Item 2</h3>
  </div>
  <div className="slide">
    <img src="image3.jpg" alt="Image 3" />
    <h3>Item 3</h3>
  </div>
</M3eSlideGroup>

{/* Uncontained 轮播 */}
<M3eSlideGroup peek="48px" showArrows>
  <M3eCard variant="outlined">
    <M3eCard.Content>Featured content</M3eCard.Content>
  </M3eCard>
  <M3eCard variant="outlined">
    <M3eCard.Content>More content</M3eCard.Content>
  </M3eCard>
</M3eSlideGroup>
```

**无障碍**：支持键盘导航（方向键）。为每个幻灯片提供 `aria-label`。

## 输入

### 复选框

**使用场景**：在选项中进行多选或确认操作。

> **@m3e/react 推荐：** `M3eCheckbox`（`@m3e/react/checkbox`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `checked` | boolean | 选中状态 |
| `indeterminate` | boolean | 不确定状态 |
| `disabled` | boolean | 禁用状态 |
| `required` | boolean | 表单验证必填 |

```tsx
import { M3eCheckbox } from '@m3e/react/checkbox';

<M3eCheckbox label="Accept terms" />
<M3eCheckbox label="Remember me" defaultChecked />
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-checkbox` | **导入**：`@material/web/checkbox/checkbox.js`

```html
<label>
  <md-checkbox></md-checkbox>
  Accept terms
</label>

<label>
  <md-checkbox checked></md-checkbox>
  Remember me
</label>
```
</details>

**无障碍**：用 `<label>` 包裹或使用 `aria-label`。复选框内置 checkbox 角色。

### 标签

**使用场景**：显示过滤条件、输入令牌或操作建议。

> **@m3e/react 推荐：** `M3eChipSet`、`M3eChip`（`@m3e/react/chips`）。

| 变体 | @m3e/react 用法 | 用途 |
|---------|---------|-----|
| Assist | `M3eChip` (`variant="assist"`) | 智能建议、快捷方式 |
| Filter | `M3eChip` (`variant="filter"`) | 过滤内容、多选 |
| Input | `M3eChip` (`variant="input"`) | 用户输入令牌（如邮件收件人） |
| Suggestion | `M3eChip` (`variant="suggestion"`) | 建议回复、查询 |

```tsx
import { M3eChipSet, M3eChip } from '@m3e/react/chips';

<M3eChipSet>
  <M3eChip variant="filter" label="Vegetarian" selected />
  <M3eChip variant="filter" label="Vegan" />
  <M3eChip variant="filter" label="Gluten-free" />
</M3eChipSet>

<M3eChipSet>
  <M3eChip variant="input" label="user@example.com" removable onRemove={handleRemove} />
</M3eChipSet>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-chip-set`、`md-assist-chip`、`md-filter-chip`、`md-input-chip`、`md-suggestion-chip`
**导入**：`@material/web/chips/*.js`

```html
<md-chip-set>
  <md-filter-chip label="Vegetarian" selected></md-filter-chip>
  <md-filter-chip label="Vegan"></md-filter-chip>
  <md-filter-chip label="Gluten-free"></md-filter-chip>
</md-chip-set>

<md-chip-set>
  <md-input-chip label="user@example.com" removable></md-input-chip>
</md-chip-set>
```
</details>

### 菜单

**使用场景**：在锚点元素附近显示操作列表或选项。

> **@m3e/react 推荐：** `M3eMenu`、`M3eMenuItem`（`@m3e/react/menu`）。

**I/O 2026 备注：** 表现力菜单更新了 Material 指南，提供更灵活、更生动的配置方式。在 Jetpack Compose 中，优先使用当前 Material3 菜单 API 和 BOM 中可用的表现力变体。在 Web 上，使用 `@m3e/react` 构建菜单，或在需要表现力行为时使用 `@material/web` 构建规范对齐的自定义变体。

| 属性（菜单） | 类型 | 描述 |
|-----------------|------|-------------|
| `anchor` | string | 锚点元素的 ID |
| `open` | boolean | 显示菜单 |
| `positioning` | string | `absolute`、`fixed`、`popover` |

```tsx
import { useState } from 'react';
import { M3eMenu, M3eMenuItem } from '@m3e/react/menu';
import { M3eButton } from '@m3e/react/button';

function MenuExample() {
  const [open, setOpen] = useState(false);

  return (
    <>
      <M3eButton variant="filled" id="menu-trigger" onClick={() => setOpen(!open)}>
        Options
      </M3eButton>
      <M3eMenu anchor="menu-trigger" open={open} onOpenChange={setOpen}>
        <M3eMenuItem headline="Edit" icon="edit" />
        <M3eMenuItem headline="Delete" icon="delete" />
      </M3eMenu>
    </>
  );
}
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-menu`、`md-menu-item`、`md-sub-menu`
**导入**：`@material/web/menu/menu.js`、`@material/web/menu/menu-item.js`

```html
<span style="position: relative;">
  <md-filled-button id="menu-trigger">Options</md-filled-button>
  <md-menu id="options-menu" anchor="menu-trigger">
    <md-menu-item>
      <div slot="headline">Edit</div>
      <md-icon slot="start">edit</md-icon>
    </md-menu-item>
    <md-menu-item>
      <div slot="headline">Delete</div>
      <md-icon slot="start">delete</md-icon>
    </md-menu-item>
  </md-menu>
</span>

<script>
  document.getElementById('menu-trigger').addEventListener('click', () => {
    document.getElementById('options-menu').open = !document.getElementById('options-menu').open;
  });
</script>
```
</details>

### 单选按钮

**使用场景**：在一组互斥选项中选择一项。

> **@m3e/react 推荐：** `M3eRadio`（`@m3e/react/radio`）。

```tsx
import { M3eRadio } from '@m3e/react/radio';

<div role="radiogroup" aria-label="Size">
  <label><M3eRadio name="size" value="s" /> Small</label>
  <label><M3eRadio name="size" value="m" defaultChecked /> Medium</label>
  <label><M3eRadio name="size" value="l" /> Large</label>
</div>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-radio` | **导入**：`@material/web/radio/radio.js`

```html
<div role="radiogroup" aria-label="Size">
  <label><md-radio name="size" value="s"></md-radio> Small</label>
  <label><md-radio name="size" value="m" checked></md-radio> Medium</label>
  <label><md-radio name="size" value="l"></md-radio> Large</label>
</div>
```
</details>

**无障碍**：将单选按钮放在带有 `role="radiogroup"` 和 `aria-label` 的容器中分组。

### 滑块

**使用场景**：在连续或离散范围内选择值。

> **@m3e/react 推荐：** `M3eSlider`（`@m3e/react/slider`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `value` | number | 当前值 |
| `min` | number | 最小值 |
| `max` | number | 最大值 |
| `step` | number | 步进增量（设为离散模式） |
| `labeled` | boolean | 显示值标签 |
| `range` | boolean | 启用范围选择 |
| `valueStart` | number | 起始值（范围模式） |
| `valueEnd` | number | 结束值（范围模式） |

```tsx
import { M3eSlider } from '@m3e/react/slider';

{/* 连续滑块 */}
<M3eSlider defaultValue={50} min={0} max={100} aria-label="Volume" />

{/* 带标签的离散滑块 */}
<M3eSlider defaultValue={3} min={1} max={10} step={1} labeled aria-label="Rating" />

{/* 范围滑块 */}
<M3eSlider range defaultValueStart={20} defaultValueEnd={80} min={0} max={100} aria-label="Price range" />
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-slider` | **导入**：`@material/web/slider/slider.js`

```html
<!-- 连续滑块 -->
<md-slider value="50" min="0" max="100" aria-label="Volume"></md-slider>

<!-- 带标签的离散滑块 -->
<md-slider value="3" min="1" max="10" step="1" labeled aria-label="Rating"></md-slider>

<!-- 范围滑块 -->
<md-slider range value-start="20" value-end="80" min="0" max="100" aria-label="Price range"></md-slider>
```
</details>

### 开关

**使用场景**：切换二元状态（开/关）。

> **@m3e/react 推荐：** `M3eSwitch`（`@m3e/react/switch`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `selected` | boolean | 开启状态 |
| `icons` | boolean | 显示开/关图标 |
| `disabled` | boolean | 禁用状态 |

```tsx
import { M3eSwitch } from '@m3e/react/switch';

<label>
  <M3eSwitch />
  Dark mode
</label>

<label>
  <M3eSwitch defaultSelected icons />
  Notifications
</label>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-switch` | **导入**：`@material/web/switch/switch.js`

```html
<label>
  <md-switch></md-switch>
  Dark mode
</label>

<label>
  <md-switch selected icons></md-switch>
  Notifications
</label>
```

**自定义**：`--md-switch-selected-handle-color`、`--md-switch-selected-track-color`
</details>

### 文本输入框

**使用场景**：接收用户的文本输入。

> **@m3e/react 推荐：** `M3eTextField`（`@m3e/react/text-field`）。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `label` | string | 标签文本 |
| `value` | string | 当前值 |
| `type` | string | 输入类型（text、email、password、number、textarea 等） |
| `placeholder` | string | 占位符文本 |
| `required` | boolean | 必填验证 |
| `disabled` | boolean | 禁用状态 |
| `error` | boolean | 错误状态 |
| `errorText` | string | 错误信息 |
| `supportingText` | string | 辅助文本 |
| `prefixText` | string | 前缀文本 |
| `suffixText` | string | 后缀文本 |
| `maxLength` | number | 字符限制（显示计数器） |
| `rows` | number | 行数（用于 textarea） |

```tsx
import { M3eTextField } from '@m3e/react/text-field';

{/* 描边（推荐用于大多数场景） */}
<M3eTextField
  variant="outlined"
  label="Email"
  type="email"
  required
  supportingText="We'll never share your email"
/>

{/* 填充 */}
<M3eTextField
  variant="filled"
  label="Search"
  type="text"
  placeholder="Type to search..."
  leadingIcon="search"
/>

{/* 带错误状态 */}
<M3eTextField
  variant="outlined"
  label="Password"
  type="password"
  error
  errorText="Password must be at least 8 characters"
  minLength={8}
/>

{/* 多行文本域 */}
<M3eTextField
  variant="outlined"
  label="Message"
  type="textarea"
  rows={4}
  maxLength={500}
/>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-filled-text-field`、`md-outlined-text-field`
**导入**：`@material/web/textfield/filled-text-field.js`、`@material/web/textfield/outlined-text-field.js`

```html
<!-- 描边（推荐用于大多数场景） -->
<md-outlined-text-field
  label="Email"
  type="email"
  required
  supporting-text="We'll never share your email">
</md-outlined-text-field>

<!-- 填充 -->
<md-filled-text-field
  label="Search"
  type="text"
  placeholder="Type to search...">
  <md-icon slot="leading-icon">search</md-icon>
</md-filled-text-field>

<!-- 带错误状态 -->
<md-outlined-text-field
  label="Password"
  type="password"
  error
  error-text="Password must be at least 8 characters"
  min-length="8">
</md-outlined-text-field>

<!-- 多行文本域 -->
<md-outlined-text-field
  label="Message"
  type="textarea"
  rows="4"
  max-length="500">
</md-outlined-text-field>
```

**自定义**：`--md-outlined-text-field-container-shape`、`--md-outlined-text-field-focus-outline-color`、`--md-filled-text-field-container-color`
</details>

#### Jetpack Compose

使用 **`androidx.compose.material3`** 中的 **`OutlinedTextField`** / **`TextField`**。在当前 Material3 版本中优先使用 **基于状态** 的 API（`TextFieldState`、`rememberTextFieldState()`）—— 参见 [包概述](https://developer.android.com/reference/kotlin/androidx/compose/material3/package-summary)。将标签、辅助文本和错误状态映射到 MD3 角色（`MaterialTheme.colorScheme`、`TextFieldDefaults`）。

```kotlin
// 示意代码 —— API 名称因 Material3 版本略有差异
val state = rememberTextFieldState("")

OutlinedTextField(
    state = state,
    label = { Text("Email") },
    supportingText = { if (isError) Text("Invalid email") },
    isError = isError,
    modifier = Modifier.fillMaxWidth()
)
```

### 日期选择器

**使用场景**：选择日期或日期范围。

> **@m3e/react 推荐：** `M3eDatepicker`（`@m3e/react/datepicker`）。

三种配置：
- **Docked**：内联日历，附着于输入框
- **Modal**：用于日期选择的完整对话框
- **Range**：选择日期范围

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | string/Date | 选中的日期 |
| `mode` | string | `docked`、`modal`、`range` |
| `min` | string/Date | 最小可选日期 |
| `max` | string/Date | 最大可选日期 |
| `locale` | string | 语言区域 |

```tsx
import { M3eDatepicker } from '@m3e/react/datepicker';
import { M3eTextField } from '@m3e/react/text-field';
import { M3eButton } from '@m3e/react/button';

{/* Docked 日期选择器 */}
<M3eDatepicker mode="docked" value="2026-09-02">
  <M3eTextField variant="outlined" label="Select date" />
</M3eDatepicker>

{/* Modal 日期选择器 */}
<M3eDatepicker mode="modal" min="2026-01-01" max="2026-12-31">
  <M3eButton variant="filled">Choose Date</M3eButton>
</M3eDatepicker>

{/* 日期范围选择 */}
<M3eDatepicker mode="range">
  <M3eTextField variant="outlined" label="Date range" />
</M3eDatepicker>
```

**无障碍**：提供清晰的标签和日期格式说明。支持键盘导航（方向键选择日期）。

### 时间选择器

**使用场景**：选择时间。

> **@m3e/react 推荐：** `M3eTimepicker`（`@m3e/react/timepicker`）。

两种配置：
- **Docked**：内联时间输入
- **Modal**：对话框中的时钟拨盘

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | string | 选中的时间（HH:MM 格式） |
| `mode` | string | `docked` 或 `modal` |
| `format` | string | `12h` 或 `24h` |
| `step` | number | 分钟步进值 |

```tsx
import { M3eTimepicker } from '@m3e/react/timepicker';
import { M3eTextField } from '@m3e/react/text-field';
import { M3eButton } from '@m3e/react/button';

{/* Docked 时间选择器 */}
<M3eTimepicker mode="docked" value="14:30" format="24h">
  <M3eTextField variant="outlined" label="Select time" />
</M3eTimepicker>

{/* Modal 时间选择器 */}
<M3eTimepicker mode="modal" format="12h" step={15}>
  <M3eButton variant="filled">Choose Time</M3eButton>
</M3eTimepicker>
```

**无障碍**：提供清晰的标签。支持键盘导航（方向键调整时间）。

## 导航

### 顶部应用栏

**使用场景**：页面顶部的导航和操作区域。

> **@m3e/react 推荐：** `M3eAppBar`（`@m3e/react/app-bar`）。

四种变体：

| 变体 | 高度 | 标题位置 | 滚动行为 |
|---------|--------|---------------|----------------|
| Center-aligned | 64dp | 居中 | 滚动时凸起 |
| Small | 64dp | 左侧 | 滚动时凸起 |
| Medium | 112dp | 左侧，底部 | 滚动时收缩为小型 |
| Large | 152dp | 左侧，底部 | 滚动时收缩为小型 |

| 属性 | 类型 | 描述 |
|------|------|------|
| `variant` | string | `center-aligned`、`small`、`medium`、`large` |
| `scrollTarget` | string | 滚动监听的目标元素 |
| `elevation` | boolean | 滚动时自动显示阴影 |

**Jetpack Compose：** `TopAppBar`、`CenterAlignedTopAppBar`、`MediumTopAppBar`、`LargeTopAppBar` 以及表现力变体（如 large flexible）可能需要 **`@OptIn(ExperimentalMaterial3ExpressiveApi::class)`**，具体取决于 BOM —— 请检查你的 `material3` 版本。

```tsx
import { M3eAppBar } from '@m3e/react/app-bar';
import { M3eIconButton } from '@m3e/react/icon-button';

{/* Small 应用栏 */}
<M3eAppBar variant="small">
  <M3eIconButton slot="navigation" aria-label="Menu" icon="menu" />
  <span slot="headline">Page Title</span>
  <M3eIconButton slot="action" aria-label="Search" icon="search" />
  <M3eIconButton slot="action" aria-label="More" icon="more_vert" />
</M3eAppBar>

{/* Large 应用栏 */}
<M3eAppBar variant="large" elevation>
  <M3eIconButton slot="navigation" aria-label="Back" icon="arrow_back" />
  <span slot="headline">Dashboard</span>
</M3eAppBar>
```

**无障碍**：使用 `<header>` 语义角色。导航按钮需要 `aria-label`。

### 底部导航栏

**使用场景**：3–5 个主要目标页面，移动设备/紧凑屏幕，持久显示。

> **@m3e/react 推荐：** `M3eNavigationBar`（`@m3e/react/navigation-bar`）。

```tsx
import { M3eNavigationBar, M3eNavigationTab } from '@m3e/react/navigation-bar';

<M3eNavigationBar>
  <M3eNavigationTab label="Home" active activeIcon="home" inactiveIcon="home" />
  <M3eNavigationTab label="Explore" activeIcon="explore" inactiveIcon="explore" />
  <M3eNavigationTab label="Profile" activeIcon="person" inactiveIcon="person" />
</M3eNavigationBar>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-navigation-bar` | **导入**：`@material/web/navigation/navigation-bar.js`

```html
<md-navigation-bar>
  <md-navigation-tab label="Home" active>
    <md-icon slot="active-icon">home</md-icon>
    <md-icon slot="inactive-icon">home</md-icon>
  </md-navigation-tab>
  <md-navigation-tab label="Explore">
    <md-icon slot="active-icon">explore</md-icon>
    <md-icon slot="inactive-icon">explore</md-icon>
  </md-navigation-tab>
  <md-navigation-tab label="Profile">
    <md-icon slot="active-icon">person</md-icon>
    <md-icon slot="inactive-icon">person</md-icon>
  </md-navigation-tab>
</md-navigation-bar>
```
</details>

### 导航抽屉

**使用场景**：目标页面较多、较大屏幕，可为模态或持久型。

> **@m3e/react 推荐：** `M3eNavigationDrawer`（`@m3e/react/navigation-drawer`）。另有 `M3eDrawerContainer`（`@m3e/react/drawer-container`）用于更灵活的布局。

| 属性 | 类型 | 描述 |
|-----------|------|-------------|
| `opened` | boolean | 打开状态 |
| `type` | string | `standard` 或 `modal` |

```tsx
import { M3eNavigationDrawer } from '@m3e/react/navigation-drawer';
import { M3eList, M3eListItem } from '@m3e/react/list';

<M3eNavigationDrawer opened>
  <div slot="headline">Mail</div>
  <M3eList>
    <M3eListItem type="button" active leadingIcon="inbox">
      Inbox
    </M3eListItem>
    <M3eListItem type="button" leadingIcon="send">
      Sent
    </M3eListItem>
  </M3eList>
</M3eNavigationDrawer>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-navigation-drawer` | **导入**：`@material/web/navigation/navigation-drawer.js`

```html
<md-navigation-drawer opened>
  <div slot="headline">Mail</div>
  <md-list>
    <md-list-item type="button" active>
      <md-icon slot="start">inbox</md-icon>
      Inbox
    </md-list-item>
    <md-list-item type="button">
      <md-icon slot="start">send</md-icon>
      Sent
    </md-list-item>
  </md-list>
</md-navigation-drawer>
```
</details>

### 导航侧栏

**使用场景**：3–7 个目标页面，中等屏幕（600–839dp），持久的侧边导航。

> **@m3e/react 推荐：** `M3eNavRail`（`@m3e/react/nav-rail`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `selected` | string | 当前选中的导航项标识 |
| `showLabels` | boolean | 始终显示标签（默认仅选中时显示） |

```tsx
import { M3eNavRail, M3eNavRailItem } from '@m3e/react/nav-rail';
import { M3eFab } from '@m3e/react/fab';

<M3eNavRail selected="home" aria-label="Main navigation">
  <M3eFab slot="fab" size="small" variant="tertiary" aria-label="Compose" icon="edit" />

  <M3eNavRailItem value="home" label="Home" active icon="home" />
  <M3eNavRailItem value="search" label="Search" icon="search" />
  <M3eNavRailItem value="favorites" label="Favorites" icon="favorite" />
  <M3eNavRailItem value="profile" label="Profile" icon="person" />
</M3eNavRail>
```

**无障碍**：使用 `<nav>` 语义角色和 `aria-label`。每个导航项需要 `aria-current` 指示当前页面。

### 搜索

**使用场景**：提供全局或上下文搜索功能。

> **@m3e/react 推荐：** `M3eSearch`（`@m3e/react/search`）。

两种模式：
- **搜索栏**：顶部应用栏区域中的持久搜索输入框
- **搜索视图**：可展开的搜索覆盖层，带有建议列表

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | string | 当前搜索文本 |
| `placeholder` | string | 占位符文本 |
| `expanded` | boolean | 展开模式（搜索视图） |
| `suggestions` | array | 建议列表 |

**I/O 2026 备注：** 表现力搜索和搜索应用栏指南增加了刷新的视觉风格、动效和更灵活的尾部图标行为。在 Jetpack Compose 中，使用当前 Material3 搜索 API 和可用的表现力应用栏变体。

```tsx
import { M3eSearch } from '@m3e/react/search';
import { M3eIconButton } from '@m3e/react/icon-button';

{/* 搜索栏 */}
<M3eSearch placeholder="Search products..." value="">
  <M3eIconButton slot="trailingIcon" aria-label="Clear" icon="close" />
</M3eSearch>

{/* 搜索视图（带建议） */}
<M3eSearch
  expanded
  placeholder="Search..."
  suggestions={[{ text: 'Material Design' }, { text: 'Components' }]}
/>
```

**无障碍**：使用 `aria-label` 或关联的 `<label>` 元素。建议列表应支持键盘导航。

### 标签页

**使用场景**：在同一视图区域内切换不同的内容。

> **@m3e/react 推荐：** `M3eTabs`、`M3eTab`（`@m3e/react/tabs`）。

| 变体 | @m3e/react 用法 | 用途 |
|---------|---------|-----|
| Primary | `M3eTab` (`variant="primary"`) | 页面内的顶级导航 |
| Secondary | `M3eTab` (`variant="secondary"`) | 主要标签页内的子分区 |

```tsx
import { M3eTabs, M3eTab } from '@m3e/react/tabs';

<M3eTabs>
  <M3eTab variant="primary" active icon="flight" label="Flights" />
  <M3eTab variant="primary" icon="hotel" label="Hotels" />
  <M3eTab variant="primary" icon="explore" label="Explore" />
</M3eTabs>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-tabs`、`md-primary-tab`、`md-secondary-tab`
**导入**：`@material/web/tabs/tabs.js`、`@material/web/tabs/primary-tab.js`、`@material/web/tabs/secondary-tab.js`

```html
<md-tabs>
  <md-primary-tab active>
    <md-icon slot="icon">flight</md-icon>
    Flights
  </md-primary-tab>
  <md-primary-tab>
    <md-icon slot="icon">hotel</md-icon>
    Hotels
  </md-primary-tab>
  <md-primary-tab>
    <md-icon slot="icon">explore</md-icon>
    Explore
  </md-primary-tab>
</md-tabs>
```
</details>

**无障碍**：标签页集合内置 tablist 角色。使用 `aria-controls` 将标签页与面板关联。

### 工具栏

**使用场景**：显示与当前页面上下文相关的常用操作。通常放置在顶部应用栏下方或上下文中适当的位置。

> **@m3e/react 推荐：** `M3eToolbar`（`@m3e/react/toolbar`）。

| 属性 | 类型 | 描述 |
|------|------|------|
| `dense` | boolean | 紧凑模式 |
| `align` | string | 对齐方式（`start`、`center`、`end`） |

```tsx
import { M3eToolbar } from '@m3e/react/toolbar';
import { M3eButton } from '@m3e/react/button';
import { M3eIconButton } from '@m3e/react/icon-button';
import { M3eDivider } from '@m3e/react/divider';

<M3eToolbar>
  <M3eButton variant="outlined" icon="format_bold">Bold</M3eButton>
  <M3eButton variant="outlined" icon="format_italic">Italic</M3eButton>
  <M3eDivider inset />
  <M3eIconButton aria-label="Undo" icon="undo" />
  <M3eIconButton aria-label="Redo" icon="redo" />
</M3eToolbar>
```

**无障碍**：工具栏内的按钮组应使用 `aria-label` 描述用途。

## 数据展示

### 列表

**使用场景**：显示结构化的内容集合。

> **@m3e/react 推荐：** `M3eList`、`M3eListItem`（`@m3e/react/list`）。

**I/O 2026 备注：** 表现力列表增加了更生动的样式和灵活的项配置。在 Compose 中，优先使用 Material3 列表模式，并保持间距、前置/后置内容、辅助文本和分隔线由令牌驱动。在 Web 上，`@m3e/react` 提供完整的列表支持；`@material/web` 适用于标准列表，表现力列表效果可能需要自定义 CSS。

| 属性（列表项） | 类型 | 描述 |
|----------------------|------|-------------|
| `type` | string | `text`（默认）、`button`、`link` |
| `href` | string | URL（当 type="link" 时） |
| `disabled` | boolean | 禁用状态 |

```tsx
import { M3eList, M3eListItem } from '@m3e/react/list';
import { M3eDivider } from '@m3e/react/divider';

<M3eList>
  {/* 单行 */}
  <M3eListItem>Single line item</M3eListItem>

  {/* 带图标的双行 */}
  <M3eListItem leadingIcon="person" headline="Jane Smith" supportingText="Senior Developer" />

  {/* 三行 */}
  <M3eListItem
    leadingIcon="mail"
    headline="Meeting notes"
    supportingText="Please review the attached notes from today's standup meeting and provide feedback."
    trailingSupportingText="3 min ago"
  />

  <M3eDivider />

  {/* 可点击项 */}
  <M3eListItem type="button" onClick={handleClick} leadingIcon="settings" trailingIcon="chevron_right">
    Settings
  </M3eListItem>
</M3eList>
```

<details>
<summary>@material/web 备选</summary>

**元素**：`md-list`、`md-list-item`
**导入**：`@material/web/list/list.js`、`@material/web/list/list-item.js`

```html
<md-list>
  <!-- 单行 -->
  <md-list-item>Single line item</md-list-item>

  <!-- 带图标的双行 -->
  <md-list-item>
    <md-icon slot="start">person</md-icon>
    <div slot="headline">Jane Smith</div>
    <div slot="supporting-text">Senior Developer</div>
  </md-list-item>

  <!-- 三行 -->
  <md-list-item>
    <md-icon slot="start">mail</md-icon>
    <div slot="headline">Meeting notes</div>
    <div slot="supporting-text">Please review the attached notes from today's standup meeting and provide feedback.</div>
    <div slot="trailing-supporting-text">3 min ago</div>
  </md-list-item>

  <md-divider></md-divider>

  <!-- 可点击项 -->
  <md-list-item type="button" onclick="handleClick()">
    <md-icon slot="start">settings</md-icon>
    <div slot="headline">Settings</div>
    <md-icon slot="end">chevron_right</md-icon>
  </md-list-item>
</md-list>
```

**插槽**：`start`（前置元素）、`end`（后置元素）、`headline`（主要文本）、`supporting-text`（辅助文本）、`trailing-supporting-text`（尾部元数据）、`overline`（标题上方文本）
</details>
