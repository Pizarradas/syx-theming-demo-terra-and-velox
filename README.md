# syx-dual-brand-demo

A practical demonstration of [SYX Design System](https://github.com/Pizarradas/syx) theming capabilities.
**One SCSS codebase. Two completely different brands.**

---

## What this shows

| | VELOX | TERRA |
|---|---|---|
| **Brand** | B2B SaaS infrastructure | Sustainable foodtech |
| **Mood** | Dark, technical, electric | Light, natural, editorial |
| **Primary color** | Electric cyan `#6EE7F7` | Forest green |
| **Heading font** | Space Grotesk (geometric) | Lora (serif) |
| **Spacing grid** | 4px (`0.25rem`) | 8px (`0.5rem`) |
| **Border radius** | 4px — sharp | 8px — soft |

Neither the HTML structure nor the SCSS components change between brands.
Only the token definitions in `themes/velox/_theme.scss` and `themes/terra/_theme.scss` differ.

---

## Live demo

| Page | URL |
|------|-----|
| VELOX landing | `landing-velox.html` |
| TERRA landing | `landing-terra.html` |
| Design System Showcase | `ds-system.html` |

---

## How to create a new theme

SYX themes are isolated token files. To create a third brand:

**1. Duplicate the template**
```
scss/themes/_template/ → scss/themes/your-brand/
```

**2. Edit `_theme.scss` — the only file you need to touch**

The entire visual personality of a brand lives in three groups of tokens:

```scss
@mixin theme-your-brand {
  :root {

    // ── 1. SPACING BASE ──────────────────────────────
    // This single value scales EVERYTHING: buttons, padding,
    // form heights, gaps, border-radius, navbar height.
    // 0.25rem = 4px grid (compact / technical)
    // 0.5rem  = 8px grid (comfortable / editorial)
    --primitive-space-base: 0.5rem;

    // ── 2. COLOR PRIMITIVES ──────────────────────────
    // Define your brand palette here.
    // Use semantic names (primary, secondary…), not shades.
    --primitive-color-brand-500: #your-primary;
    --primitive-color-brand-400: #your-primary-lighter;
    --primitive-color-brand-600: #your-primary-darker;
    // ...backgrounds, text, borders, states

    // ── 3. TYPOGRAPHY ────────────────────────────────
    --primitive-font-family-heading: "Your Heading Font", serif;
    --primitive-font-family-body:    "Your Body Font", sans-serif;

    // ── Then wire primitives → semantic → component ──
    --semantic-color-primary: var(--primitive-color-brand-500);
    --semantic-font-family-heading: var(--primitive-font-family-heading);
    // ...
  }
}
```

**3. Register the theme**

```scss
// scss/styles-theme-your-brand.scss
@use 'themes/your-brand/setup' as *;
@include theme-your-brand-setup();
```

**4. Compile**
```bash
# With Prepros (recommended) — just add the file to the project
# Or with Dart Sass CLI:
sass scss/styles-theme-your-brand.scss css/styles-theme-your-brand.css
```

---

## ⚠️ Spacing token gotcha

The most impactful — and easy to overlook — setting is `--primitive-space-base`.

Every size that SYX components use derives from it via `calc()`:

```scss
--primitive-space-4: calc(4 * var(--primitive-space-base)); // 16px @ 4px | 32px @ 8px
--component-button-size-md: calc(1.75 * var(--base-measure)); // 7px @ 4px  | 14px @ 8px
--form-height-size: calc(6 * var(--base-measure));             // 24px @ 4px | 48px @ 8px
```

**Change `--primitive-space-base` and button heights, form fields, gaps, and padding all rescale automatically.**
This is intentional — it means a compact tech brand (VELOX, 4px) and a comfortable editorial brand (TERRA, 8px) look and feel proportionally distinct without overriding individual components.

If a component feels too tight or too spacious in your new theme, **start here** before touching individual component tokens.

---

## Project structure

```
syx-dual-brand-demo/
├── css/                          # Compiled output (ready to use)
│   ├── styles-theme-velox.css
│   └── styles-theme-terra.css
├── scss/
│   ├── abstracts/tokens/
│   │   ├── primitives/_spacing.scss  ← base scale (shared)
│   │   ├── semantic/                 ← contextual aliases
│   │   └── components/               ← per-component tokens
│   ├── themes/
│   │   ├── velox/_theme.scss         ← VELOX overrides
│   │   └── terra/_theme.scss         ← TERRA overrides
│   └── ...atoms, molecules, organisms
├── landing-velox.html
├── landing-terra.html
└── ds-system.html                ← live toggle between brands
```

---

## Based on

[SYX Design System](https://github.com/Pizarradas/syx) — token-driven SCSS system built on Atomic Design.
MIT License · Built by [José Luis Pizarro](https://linkedin.com/in/pizarradas)
