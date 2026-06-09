# Claude Desktop App — Visual Style Guide

A complete reference for replicating the Claude.ai desktop aesthetic in **both light and dark modes**, covering every UI component, chart, micro-interaction, and design token. Drop these tokens into any project to get the look.

This is a learning document — values are observed/derived from the live app and rounded into clean, reusable tokens. Not an official Anthropic spec.

---

## Table of Contents

1. [Design Philosophy](#1-design-philosophy)
2. [Color System](#2-color-system)
3. [Typography](#3-typography)
4. [Spacing & Layout](#4-spacing--layout)
5. [Border Radius](#5-border-radius)
6. [Shadows & Elevation](#6-shadows--elevation)
7. [Z-Index Scale](#7-z-index-scale)
8. [Buttons](#8-buttons)
9. [Inputs & Forms](#9-inputs--forms)
10. [Composer](#10-composer)
11. [Sidebar & Navigation](#11-sidebar--navigation)
12. [Cards](#12-cards)
13. [Popovers & Menus](#13-popovers--menus)
14. [Modals & Dialogs](#14-modals--dialogs)
15. [Drawers & Sheets](#15-drawers--sheets)
16. [Multi-Panel Split Layouts](#16-multi-panel-split-layouts)
17. [Tooltips](#17-tooltips)
18. [Tabs & Segmented Controls](#18-tabs--segmented-controls)
19. [Tables](#19-tables)
20. [Lists](#20-lists)
21. [Accordions](#21-accordions)
22. [Chat Messages](#22-chat-messages)
23. [Badges, Pills & Status](#23-badges-pills--status)
24. [Avatars](#24-avatars)
25. [Toggles, Checkboxes, Radios](#25-toggles-checkboxes-radios)
26. [Sliders & Progress](#26-sliders--progress)
27. [Toasts, Banners & Alerts](#27-toasts-banners--alerts)
28. [Empty, Loading & Error States](#28-empty-loading--error-states)
29. [Command Palette & Search](#29-command-palette--search)
30. [Charts & Data Viz](#30-charts--data-viz)
31. [Code Blocks & Syntax](#31-code-blocks--syntax)
32. [Markdown Rendering](#32-markdown-rendering)
33. [Iconography](#33-iconography)
34. [Keyboard Chips & Shortcuts](#34-keyboard-chips--shortcuts)
35. [Scrollbars & Selection](#35-scrollbars--selection)
36. [Motion System](#36-motion-system)
37. [Focus & Accessibility](#37-focus--accessibility)
38. [Responsive & Density](#38-responsive--density)
39. [Theme Implementation](#39-theme-implementation)
40. [Recognition Checklist](#40-recognition-checklist)
41. [What NOT to Do](#41-what-not-to-do)

---

## 1. Design Philosophy

Five principles drive every token below:

1. **Warm minimalism.** Built on warm off-whites and warm dark browns — never pure `#FFFFFF`, never `#000000`, never cool slate. There is always a peach/cream/sienna undertone.
2. **Soft, low-contrast surfaces.** Subtle background tints and small radius differences carry hierarchy — not heavy borders or strong shadows.
3. **Typography does the heavy lifting.** Serif for "moments" (greeting, AI output, display headlines) + clean sans for UI chrome. Lose the serif and it stops looking like Claude.
4. **Light and dark are siblings, not opposites.** Same design language, warm darks instead of warm lights. Surface relationships stay identical; the orange accent barely shifts.
5. **Quiet motion.** Almost nothing animates over 200ms. No bouncy springs in chrome. Hover changes are near-instant.

---

## 2. Color System

All colors as CSS variables. Components **never** hardcode hex.

```css
:root { /* light mode = default */ }
[data-theme="dark"] { /* overrides */ }
```

### 2.1 Brand & accent

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--accent-primary` | `#C96442` | `#D97757` | Send button, links, brand highlights |
| `--accent-primary-hover` | `#B5573A` | `#C96442` | Hover |
| `--accent-primary-pressed` | `#9E4A30` | `#B5573A` | Active/pressed |
| `--accent-soft` | `#F4E5D9` | `#3E2C22` | Tinted bg for accent badges |
| `--accent-soft-border` | `#E8D0BC` | `#5A3E2E` | Border on accent-tinted surfaces |
| `--accent-soft-text` | `#9E4A30` | `#E8A887` | Text on accent-soft surfaces |

### 2.2 Surface palette

Hierarchy: sidebar darkest, base mid, surface lightest (in light mode). Same relative ordering inverts in dark mode.

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--bg-base` | `#FAF9F5` | `#262624` | App canvas |
| `--bg-surface` | `#FFFFFF` | `#30302E` | Composer, cards, popovers, modals |
| `--bg-surface-raised` | `#FFFFFF` | `#363634` | Doubly-elevated (popover-on-modal) |
| `--bg-sidebar` | `#F5F4EE` | `#1F1E1D` | Left sidebar |
| `--bg-hover` | `#EFEEE6` | `#3A3A37` | Hover on rows, ghost buttons, user-message bubble |
| `--bg-selected` | `#E9E6DC` | `#454541` | Selected sidebar item, active nav |
| `--bg-input` | `#FFFFFF` | `#30302E` | Text input fields |
| `--bg-code` | `#F5F2EC` | `#1F1E1D` | Code blocks, inline code |
| `--bg-overlay` | `rgba(40, 35, 28, 0.32)` | `rgba(0, 0, 0, 0.6)` | Modal scrim |

**Mental model:** Light mode = "cream paper with paper-white cards stacked on top." Dark mode = "dark walnut wood with slightly-lighter walnut panels stacked on top."

#### 2.2.1 Surface tone presets

The surface family above can be re-tuned without touching accent, text, or border tokens. Three presets ship; theme (light/dark) and tone (warm/neutral/sienna) are orthogonal.

Applied via `:root[data-tone="..."]` — overrides only `--bg-base`, `--bg-surface`, `--bg-surface-raised`, `--bg-sidebar`, `--bg-hover`, `--bg-selected`, `--bg-input`, `--bg-code`. Accent, text, borders, shadows are untouched so the brand identity is preserved across tones.

| Tone | Mood | Light `--bg-base` | Dark `--bg-base` |
|---|---|---|---|
| `warm` (default) | Claude — peach/cream undertone | `#FAF9F5` | `#262624` |
| `neutral` | Greyscale — undertone stripped | `#F7F7F7` | `#242424` |
| `sienna` | Warmer than Claude — pushed toward orange | `#FAF4EA` | `#2A2520` |

**Light-mode neutral surfaces:** base `#F7F7F7`, surface `#FFFFFF`, sidebar `#F1F1F1`, hover `#EBEBEB`, selected `#E4E4E4`, code `#F1F1F1`.
**Light-mode sienna surfaces:** base `#FAF4EA`, surface `#FFFCF6`, sidebar `#F2EADC`, hover `#EAE0CC`, selected `#E2D6BD`, code `#F1E7D4`.
**Dark-mode neutral surfaces:** base `#242424`, surface `#2E2E2E`, surface-raised `#343434`, sidebar `#1C1C1C`, hover `#383838`, selected `#424242`, code `#1C1C1C`.
**Dark-mode sienna surfaces:** base `#2A2520`, surface `#36302B`, surface-raised `#3C362F`, sidebar `#221D17`, hover `#403930`, selected `#4B433A`, code `#221D17`.

Surface hierarchy and relative tonal gaps stay the same across all three tones — only the hue shifts. A component built against tokens looks correct in all 6 combinations (3 tones × 2 themes).

### 2.3 Text

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--text-primary` | `#1F1E1D` | `#F5F4EE` | Body, headings |
| `--text-secondary` | `#5C5B57` | `#B8B6AE` | Subtitles, secondary labels |
| `--text-tertiary` | `#8C8A82` | `#86847C` | Timestamps, hints, placeholders |
| `--text-disabled` | `#BFBDB5` | `#5C5B57` | Disabled controls |
| `--text-on-accent` | `#FFFFFF` | `#FFFFFF` | Text on the orange button |
| `--text-link` | `#C96442` | `#E08968` | Inline links |
| `--text-selection` | `rgba(201, 100, 66, 0.25)` | `rgba(217, 119, 87, 0.30)` | `::selection` background |

### 2.4 Borders & dividers

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--border-default` | `#E8E6DC` | `#3A3A37` | Inputs, cards |
| `--border-strong` | `#D4D2C7` | `#4A4A46` | Focused/active states |
| `--border-divider` | `#EFEEE6` | `#2D2D2B` | Hairlines between list items |
| `--border-focus-ring` | `rgba(201, 100, 66, 0.35)` | `rgba(217, 119, 87, 0.45)` | 3px focus ring |

### 2.5 Semantic colors

| Token | Light bg / fg | Dark bg / fg |
|---|---|---|
| `--success` | `#E6F1EA` / `#3F8F5C` | `#243029` / `#6FB587` |
| `--warning` | `#FAEED9` / `#C77A2C` | `#3A2E1C` / `#E0A24E` |
| `--error` | `#F7E0DC` / `#B33A2C` | `#3A2522` / `#E08070` |
| `--info` | `#E1EAF5` / `#3D6AA0` | `#1E2A3A` / `#7AA0CC` |

### 2.6 Data viz palette

For charts. See [§30](#30-charts--data-viz) for usage.

| Token | Light | Dark | Use |
|---|---|---|---|
| `--chart-1` | `#C96442` | `#D97757` | Series 1 (brand orange) |
| `--chart-2` | `#3D6AA0` | `#7AA0CC` | Series 2 (warm blue) |
| `--chart-3` | `#3F8F5C` | `#6FB587` | Series 3 (green) |
| `--chart-4` | `#A66B3A` | `#C9905E` | Series 4 (warm tan) |
| `--chart-5` | `#7B5BA6` | `#A88BC9` | Series 5 (warm purple) |
| `--chart-6` | `#C77A2C` | `#E0A24E` | Series 6 (amber) |
| `--chart-7` | `#5C8C8A` | `#8FB3B0` | Series 7 (muted teal) |
| `--chart-8` | `#B33A6B` | `#D9709A` | Series 8 (warm rose) |
| `--chart-grid` | `#EFEEE6` | `#2D2D2B` | Gridlines |
| `--chart-axis` | `#8C8A82` | `#86847C` | Axis labels |

---

## 3. Typography

### 3.1 Font families

```css
--font-serif: "Tiempos Text", "Tiempos", "Source Serif Pro", "Charter",
              "Iowan Old Style", Georgia, serif;
--font-sans:  "Styrene B", "Söhne", "Inter", -apple-system, BlinkMacSystemFont,
              "Segoe UI", system-ui, sans-serif;
--font-mono:  "JetBrains Mono", "Fira Code", "SF Mono", "Cascadia Code",
              Menlo, Consolas, monospace;
```

**Allocation rules — the single most important detail:**

- **Serif** → AI message body, large greetings, display headlines, the Claude wordmark.
- **Sans** → All UI chrome: buttons, sidebar, menus, labels, user's typed message, settings, tooltips.
- **Mono** → Code, technical identifiers, kbd chips.

Free alternatives: **Source Serif Pro** + **Inter** + **JetBrains Mono**.

### 3.2 Type scale

| Token | Size | Line height | Weight | Use |
|---|---|---|---|---|
| `--text-xs` | 11px | 16px | 500 | Timestamps, badges, table headers |
| `--text-sm` | 13px | 20px | 400/500 | Sidebar, secondary UI, table cells |
| `--text-base` | 15px | 24px | 400 | **Default body** (chat messages) |
| `--text-md` | 16px | 26px | 400 | Composer input |
| `--text-lg` | 18px | 28px | 500 | Section titles, modal headers |
| `--text-xl` | 22px | 30px | 500 | Page headings |
| `--text-2xl` | 28px | 36px | 400 (serif) | "How can I help you?" |
| `--text-3xl` | 36px | 44px | 400 (serif) | Greeting display |
| `--text-4xl` | 48px | 56px | 400 (serif) | Marketing/onboarding heroes |

**Headings in serif use weight 400 or 500, never bold.** The serif's natural weight does the work.

### 3.3 Tracking

- Body sans: `letter-spacing: -0.003em`.
- Display serif: `letter-spacing: -0.02em` to `-0.025em`.
- Uppercase labels (section heads, badges): `letter-spacing: 0.06em`.
- Mono: `letter-spacing: 0`.
- Paragraph spacing in chat: `margin-bottom: 1em`.
- Max line length: **~68ch**.

### 3.4 Dark-mode rendering

In dark mode only, add to body:
```css
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
```
Light text on warm dark brown looks heavy without it.

---

## 4. Spacing & Layout

### 4.1 Spacing scale (4px base)

```
--space-0:  0     --space-6:  24px
--space-1:  4px   --space-8:  32px
--space-2:  8px   --space-10: 40px
--space-3:  12px  --space-12: 48px
--space-4:  16px  --space-16: 64px
--space-5:  20px  --space-20: 80px
```

### 4.2 Layout grid

- **Sidebar:** 260px (collapsible to 56px icon rail).
- **Conversation column:** 768px max, centered. Never wider, even on 4K.
- **Top bar:** 56px tall.
- **Composer:** centered, matches 768px column, padded `--space-6` from bottom.
- **Page padding:** `--space-6` chat canvas, `--space-4` inside cards.

### 4.3 Component padding (memorize)

| Component | Padding |
|---|---|
| Button (sm / md / lg) | `6px 10px` / `8px 14px` / `10px 18px` |
| Input field | `10px 14px` |
| Composer textarea | `14px 16px` |
| Card | `20px` |
| Modal body | `24px` |
| Popover/menu | `6px` outer, `8px 12px` per item |
| Sidebar item | `8px 12px` |
| Tooltip | `6px 10px` |
| Table cell | `10px 12px` |
| Toast | `12px 16px` |
| Banner | `12px 16px` |

---

## 5. Border Radius

**No element is fully square** — minimum 4px radius everywhere.

```
--radius-xs:   4px    /* badges, tags, inline code */
--radius-sm:   6px    /* small buttons, inline pills */
--radius-md:   8px    /* default — inputs, menu items */
--radius-lg:   12px   /* cards, popovers, dropdowns */
--radius-xl:   16px   /* composer, modals, large panels */
--radius-2xl:  20px   /* attachment chips, user message bubbles */
--radius-full: 9999px /* avatars, send button, status dots */
```

**Critical applications:**
- Composer: `--radius-xl`. Send button: `--radius-full`.
- User bubble: `--radius-2xl`. AI message: **no radius — no bubble**.
- Modals: `--radius-xl`. Code blocks: `--radius-lg`. Tooltips: `--radius-md`.

---

## 6. Shadows & Elevation

Soft, low-spread, warm-tinted — never the cool-gray Material default.

### 6.1 Light mode

```css
--shadow-color: 30 25 20;  /* warm brown */

--shadow-xs:  0 1px 2px   rgb(var(--shadow-color) / 0.04);
--shadow-sm:  0 2px 6px   rgb(var(--shadow-color) / 0.06);
--shadow-md:  0 4px 12px  rgb(var(--shadow-color) / 0.08),
              0 2px 4px   rgb(var(--shadow-color) / 0.04);
--shadow-lg:  0 12px 28px rgb(var(--shadow-color) / 0.12),
              0 4px 8px   rgb(var(--shadow-color) / 0.06);
--shadow-xl:  0 24px 48px rgb(var(--shadow-color) / 0.18),
              0 8px 16px  rgb(var(--shadow-color) / 0.08);
```

### 6.2 Dark mode

```css
[data-theme="dark"] {
  --shadow-color: 0 0 0;

  --shadow-xs:  0 1px 2px   rgb(var(--shadow-color) / 0.20);
  --shadow-sm:  0 2px 6px   rgb(var(--shadow-color) / 0.28);
  --shadow-md:  0 4px 12px  rgb(var(--shadow-color) / 0.36),
                0 2px 4px   rgb(var(--shadow-color) / 0.24);
  --shadow-lg:  0 12px 28px rgb(var(--shadow-color) / 0.50),
                0 4px 8px   rgb(var(--shadow-color) / 0.32);
  --shadow-xl:  0 24px 48px rgb(var(--shadow-color) / 0.60),
                0 8px 16px  rgb(var(--shadow-color) / 0.40);
  --surface-highlight: inset 0 1px 0 rgb(255 255 255 / 0.04);
}
```

**Key principle:** in dark mode, **borders do more work than shadows.** Resting elevation is carried by border contrast against the canvas. Shadows kick in for floating elements (popovers, modals).

### 6.3 Where to use what

| Token | Use |
|---|---|
| `--shadow-xs` | Resting composer (light), input fields with elevation |
| `--shadow-sm` | Hover on cards, dropdown triggers |
| `--shadow-md` | Popovers, dropdown menus, tooltips |
| `--shadow-lg` | Modals, slide-overs, drawers |
| `--shadow-xl` | Top-level pickers (model switcher, command palette) |

---

## 7. Z-Index Scale

```
--z-base:        0
--z-dropdown:    100   /* native select, autocomplete */
--z-sticky:      200   /* sticky table headers, sticky nav */
--z-fixed:       300   /* top bar, fixed footers */
--z-popover:     400   /* popovers, menus */
--z-tooltip:     500   /* tooltips above popovers */
--z-overlay:     600   /* modal scrim */
--z-modal:       700   /* modal content */
--z-drawer:      800   /* full-screen drawers */
--z-toast:       900   /* toasts above everything */
--z-command:     1000  /* command palette (top-of-stack) */
```

---

## 8. Buttons

### 8.1 Sizes

| Size | Height | Padding | Font | Icon |
|---|---|---|---|---|
| sm | 28px | `6px 10px` | 12px / 500 | 14px |
| md (default) | 36px | `8px 14px` | 14px / 500 | 16px |
| lg | 44px | `10px 18px` | 15px / 500 | 18px |

### 8.2 Variants

**Primary (orange):**
- Bg: `--accent-primary` → `--accent-primary-hover` → `--accent-primary-pressed`.
- Text: `--text-on-accent` (white in both modes).
- Radius: `--radius-md` rectangular, `--radius-full` for circular send.
- Disabled: 40% opacity, no hover.

**Secondary:**
- Bg: transparent → `--bg-hover` on hover.
- Text: `--text-primary`.
- Border: 1px `--border-default` → `--border-strong` on hover.

**Ghost (icon buttons in chrome):**
- Bg: transparent → `--bg-hover`.
- 32×32px square (md), `--radius-md`.
- Icon: `--text-secondary` resting, `--text-primary` hover.

**Destructive:**
- Same shape as secondary, but text/border use `--error` foreground.

**Tertiary / link button:**
- No bg, no border. Text colored `--text-link`.
- Underline on hover only.

### 8.3 Button groups

- Buttons within a group share a single border, divided by 1px `--border-default` separators.
- First/last get rounded outer corners; middle is `--radius-0`.
- Active state: `--bg-selected` background, `--text-primary` text.

### 8.4 Loading state

- Replace icon (or prepend if no icon) with a 14px spinner.
- Spinner: 1.5px stroke `--text-on-accent` (on primary) or `--text-secondary` (on others), 0.8s linear rotation.
- Text remains, slightly faded (80% opacity).

---

## 9. Inputs & Forms

### 9.1 Text input

- Bg: `--bg-input`.
- Border: 1px `--border-default` → `--border-strong` on focus.
- Radius: `--radius-md`. Padding: `10px 14px`.
- Font: sans 14px, color `--text-primary`. Placeholder `--text-tertiary`.
- Focus: 3px ring of `--border-focus-ring`, offset 1px from border.
- Disabled: bg `--bg-hover`, text `--text-disabled`, no border change.

**Dark-mode contrast (important):** when an input sits inside a `--bg-surface` container (modal, card, popover, sheet, pane), its background must drop to `--bg-base` so the field reads as a sunken well against the surface. Resting border in this case bumps from `--border-default` to `--border-strong`, and focus colour becomes `--accent-primary` directly (no ring). Without this, input and surface collapse to the same tone in dark mode and the field disappears.

Inverse case: if the pane is `--bg-sidebar` (darker than canvas), the input takes `--bg-surface` instead, so it still reads lighter than its container.

### 9.2 Textarea

- Same as input but `min-height: 80px`, vertical resize allowed.
- Inside the composer: no own border, inherits surface (see [§10](#10-composer)).

### 9.3 Select / dropdown trigger

- Same shape as input. Right side has a 16px chevron-down icon (`--text-secondary`).
- 8px gap between text and icon.

### 9.4 Search input

- Leading 16px search icon (`--text-tertiary`), 10px gap before text.
- Optional trailing clear button (X, ghost-style 24×24px) when text present.
- Same border/focus treatment as text input.

### 9.5 File upload

- **Drop zone:** dashed 1.5px border `--border-strong`, radius `--radius-lg`, padding `--space-8`. Centered icon (24px) + text "Drag files here or click to browse". On drag-over: border becomes solid `--accent-primary`, bg `--accent-soft`.
- **File chip (after upload):** pill, `--radius-2xl`, height 32px, bg `--bg-base` (light) / `--bg-hover` (dark). Leading file-type icon, filename (truncated), trailing X button.

### 9.6 Field structure

```
[Label]              ← 13px sans 500, color --text-primary, mb 6px
[Input]
[Helper text]        ← 12px sans 400, color --text-tertiary, mt 6px
```

Required indicator: red asterisk (`*` in `--error` fg) right after label, no space.

### 9.7 Validation states

| State | Border | Helper text | Icon |
|---|---|---|---|
| Default | `--border-default` | `--text-tertiary` | none |
| Focused | `--border-strong` + ring | `--text-tertiary` | none |
| Error | `--error` fg | `--error` fg | 16px alert-circle, leading |
| Success | `--success` fg | `--success` fg | 16px check-circle, leading |
| Warning | `--warning` fg | `--warning` fg | 16px alert-triangle |

Error ring: 3px of error fg at 25% alpha.

### 9.8 Field groups

- Vertical stack: 16px gap between fields, 4px gap between label/input/helper within a field.
- Horizontal: 12px gap.
- Fieldset legend: 13px sans 500 `--text-secondary`, uppercase tracked optional.

### 9.9 Multi-step forms

- Stepper at top: numbered circles connected by 1px lines.
- Active step: filled `--accent-primary`, white number.
- Completed: filled `--success` fg, white check icon.
- Pending: 1.5px border `--border-strong`, number `--text-tertiary`.
- Connector line: `--border-default` pending, `--accent-primary` to active step.

---

## 10. Composer

The design centerpiece.

| Property | Light | Dark |
|---|---|---|
| Background | `--bg-surface` | `--bg-surface` (`#30302E`) |
| Border resting | 1px `--border-default` | 1px `--border-default` |
| Shadow resting | `--shadow-xs` | none — border carries it |
| Border focused | `--border-strong` | `--border-strong` |
| Shadow focused | `--shadow-sm` | `--shadow-sm` |
| Optional inner highlight | none | `inset 0 1px 0 rgb(255 255 255 / 0.04)` |

**Both modes:**
- Radius: `--radius-xl` (16px).
- Padding: `14px 16px 12px 16px`.
- Min-height: 56px. Max-height: ~200px before scrolling.
- Inner toolbar (~36px): attach paperclip left, model picker, tools, send button right. 4px gap between.
- Send button: circular 32×32px, `--accent-primary`, white arrow-up icon. 50% opacity when textarea is empty.
- Attachment chips above input: pill `--radius-2xl`, height 32px, bg `--bg-base`.
- **No focus ring** — the border + shadow shift is the affordance.

---

## 11. Sidebar & Navigation

### 11.1 Sidebar

- Width 260px, bg `--bg-sidebar`.
- Top: serif wordmark + "New chat" button (full-width, ghost with subtle accent on hover).
- Section labels: uppercase tracked sans, 11px, `letter-spacing: 0.06em`, `--text-tertiary`.
- Conversation rows: 36px, padding `8px 12px`, `--radius-md`, single-line truncated. Hover `--bg-hover`. Active `--bg-selected`.
- Right-side actions (delete, rename) appear only on hover, fade in 150ms.
- Bottom: avatar + name row → opens account menu.

**Critical:** sidebar is **darker** than canvas in both modes (not the common Material pattern of lighter-than-canvas).

### 11.2 Top bar

- Height 56px, bg `--bg-base` (matches canvas — no separate color), 1px bottom border `--border-divider`.
- Left: breadcrumbs or page title (15px sans 500). Right: action buttons (ghost, 32×32px).

### 11.3 Breadcrumbs

- Sans 13px, `--text-secondary` for ancestors, `--text-primary` for current.
- Separator: 12px chevron-right icon, `--text-tertiary`, 6px gap on each side.
- Hover on ancestor: underline + `--text-primary`.

### 11.4 Tabs (see [§18](#18-tabs--segmented-controls) for details)

### 11.5 Pagination

- Numbered buttons: 32×32px, `--radius-md`, ghost style.
- Current page: bg `--bg-selected`, text `--text-primary`, weight 500.
- Prev/Next: same size with chevron icons. Disabled when at boundary (40% opacity).
- Ellipsis (`…`) between non-adjacent ranges, no button styling.

---

## 12. Cards

### 12.1 Standard card

- Bg: `--bg-surface`.
- Border: 1px `--border-default`.
- Radius: `--radius-lg` (12px).
- Padding: `--space-5` (20px).
- Shadow: none resting, `--shadow-sm` on hover (only if interactive).

**Structure:**
- Header: title (15px sans 500) + optional action (ghost icon button, top-right).
- Body: 13px `--text-secondary`, 12px gap from header.
- Footer (optional): 1px top border `--border-divider`, 12px above content, padding-top 16px.

### 12.2 Interactive card (clickable)

- Hover: shadow `--shadow-sm`, border `--border-strong`, `transform: translateY(-1px)`, transition 120ms.
- Active: `transform: translateY(0)`, shadow `--shadow-xs`.

### 12.3 Selected card

- Border: 1.5px `--accent-primary`.
- Bg tint: `--accent-soft` at very low opacity, or keep `--bg-surface` and rely on border alone.
- Top-right corner: small filled `--accent-primary` checkmark badge (optional).

### 12.4 Compact card variant

- Padding: `--space-4` (16px).
- Title: 14px. Body: 12px.
- Use in dense grids (3+ per row).

### 12.5 Card grid

- Default gap: `--space-4` (16px).
- Min card width before wrapping: 240px.
- `grid-template-columns: repeat(auto-fill, minmax(240px, 1fr))`.

### 12.6 KPI / metric tile

The "label + big number + delta + sparkline" pattern that lives at the top of every dashboard. Mono on the value is non-negotiable — sans on big numbers feels imprecise.

**Container:** card per [§12.1](#121-standard-card), padding `--space-5` (20px). Use the compact `--space-4` (16px) variant when 4+ tiles sit per row.

**Anatomy:**

| Sub-element | Size | Token | Notes |
|---|---|---|---|
| Label | 11px / 500 | `--text-tertiary` | Uppercase, `letter-spacing: 0.06em`, mb 8px |
| Value | 28px / 500 mono | `--text-primary` | 22px in compact density |
| Delta arrow | 12px | semantic | Inline glyph, 4px gap from text |
| Delta text | 12px sans, mono digits | `--success` / `--error` / `--text-secondary` | Format `↑ 4.2pp vs last week` |
| Sparkline | 24px tall | `--chart-1` or contextual | Width: full card minus padding |

**Delta colour rule:**

- Movement in the "good" direction → `--success` fg.
- Movement in the "bad" direction → `--error` fg.
- Neutral or below the change threshold → `--text-secondary`.

The "good" direction is metric-dependent: for a churn KPI, `↓` is `--success`; for revenue, `↑` is `--success`.

**Format:** `↑ 4.2pp vs last week` or `↓ 12 vs yesterday`. Keep digits in mono, period words in sans.

**Optional sparkline:** 24px-tall, per [§30.7](#307-sparkline). Sits below the delta with 8px gap. Width is full card width minus padding so it reads as the "underline" of the value.

**Optional category dot:** 4px filled circle, semantic colour, 1.5px to the left of the label. Use only for status tiles (e.g., "Pipeline health: green").

**Hover:** none. Tiles are not interactive by default. If clickable, follow [§12.2](#122-interactive-card-clickable) — don't invent a third hover state.

---

## 13. Popovers & Menus

### 13.1 Popover

- Bg: `--bg-surface` (or `--bg-surface-raised` if stacked on a modal).
- Border: 1px `--border-default`.
- Radius: `--radius-lg`.
- Shadow: `--shadow-md`.
- Outer padding: 6px.
- Animation: fade + 4px translate-y on open, 120ms `ease-out`.
- Arrow (optional): 8px triangle, same bg + border continuation.

### 13.2 Menu item

- Padding: `8px 12px`, radius `--radius-md`.
- Hover: bg `--bg-hover`. Disabled: 40% opacity, no hover.
- Text: 13px sans, `--text-primary`.
- Leading icon: 16px, `--text-secondary`, 10px gap from text.
- Trailing accessory: keyboard shortcut chip ([§34](#34-keyboard-chips--shortcuts)) right-aligned, or chevron-right (16px) for nested.
- Selected (radio-style menu): leading 16px check icon `--accent-primary`.
- Destructive item: text `--error` fg, leading icon also `--error`.

### 13.3 Section dividers

- 1px `--border-divider`, 6px vertical margin.
- Optional section label above: uppercase 10px tracked sans, `--text-tertiary`, padding `8px 12px 4px`.

### 13.4 Submenus

- Open on hover with 80ms delay, close with 200ms delay.
- Slide in from right edge of parent, 4px translate-x animation 120ms.
- Same styling as parent menu.

---

## 14. Modals & Dialogs

### 14.1 Structure

- **Scrim:** `--bg-overlay`, fade in 150ms.
- **Container:** `--bg-surface`, `--radius-xl`, `--shadow-xl`. Max-width 520px (forms) or 720px (large content). In dark, optionally add `--surface-highlight` inset.
- **Header:** 18px sans 500, padding 20px. Optional close X top-right (ghost button, 32×32px).
- **Body:** padding 24px, scrollable if too tall.
- **Footer:** padding 16px, 1px top `--border-divider`. Right-aligned actions: secondary then primary, 8px gap.

### 14.2 Animations

- Open: scrim fade 150ms + container fade + scale `0.96 → 1.0` over 180ms `cubic-bezier(0.2, 0, 0, 1)`.
- Close: reverse, slightly faster (140ms).
- Backdrop click closes (unless destructive form is dirty — show confirm).

### 14.3 Confirmation dialog

- Smaller (max-width 420px).
- Optional 24px leading icon (warning / error / info color) + 12px gap.
- Body text: 14px `--text-secondary`.
- Destructive primary button uses destructive variant.

### 14.4 Full-screen modal (mobile / takeover)

- Removes scrim, fills viewport.
- Top bar with close (left) + title (center) + primary action (right).
- Use only for complex multi-step flows.

---

## 15. Drawers & Sheets

### 15.1 Side drawer

- Slides from right (or left), width 400px (or 560px wide variant).
- Bg `--bg-surface`, shadow `--shadow-lg` on the canvas-facing edge.
- 1px border `--border-default` on the canvas-facing edge.
- Radius: 0 on canvas-facing edge, `--radius-lg` on outer edges (only if not flush to viewport edge).
- Animation: translate-x from `100%` to `0`, 240ms `cubic-bezier(0.2, 0, 0, 1)`.
- Scrim: optional, lighter than modal scrim (`rgba(40, 35, 28, 0.18)` light / `rgba(0, 0, 0, 0.4)` dark).

### 15.2 Bottom sheet

- Slides from bottom, max-height 80vh.
- Top edge: `--radius-xl` corners, optional 36×4px drag handle (`--border-strong`) centered 8px from top.
- Same animation pattern as side drawer but on Y axis.

### 15.3 Header/footer pattern

Same as modal: header with title + close, optional footer with actions, scrollable body in between.

---

## 16. Multi-Panel Split Layouts

For dashboards, inboxes, file managers, triage queues — anywhere multiple persistent surfaces sit side-by-side instead of stacking. Real-world parallels: Mail.app (3-pane), Linear (sidebar + canvas), Claude's own canvas + sidebar split.

### 16.1 Layout shapes

| Shape | Columns | Typical use |
|---|---|---|
| 2-panel | sidebar + canvas | Settings, Linear, Claude chat |
| 3-panel | nav + list + content | Mail, inbox-style triage |
| 4-panel | nav + list + content + assistant | File manager, full triage with AI sidekick |

```
┌──────────┬────────────────┬─────────────────────────┬──────────────┐
│          │                │                         │              │
│   nav    │     list       │        content          │   assistant  │
│ 220–260  │   320–400      │   flex-1 (min 480)      │   320–400    │
│          │                │                         │              │
│          │                │                         │              │
└──────────┴────────────────┴─────────────────────────┴──────────────┘
```

### 16.2 Outer canvas

- Bg: `--bg-base`.
- Padding: `--space-4` (16px) around the panel cluster — gives the rounded corners room to breathe against the canvas.
- Gap between panels: `--space-3` (12px).

### 16.3 Panel container

Each panel is a card per [§12](#12-cards):

- Bg: per its role (see §16.4).
- Border: 1px `--border-default` in light mode; **1px `--border-strong` in dark mode** — list and content panels share `--bg-surface`, so the seam between them only reads if borders are stronger than canvas hairlines.
- Internal header bottom border: `--border-divider` (light) / `--border-default` (dark) — same logic.
- Radius: `--radius-lg` (12px).
- `overflow: hidden` on the panel root so inner scrollers and headers clip to the rounded corners.
- **No shadow at rest.** Borders carry it. Shadows only kick in when a panel is dragged or floated above (then `--shadow-lg`).

### 16.4 Background hierarchy

The brown-on-cream layering is the trick — each adjacent panel must be a different surface tone so the seams read as separation, not noise.

| Panel | Bg token | Note |
|---|---|---|
| Navigation | `--bg-sidebar` | Darker than canvas (matches [§11.1](#111-sidebar)) |
| List | `--bg-surface` | Lightest, draws focus |
| Content | `--bg-surface` | Same as list — they're peers |
| Assistant / secondary | `--bg-base` | Recedes; matches canvas tone |

### 16.5 Panel widths

| Panel | Width | Min | Max |
|---|---|---|---|
| Navigation | 220–260px | 200px | 320px |
| List | 320–400px | 280px | 480px |
| Content | flex-1 | 480px | — |
| Assistant | 320–400px | 280px | 480px |

### 16.6 Resizable dividers (optional)

- Hot zone: 4px wide. Visual at rest: 1px `--border-default` line (lives inside the gap).
- Hover: 2px `--accent-primary` line, cursor `col-resize`. Per [§35.3](#353-drag-handles).
- Drag: panel widths update live; persist to localStorage on drop.
- Double-click: reset to default width.

### 16.7 Responsive collapse

| Viewport | Behavior |
|---|---|
| ≥ `--bp-xl` (1280px) | All panels visible |
| `--bp-lg` to `--bp-xl` | Rightmost panel (assistant) collapses to a slide-over drawer ([§15.1](#151-side-drawer)), triggered by a chrome icon |
| `--bp-md` to `--bp-lg` | List + content visible, nav collapses to icon rail (56px) |
| < `--bp-md` (768px) | Single-panel router. Show one at a time, back button in top-left chrome to step up the hierarchy |

### 16.8 Header alignment

Each panel's internal header is 48px tall, padding `--space-3 --space-4`, 1px bottom border `--border-divider`. Header rows align across panels even when content scrolls underneath.

---

## 17. Tooltips

Always inverted from current theme.

| | Light | Dark |
|---|---|---|
| Bg | `#1F1E1D` | `#F5F4EE` |
| Text | `#F5F4EE` | `#1F1E1D` |

- 12px sans 500.
- Padding: `6px 10px`.
- Radius: `--radius-md`.
- Shadow: `--shadow-sm`.
- 6px arrow same color as bg.
- Show delay: 400ms. Hide delay: 80ms.
- Animation: fade + 4px translate (away from anchor) 120ms `ease-out`.
- Max-width: 240px, wraps for longer.

---

## 18. Tabs & Segmented Controls

### 18.1 Tabs (line style — used in pages)

- Container: 1px bottom border `--border-divider`.
- Tab: padding `10px 16px`, sans 14px 500, `--text-secondary`.
- Hover: `--text-primary`.
- Active: `--text-primary`, 2px bottom border `--accent-primary` (sits on top of container border, same baseline).
- Active border animation: slides between tabs, 240ms `cubic-bezier(0.2, 0, 0, 1)`.

### 18.2 Segmented control (pill style — used in toolbars)

- Container: bg `--bg-hover`, radius `--radius-md`, padding 3px.
- Segment: radius `--radius-sm` (one less than container), padding `6px 12px`, sans 13px 500.
- Active: bg `--bg-surface`, shadow `--shadow-xs`, `--text-primary`.
- Inactive: transparent, `--text-secondary`. Hover: `--text-primary`.

### 18.3 Vertical tabs

- Width 200px, items left-aligned.
- Active indicator: 2px left border `--accent-primary` instead of bottom.
- Item: padding `8px 12px`, radius `--radius-md`, hover `--bg-hover`, active `--bg-selected`.

### 18.4 Filter pill row

The "active filters above a list / table" pattern. Distinct from a segmented control (mutually-exclusive choice) and from a closeable tag ([§23.4](#234-tag-closeable), free-form pills): each pill represents a *constraint* applied to the data below.

**Container:** horizontal flex, gap `--space-2` (8px), wraps. Sits just below the view header, padding-bottom `--space-3` (12px).

**Active filter pill:**

| Property | Value |
|---|---|
| Height | 28px |
| Padding | `4px 10px 4px 8px` (extra left for icon) |
| Bg | `--accent-soft` |
| Text | `--accent-soft-text`, 12px sans 500 |
| Border | 1px `--accent-soft-border` |
| Radius | `--radius-full` |
| Leading icon | 12px filter-type icon, 6px gap |
| Trailing close | 12px X icon, hover bg darkens 8% |

**"Add filter" trigger pill:** same shape but bg transparent, dashed `1px --border-default` border, `--text-secondary` text + leading 12px plus icon. Shows when no filter is active, and as a perpetual trailing pill to add more.

**"Clear all" link:** only renders when 2+ filters are active. Right-aligned in the row, 12px sans 500 `--text-link`, no underline at rest, underline on hover (per [§32.6](#326-links)).

```css
.filter-pill {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  height: 28px;
  padding: 4px 10px 4px 8px;
  background: var(--accent-soft);
  color: var(--accent-soft-text);
  border: 1px solid var(--accent-soft-border);
  border-radius: var(--radius-full);
  font: 500 12px var(--font-sans);
}

.filter-pill__close {
  width: 14px; height: 14px;
  border-radius: var(--radius-full);
  display: grid; place-items: center;
}
.filter-pill__close:hover {
  background: color-mix(in srgb, var(--accent-soft), #000 8%);
}

.filter-pill--add {
  background: transparent;
  border: 1px dashed var(--border-default);
  color: var(--text-secondary);
}
```

---

## 19. Tables

### 19.1 Structure

- Bg: inherits canvas (no own bg).
- Header row: bg `--bg-base`, 1px bottom border `--border-default`. Sticky on scroll (z-index `--z-sticky`).
- Header cell: sans 11px 500 uppercase, `letter-spacing: 0.06em`, `--text-tertiary`. Padding `10px 12px`. Left-aligned (numbers right-aligned).
- Body row: 1px bottom border `--border-divider`. Hover: bg `--bg-hover`.
- Body cell: sans 13px, `--text-primary`. Padding `10px 12px`.
- Selected row: bg `--accent-soft` (light) or 8% `--accent-primary` overlay (dark).

### 19.2 Density variants

| Density | Row height | Cell padding |
|---|---|---|
| Compact | 32px | `6px 12px` |
| Default | 40px | `10px 12px` |
| Comfortable | 52px | `14px 12px` |

### 19.3 Sorting

- Sortable header: cursor pointer, hover bg `--bg-hover`.
- Sort icon: 12px chevron, leading or trailing depending on alignment.
- Active sort: `--text-primary`, icon filled.
- Inactive: `--text-tertiary`, icon at 50% opacity (only visible on hover).

### 19.4 Selection

- Leading checkbox column, 40px wide.
- Header checkbox: select-all (indeterminate state when partial).
- Selected rows: bg shift as above + leading 3px `--accent-primary` border on the row's left edge.

### 19.5 Empty state in table

- Single full-width row with 80px min-height.
- Centered: 32px icon `--text-tertiary`, 8px gap, 13px `--text-secondary` message.

### 19.6 Inline editing

- Cell on click: border `--border-strong`, ring 2px `--border-focus-ring`, becomes input.
- Save on blur or Enter, cancel on Esc.

### 19.7 Conditional formatting

Cell colour-coded by value — drop-off heatmap as table, monthly performance trackers, conversion grids. The cell *is* the data point, so don't colour-code the entire row.

**Monotonic positive scale** (e.g., conversion rate): bottom = `--bg-base`, top = `--accent-primary`. Interpolate with `color-mix`:

```css
background: color-mix(in srgb, var(--bg-base), var(--accent-primary) 65%);
```

**Diverging scale** (gain / loss): `--info` fg → `--bg-base` (zero) → `--accent-primary`. When the diverging axis is performance, use `--success` fg ↔ `--error` fg instead.

**Cell text:** stays `--text-primary` until the cell bg crosses ~50% saturation, then switches to `--text-on-accent` (white). A simple threshold check on the interpolation percent is fine.

**Cell padding:** `8px 12px` — slightly tighter than [§19.1](#191-structure) default to make the colour read.

**Cell gap:** 2px between coloured cells, achieved with:

```css
table { border-collapse: separate; border-spacing: 2px; }
```

So each cell reads as its own swatch, like a heatmap ([§30.9](#309-heatmap)).

**Hover:** 1.5px outline `--text-primary` on the cell. Tooltip per [§30.10](#3010-tooltips-on-data) shows the underlying value (the colour is the lossy summary).

**Inline legend:** below the table, 4 swatches at quartile breaks + numeric ranges:

```
[░░░] 0–25%   [▒▒▒] 25–50%   [▓▓▓] 50–75%   [███] 75–100%
```

11px sans, `--text-tertiary`, swatches `--radius-xs` 12×12px.

### 19.8 Dashboard-dense data tables

The "data dense" variant for KPI dashboards / tracker views, where you need to see 30+ rows without scrolling. Tighter than [§19.2](#192-density-variants) compact, and intentionally so.

| Property | Value |
|---|---|
| Row height | 28px |
| Cell padding | `4px 10px` |
| Body text | sans 12px (numbers in mono 12px JetBrains Mono) |
| Header text | sans 10px 500 uppercase, `letter-spacing: 0.06em` |
| Row separator | `1px solid color-mix(in srgb, var(--border-divider), transparent 50%)` (full strength `--border-divider` is too heavy at this density) |

**Zebra striping (optional):** even rows `--bg-base`, odd rows transparent. Skip the per-row border in this case — the alternating bg carries it.

**Numerics:** right-aligned. Units in `--text-tertiary` 10px, sit to the right of the number with 4px gap.

**Inline mini-charts:** sparklines and mini bars live in cells at 16–24px tall, full cell width. See [§30.7](#307-sparkline) for sparkline styling. Mini bars: 4px tall, radius `--radius-full`, fill `--accent-primary`, track transparent.

**No hover lift, no row shadow.** Hover bg `--bg-hover` is sufficient — anything more reads as fidget at this density.

---

## 20. Lists

### 20.1 Standard list

- Vertical stack, no gap (rows touch).
- Each row: padding `12px 16px`, 1px bottom border `--border-divider`.
- Hover: bg `--bg-hover`.
- Last row: no bottom border.

### 20.2 Two-line list item

```
[Avatar/Icon]  [Title             ]  [Trailing meta]
               [Subtitle (smaller)]
```

- Title: 14px sans 500, `--text-primary`.
- Subtitle: 12px sans, `--text-secondary`.
- Trailing: 11px `--text-tertiary` (timestamp) or action icons.
- Avatar/icon column: 40px wide.

### 20.3 Description list

- Term: 13px sans 500, `--text-secondary`.
- Description: 14px sans 400, `--text-primary`.
- Layout: stacked or 2-column (term left ~30%, description right).

### 20.4 Bulleted/numbered (in markdown)

See [§32](#32-markdown-rendering).

### 20.5 Severity-coded rows

The "leading 8px coloured dot or 3px accent strip" pattern used in inboxes, attention queues, alert lists, error logs. Two affordances; pick one per list.

**Variant A — 8px severity dot:**

| Property | Value |
|---|---|
| Position | Leading column, vertically centered with row text |
| Size | 8px (default) / 6px (compact density) |
| Halo (optional) | 1px border at 30% alpha of dot colour — helps it pop on busy bgs |

| Severity | Colour |
|---|---|
| Info | `--info` fg |
| Warning | `--warning` fg |
| Error | `--error` fg |
| Resolved / positive | `--success` fg |
| Unread | `--accent-primary` (per [§23.2](#232-count-badge-notification-dotnumber)) |

**Variant B — 3px leading accent strip:**

- Position: row's left edge, full row height, 3px wide.
- Use when severity needs to read at a glance even when the row is below the fold (long inboxes, tail logs).
- Selected rows: combine — strip becomes `--accent-primary`, severity dot stays its own colour.

**Combination rule:** only ONE severity affordance per row. Don't stack a dot AND a strip unless the strip means "selected" and the dot means "severity" — that's the legitimate exception.

**Accessibility:** severity colour alone is insufficient for color-blind users. Pair with a leading icon or text label per [§37](#37-focus--accessibility). E.g., the `alert-triangle` icon for warning, `alert-circle` for error — same icons mapped in [§33.5](#335-canonical-icon-map).

---

## 21. Accordions

- Container: each item has 1px bottom border `--border-divider`. First item has top border too. No outer card.
- Header row: padding `14px 0`, sans 14px 500, `--text-primary`. Trailing 16px chevron-down, rotates 180° when open (180ms `ease-out`).
- Hover: `--text-primary` (full opacity, no bg change). Cursor pointer.
- Body: padding `0 0 16px`, 13px `--text-secondary`.
- Animation: height auto (use `grid-template-rows: 0fr → 1fr` trick for smooth), 200ms `cubic-bezier(0.2, 0, 0, 1)`.

---

## 22. Chat Messages

The most distinctive layout.

| Element | Light | Dark |
|---|---|---|
| **User bubble bg** | `--bg-hover` (`#EFEEE6`) | `--bg-hover` (`#3A3A37`) |
| **User bubble radius** | `--radius-2xl` (20px) | same |
| **User bubble padding** | `12px 16px` | same |
| **User bubble max-width** | 80% of column, right-aligned | same |
| **User text** | sans 15px `--text-primary` | same |
| **Assistant bg** | **none** | **none** |
| **Assistant border** | **none** | **none** |
| **Assistant text** | **serif** 16px / 26px line-height `--text-primary` | same |
| **Assistant max-width** | full column (768px) | same |
| **Message gap** | `--space-6` (24px) between turns | same |

**The serif/sans split between assistant and user is non-negotiable.** This is the single most recognizable design choice in the app. In dark mode, the assistant's serif on the bare brown canvas is one of the prettiest moments — don't break it with a card or background.

### 22.1 Message hover actions

- Below assistant message, on hover: row of ghost icon buttons (24×24px each, 4px gap): copy, retry, thumbs-up, thumbs-down.
- Fade in over 120ms when hovering anywhere on the message block.

### 22.2 Streaming indicator

- 8px filled circle, `--text-tertiary`.
- 1.2s sine-eased opacity loop: 0.3 → 1.0 → 0.3.
- Appears at end of currently-streaming text, removed when stream completes.

### 22.3 Tool / function call display

- Inset card: bg `--bg-code`, radius `--radius-lg`, padding 12px 16px.
- Header: 12px sans 500, `--text-secondary`, leading 14px tool icon.
- Body: mono 12px, `--text-secondary`, collapsed by default with chevron.

### 22.4 Streaming text cadence

How the assistant's text actually arrives on screen. Distinct from [§22.2](#222-streaming-indicator) (the trailing caret) — this is about pacing the words themselves.

**Reveal granularity:** per-token (~3–5 chars at a time), not per-character. Per-char looks janky and fights the reader's saccade.

**Cadence:** **25–35 ms per token**, equivalent to ~150–200 wpm reading speed. Faster feels artificial (the reader can't keep up with their eyes); slower feels frustrating.

**Easing:** linear cadence is fine for body text. Start the first 200ms with a sine ease-in (slower opening) so the appearance feels less abrupt — `cubic-bezier(0.4, 0, 0.6, 1)` on the opacity of the first ~6 tokens.

**Punctuation pause:** insert a one-tick (~30ms) pause after `.` `!` `?` to add natural breath. **No pause for `,` or `;`** — too disruptive at this cadence.

**Markdown rendering:** defer parse until a complete block (paragraph, heading, list item) lands. Mid-stream rendering of partial markdown (`**half-bold` → `**half-bold**`) causes layout flicker — render as plain text until the closing token arrives.

**Code blocks:** stream into a `<pre>` first; apply syntax highlighting on stream complete. Highlighting partial code re-paints the entire block on every token.

**Streaming indicator:** 8px circle `--text-tertiary`, 1.2s sine opacity 0.3 → 1.0 → 0.3. Anchored at the end of the latest token, repositions on every render. See [§22.2](#222-streaming-indicator).

| Param | Value |
|---|---|
| Token reveal | 25–35 ms |
| Sentence-end pause | +30 ms after `.` `!` `?` |
| First-200ms easing | `cubic-bezier(0.4, 0, 0.6, 1)` |
| Indicator | 8px circle, 1.2s sine, `--text-tertiary` |

---

## 23. Badges, Pills & Status

### 23.1 Badge

- Height 22px, padding `2px 8px`, radius `--radius-full`.
- Text: 11px sans 500.
- Variants:
  - Neutral: `--bg-hover` + `--text-secondary`.
  - Accent: `--accent-soft` + `--accent-soft-text`.
  - Success/warning/error/info: pair semantic bg + fg.
- Optional leading 12px icon, 4px gap.

### 23.2 Count badge (notification dot/number)

- Small dot variant: 8px circle, `--accent-primary`. Used for unread indicators.
- Number variant: 18px height, `--radius-full`, padding `0 6px`, bg `--accent-primary` (or `--error` for alerts), text white 10px 600. Min-width 18px.

### 23.3 Status indicator (presence)

- 8px circle with 2px white border (matches surface).
- Online: `--success` fg. Away: `--warning` fg. Busy: `--error` fg. Offline: `--text-tertiary`.
- Positioned at bottom-right of avatar.

### 23.4 Tag (closeable)

- Same as badge but height 24px, with trailing 12px X button (clickable, hover bg darkens 8%).

---

## 24. Avatars

### 24.1 Sizes

| Size | Dimension | Use |
|---|---|---|
| xs | 20px | Inline text, table cells |
| sm | 28px | List items, comments |
| md | 36px | Default UI, sidebar bottom |
| lg | 48px | Profile cards |
| xl | 80px | Profile pages |

### 24.2 Style

- Radius: `--radius-full`.
- Border: 1px `--border-default` on light backgrounds (helps separation when avatar is white-heavy).
- Image: `object-fit: cover`.

### 24.3 Initials fallback

- Bg: pseudo-random from a fixed warm palette (NOT bright primary colors). Use 6–8 muted tones derived from the chart palette at 30% saturation.
- Text: white, sans 500. Size = `floor(avatar size × 0.4)`.
- 1–2 initials, uppercase.

### 24.4 Avatar group (stack)

- Negative margin overlap: `-8px` (sm), `-10px` (md).
- 2px solid `--bg-base` border on each (creates the gap appearance).
- Trailing "+N" avatar for overflow: bg `--bg-hover`, text `--text-secondary`.

---

## 25. Toggles, Checkboxes, Radios

### 25.1 Toggle

- Track: 32×18px, `--radius-full`. Off: `--border-strong`. On: `--accent-primary`.
- Knob: 14×14px white circle, `--shadow-xs` (light) or `--shadow-sm` (dark).
- Animation: 180ms `cubic-bezier(0.2, 0, 0, 1)`.

### 25.2 Checkbox

- 16×16px, `--radius-xs`.
- Unchecked: 1.5px border `--border-strong`, transparent bg.
- Checked: filled `--accent-primary`, white 12px checkmark.
- Indeterminate: filled `--accent-primary`, white 8×2px horizontal bar.
- Hover (unchecked): bg `--bg-hover`.
- Animation: scale + check stroke draw, 140ms.

### 25.3 Radio

- 16×16px circle.
- Unchecked: 1.5px border `--border-strong`.
- Checked: 1.5px border `--accent-primary`, 6px center dot `--accent-primary`.
- Animation: dot scale `0 → 1` 140ms.

### 25.4 Group spacing

- Vertical: 12px between items.
- Horizontal: 20px between items.
- Label: 14px sans, 8px gap from control. Cursor pointer on label.

In dark mode, **the white knob and checkmark stay white — they don't invert.** Orange stays orange.

---

## 26. Sliders & Progress

### 26.1 Slider

- Track: full-width, 4px tall, radius `--radius-full`, bg `--border-default`.
- Filled track (left of thumb): `--accent-primary`.
- Thumb: 16px circle, white bg, 1.5px border `--accent-primary`, `--shadow-sm`.
- Hover: thumb scales to 18px, 140ms.
- Active (dragging): thumb scales to 20px, ring 6px `--border-focus-ring`.
- Tick marks (optional): 2×2px squares, `--text-tertiary`, above track.
- Value label (on hover): tooltip-style, 6px above thumb.

### 26.2 Linear progress

- 4px tall, radius `--radius-full`, track `--border-default`, fill `--accent-primary`.
- Determinate: width animates to value over 240ms `ease-out`.
- Indeterminate: 30% wide gradient bar slides L→R, 1.5s loop, `cubic-bezier(0.65, 0, 0.35, 1)`.

### 26.3 Circular progress (spinner)

- 16px / 20px / 24px diameter.
- Stroke: 2px (small) or 2.5px (large), `--text-secondary` track at 20% opacity, `--text-secondary` arc at full opacity (or `--accent-primary` for emphasis).
- Indeterminate: 270° arc rotates 0.8s linear, with the arc itself growing/shrinking 0.6 → 0.85 of circumference (1.4s loop).
- Determinate: arc grows from 0 to value, 300ms `ease-out`.

### 26.4 Step progress

- See multi-step form stepper in [§9.9](#99-multi-step-forms).

---

## 27. Toasts, Banners & Alerts

### 27.1 Toast (transient, bottom-right)

- Bg: `--bg-surface`. Border: 1px `--border-default`. Shadow: `--shadow-lg`. Radius: `--radius-lg`.
- Padding: `12px 16px`. Min-width 280px, max-width 420px.
- Layout: leading 16px icon (semantic color) + body + optional trailing close X.
- Title: 14px sans 500. Description: 13px `--text-secondary`, 4px below title.
- Optional action button: link-style, right-aligned, `--accent-primary`.
- Animation: slide-in from bottom-right (translate-y 16px + fade), 200ms `cubic-bezier(0.2, 0, 0, 1)`.
- Auto-dismiss: 5s default, 8s for actionable, persistent for errors.

### 27.2 Banner (persistent, top of section)

- Full-width, radius `--radius-md`.
- Padding `12px 16px`. Bg: semantic `--*-bg`. Border: 1px semantic fg at 30% alpha.
- Icon (semantic fg) + text + trailing close (optional).
- Lives inline within content, not floating.

### 27.3 Inline alert (within forms/cards)

- Same as banner but smaller padding `10px 12px`, smaller text 13px.

### 27.4 Snackbar (action confirmation)

- Variant of toast: bg `--text-primary` (inverted, like tooltip), text `--bg-surface`.
- Use for "Item moved to trash. Undo" type messages.

---

## 28. Empty, Loading & Error States

### 28.1 Empty state

- Centered vertically and horizontally in the container.
- Top: 48px icon, `--text-tertiary` (or muted illustration).
- Title: 16px sans 500 `--text-primary`, 12px below icon.
- Description: 13px `--text-secondary`, max-width 320px, 6px below title.
- Action button (optional): primary or secondary, 16px below description.

### 28.2 Skeleton loader

- Bg: `--bg-hover`.
- Radius: matches the element being loaded (text → `--radius-xs`, avatar → `--radius-full`, card → `--radius-lg`).
- Animation: shimmer gradient — 200% width linear gradient `--bg-hover → --bg-base → --bg-hover` (light) or `--bg-hover → --border-default → --bg-hover` (dark), translate-x `-100% → 100%`, 1.6s linear infinite.
- Text skeletons: 14px tall, varied widths (60%, 90%, 70%) for natural feel.

### 28.3 Loading overlay

- Covers parent, bg `--bg-surface` at 70% opacity.
- Centered spinner ([§26.3](#263-circular-progress-spinner)) at 24px.
- 200ms fade-in delay (don't flash for fast loads).

### 28.4 Error state

- Same structure as empty state but icon is `--error` fg (alert-circle or X-circle, 48px).
- Title: 16px sans 500 `--text-primary` (NOT error red — keep readable).
- Description: 13px `--text-secondary`.
- Primary action: "Try again" button.

### 28.5 Inline retry

- Within content where data failed: 1px border `--error` fg at 40% alpha, bg `--error` bg, padding `12px 16px`, radius `--radius-md`.
- Text: 13px `--error` fg + retry link.

---

## 29. Command Palette & Search

### 29.1 Command palette

- Triggered by ⌘K / Ctrl+K.
- Centered, top 20% of viewport, max-width 640px.
- Bg `--bg-surface`, radius `--radius-xl`, shadow `--shadow-xl`.
- Scrim: `--bg-overlay` but lighter (50% of normal modal scrim).
- **Search input at top:** 56px tall, no border, just bottom 1px `--border-divider`. Leading 18px search icon, 12px gap. Sans 16px, no placeholder italics.
- **Results list:** max-height 400px, scrollable. Items 40px tall, padding `8px 16px`, hover `--bg-hover`, keyboard-focused `--bg-selected`.
- **Result item:** leading 16px icon `--text-secondary` + title (14px) + trailing kbd chip or category label (11px `--text-tertiary`).
- **Section headers:** 11px uppercase tracked, `--text-tertiary`, padding `12px 16px 6px`.
- **Footer (optional):** 1px top `--border-divider`, padding `8px 16px`. Shows kbd hints (↑↓ navigate, ↵ select, Esc close).

### 29.2 Search results / autocomplete

- Same item style as command palette but inline (no scrim).
- Anchored to input, `--shadow-md`, `--radius-lg`, 4px below input.
- Highlighted query match: bg `--accent-soft`, text `--accent-soft-text`, no underline.

---

## 30. Charts & Data Viz

Charts inherit the warm aesthetic — no neon, no pure black gridlines.

### 30.1 General chart styling

- **Background:** transparent (chart sits on its container's bg, usually `--bg-surface`).
- **Gridlines:** 1px `--chart-grid`, dashed `4 4` for non-baseline gridlines, solid for axis baselines.
- **Axes labels:** sans 11px, `--chart-axis`. Tick padding 8px from axis.
- **Axis title:** sans 12px 500, `--text-secondary`, 12px gap from labels.
- **Legend:** sans 12px, `--text-secondary`. Color swatches: 10px squares, `--radius-xs`, 6px gap from text. Items 16px gap apart.
- **Chart title (above):** sans 14px 500, `--text-primary`, 12px below.
- **Chart subtitle:** sans 12px, `--text-tertiary`, 4px below title.

### 30.2 Color application

- Single series → `--chart-1` (orange, brand).
- 2+ series → `--chart-1`, `--chart-2`, `--chart-3`, … in declared order.
- Comparison (good/bad): `--success` fg / `--error` fg.
- Categorical (no inherent order): use the 8-color palette. Avoid using `--chart-1` for "neutral" categories — it's the brand color and pulls focus.

### 30.3 Bar chart

- Bar radius: `--radius-xs` on the top edge only (or `--radius-sm` for thicker bars > 24px wide).
- Gap between bars in a group: 8px. Gap between groups: 24px.
- Hover: bar gets a `--shadow-sm` and brightens 4% (CSS `filter: brightness(1.04)`).
- Negative bars: same color, just inverted direction. Optional subtle pattern fill at 20% opacity.

### 30.4 Line chart

- Stroke: 2px, no dasharray for primary series. Use `2 4` dasharray for forecast/projection portions.
- Smoothing: monotone curve (NOT bezier — it overshoots and looks dishonest).
- Data points: hidden by default, 4px filled circles on hover (matching line color), with 8px outer ring (line color at 20% alpha).
- Area under line (if filled): same color as line at 12% alpha (light) or 18% (dark). Linear gradient fading to 0% at the bottom looks even better.

### 30.5 Area / stacked area

- Top stroke: 1.5px, full opacity.
- Fill: 16% alpha of stroke color.
- Stack order: largest series on bottom.

### 30.6 Pie / donut

- No outer stroke between segments; instead, 2px gap between segments (achieved with 2px `--bg-surface` stroke).
- Donut hole: 60% of outer radius.
- Center label (donut): sans 24px 500 `--text-primary` (the value) + 12px `--text-secondary` (the label) below.
- Don't use more than 6 segments. Bucket the rest into "Other."

### 30.7 Sparkline

- 1.5px stroke, `--chart-1` or contextual semantic color.
- No axis, no labels.
- Inline with text or in table cells. Height: 24–32px. Width: 80–120px.
- Optional: 3px dot at the latest point.

### 30.8 Scatter

- 6px circles, fill at 70% alpha (so overlaps are visible).
- Hover: 10px scale + full opacity + 1.5px ring of color.

### 30.9 Heatmap

- Cell radius: `--radius-xs`.
- Cell gap: 2px.
- Color scale: linear interpolation from `--bg-base` (zero) → `--accent-primary` (max). For diverging data, use `--info` fg (negative) → `--bg-base` (zero) → `--accent-primary` (positive).
- Hover: 1.5px outline `--text-primary` on the cell.

### 30.10 Tooltips on data

- Style: see [§17](#17-tooltips) tooltips, but slightly larger.
- Padding: `8px 12px`.
- Layout: bold title (the X-axis value, 12px 500) + each series on its own line: 8px color square + label + value (right-aligned, mono for numbers).
- Position: anchored to cursor, offset 12px right + 0px vertical. Flips to left when near right edge.
- Animation: fade 80ms — fastest in the system, since data tooltips track the cursor.

### 30.11 Axes & ranges

- Y-axis: starts at 0 unless data range demands otherwise (then call it out with a ⚠ break-mark).
- X-axis ticks: at most ~7 visible labels for readability.
- Format numbers: thousands separator, abbreviate over 1k (1.2k, 3.4M). Use mono font for axis numerals.
- Currency: locale-aware symbol prefix, no decimals if value > 100.

### 30.12 Loading & empty for charts

- Loading: skeleton with the chart's bounding box at `--bg-hover` and animated shimmer ([§28.2](#282-skeleton-loader)).
- Empty: centered 32px chart-bar icon `--text-tertiary` + "No data to display" 13px `--text-secondary`.

---

## 31. Code Blocks & Syntax

### 31.1 Block

- Bg: `--bg-code`.
- Radius: `--radius-lg`.
- Border: none in light mode, 1px `--border-default` in dark mode (helps separate from canvas).
- Padding: 16px (top toolbar) + 16px around code.
- Font: mono 13px, line-height 20px.
- Top toolbar: language label (left, 11px sans 500 uppercase tracked, `--text-tertiary`) + copy button (right, ghost 24×24px). 1px bottom `--border-divider`.
- Horizontal scroll: yes. Custom scrollbar ([§35](#35-scrollbars--selection)).
- Line numbers (optional): `--text-tertiary`, mono 12px, 16px right padding, 1px right `--border-divider`. Selectable: false.

### 31.2 Inline code

- Bg: `--bg-code`.
- Radius: `--radius-xs`.
- Padding: `1px 5px`.
- Font: mono 0.92em (relative to surrounding text).
- Color: `--text-primary`. No syntax highlighting in inline code.

### 31.3 Syntax highlighting tokens

Warm, low-saturation palette — never neon.

| Token | Light | Dark |
|---|---|---|
| Comment | `#8C8A82` (italic) | `#86847C` (italic) |
| Keyword | `#9E4A30` | `#E8A887` |
| String | `#3F8F5C` | `#6FB587` |
| Number | `#7B5BA6` | `#A88BC9` |
| Function | `#3D6AA0` | `#7AA0CC` |
| Variable | `--text-primary` | `--text-primary` |
| Operator | `--text-secondary` | `--text-secondary` |
| Type | `#C77A2C` | `#E0A24E` |
| Property | `#A66B3A` | `#C9905E` |
| Punctuation | `--text-tertiary` | `--text-tertiary` |

### 31.4 Diff view

- Add line: bg `--success` bg at 50% opacity, leading `+` in `--success` fg.
- Remove line: bg `--error` bg at 50% opacity, leading `-` in `--error` fg.
- Word-level diff: bg full-opacity semantic color, padding `0 2px`, radius `--radius-xs`.

---

## 32. Markdown Rendering

How assistant-rendered markdown should look.

### 32.1 Headings (within prose)

- H1: serif 28px / 36px, weight 500, margin `32px 0 12px`.
- H2: serif 22px / 30px, weight 500, margin `28px 0 12px`.
- H3: sans 17px / 26px, weight 600, margin `24px 0 8px`.
- H4: sans 15px / 24px, weight 600, margin `20px 0 8px`.
- H5/H6: sans 14px, weight 600, `--text-secondary`.

### 32.2 Paragraphs

- Default body, 1em margin-bottom.

### 32.3 Lists

- Bullet: `disc` first level, `circle` nested, `square` 3rd level.
- Numbered: `decimal` first, `lower-alpha` nested, `lower-roman` 3rd.
- Padding-left: 24px. Item gap: 4px.

### 32.4 Blockquote

- Left border: 3px `--border-strong`.
- Padding: `0 0 0 16px`.
- Color: `--text-secondary`.
- Italic optional (in serif contexts, italic looks good).

### 32.5 Horizontal rule

- 1px `--border-divider`, 24px vertical margin.
- Never use `<hr>` styling like a beveled divider — keep it a clean hairline.

### 32.6 Links

- Color: `--text-link`.
- Underline: 1px, offset 2px from baseline (`text-underline-offset: 2px`), thickness `1px`.
- Hover: underline thickens to 2px, no color change.

### 32.7 Tables

- Use the table styles from [§19](#19-tables), but headers can be sentence-case (not uppercase tracked) within markdown for readability.

---

## 33. Iconography

### 33.1 Style

- **Library:** Lucide-style line icons.
- **Stroke:** 1.75px (default). Use 2px on icons rendered below 14px to maintain visibility.
- **Caps:** square (`stroke-linecap: square`).
- **Joins:** round (`stroke-linejoin: round`).
- **Fill:** none (line-only). Filled variants reserved for status (filled star = starred, filled bookmark = saved).

### 33.2 Sizes

| Size | Use |
|---|---|
| 12px | Tight inline (badges, kbd chips) |
| 14px | Inline with body text |
| 16px | Default UI (buttons, menu items) |
| 18px | Button rows, comfortable density |
| 20px | Sidebar nav, toolbar |
| 24px | Modal headers, large actions |
| 32px | Empty states, prominent affordances |
| 48px | Hero empty states, error illustrations |

### 33.3 Color

- Default: matches surrounding text.
- Resting in chrome: `--text-secondary`.
- Hover/active: `--text-primary`.
- Semantic: use semantic fg colors for status icons.
- Never use the brand orange for purely decorative icons — reserve it for primary actions.

### 33.4 Custom illustrations

- Line-art style matching icon stroke logic (1.75px, square cap, round join).
- Color palette: 2–3 muted warm tones from the chart palette, no gradients.
- Background shapes filled at 8–16% alpha for depth.

### 33.5 Canonical icon map

The standard Lucide icon for each common UI affordance. Pick from this list before reaching for a custom glyph — recognisability beats novelty.

| Affordance | Lucide name | Use cases |
|---|---|---|
| Search | `search` | Search inputs, find bar |
| Mail / inbox | `mail`, `inbox` | Email list, attention queue |
| Bell | `bell` | Notification panel trigger |
| Settings | `settings` | Settings menu, config |
| User | `user`, `users` | Account, members |
| Plus | `plus` | New / add / create |
| Filter | `filter` | Filter trigger |
| Sort | `arrow-up-down` | Sortable columns |
| Sun / Moon | `sun`, `moon` | Theme toggle |
| Trending up | `trending-up` | Positive metric, growth |
| Trending down | `trending-down` | Negative metric, drop |
| Alert triangle | `alert-triangle` | Warning |
| Alert circle | `alert-circle` | Error / attention |
| Check circle | `check-circle` | Success state |
| Info | `info` | Info banner, help icon |
| Clock | `clock` | Time, latency, schedule |
| Calendar | `calendar` | Date range, schedule |
| Chevron right | `chevron-right` | Drill-in, expand |
| Chevron down | `chevron-down` | Dropdown, accordion closed |
| Arrow up | `arrow-up` | Send, increment |
| X | `x` | Close, dismiss, clear |
| More horizontal | `more-horizontal` | Overflow menu |
| Copy | `copy` | Copy to clipboard |
| Edit | `edit-3` | Inline edit |
| Trash | `trash-2` | Delete |
| Archive | `archive` | Archive |
| Download | `download` | Export, save |
| Upload | `upload` | Attach, import |
| Refresh | `refresh-cw` | Refresh, retry |
| External link | `external-link` | Outbound link |

If you can't pick between two icons, pick the one with fewer strokes — Claude's chrome leans towards calm. Filled variants only for status (filled star = saved, filled bookmark = bookmarked) per [§33.1](#331-style).

---

## 34. Keyboard Chips & Shortcuts

### 34.1 Kbd chip

- Min-width 20px, height 20px, padding `0 6px`.
- Bg: `--bg-base` (light) or `--bg-hover` (dark).
- Border: 1px `--border-default`. Bottom border: 1.5px (subtle 3D effect).
- Radius: `--radius-xs`.
- Font: mono 11px, `--text-secondary`.
- Letter-spacing: 0.

### 34.2 Combo

- Multiple chips with 4px gap, no `+` between them.
- E.g., `⌘` `K` instead of `⌘+K`.

### 34.3 Inline in menus

- Right-aligned to menu item, vertically centered.
- 12px gap from menu text.

---

## 35. Scrollbars & Selection

### 35.1 Custom scrollbar

```css
::-webkit-scrollbar { width: 10px; height: 10px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb {
  background: var(--border-default);
  border-radius: var(--radius-full);
  border: 2px solid var(--bg-base); /* creates inset effect */
}
::-webkit-scrollbar-thumb:hover {
  background: var(--border-strong);
}
```

- Show only on hover of scrollable container (use `:hover` to switch from `transparent` to color).
- Firefox: `scrollbar-width: thin; scrollbar-color: var(--border-default) transparent;`

### 35.2 Text selection

```css
::selection {
  background: var(--text-selection);
  color: var(--text-primary);
}
```

- Selection bg is the orange at low alpha — ties to brand without overpowering.
- Don't override text color (keep `--text-primary`).

### 35.3 Drag handles

- Vertical handle (for resizable panels): 4px wide hot zone, but only shows 1px line `--border-default` at rest. Hover: 2px `--accent-primary`.
- Grip (for sortable list items): 6-dot pattern (`⋮⋮`), 16px tall, `--text-tertiary`, on left of row, only visible on row hover.

### 35.4 Drop zones

- During drag: dashed 1.5px `--accent-primary` border on valid targets, bg `--accent-soft` at 50% alpha.
- Insertion line (for sortable lists): 2px solid `--accent-primary`, full width, animated in over 100ms.

---

## 36. Motion System

Quiet, fast, purposeful. Almost everything is under 200ms.

### 36.1 Duration tokens

| Token | Duration | Use |
|---|---|---|
| `--motion-instant` | 80ms | Hover bg, color shifts, tooltip fade |
| `--motion-fast` | 120ms | Dropdowns, popovers, tooltip slide |
| `--motion-base` | 180ms | Modals, toggles, accordions |
| `--motion-medium` | 240ms | Drawers, tab indicator slide |
| `--motion-slow` | 320ms | Page/route transitions |
| `--motion-spring` | 400ms | Reserved — copy-confirm, success bounce |

### 36.2 Easing tokens

| Token | Curve | Use |
|---|---|---|
| `--ease-linear` | `linear` | Spinners, indeterminate progress |
| `--ease-out` | `cubic-bezier(0, 0, 0.2, 1)` | Most enter animations |
| `--ease-in` | `cubic-bezier(0.4, 0, 1, 1)` | Most exit animations |
| `--ease-in-out` | `cubic-bezier(0.4, 0, 0.2, 1)` | Position changes (tab indicator) |
| `--ease-emphasized` | `cubic-bezier(0.2, 0, 0, 1)` | Modals, drawers, important UI |
| `--ease-bounce` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Reserved spring use |

### 36.3 Standard animation patterns

**Fade-in (tooltips, popovers):**
```css
opacity: 0 → 1; transform: translateY(4px) → translateY(0);
duration: 120ms; easing: --ease-out;
```

**Modal enter:**
```css
opacity: 0 → 1; transform: scale(0.96) → scale(1);
duration: 180ms; easing: --ease-emphasized;
```

**Drawer enter:**
```css
transform: translateX(100%) → translateX(0);
duration: 240ms; easing: --ease-emphasized;
```

**Hover lift (card):**
```css
transform: translateY(0) → translateY(-1px);
box-shadow: --shadow-xs → --shadow-sm;
duration: 120ms; easing: --ease-out;
```

**Press (button):**
```css
transform: scale(1) → scale(0.98);
duration: 80ms; easing: --ease-out;
```

**Page transition:**
```css
opacity: 0 → 1; transform: translateY(8px) → translateY(0);
duration: 320ms; easing: --ease-emphasized;
```

### 36.4 Stagger

When animating multiple items in (list rows, grid cards):
- Delay between items: 30ms.
- Cap total stagger at 8 items max (~240ms total) — beyond that, animate as a group.

### 36.5 Micro-interactions

- **Copy confirm:** clipboard icon morphs to checkmark, scales 1 → 1.15 → 1, color flash to `--success` fg, returns after 1.4s.
- **Like/star toggle:** scale `1 → 0.8 → 1.15 → 1` (250ms), color shift to brand on activate.
- **Send button:** when message sends, brief scale `1 → 0.92 → 1` and arrow icon shifts up-right 2px before resetting.
- **Theme toggle:** the icon rotates 180° (sun↔moon) over 240ms; the page itself flash-cuts (no color animation).
- **Notification arrival:** toast slides in (translate-x 16px → 0) + fade, with a tiny `+1px` overshoot at the end (subtle bounce, not springy).
- **Streaming text caret:** 8px filled circle at end of stream, opacity sine-eases 0.3 → 1 → 0.3 over 1.2s.
- **Sidebar item active:** the `--bg-selected` doesn't animate in — it cuts immediately on click for responsiveness. Only secondary indicators (left border) animate.

### 36.6 What NOT to animate

- Background color of the entire page (theme switch).
- Text color changes within static text.
- Border colors on hover (just swap — animating them looks slow).
- Anything that fights with user typing or scrolling.

### 36.7 Reduced motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```
Keep opacity fades (under 80ms) — they help orient without inducing motion sickness.

---

## 37. Focus & Accessibility

- **Focus ring:** 3px `--border-focus-ring`, offset 1px from element. Only on `:focus-visible`.
- **Min tap target:** 32×32px for icon buttons, 36px row height for menu items.
- **Contrast:** body text ≥7:1 light, ≥8:1 dark. Secondary text ≥4.5:1.
- **Skip-to-content link:** visually hidden until focused, then top-left, primary button styled.
- **Live regions:** toasts use `role="status"` (polite) or `role="alert"` (assertive for errors).
- **Form labels:** always visible labels (not just placeholders). Placeholders are hints, not labels.
- **Keyboard nav:** Tab order matches visual order. Esc closes the topmost overlay. ⌘/ opens shortcuts cheatsheet.

---

## 38. Responsive & Density

### 38.1 Breakpoints

```
--bp-sm: 640px   /* phone landscape */
--bp-md: 768px   /* tablet portrait */
--bp-lg: 1024px  /* tablet landscape, small laptop */
--bp-xl: 1280px  /* desktop */
--bp-2xl: 1536px /* large desktop */
```

### 38.2 Mobile adaptations

- Sidebar collapses to drawer (slide from left, full scrim).
- Composer takes full width, padded `--space-4`.
- Conversation column: 100% - `--space-8`, no max-width.
- Modals: full-screen below `--bp-md` (no scrim, no radius on viewport edges).
- Tap targets: bump min size to 44×44px.
- Hover states: still defined but supplemented with `:active` since hover doesn't fire on touch.

### 38.3 Density modes

User-toggleable in settings:

| | Comfortable (default) | Compact |
|---|---|---|
| Row height (lists) | 40px | 32px |
| Button md height | 36px | 32px |
| Card padding | 20px | 16px |
| Sidebar item | 36px | 30px |
| Body text | 15px | 14px |

Apply via `data-density="compact"` on the root.

---

## 39. Theme Implementation

### 39.1 Token declarations

```css
:root {
  --bg-base: #FAF9F5;
  --bg-surface: #FFFFFF;
  --text-primary: #1F1E1D;
  --accent-primary: #C96442;
  /* …all light tokens… */
}

[data-theme="dark"] {
  --bg-base: #262624;
  --bg-surface: #30302E;
  --text-primary: #F5F4EE;
  --accent-primary: #D97757;
  /* …all dark overrides… */
}

@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* mirror dark overrides for system preference */
  }
}
```

### 39.2 First-paint script (FOUC prevention)

Inline in the document `<head>` **before any stylesheet link**, so the `data-theme` attribute is set before the first paint and CSS variables resolve to the correct values immediately. No `async`, no `defer` — this script must run synchronously.

If the user has a saved preference, honour it. Otherwise fall back to the OS preference. If both fail (private browsing, exceptions), default to light.

```html
<script>
// Set theme before first paint — must be inline, before any stylesheet link.
(function () {
  try {
    var k = 'app-theme';
    var saved = localStorage.getItem(k);
    if (saved === 'dark' || saved === 'light') {
      document.documentElement.setAttribute('data-theme', saved);
    } else if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
      document.documentElement.setAttribute('data-theme', 'dark');
    } else {
      document.documentElement.setAttribute('data-theme', 'light');
    }
  } catch (e) {
    document.documentElement.setAttribute('data-theme', 'light');
  }
})();
</script>
```

Skipping this script causes a flash of the wrong theme (FOUC) on every page load — light mode briefly visible before dark mode applies, or vice versa. Visible for 80–200ms depending on stylesheet weight; long enough to be noticeable, short enough to be deniable. Don't ship without it.

### 39.3 Runtime toggle

```js
function setTheme(theme) {
  document.documentElement.setAttribute('data-theme', theme);
  localStorage.setItem('app-theme', theme);
}
```

**Don't animate the page background during theme switch.** Flash-cut. Use a 150ms scrim fade if you need a transition feel.

---

## 40. Recognition Checklist

If 12+ of these are true, you'll get the Claude look.

**Light mode:**
1. ☐ Background is warm off-white (`#FAF9F5`), not pure white.
2. ☐ Borders are warm (`#E8E6DC`), not cool gray.
3. ☐ Sidebar is one shade *darker* than the canvas, not lighter.
4. ☐ Shadows are warm-tinted and very soft.

**Dark mode:**
5. ☐ Background is warm dark brown (`#262624`), not slate/charcoal/black.
6. ☐ Sidebar is darker than canvas (`#1F1E1D`).
7. ☐ Surfaces are *lighter* browns (`#30302E`), creating layered depth.
8. ☐ Borders do more work than shadows for resting elevation.

**Both modes:**
9. ☐ Primary action is terracotta orange, not blue.
10. ☐ Long-form content is in a **serif**, UI is sans.
11. ☐ Composer has 16px radius, soft elevation, sits at bottom centered.
12. ☐ Send button is circular and orange.
13. ☐ User messages have a soft bubble; assistant messages have **no bubble**.
14. ☐ Conversation column capped at ~768px on any screen.
15. ☐ Almost no fully square corners; almost no fully circular elements except avatars.
16. ☐ Hover/transition durations are ≤180ms; no bouncy springs in chrome.
17. ☐ Charts use the warm 8-color palette, not Material/Tailwind defaults.
18. ☐ Scrollbars are slim, warm-tinted, only visible on hover.
19. ☐ Code blocks use warm syntax colors (no neon greens or hot pinks).
20. ☐ Tooltips invert from the current theme.

**Dashboard / dense data:**
21. ☐ KPI tile values are in **JetBrains Mono**, not sans, with 11px uppercase tracked labels above.
22. ☐ Multi-panel layouts use distinct surface tones per panel (sidebar/list/content/assistant), borders carry separation, no resting shadows.
23. ☐ Filter pills use `--accent-soft` bg with the leading icon + trailing X pattern, not generic chip styling.
24. ☐ Inline first-paint theme script runs before any stylesheet link — no FOUC on reload.

---

## 41. What NOT to Do

- ❌ Pure white (`#FFF`) or pure black (`#000`) as base colors.
- ❌ Slate-gray or blue-gray dark mode — Claude's dark mode is brown.
- ❌ Blue primary action. Even a "warm blue."
- ❌ Assistant text inside a bubble or card — must be flush on canvas.
- ❌ Bold serif (`font-weight: 700`). Stick to 400/500.
- ❌ Standard Material drop shadow — too cool, too sharp.
- ❌ Sidebar lighter than canvas. It's darker in both modes.
- ❌ Animating the page background during theme switching.
- ❌ System gray (`#666`, `#999`) for secondary text — too cool.
- ❌ Borders on icon buttons — they're ghost-style only.
- ❌ Dividers between every list item; use spacing instead.
- ❌ Inverting the accent orange in dark mode.
- ❌ Neon syntax highlighting (bright green strings, hot pink keywords).
- ❌ Tailwind/Material default chart colors. Build the chart palette deliberately.
- ❌ Spinner for everything — prefer skeleton screens for known shapes.
- ❌ Modal scrims that animate from 0 to 100% slowly. Make them snappy.
- ❌ Bouncy springs in chrome (sidebar, buttons, menus). Reserve for delight moments only.
- ❌ Hover effects with transitions over 200ms — feels laggy.
- ❌ Bezier-smoothed line charts — they overshoot data and look dishonest. Use monotone.
- ❌ Y-axes that don't start at zero (without a clear visual break-mark).
- ❌ Always-visible scrollbars on inner panels — show on hover only.
- ❌ Placeholder text doing the job of a label.

---

*This document is a learning reference derived from observation of the Claude desktop app. It is not an official Anthropic design specification — exact production values may differ.*
