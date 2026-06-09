# Oregano — Healthcare UI Design System

A calm, confident design system for healthcare software: deep teal for trust, warm coral for action, soft cream surfaces, light/regular grotesque type.

## Contents

| File | Purpose |
|------|---------|
| `index.html` | Styleguide — color, typography (H1–H6), buttons, cards, forms, components + token reference |
| `oregano.css` | Shared token + component library (link this from new pages) |
| `pages.html` | Hub linking the styleguide + sample pages |
| `ltc-landing.html` | Sample: long-term care marketing landing page (**eCareLTC**) |
| `signup.html` | Sample: split-screen sign-up |
| `onboarding.html` | Sample: interactive 4-step onboarding wizard |

## Foundations

- **Color:** primary teal `#003A43`, dark surface `#004952`, coral action `#FF8D6E` (hover `#F37E5E`), page cream `#F8F3EB`, sage surface `#EBF0EF`, body `#232323`.
- **Type:** Akkurat LL / Akkurat Mono LL (licensed) — free stand-ins **Inter** + **Space Mono** loaded per page; **Lora** accent serif. Headings set Light/Regular, tight `-0.04em` tracking.
- **Radius:** cards `16px` & `24px`, pills `100px`. Body `18px / 1.5`.

## Usage

Open `pages.html` in a browser. New pages link `oregano.css` and use the `:root` tokens.

> Akkurat is commercial; drop the licensed webfonts in via `@font-face` and the `--font-sans` variable for pixel-accurate output.
