---
name: tailwind
description: Reference for Tailwind CSS v4 patterns, theme configuration, and styling. Use when working with styles, theming, dark mode, or CSS.
allowed-tools: Read, Grep, Glob, WebFetch, WebSearch
argument-hint: "[topic]"
---

# Tailwind CSS v4 Reference

This project uses **Tailwind CSS v4** with the Vite plugin. All configuration is done in CSS — there is no `tailwind.config.js`.

## Setup

```js
// astro.config.mjs
import tailwindcss from '@tailwindcss/vite';
export default defineConfig({
  vite: { plugins: [tailwindcss()] },
});
```

## Configuration (in `src/styles/global.css`)

### Theme
All theme tokens are defined with `@theme`:
```css
@import "tailwindcss";

@theme {
  --color-ubuntu-orange: #E95420;
  --color-ubuntu-aubergine: #77216F;
  --font-family-sans: 'Ubuntu', ui-sans-serif, system-ui, sans-serif;
  --font-family-mono: 'Ubuntu Mono', ui-monospace, monospace;
}
```
- `@theme` values become Tailwind utilities (e.g. `bg-ubuntu-orange`, `font-sans`)
- Use standard Tailwind naming: `--color-*`, `--font-family-*`, `--spacing-*`, etc.

### Dark Mode
Class-based dark mode with custom variant:
```css
@custom-variant dark (&:where(.dark, .dark *));
```
- Toggle by adding/removing `.dark` class on `<html>`
- Use in markup: `dark:bg-gray-900`, `dark:text-white`

### Runtime Surface Colors
For colors that must toggle at runtime (not just via `dark:` prefix), use CSS custom properties:
```css
:root {
  --surface-primary: #FFFFFF;
  --text-primary: #111111;
}
.dark {
  --surface-primary: #1A1A2E;
  --text-primary: #E8E8E8;
}
```
Apply via inline styles: `style="background-color: var(--surface-primary);"`

## Conventions in This Project

1. **Brand colors** → `@theme` utilities (`bg-ubuntu-orange`, `text-ubuntu-aubergine`)
2. **Surface/text colors** → CSS custom properties via `style` attributes (`var(--surface-card)`)
3. **Dark mode** → class-based, toggled via JS, persisted in localStorage
4. **Component styles** → Tailwind utility classes in markup, minimal custom CSS
5. **Special elements** → Custom CSS for `kbd`, `.guide-content` prose styles
6. **No `@apply`** — use utility classes directly in markup

## Available Custom Properties

| Property | Light | Dark | Usage |
|----------|-------|------|-------|
| `--surface-primary` | `#FFFFFF` | `#1A1A2E` | Page background |
| `--surface-secondary` | `#F7F7F7` | `#16213E` | Section backgrounds |
| `--surface-card` | `#FFFFFF` | `#1E1E3A` | Card backgrounds |
| `--text-primary` | `#111111` | `#E8E8E8` | Headings, body text |
| `--text-secondary` | `#555555` | `#B0B0C0` | Descriptions |
| `--text-muted` | `#888888` | `#777790` | Breadcrumbs, hints |
| `--border-color` | `#E5E5E5` | `#2E2E4E` | Card/table borders |
| `--kbd-bg` | `#F7F7F7` | `#2A2A4A` | Keyboard key background |
| `--table-stripe` | `#F9F9F9` | `#1E1E36` | Alternating table rows |

## If asked about a specific Tailwind topic ($ARGUMENTS)

1. Check `src/styles/global.css` for existing patterns
2. Reference the Tailwind v4 docs at https://tailwindcss.com/docs if needed
3. Always use v4 syntax — `@theme`, `@custom-variant`, CSS-based config
4. Never create a `tailwind.config.js` — everything goes in CSS
