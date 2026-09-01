---
name: material-3
description: >
  Implement Google's Material Design 3 (Material You) UI system for the Web.
  Covers MD3 design tokens as CSS custom properties, @material/web components
  (maintenance mode), theming, dark mode, layout, and M3 Expressive guidance
  for CSS. Use when: "material design", "MD3", "material you", "material component",
  "md3 button", "@material/web", "--md-sys-color".
user-invokable: true
argument-hint: "[component|theme|layout|scaffold|audit] [description or URL]"
---

# Material Design 3 (Web)

This skill guides implementation of Google's Material Design 3 (MD3) on the **web** — a personal, adaptive, expressive design system. MD3 uses dynamic color, tonal surfaces, rounded shapes, and spring-based motion to create UIs that feel alive and personal. On the Web this is realized through **CSS custom properties** (`--md-sys-*`) and the **`@material/web`** component library (maintenance mode).

**Key differences from MD2:**
- Tonal surfaces replace elevation shadows as the primary depth cue
- Dynamic color generates full schemes from a single seed color
- Fully rounded corners by default
- Spring-based motion physics (Expressive) — on Web, falls back to easing/duration tokens
- 3 levels of user-controlled contrast (standard/medium/high)

**Relationship with frontend-design skill:**
When both skills are active, MD3 provides the design system (tokens, components, layout rules) and frontend-design provides creative direction within those constraints. MD3 rules take precedence for component structure and token usage. Note: **Roboto / Roboto Flex IS the correct default typeface in MD3 on the Web** — the frontend-design guidance to avoid Roboto does not apply when implementing MD3.

## Decision Tree

**What are you building?**
```
Full app scaffold        → See "Common Patterns: App Shell" + references/layout-and-responsive.md
Single component         → See "Component Quick Reference" table → references/component-catalog.md
Custom theme             → See references/theming-and-dynamic-color.md
Form / input layout      → See references/component-catalog.md § Input Components
Navigation structure     → See references/navigation-patterns.md
Data display             → See references/component-catalog.md § Data Display
```

**What approach?**
```
@material/web (vanilla)  → Individual imports + CSS custom properties
Web (React/Vue/Svelte)   → CSS custom properties + wrapper components (no official React lib)
Web (CSS-only)           → MD3 token values as CSS custom properties (no <md-*> elements)
```

## Design Token System

