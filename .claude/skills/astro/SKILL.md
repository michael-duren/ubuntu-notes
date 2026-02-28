---
name: astro
description: Reference for Astro.js patterns, components, content collections, and project structure. Use when working with Astro files, pages, layouts, or content.
allowed-tools: Read, Grep, Glob, WebFetch, WebSearch
argument-hint: "[topic]"
---

# Astro.js Reference

You are helping with an **Astro 5** static site. Follow these conventions and patterns.

## Project Structure

```
src/
├── content.config.ts      # Content collection definitions
├── data/                  # Static JSON data
├── content/guides/        # Markdown content (glob loader)
├── styles/global.css      # Global CSS + Tailwind
├── layouts/BaseLayout.astro
├── components/            # Reusable .astro components
└── pages/                 # File-based routing
```

## Key Patterns

### Pages & Layouts
- Pages live in `src/pages/` — each `.astro` file becomes a route
- All pages use `BaseLayout.astro` which includes the HTML shell, fonts, dark mode script, header, footer, and search modal
- Frontmatter (between `---` fences) runs at build time on the server

### Content Collections (Astro 5 API)
- Defined in `src/content.config.ts` using `defineCollection` and `z` from `astro:content`
- Uses `glob()` loader from `astro/loaders` for markdown files
- **Render markdown**: use standalone `render()` function, NOT `entry.render()`:
  ```astro
  import { getCollection, render } from 'astro:content';
  const entries = await getCollection('guides');
  const { Content } = await render(entry);
  ```

### Components
- `.astro` components accept typed props via `Astro.props`
- Define `interface Props` in frontmatter for type safety
- Client-side JS goes in `<script>` tags (bundled by Vite)
- For inline scripts that must run immediately: `<script is:inline>`
- Re-initialize after page transitions: `document.addEventListener('astro:after-swap', initFn)`

### Static Data
- JSON files in `src/data/` can be imported directly: `import data from '../data/file.json'`
- Embedded in pages at build time — no runtime fetch needed

### Client-Side Interactivity
- Astro ships zero JS by default — only `<script>` tags in components get bundled
- For framework components (React, etc.), use `client:*` directives
- This project uses vanilla JS in `<script>` tags for search, theme toggle, and mobile menu

## If asked about a specific Astro topic ($ARGUMENTS)

1. Check the project files first for existing patterns
2. Reference the Astro docs at https://docs.astro.build if needed
3. Always use Astro 5 APIs (standalone `render()`, content layer with loaders)
4. Keep solutions static-first — avoid SSR unless explicitly requested
