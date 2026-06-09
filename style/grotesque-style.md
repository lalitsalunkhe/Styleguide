# Coupon Engine — Portable Style Guide

A copy-paste-ready style guide for replicating this app's visual language on another React + Tailwind codebase. Everything in this document is self-contained: paste the snippets, run `yarn add` on the listed deps, and you're done.

> Stack assumed: **React + Tailwind CSS + Shadcn UI + Lucide icons + Sonner toasts**.
> Aesthetic: **Swiss minimalism** — mostly white, sharp corners, restrained colour, dense data, restrained motion.

---

## Table of Contents

1. [Setup](#1-setup)
2. [Design tokens (CSS variables)](#2-design-tokens-css-variables)
3. [Tailwind config additions](#3-tailwind-config-additions)
4. [Fonts](#4-fonts)
5. [Global CSS (utilities, animations, scrollbars)](#5-global-css-utilities-animations-scrollbars)
6. [Color usage matrix](#6-color-usage-matrix)
7. [Typography scale](#7-typography-scale)
8. [Iconography rules](#8-iconography-rules)
9. [Spacing & layout](#9-spacing--layout)
10. [Component patterns (copy-paste)](#10-component-patterns-copy-paste)
11. [Page header pattern](#11-page-header-pattern)
12. [Two-step OTP flow](#12-two-step-otp-flow)
13. [Type-ahead with "+ Add new"](#13-type-ahead-with--add-new)
14. [DataGrid conventions](#14-datagrid-conventions)
15. [Status pills](#15-status-pills)
16. [Empty states](#16-empty-states)
17. [Toasts](#17-toasts)
18. [Interaction & motion rules](#18-interaction--motion-rules)
19. [Naming conventions](#19-naming-conventions)
20. [Test IDs](#20-test-ids)
21. [Accessibility checklist](#21-accessibility-checklist)
22. [PR checklist](#22-pr-checklist)

---

## 1. Setup

### Dependencies

```bash
# Core UI primitives
yarn add lucide-react sonner

# Shadcn UI primitives — install only the ones you use:
# https://ui.shadcn.com/docs/installation/manual
# Components used by this style guide:
#   button, input, label, dialog, popover, dropdown-menu,
#   switch, checkbox, select, textarea, sonner

# Or via the shadcn CLI:
npx shadcn-ui@latest add button input label dialog popover dropdown-menu \
                          switch checkbox select textarea sonner
```

### Folder layout

```
src/
├── components/
│   ├── ui/                  ← Shadcn primitives (don't edit unless extending tokens)
│   ├── DataGrid.jsx         ← sort/page/select/filter primitives (one per project)
│   └── PhoneInput.jsx       ← country-code + national digits combo (single source of truth)
├── lib/
│   ├── api.js
│   ├── toast.js             ← `export { toast } from "sonner"`
│   └── utils.js             ← `cn()` helper from shadcn
├── pages/
└── index.css                ← tokens, fonts, .overline, .tabular, animations
```

---

## 2. Design tokens (CSS variables)

Paste at the top of `src/index.css` (after `@tailwind` directives):

```css
:root {
    /* Surfaces */
    --bg: #ffffff;
    --surface: #f8f9fa;
    --surface-2: #f1f3f5;

    /* Text */
    --text: #111827;
    --text-muted: #6b7280;

    /* Borders */
    --border: #e5e7eb;
    --border-focus: #9ca3af;

    /* Brand */
    --primary: #111827;
    --primary-fg: #ffffff;
    --accent: #2b5a84;
    --accent-hover: #20466a;

    /* Status */
    --success: #2e6d4e;
    --success-bg: #edf5f0;
    --warning: #b45309;
    --warning-bg: #fffbeb;
    --danger: #991b1b;
    --danger-bg: #fef2f2;
}

/* Shadcn requires HSL-formatted vars too */
@layer base {
    :root {
        --background: 0 0% 100%;
        --foreground: 221 39% 11%;
        --card: 0 0% 100%;
        --card-foreground: 221 39% 11%;
        --popover: 0 0% 100%;
        --popover-foreground: 221 39% 11%;
        --primary: 221 39% 11%;
        --primary-foreground: 0 0% 100%;
        --secondary: 210 17% 98%;
        --secondary-foreground: 221 39% 11%;
        --muted: 210 17% 95%;
        --muted-foreground: 220 9% 46%;
        --accent: 210 50% 34%;
        --accent-foreground: 0 0% 100%;
        --destructive: 0 70% 35%;
        --destructive-foreground: 0 0% 100%;
        --border: 220 13% 91%;
        --input: 220 13% 91%;
        --ring: 220 9% 46%;
        --radius: 0.125rem; /* sharp corners — 2px */
    }
}
```

> The radius is **`0.125rem` (2px)**. This is the single most important value: sharp corners are the visual signature of this style.

---

## 3. Tailwind config additions

`tailwind.config.js` — keep the Shadcn defaults and add no custom colour palette. The point is to live within `gray-*`, `green-*`, `red-*`, `amber-*` and use them deliberately. Only register the content paths and the Shadcn plugin.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ["class"],
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
};
```

---

## 4. Fonts

Three families, each with one job:

```css
/* In index.css */
@import url("https://api.fontshare.com/v2/css?f[]=cabinet-grotesk@400,500,700,800&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap");

html, body {
    font-family: "Manrope", ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-feature-settings: "cv11", "ss01";
    background: var(--bg);
    color: var(--text);
    margin: 0;
    padding: 0;
}

.font-display {
    font-family: "Cabinet Grotesk", "Manrope", ui-sans-serif, system-ui, sans-serif;
    letter-spacing: -0.02em;
}

.font-mono {
    font-family: "JetBrains Mono", ui-monospace, monospace;
}
```

| Family | Class | Use for |
|--------|-------|---------|
| **Cabinet Grotesk** | `font-display` | Page titles, dialog titles, big numbers |
| **Manrope** | (default body) | Everything else |
| **JetBrains Mono** | `font-mono` | Coupon codes, IDs, timestamps, dial codes |

---

## 5. Global CSS (utilities, animations, scrollbars)

Append to `index.css`:

```css
/* Tabular numbers — for any number/date/code that lives in a column */
.tabular {
    font-variant-numeric: tabular-nums;
}

/* Overline — every form label, section header, column header */
.overline {
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-size: 11px;
    font-weight: 600;
    color: var(--text-muted);
    text-decoration: none !important;
}
.overline button,
th.overline button,
th.overline span {
    text-transform: uppercase;
    letter-spacing: 0.12em;
}

/* Subtle card */
.card-flat {
    background: #fff;
    border: 1px solid var(--border);
    border-radius: 4px;
}

/* Sticky summary side panel */
.sticky-summary { position: sticky; top: 88px; }

/* Utilisation bar (success / amber / red variants) */
.util-bar {
    height: 6px;
    background: #f1f3f5;
    border-radius: 2px;
    overflow: hidden;
}
.util-bar > span        { display: block; height: 100%; background: var(--success); }
.util-bar.amber > span  { background: var(--warning); }
.util-bar.red > span    { background: var(--danger); }

/* Custom scrollbar */
::-webkit-scrollbar         { width: 10px; height: 10px; }
::-webkit-scrollbar-track   { background: transparent; }
::-webkit-scrollbar-thumb   { background: #e5e7eb; border-radius: 2px; }
::-webkit-scrollbar-thumb:hover { background: #cbd5e1; }

/* Sharp focus ring — keyboard only */
*:focus-visible {
    outline: 2px solid var(--text);
    outline-offset: 2px;
    border-radius: 2px;
}

input, textarea, select { font-family: inherit; }

/* Swiss grid background pattern (used on login hero) */
.grid-bg {
    background-image:
        linear-gradient(to right, rgba(17, 24, 39, 0.04) 1px, transparent 1px),
        linear-gradient(to bottom, rgba(17, 24, 39, 0.04) 1px, transparent 1px);
    background-size: 48px 48px;
}

/* Grain / noise texture overlay (give plain whites depth) */
.grain-overlay::before {
    content: "";
    position: absolute;
    inset: 0;
    pointer-events: none;
    opacity: 0.35;
    mix-blend-mode: multiply;
    background-image: url("data:image/svg+xml;utf8,<svg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/><feColorMatrix values='0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.05 0'/></filter><rect width='100%25' height='100%25' filter='url(%23n)'/></svg>");
}

/* Animations -------------------------------------------------------------- */
@keyframes float-soft {
    0%, 100% { transform: translate3d(0, 0, 0) rotate(-2deg); }
    50%      { transform: translate3d(0, -10px, 0) rotate(-1deg); }
}
@keyframes float-soft-rev {
    0%, 100% { transform: translate3d(0, 0, 0) rotate(3deg); }
    50%      { transform: translate3d(0, 12px, 0) rotate(2deg); }
}
@keyframes drift {
    0%, 100% { transform: translate3d(0, 0, 0); }
    50%      { transform: translate3d(6px, -6px, 0); }
}
@keyframes blink-caret {
    0%, 50%       { opacity: 1; }
    50.01%, 100%  { opacity: 0; }
}
@keyframes slide-up-fade {
    0%   { opacity: 0; transform: translateY(8px); }
    100% { opacity: 1; transform: translateY(0); }
}

.animate-float-soft     { animation: float-soft 6.5s ease-in-out infinite; }
.animate-float-soft-rev { animation: float-soft-rev 7.5s ease-in-out infinite; }
.animate-drift          { animation: drift 9s ease-in-out infinite; }
.animate-caret          { animation: blink-caret 1.1s step-end infinite; }
.animate-rise           { animation: slide-up-fade 520ms cubic-bezier(0.22, 1, 0.36, 1) both; }

/* Liquid-glass card (used for the hero coupon ticket) */
.coupon-ticket {
    position: relative;
    background: linear-gradient(135deg, rgba(255,255,255,0.7) 0%, rgba(255,255,255,0.35) 100%);
    border-radius: 18px;
    backdrop-filter: blur(22px) saturate(1.3);
    -webkit-backdrop-filter: blur(22px) saturate(1.3);
    box-shadow:
        0 1px 0 0 rgba(255,255,255,0.9) inset,
        0 -1px 0 0 rgba(17,24,39,0.03) inset,
        0 30px 60px -30px rgba(17,24,39,0.28),
        0 12px 24px -12px rgba(17,24,39,0.14);
    overflow: hidden;
}
.coupon-ticket::before {
    content: "";
    position: absolute; top: -50%; left: -20%;
    width: 140%; height: 70%;
    background: radial-gradient(ellipse at center, rgba(255,255,255,0.85) 0%, rgba(255,255,255,0) 70%);
    pointer-events: none; opacity: 0.5; mix-blend-mode: overlay; z-index: 1;
}
.coupon-ticket .ticket-body { position: relative; z-index: 2; }

.glass-chip {
    background: rgba(255,255,255,0.55);
    backdrop-filter: blur(14px) saturate(1.4);
    -webkit-backdrop-filter: blur(14px) saturate(1.4);
    box-shadow:
        0 1px 0 0 rgba(255,255,255,0.9) inset,
        0 10px 24px -12px rgba(17,24,39,0.2);
}
.glass-chip-dark {
    background: rgba(17,24,39,0.78);
    backdrop-filter: blur(14px) saturate(1.4);
    -webkit-backdrop-filter: blur(14px) saturate(1.4);
    color: #fff;
    box-shadow: 0 10px 24px -12px rgba(17,24,39,0.4);
}
```

---

## 6. Color usage matrix

| Role | Tailwind class | Hex | When to use |
|------|---------------|-----|-------------|
| **Primary text** | `text-gray-900` | `#111827` | Body, headlines, primary row content |
| **Muted text** | `text-gray-500` / `text-gray-600` | `#6b7280` | Helper text, timestamps, metadata |
| **Disabled / placeholder** | `text-gray-400` | `#9ca3af` | Empty cells (`—`), placeholders |
| **Background** | `bg-white` | `#fff` | Pages, cards, dialogs |
| **Surface (subtle)** | `bg-gray-50/30` | `#f8f9fa` | Page wrappers |
| **Surface (table head)** | `bg-gray-50/60` | `#f1f3f5` | Table headers, hover rows |
| **Border** | `border-gray-200` | `#e5e7eb` | Default borders everywhere |
| **Border focus / active** | `border-gray-900` | `#111827` | Selected card, active filter, focused input |
| **Primary button** | `bg-gray-900 hover:bg-black text-white` | — | The single primary action per view |
| **Success pill** | `bg-green-50 text-green-700` | — | Active, Delivered, Redeemed |
| **Warning pill** | `bg-amber-50 text-amber-700` | — | Partial / pending hints |
| **Danger pill** | `bg-red-50 text-red-700` | — | Errors, destructive items, invalid |

**Hard rules**

- Only **one** primary (filled black) button per visible scope.
- Destructive actions are red **text** on a button, not a red fill — unless the user has already typed a confirmation.
- No purple, violet, neon, or rainbow gradients. Gradients are reserved for the liquid-glass hero card.

---

## 7. Typography scale

```
H1   font-display text-3xl md:text-4xl font-bold tracking-tight   ← page titles
H2   font-display text-xl font-semibold                           ← card / dialog titles
H3   text-sm font-medium                                          ← row primary text
Body text-sm                                                      ← paragraphs, table cells
Meta text-xs text-gray-500                                        ← timestamps, hints
Tiny text-[11px] / text-[10px]                                    ← micro labels (status pills)
Overline .overline                                                ← all labels, column headers
```

Numbers must use `tabular`. Codes / IDs / phones use `font-mono tabular`.

```jsx
<span className="text-sm font-mono font-semibold tabular">AMURA20-7K9P</span>
<span className="font-display text-4xl font-bold tabular">₹4,250</span>
```

---

## 8. Iconography rules

- Library: [`lucide-react`](https://lucide.dev). **No emoji icons in product UI.** (Emojis OK as flag chips in country pickers — those are content, not icons.)
- Default sizes:
  - `size={12}` — inside chips / status pills
  - `size={14}` — inline with text, buttons
  - `size={18}` — empty-state hero icons
- Always `strokeWidth={1.5}` (gives a refined line that pairs with our small type).
- Color: inherit from parent (`text-gray-500` for secondary).

```jsx
import { Send } from "lucide-react";
<Send size={14} strokeWidth={1.5} className="text-gray-500" />
```

---

## 9. Spacing & layout

- Use Tailwind's spacing scale, prefer multiples of 4 (`gap-2`, `gap-3`, `gap-5`, `mb-8`).
- **Page shell**: `max-w-7xl mx-auto px-6 md:px-10 py-10`.
- **Cards**: `border border-gray-200 rounded-sm` or `.card-flat`.
- **Dialogs**: `rounded-sm max-w-lg border-gray-200` (small dialogs `max-w-md`).
- **Section spacing**: `mb-8` between large blocks; `space-y-4` inside dialogs; `space-y-1.5` for tight lists.

Critical: **corners are sharp**. Use `rounded-sm` (2px) on every container. The only exception is pill chips / floating glass chips (`rounded-full`).

---

## 10. Component patterns (copy-paste)

### 10.1 Buttons

```jsx
import { Button } from "@/components/ui/button";

// PRIMARY (max one per scope)
<Button className="rounded-sm bg-gray-900 hover:bg-black text-white gap-2 h-10" data-testid="save-btn">
  <Send size={14} strokeWidth={1.5} /> Distribute
</Button>

// SECONDARY
<Button variant="outline" className="rounded-sm h-10" data-testid="cancel-btn">Cancel</Button>

// ICON-ONLY (toolbar)
<button
  type="button"
  className="h-9 w-9 border border-gray-200 rounded-sm flex items-center justify-center hover:border-gray-900 transition-colors text-gray-700"
  aria-label="Back"
  data-testid="back-btn"
>
  <ArrowLeft size={14} strokeWidth={1.5} />
</button>

// IN-ROW ACTION (3-dots wrapper)
<button
  type="button"
  className="h-7 w-7 inline-flex items-center justify-center rounded-sm text-gray-500 hover:text-gray-900 hover:bg-gray-100"
  aria-label="Row actions"
>
  <MoreVertical size={14} strokeWidth={1.5} />
</button>
```

Heights cheat sheet: `h-11` (login big), `h-10` (default), `h-9` (toolbar), `h-7` (in-row).

### 10.2 Inputs / Forms

```jsx
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

<div>
  <Label className="overline" htmlFor="email">Email</Label>
  <Input
    id="email"
    type="email"
    placeholder="you@company.com"
    className="mt-1.5 h-10 rounded-sm"
    data-testid="email-input"
  />
</div>
```

Numeric / phone / date inputs add `tabular`:
```jsx
<Input type="tel" className="mt-1.5 h-10 rounded-sm tabular" />
```

### 10.3 Dialog (standard)

```jsx
<Dialog open={open} onOpenChange={(v) => !v && onClose()}>
  <DialogContent className="rounded-sm max-w-lg border-gray-200" data-testid="my-dialog">
    <DialogHeader>
      <p className="overline">Onboard partner</p>
      <DialogTitle className="font-display text-xl mt-1">Add an Account</DialogTitle>
      <DialogDescription className="text-sm text-gray-600">
        Pick the organisation and add its primary point of contact.
      </DialogDescription>
    </DialogHeader>

    <div className="space-y-4">
      {/* form body */}
    </div>

    <DialogFooter>
      <Button variant="outline" className="rounded-sm" onClick={onClose}>Cancel</Button>
      <Button className="rounded-sm bg-gray-900 hover:bg-black gap-2" onClick={submit} disabled={busy}>
        {busy && <Loader2 size={14} className="animate-spin" />}
        {busy ? "Saving…" : "Save"}
      </Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

**Mandatory dialog (cannot dismiss)** — set all three handlers to prevent default:

```jsx
<DialogContent
  className="rounded-sm max-w-md border-gray-200 [&>button]:hidden"
  onEscapeKeyDown={(e) => e.preventDefault()}
  onPointerDownOutside={(e) => e.preventDefault()}
  onInteractOutside={(e) => e.preventDefault()}
>
```

### 10.4 Switch

```jsx
import { Switch } from "@/components/ui/switch";

<div className="flex items-center justify-between">
  <Label className="overline">Active</Label>
  <Switch checked={value} onCheckedChange={setValue} />
</div>
```

> Make sure the unchecked state is visible on white backgrounds. In the Shadcn `switch.jsx`, force:
> ```
> data-[state=checked]:bg-gray-900 data-[state=unchecked]:bg-gray-300
> ```

---

## 11. Page header pattern

Canonical page header — title block on the left, actions on the right.

```jsx
<div className="flex items-end justify-between mb-8 gap-4 flex-wrap">
  <div>
    <p className="overline">{isEdit ? "Edit" : "Create"}</p>
    <h1 className="font-display text-3xl md:text-4xl font-bold mt-1">New Coupon Parcel</h1>
  </div>
  <div className="flex items-center gap-2 flex-wrap justify-end">
    <Button variant="outline" className="rounded-sm h-10" onClick={() => nav(-1)}>Cancel</Button>
    <Button variant="outline" className="rounded-sm h-10" onClick={saveDraft} disabled={busy}>Save as Draft</Button>
    <Button className="rounded-sm h-10 bg-gray-900 hover:bg-black gap-2" onClick={submit} disabled={busy}>
      <Send size={14} strokeWidth={1.5} /> Distribute coupons
    </Button>
  </div>
</div>
```

> Bottom action bars are **deprecated**. Always promote primary actions to the top header.

---

## 12. Two-step OTP flow

A reusable single-screen, two-step flow used for sign-in and identity verification.

### Step state

```jsx
const [step, setStep] = useState("phone"); // 'phone' | 'otp'
const [phone, setPhone] = useState("");
const [otp, setOtp] = useState("");
const [resendIn, setResendIn] = useState(0);

useEffect(() => {
  if (step !== "otp" || resendIn <= 0) return;
  const t = setInterval(() => setResendIn((s) => (s > 0 ? s - 1 : 0)), 1000);
  return () => clearInterval(t);
}, [step, resendIn]);
```

### OtpBoxes component

```jsx
function OtpBoxes({ value, onChange, onComplete, length = 4 }) {
  const refs = useRef([]);
  const set = (i, ch) => {
    const next = (value.split("").map((d, idx) => (idx === i ? ch : d)).join("") + " ".repeat(length))
      .slice(0, length).trim();
    onChange(next);
    if (ch && i < length - 1) refs.current[i + 1]?.focus();
    if (next.length === length && onComplete) onComplete(next);
  };
  const onKey = (i, e) => {
    if (e.key === "Backspace" && !value[i] && i > 0) refs.current[i - 1]?.focus();
    if (e.key === "ArrowLeft" && i > 0) refs.current[i - 1]?.focus();
    if (e.key === "ArrowRight" && i < length - 1) refs.current[i + 1]?.focus();
  };
  const onPaste = (e) => {
    const text = (e.clipboardData.getData("text") || "").replace(/\D/g, "").slice(0, length);
    if (!text) return;
    e.preventDefault();
    onChange(text);
    refs.current[Math.min(text.length, length - 1)]?.focus();
    if (text.length === length && onComplete) onComplete(text);
  };
  return (
    <div className="flex items-center gap-2" onPaste={onPaste} data-testid="otp-boxes">
      {Array.from({ length }).map((_, i) => (
        <input
          key={i}
          ref={(el) => (refs.current[i] = el)}
          type="text"
          inputMode="numeric"
          maxLength={1}
          value={value[i] || ""}
          onChange={(e) => set(i, e.target.value.replace(/\D/g, "").slice(-1))}
          onKeyDown={(e) => onKey(i, e)}
          className="h-14 w-12 sm:w-14 rounded-sm border border-gray-300 text-center font-mono text-xl font-semibold focus:border-gray-900 focus:outline-none focus:ring-0"
          data-testid={`otp-${i}`}
        />
      ))}
    </div>
  );
}
```

Use a "back" link to return to step 1:
```jsx
<button type="button" onClick={() => setStep("phone")}
  className="inline-flex items-center gap-1.5 text-xs uppercase tracking-wider text-gray-500 hover:text-gray-900 mb-6">
  <ArrowLeft size={12} strokeWidth={1.5} /> Change number
</button>
```

While mocked, surface the test code clearly:
```jsx
<div className="flex items-center gap-2 text-xs text-gray-600 bg-gray-50 border border-dashed border-gray-300 rounded-sm p-3">
  <ShieldCheck size={14} strokeWidth={1.5} className="text-gray-500 shrink-0" />
  <span>Mock mode — use code <span className="font-mono font-semibold text-gray-900">1234</span> to sign in.</span>
</div>
```

---

## 13. Type-ahead with "+ Add new"

Lets users pick from a list **or** create a new entry inline. Pattern is two rows in the popover when the typed query has no exact match.

```jsx
function TypeAheadPicker({ options, value, onChange, existingNames = new Set() }) {
  const [open, setOpen] = useState(false);
  const [query, setQuery] = useState(value || "");
  const inputRef = useRef(null);

  useEffect(() => {
    if (value) setQuery(value);
    else if (!open) setQuery("");
  }, [value, open]);

  const q = query.trim();
  const qLower = q.toLowerCase();
  const matches = options.filter((c) => c.name.toLowerCase().includes(qLower));
  const exact = options.find((c) => c.name.toLowerCase() === qLower);
  const canCreate = q.length >= 2 && !exact && !existingNames.has(q);

  const commit = (item) => { onChange(item); setQuery(item.name); setOpen(false); };
  const createCustom = () => canCreate && commit({ name: q, sector: "Custom" });

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <div
          tabIndex={-1}
          onClick={() => { setOpen(true); setTimeout(() => inputRef.current?.focus(), 0); }}
          className={`mt-2 h-10 w-full rounded-sm border px-3 flex items-center gap-2 cursor-text outline-none focus:outline-none focus-visible:outline-none focus:ring-0 transition-colors ${open || value ? "border-gray-900" : "border-gray-200 hover:border-gray-900"}`}
        >
          <Search size={14} strokeWidth={1.5} className="text-gray-400 shrink-0" />
          <input
            ref={inputRef}
            value={query}
            onFocus={() => setOpen(true)}
            onChange={(e) => { setQuery(e.target.value); setOpen(true); if (value) onChange(null); }}
            onKeyDown={(e) => {
              if (e.key === "Enter") { e.preventDefault(); if (exact && !existingNames.has(exact.name)) commit(exact); else if (canCreate) createCustom(); }
              if (e.key === "Escape") setOpen(false);
              if (e.key === "ArrowDown") setOpen(true);
            }}
            placeholder="Type to search or add…"
            className="flex-1 h-full bg-transparent outline-none text-sm placeholder:text-gray-400"
          />
          <ChevronDown size={14} strokeWidth={1.5} className="text-gray-500 shrink-0" />
        </div>
      </PopoverTrigger>
      <PopoverContent align="start" sideOffset={6} onOpenAutoFocus={(e) => e.preventDefault()} className="w-96 p-0 rounded-sm border-gray-200">
        <div className="max-h-72 overflow-y-auto">
          {canCreate && (
            <button type="button" onClick={createCustom}
              className="w-full text-left px-3 py-2.5 border-b border-gray-100 flex items-center gap-2 bg-gray-50 hover:bg-gray-100">
              <span className="inline-flex h-5 w-5 items-center justify-center rounded-full bg-gray-900 text-white text-[11px] font-bold">+</span>
              <div>
                <div className="text-sm">Add <span className="font-semibold">{q}</span> as new</div>
                <div className="text-[11px] text-gray-500 mt-0.5">Hit Enter to create</div>
              </div>
            </button>
          )}
          {matches.map((c) => {
            const disabled = existingNames.has(c.name);
            return (
              <button key={c.name} type="button" disabled={disabled}
                onClick={() => !disabled && commit(c)}
                className={`w-full text-left px-3 py-2 border-b border-gray-100 last:border-0 flex items-center justify-between ${disabled ? "opacity-40" : "hover:bg-gray-50"}`}>
                <div>
                  <div className="text-sm font-medium">{c.name}</div>
                  <div className="text-[11px] uppercase tracking-wider text-gray-500 mt-0.5">{c.sector}</div>
                </div>
                {disabled && <span className="text-[10px] uppercase tracking-wider text-gray-400">Already added</span>}
              </button>
            );
          })}
        </div>
      </PopoverContent>
    </Popover>
  );
}
```

> **Why `tabIndex={-1}` + `outline-none focus:outline-none focus-visible:outline-none focus:ring-0`?** When Radix opens the popover it focuses the trigger; without these the wrapping `<div>` shows a stray black outline. Always apply on custom-div triggers.

---

## 14. DataGrid conventions

Every grid must:

1. Use a sortable header component (the project's `<SortTh>`) — not raw `<th>`.
2. Render filters with a single shared `<FilterBar>` so toolbars match.
3. Default to **15 rows per page**.
4. Support multi-selection via `<SelectHead>` / `<SelectCell>` — and surface a `<BulkBar>` when rows are picked.
5. Set `data-testid="<entity>-row-<id>"` on every row.

Skeleton:

```jsx
const filterDefs = useMemo(() => [
  { field: "code", label: "Code", type: "text" },
  { field: "redeemed", label: "Status", type: "select",
    options: [{ value: "redeemed", label: "Redeemed" }, { value: "unredeemed", label: "Unredeemed" }],
    getValue: (r) => r.redeemed ? "redeemed" : "unredeemed" },
  { field: "redeemed_at", label: "Redeemed on", type: "date-range" },
], []);
const filters = useFilters(rows, filterDefs);
const { rows: visible, sort, toggleSort, page, setPage, totalPages, total, perPage } =
  useSortPage(filters.filtered, { defaultSort: { field: "code", dir: "asc" }, perPage: 15, getValue });
const sel = useRowSelection(visible.map((r) => r.code));

return (
  <section className="card-flat">
    <div className="p-5 border-b border-gray-200 flex items-center justify-between gap-3 flex-wrap">
      <h3 className="font-display font-semibold">Codes</h3>
      <FilterButton filters={filters} testid="codes-filter-toggle" />
    </div>
    <FilterBar filters={filters} defs={filterDefs} testid="codes-filter-bar" gridCols={4} />
    <BulkBar selection={sel} label={sel.count === 1 ? "code" : "codes"}>
      <BulkButton onClick={bulkCopy}><Copy size={12} strokeWidth={1.5} /> Copy</BulkButton>
      <BulkButton onClick={bulkExport}><Download size={12} strokeWidth={1.5} /> Export</BulkButton>
    </BulkBar>
    <table className="w-full text-sm">
      <thead>
        <tr className="text-left bg-gray-50/60 border-b border-gray-200">
          <SelectHead ids={visible.map((r) => r.code)} selection={sel} />
          <SortTh field="code" sort={sort} onToggle={toggleSort} className="px-5 py-3">Code</SortTh>
          <SortTh field="redeemed" sort={sort} onToggle={toggleSort} className="px-5 py-3">Status</SortTh>
        </tr>
      </thead>
      <tbody>
        {visible.map((r) => (
          <tr key={r.code} className="border-t border-gray-100 hover:bg-gray-50">
            <SelectCell id={r.code} selection={sel} />
            <td className="px-5 py-3 font-mono font-semibold">{r.code}</td>
            <td className="px-5 py-3">{/* status pill */}</td>
          </tr>
        ))}
      </tbody>
    </table>
    <Pagination page={page} totalPages={totalPages} setPage={setPage} total={total} perPage={perPage} label="codes" />
  </section>
);
```

---

## 15. Status pills

```jsx
// Neutral
<span className="inline-flex items-center gap-1 text-[10px] uppercase tracking-wider bg-gray-50 text-gray-600 rounded-sm px-1.5 py-0.5">
  <Circle size={8} strokeWidth={1.5} /> Unredeemed
</span>

// Positive
<span className="inline-flex items-center gap-1 text-[10px] uppercase tracking-wider bg-green-50 text-green-700 rounded-sm px-1.5 py-0.5">
  <Check size={10} strokeWidth={2} /> Redeemed
</span>

// Warning
<span className="inline-flex items-center gap-1 text-[10px] uppercase tracking-wider bg-amber-50 text-amber-700 rounded-sm px-1.5 py-0.5">
  Partial
</span>

// Danger
<span className="inline-flex items-center gap-1 text-[10px] uppercase tracking-wider bg-red-50 text-red-700 rounded-sm px-1.5 py-0.5">
  <AlertTriangle size={10} strokeWidth={2} /> Expired
</span>
```

Always: `text-[10px]`, `uppercase`, `tracking-wider`, `rounded-sm`, `px-1.5 py-0.5`.

---

## 16. Empty states

```jsx
<div className="border border-dashed border-gray-300 rounded-sm p-8 text-center" data-testid="empty">
  <Wand2 size={18} strokeWidth={1.5} className="mx-auto text-gray-400 mb-2" />
  <p className="text-sm font-medium text-gray-900">No coupons generated yet</p>
  <p className="text-xs text-gray-500 mt-1 mb-4">
    Click <span className="font-semibold">Generate Unique Codes</span> above to create the coupons for this parcel.
  </p>
  <Button className="rounded-sm h-9 bg-gray-900 hover:bg-black gap-2">
    <Wand2 size={14} strokeWidth={1.5} /> Generate Unique Codes
  </Button>
</div>
```

Dashed border + centred icon + headline + helper paragraph + optional CTA.

---

## 17. Toasts

```js
// lib/toast.js
export { toast } from "sonner";

// Mount once at the root:
import { Toaster } from "sonner";
<Toaster richColors position="bottom-right" closeButton />
```

```jsx
toast.success("Acme Wellness added");
toast.error(formatApiError(e.response?.data?.detail) || e.message);
toast("Continuing without a coupon"); // neutral
```

- Use toasts for confirmations and non-critical errors.
- Critical destructive flows (delete, recall) require a confirmation **dialog**, not just a toast.

---

## 18. Interaction & motion rules

- Hover is feedback, not decoration. Default hover for cards / inputs / row containers:
  ```css
  border-gray-200 → border-gray-900   /* stroke darkens */
  bg-white       → bg-gray-50         /* surface lifts */
  ```
- Transitions go on **specific properties only**: `transition-colors`, `transition-transform`, `transition-opacity`. **Never** `transition-all` (it breaks animations).
- Focus rings: `focus-visible:` only. Pointer focus usually shouldn't paint a ring. For custom popover triggers wrapped in `<div>`, kill the inherited Radix outline:
  ```
  outline-none focus:outline-none focus-visible:outline-none focus:ring-0
  ```
- Stagger reveals on hero / login: `animate-rise` + `style={{ animationDelay: "220ms" }}`.

---

## 19. Naming conventions

User-facing labels follow this terminology — be ruthless and search before you commit:

| Use | Don't use |
|-----|-----------|
| Account | Tenant |
| Item | Service |
| **Quantity** | Number of coupons / Total coupons |
| Distribute | Assign |
| Mobile | Phone (in user-facing labels) |

---

## 20. Test IDs

Every interactive element gets a stable `data-testid` so Playwright tests and AI agents can rely on it.

- Format: `kebab-case-action-purpose`. Example: `login-send-otp`, `acct-create-submit`.
- Every row: `data-testid="<entity>-row-<id>"`.
- Every error block: `data-testid="<flow>-error"`.
- Bulk actions: `data-testid="<grid>-bulk-<action>"`.

CSS classes change. Test IDs don't.

---

## 21. Accessibility checklist

- All buttons: real `<button type="button">` (or Shadcn `Button`) with `aria-label` if icon-only.
- All inputs: paired visible `<Label>` (preferred) or `aria-label`.
- Toggles: Shadcn `<Switch>` exposes `aria-pressed` automatically.
- Tab order: only one element per row tabbable. Use `tabIndex={-1}` on row wrappers that forward focus.
- Color contrast: never put `text-gray-400` on `bg-white` for primary content (reserved for placeholders).
- Mandatory dialogs: announce via `DialogTitle` + `DialogDescription` so screen readers narrate the obstruction.

---

## 22. PR checklist

- [ ] No purple gradients, no decorative emoji, no `rounded-md` / `rounded-lg`.
- [ ] Every interactive element has a `data-testid`.
- [ ] Numbers use `tabular`. Codes / IDs use `font-mono`.
- [ ] Page title block uses the H1 + overline pattern; primary actions live on the right.
- [ ] Tables use a shared sort/page hook + `<SortTh>`.
- [ ] One primary (filled black) button per visible scope.
- [ ] Lint passes; no `transition-all`.
- [ ] Self-tested via screenshot (or full agent run if 3+ flows).

---

## TL;DR — the 7 rules that define this look

1. **Sharp corners** — `rounded-sm` (2px) everywhere except chips.
2. **Three fonts** — Cabinet Grotesk (display), Manrope (body), JetBrains Mono (numbers/codes).
3. **One primary button** — solid `bg-gray-900` per visible scope. Everything else is outline.
4. **Overlines for labels** — uppercase 11px 0.12em tracking. No exceptions.
5. **Tabular numbers** — every count, date, code uses `.tabular`.
6. **Lucide icons only** — `strokeWidth={1.5}`, `size={14}` default.
7. **Test IDs on every interactive** — `kebab-case-action-purpose`.

Follow these seven and the rest emerges naturally.

---

*This guide is portable. Drop it in another React+Tailwind app, run the install commands, paste the CSS, and you're shipping in the same visual language within an hour.*