All MD3 tokens use the `md.sys` namespace, exposed on the web as **CSS custom properties** (`--md-sys-*`). Generate color values with the [Material Theme Builder](https://m3.material.io/theme-builder) or `@material/material-color-utilities`.

### Color Tokens (`--md-sys-color-*`)
| Token | Purpose |
|-------|---------|
| `primary` | High-emphasis fills, text, icons against surface |
| `on-primary` | Text/icons on primary |
| `primary-container` | Standout fill for key components (FAB, etc.) |
| `on-primary-container` | Text/icons on primary-container |
| `secondary` / `on-secondary` | Less prominent accents |
| `secondary-container` / `on-secondary-container` | Recessive components (tonal buttons) |
| `tertiary` / `on-tertiary` | Contrasting accents |
| `tertiary-container` / `on-tertiary-container` | Complementary containers |
| `error` / `on-error` | Error states (static — doesn't change with dynamic color) |
| `error-container` / `on-error-container` | Error container fills |
| `surface` | Default background |
| `on-surface` | Text/icons on any surface |
| `on-surface-variant` | Lower-emphasis text/icons on surface |
| `surface-container-lowest` | Lowest-emphasis container |
| `surface-container-low` | Low-emphasis container |
| `surface-container` | Default container (nav areas) |
| `surface-container-high` | High-emphasis container |
| `surface-container-highest` | Highest-emphasis container |
| `surface-dim` / `surface-bright` | Maintain relative brightness across light/dark |
| `inverse-surface` / `inverse-on-surface` / `inverse-primary` | Contrasting elements (snackbars) |
| `outline` | Important boundaries (text field borders) |
| `outline-variant` | Decorative elements (dividers) |

Full details: `references/color-system.md`

### Typography Tokens (`--md-sys-typescale-*`)
| Scale | Sizes | Use |
|-------|-------|-----|
| Display | L / M / S | Hero text, large numbers |
| Headline | L / M / S | Section headers |
| Title | L / M / S | Smaller headers, card titles |
| Body | L / M / S | Paragraph text, descriptions |
| Label | L / M / S | Buttons, chips, captions |

Each style has tokens for: `-font`, `-weight`, `-size`, `-line-height`, `-tracking`
Plus 15 **emphasized** variants (higher weight) via `--md-sys-typescale-emphasized-*`

Full details: `references/typography-and-shape.md`

### Shape Tokens (`--md-sys-shape-corner-*`)
| Token | Value | Example components |
|-------|-------|-------------------|
| `none` | 0dp | — |
| `extra-small` | 4dp | Chips, snackbars |
| `small` | 8dp | Text fields, menus |
| `medium` | 12dp | Cards |
| `large` | 16dp | FABs, navigation drawer |
| `large-increased` | 20dp | (Expressive) |
| `extra-large` | 28dp | Dialogs, bottom sheets |
| `extra-large-increased` | 32dp | (Expressive) |
| `extra-extra-large` | 48dp | (Expressive) |
| `full` | 9999px | Buttons, chips, badges |

### Elevation Levels
| Level | DP | Tonal offset | Use |
|-------|-----|-------------|-----|
| 0 | 0dp | None | Flat surfaces, most components at rest |
| 1 | 1dp | +5% primary | Elevated cards, modal sheets |
| 2 | 3dp | +8% primary | Menus, nav bar, scrolled app bar |
| 3 | 6dp | +11% primary | FAB, dialogs, search, date/time pickers |
| 4 | 8dp | +12% primary | (hover/focus increase only) |
| 5 | 12dp | +14% primary | (hover/focus increase only) |

Elevation in MD3 is communicated through **tonal surface color**, not shadows. Shadows are only used when needed for additional protection against busy backgrounds.

### Motion
MD3 Expressive (May 2025) introduced spring-based motion physics for components — **not available in `@material/web`**. On the Web, continue to use the easing/duration system for transitions (enter/exit/shared-axis):

| Easing | Duration | Transition type |
|--------|----------|-----------------|
| Emphasized | 500ms | Begin and end on screen |
| Emphasized decelerate | 400ms | Enter the screen |
| Emphasized accelerate | 200ms | Exit the screen |
| Standard | 300ms | Begin and end on screen (utility) |
| Standard decelerate | 250ms | Enter screen (utility) |
| Standard accelerate | 200ms | Exit screen (utility) |

CSS easing values:
- Emphasized: `cubic-bezier(0.2, 0, 0, 1)`
- Emphasized decelerate: `cubic-bezier(0.05, 0.7, 0.1, 1)`
- Emphasized accelerate: `cubic-bezier(0.3, 0, 0.8, 0.15)`
- Standard: `cubic-bezier(0.2, 0, 0, 1)`
- Standard decelerate: `cubic-bezier(0, 0, 0, 1)`
- Standard accelerate: `cubic-bezier(0.3, 0, 1, 1)`

## Component Quick Reference

| Component | Web Element | Key Variants | Category |
|-----------|------------|--------------|----------|
| Button | `md-filled-button`, `md-outlined-button`, `md-text-button`, `md-elevated-button`, `md-filled-tonal-button` | Filled, Outlined, Text, Elevated, Tonal; 5 sizes (XS–XL); toggle | Actions |
| Button group | `md-button-group` | Standard, connected | Actions |
| Extended FAB | `md-extended-fab` | Surface, Primary, Secondary, Tertiary | Actions |
| FAB | `md-fab` | Small, Medium, Large | Actions |
| Icon button | `md-icon-button`, `md-filled-icon-button`, `md-filled-tonal-icon-button`, `md-outlined-icon-button` | Standard, Filled, Filled Tonal, Outlined | Actions |
| Progress indicator | `md-linear-progress`, `md-circular-progress` | Linear, Circular; determinate/indeterminate | Communication |
| Divider | `md-divider` | Full-width, Inset | Containment |
| Dialog | `md-dialog` | Basic, Full-screen | Containment |
| Checkbox | `md-checkbox` | — | Input |
| Chips | `md-chip-set`, `md-assist-chip`, `md-filter-chip`, `md-input-chip`, `md-suggestion-chip` | Assist, Filter, Input, Suggestion | Input |
| Menu | `md-menu`, `md-menu-item` | — | Input |
| Radio button | `md-radio` | — | Input |
| Slider | `md-slider` | Continuous, Discrete, Range | Input |
| Switch | `md-switch` | With/without icon | Input |
| Text field | `md-filled-text-field`, `md-outlined-text-field` | Filled, Outlined | Input |
| Navigation bar | `md-navigation-bar` | — | Navigation |
| Navigation drawer | `md-navigation-drawer` | Standard, Modal | Navigation |
| Tabs | `md-tabs`, `md-primary-tab`, `md-secondary-tab` | Primary, Secondary | Navigation |
| List | `md-list`, `md-list-item` | One-line, Two-line, Three-line | Data Display |

**Note:** Components not listed above (FAB menu, segmented button, split button, badge, loading indicator, snack, tooltip, card, carousel, bottom sheet, side sheet, date picker, time picker, app bar, navigation rail, search, toolbar) don't have `@material/web` implementations yet. Use CSS custom properties with standard HTML for these.

Full component details with code examples: `references/component-catalog.md`

## @material/web

**Important:** Per [Material Design 3 for Web](https://m3.material.io/develop/web), **Material Web Components are in maintenance mode** and **M3 Expressive is not implemented on Web**. Use `@material/web` for token-backed web UIs when appropriate, but do not treat it as equivalent to a full-stack implementation for current Expressive features.

### Setup

```bash
npm install @material/web
```

### Import Components Individually

Always import only the components you use — importing the entire package bloats the bundle:

```javascript
// Good — individual imports
import '@material/web/button/filled-button.js';
import '@material/web/button/outlined-button.js';
import '@material/web/textfield/outlined-text-field.js';
import '@material/web/icon/icon.js';

// Bad — never do this
import '@material/web'; // imports everything
```

### Basic Usage

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://fonts.googleapis.com/css2?family=Roboto+Flex:wght@400;500;700&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/icon?family=Material+Symbols+Outlined" rel="stylesheet">
</head>
<body>
  <md-filled-button>Get started</md-filled-button>
  <md-outlined-text-field label="Email" type="email"></md-outlined-text-field>

  <script type="module">
    import '@material/web/button/filled-button.js';
    import '@material/web/textfield/outlined-text-field.js';
  </script>
</body>
</html>
```

### Theming with CSS Custom Properties

Apply a custom theme by setting CSS custom properties on `:root` or any ancestor:

```css
:root {
  /* Color scheme (generate with @material/material-color-utilities) */
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

  /* Typography */
  --md-sys-typescale-body-large-font: 'Roboto Flex', sans-serif;
  --md-sys-typescale-body-large-size: 1rem;
  --md-sys-typescale-body-large-weight: 400;
  --md-sys-typescale-body-large-line-height: 1.5rem;

  /* Shape */
  --md-sys-shape-corner-full: 9999px;
  --md-sys-shape-corner-medium: 12px;
}
```

### Component-Level Overrides

Override individual component tokens for specific customization:

```css
md-filled-button {
  --md-filled-button-container-color: var(--md-sys-color-primary);
  --md-filled-button-label-text-color: var(--md-sys-color-on-primary);
  --md-filled-button-container-shape: var(--md-sys-shape-corner-full);
  --md-filled-button-container-height: 40px;
}

md-outlined-text-field {
  --md-outlined-text-field-container-shape: var(--md-sys-shape-corner-small);
  --md-outlined-text-field-focus-outline-color: var(--md-sys-color-primary);
}
```

### Dark Theme

Apply dark theme by overriding color tokens on a class or media query:

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

Full theming guide: `references/theming-and-dynamic-color.md`

## Common Patterns

### App Shell

Standard MD3 app with responsive navigation + top app bar + content area:

```html
<div class="md3-app">
  <nav class="md3-nav-rail" aria-label="Main navigation">
    <md-fab size="small" aria-label="Compose">
      <md-icon slot="icon">edit</md-icon>
    </md-fab>
    <md-navigation-bar>
      <md-navigation-tab label="Home">
        <md-icon slot="active-icon">home</md-icon>
        <md-icon slot="inactive-icon">home</md-icon>
      </md-navigation-tab>
      <md-navigation-tab label="Search">
        <md-icon slot="active-icon">search</md-icon>
        <md-icon slot="inactive-icon">search</md-icon>
      </md-navigation-tab>
    </md-navigation-bar>
  </nav>
  <main class="md3-content">
    <header class="md3-top-app-bar">
      <h1 class="md3-top-app-bar__title" style="font: var(--md-sys-typescale-title-large)">
        Page Title
      </h1>
    </header>
    <div class="md3-body">
      <!-- Content here -->
    </div>
  </main>
</div>
```

```css
.md3-app {
  display: flex;
  min-height: 100vh;
  background: var(--md-sys-color-surface);
  color: var(--md-sys-color-on-surface);
}

.md3-nav-rail {
  width: 80px;
  background: var(--md-sys-color-surface);
  border-right: 1px solid var(--md-sys-color-outline-variant);
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 12px;
  gap: 12px;
}

.md3-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.md3-top-app-bar {
  height: 64px;
  padding: 0 16px;
  display: flex;
  align-items: center;
  background: var(--md-sys-color-surface);
}

.md3-body {
  padding: 24px;
  flex: 1;
}

/* Responsive: switch to bottom nav on compact */
@media (max-width: 599px) {
  .md3-app { flex-direction: column; }
  .md3-nav-rail {
    order: 1;
    width: 100%;
    flex-direction: row;
    justify-content: center;
    border-right: none;
    border-top: 1px solid var(--md-sys-color-outline-variant);
    padding: 0;
  }
}
```

### Card Grid

```html
<div class="md3-card-grid">
  <div class="md3-card md3-card--outlined">
    <img src="image.jpg" alt="Description" class="md3-card__media">
    <div class="md3-card__content">
      <h3 style="font: var(--md-sys-typescale-title-medium)">Card Title</h3>
      <p style="font: var(--md-sys-typescale-body-medium); color: var(--md-sys-color-on-surface-variant)">
        Supporting text for this card.
      </p>
    </div>
    <div class="md3-card__actions">
      <md-text-button>Learn more</md-text-button>
      <md-filled-tonal-button>Action</md-filled-tonal-button>
    </div>
  </div>
</div>
```

```css
.md3-card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 16px;
}

.md3-card--outlined {
  border: 1px solid var(--md-sys-color-outline-variant);
  border-radius: var(--md-sys-shape-corner-medium, 12px);
  background: var(--md-sys-color-surface);
  overflow: hidden;
}

.md3-card__content { padding: 16px; }
.md3-card__actions { padding: 8px 16px 16px; display: flex; gap: 8px; justify-content: flex-end; }
.md3-card__media { width: 100%; aspect-ratio: 16/9; object-fit: cover; }
```

### Form Layout

```html
<form class="md3-form">
  <md-outlined-text-field label="Full name" required></md-outlined-text-field>
  <md-outlined-text-field label="Email" type="email" required></md-outlined-text-field>
  <md-outlined-text-field label="Message" type="textarea" rows="4"></md-outlined-text-field>
  <div class="md3-form__actions">
    <md-text-button type="reset">Cancel</md-text-button>
    <md-filled-button type="submit">Submit</md-filled-button>
  </div>
</form>
```

```css
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

More patterns: `references/navigation-patterns.md`, `references/layout-and-responsive.md`

## Anti-Patterns

**Never do these when implementing MD3 on the Web:**

- **Mix MD2 and MD3 libraries**: Don't use `@material/mdc-*` (MD2) alongside `@material/web` (MD3). They have incompatible APIs and styling.
- **Hardcode colors**: Always use `var(--md-sys-color-*)` tokens, never raw hex/rgb values. Hardcoded colors break dynamic theming, dark mode, and contrast adjustment.
- **Ignore tonal pairing**: Only combine colors in their intended pairs (e.g., `primary` + `on-primary`, `surface-container` + `on-surface`). Arbitrary pairings break contrast in dynamic color and high contrast modes.
- **Use `outline` for dividers**: Use `outline-variant` for dividers. `outline` is for important boundaries like text field borders.
- **Import all of @material/web**: Always import individual component modules. Barrel imports include every component and destroy bundle size.
- **Use `border-radius` directly**: Use shape tokens (`var(--md-sys-shape-corner-medium)`) so shapes stay consistent with theming.
- **Use shadows for elevation by default**: MD3 communicates elevation through tonal surface color, not shadows. Only add shadows when elements need extra separation from busy backgrounds.
- **Apply frontend-design "avoid Roboto" rule**: On the Web, Roboto or Roboto Flex is the default MD3 typeface. Replace only when intentionally customizing the type scale.
- **Assume SSR compatibility**: `@material/web` uses Web Components (custom elements) which require JavaScript to render. They won't produce meaningful HTML in SSR without additional hydration strategies.
- **Ignore large screens**: MD3 is designed for all screen sizes. Don't ship phone-only layouts — use multi-pane at 600dp+ and constrain content to a max width (840–1040dp) on Large (1200dp+) and Extra-large (1600dp+) windows. Endless-width text lines are unreadable.

## MD3 Compliance Audit

When invoked with `audit` as the argument (e.g., `/material-3 audit`), or when asked to audit/review MD3 compliance, analyze the target page and produce a compliance report.

### Audit Procedure

1. **Identify the target**: The user provides a URL (use browser tools to inspect), file paths (read source), or a running app.
2. **Inspect the following categories** and score each 0–10:

| Category | What to check |
|----------|--------------|
| **Color tokens** | `--md-sys-color-*` / generated CSS. Proper tonal pairing (`onX` on `X`). Dark theme. |
| **Typography** | MD3 typescale tokens; correct roles (Display, Headline, Title, Body, Label). |
| **Shape** | `var(--md-sys-shape-*)`. Buttons: full; cards: medium; avoid magic numbers. |
| **Elevation** | Tonal elevation; hover/focus where relevant. |
| **Components** | `@material/web` or spec-aligned HTML/CSS. Correct variants. |
| **Layout** | Canonical layouts; readable max width on large widths. |
| **Navigation** | Bar / rail / drawer patterns per size class. |
| **Motion** | CSS motion tokens (easing/duration) as fallback. |
| **Accessibility** | Verify contrast: UI components often need **3:1** for large text/borders and **4.5:1** for normal text (WCAG 2.x). ARIA, keyboard navigation, touch targets (~48dp). |
| **Theming** | CSS custom properties on `:root` or subtree. |

3. **Generate the report**:

```
# MD3 Compliance Audit Report

Target: [URL or file path]
Date: [date]
Overall Score: [X/100]

## Scores by Category
| Category       | Score | Status |
|----------------|-------|--------|
| Color tokens   | X/10  | [pass/warn/fail] |
| Typography     | X/10  | [pass/warn/fail] |
| Shape          | X/10  | [pass/warn/fail] |
| Elevation      | X/10  | [pass/warn/fail] |
| Components     | X/10  | [pass/warn/fail] |
| Layout         | X/10  | [pass/warn/fail] |
| Navigation     | X/10  | [pass/warn/fail] |
| Motion         | X/10  | [pass/warn/fail] |
| Accessibility  | X/10  | [pass/warn/fail] |
| Theming        | X/10  | [pass/warn/fail] |

## Critical Issues
[List items scoring 0-3 with specific file:line references and fixes]

## Warnings
[List items scoring 4-6 with recommendations]

## Passing
[List items scoring 7-10 with notes on what's done well]

## Recommended Fixes (Priority Order)
1. [Most impactful fix first]
2. ...
```

### Audit Methods

**For a live URL** (browser or devtools):
- Inspect computed styles and CSS variables (`--md-sys-*`)
- Resize viewport or use responsive mode for breakpoints
- Capture screenshots at key widths if helpful

**For source code** (file paths provided):
- HTML/JSX/Vue/Svelte; CSS/SCSS for tokens
- Check imports for `@material/web` vs `@material/mdc-*` (MD2)

**Quick checks** (adapt paths to your stack):
```
# Web: hardcoded colors
grep -rn '#[0-9a-fA-F]\{3,8\}' --include='*.css' --include='*.scss'

# MD2 on web
grep -rn '@material/mdc-' --include='*.js' --include='*.ts'
```

**Browser automation** (if your environment exposes MCP browser tools): navigate, snapshot DOM/CSS variables, resize for breakpoints — optional, not required.

### Scoring Guide

- **9-10**: Fully MD3 compliant, uses correct tokens and patterns
- **7-8**: Mostly compliant, minor issues (e.g., a few hardcoded values)
- **4-6**: Partially compliant, some MD3 patterns but significant gaps
- **1-3**: Major violations, mostly non-MD3 or MD2 patterns
- **0**: Not applicable or completely absent

Status thresholds: **pass** (7+), **warn** (4-6), **fail** (0-3)

## Reference Documents

- `references/color-system.md` — Color roles, tonal palettes, dynamic color, CSS mapping
- `references/typography-and-shape.md` — Type scale, shape corners, elevation, motion, Expressive notes
- `references/component-catalog.md` — Components: `@material/web` where applicable, CSS fallbacks
- `references/navigation-patterns.md` — Navigation selection, adaptive patterns
- `references/layout-and-responsive.md` — Breakpoints, canonical layouts
- `references/theming-and-dynamic-color.md` — Theming: CSS custom properties and dynamic color
