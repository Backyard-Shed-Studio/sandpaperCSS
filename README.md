# CpoSS

Reusable CSS files shared across projects. CSS only, no build step.

## Usage

```bash
pnpm add cposs
```

Import everything:

```css
@import "cposs";
```

Or a single file (`reset.css` → `cposs/reset`):

```css
@import "cposs/reset";
```

In a Nuxt project, add it to `nuxt.config.ts`:

```ts
export default defineNuxtConfig({
  css: ["cposs"],
});
```

## Files

| File | Content |
|---|---|
| `sizing.css` | width, height, aspect ratio |
| `display.css` | display, centering, overflow |
| `flex.css` | direction, wrap, justify, align, grow, shrink, gap |
| `position.css` | position, offsets, z-index, isolate |
| `spacing.css` | margins, padding |
| `text.css` | alignment, transform, weight, font size |
| `fluid-text.css` | fluid font sizes that scale with the viewport width (see below) |
| `interaction.css` | cursor, selection, pointer events |
| `media.css` | object-fit |
| `forms.css` | hidden inputs |
| `debug.css` | temporary borders and backgrounds |

## Fluid text

`fluid-text.css` defines fluid sizes that scale smoothly between `--fluid-vmin` and `--fluid-vmax`:

| Class | Custom property | Step |
|---|---|---|
| `.fluidTextXS` | `--fluid-text-xs` | base ÷ small scale² |
| `.fluidTextS` | `--fluid-text-s` | base ÷ small scale |
| `.fluidTextBase` | `--fluid-text-base` | base (the reference size, main reading text) |
| `.fluidTextL` | `--fluid-text-l` | base × scale |
| `.fluidTextXL` | `--fluid-text-xl` | base × scale² |
| `.fluidTextXXL` | `--fluid-text-xxl` | base × scale³ |
| `.fluidTextH6` | `--fluid-text-h6` | base × 0.67 (browser default h6) |
| `.fluidTextH5` | `--fluid-text-h5` | base × 0.83 (browser default h5) |
| `.fluidTextH4` | `--fluid-text-h4` | base × 1 (browser default h4) |
| `.fluidTextH3` | `--fluid-text-h3` | base × 1.17 (browser default h3) |
| `.fluidTextH2` | `--fluid-text-h2` | base × 1.5 (browser default h2) |
| `.fluidTextH1` | `--fluid-text-h1` | base × 2 (browser default h1) |

Every class follows the same pattern: `fluidText` + a size name.

- **`XS`, `S`, `Base`, `L`, `XL`, `XXL`** are for body text. `Base` is the reference size: smaller ones go down and bigger ones go up on the type scale. Use them for text that needs to stand out.
- **`H1` to `H6`** stay close to the browser's default heading sizes, but relative to the fluid base, so they scale with the viewport. They don't use the type scale.
- **Everything is opt-in.** No tag is styled automatically, and any class works on any tag (an `<h1 class="fluidTextS">` or a `<p class="fluidTextH2">` is fine). For something more specific, use `L`/`XL`/`XXL` or define your own class.

Project-specific values go in the project's own CSS, imported **after** cposs, by redefining the custom properties in `:root`:

```css
@import "cposs";

:root {
  --fluid-vmin: 400px;
  --fluid-vmax: 1200px;
  --base-max: 1.25rem;
  --fluid-text-h1: calc(var(--base) * 2.5);
}
```

Settings: `--fluid-vmin`, `--fluid-vmax`, `--base-min`, `--base-max`, `--type-scale-small`, `--type-scale-large`.
Each step is `--base * --type-scale ^ step`.

## Adding a file

1. Create `my-file.css` at the root of the package.
2. Add `@import "./my-file.css";` to `index.css`.

Every root-level `.css` file is published and importable as `cposs/my-file`.

## Local development

In the consuming project, link the package instead of installing it:

```json
"cposs": "link:../CpoSS"
```

## Publishing

```bash
npm version patch   # or minor / major
npm publish
```
