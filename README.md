<div align="center">

<img src="./zeus-css-logo.png" alt="Zeus CSS" width="420">

### A Premium, Zero-Emission, Fluid-Scaling CSS Framework

Built on a pure Sass/SCSS engine. No runtime. No config bloat. No `md:` breakpoint soup for things that should just *fluidly scale*.

[![npm version](https://img.shields.io/npm/v/zeus-css?color=2f6bff&label=npm&style=flat-square)](https://www.npmjs.com/package/zeus-css)
[![npm downloads](https://img.shields.io/npm/dm/zeus-css?color=2f6bff&style=flat-square)](https://www.npmjs.com/package/zeus-css)
[![bundle size](https://img.shields.io/bundlephobia/minzip/zeus-css?color=ff8a00&style=flat-square)](https://bundlephobia.com/package/zeus-css)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-ff8a00?style=flat-square)](#contributing)

**[zeuscss.com](https://zeuscss.com) · [npm](https://www.npmjs.com/package/zeus-css)**

</div>

<br>

## Why Zeus CSS

Most CSS frameworks make you pick a side: a heavyweight utility system that emits everything and needs PurgeCSS to be usable, or a component kit that fights you the moment you need something custom. Zeus CSS is built around one idea instead — **design tokens that scale themselves**, exposed through a `@forward`-based SCSS API that emits *nothing* until you actually use it.

| | |
|---|---|
| 🌊 **Fluid Linear Interpolation Engine** | A mathematical `clamp()` generator drives typography, spacing, and sizing across container-query bounds (`cqi`) — no `vw` hacks, no wall of media queries for things that should just scale. |
| 🧬 **Recursive Token Pipeline** | Sass maps for color, type, space, radius, shadow, and z-index are recursively walked and emitted as CSS Custom Properties — author once in SCSS, consume everywhere as `var(--*)`. |
| 🛡️ **`@property` Type Safety** | Key tokens are registered with native `@property`, giving the browser real type-checking and safe interpolation instead of stringly-typed custom properties. |
| 🪶 **Zero-Emission SCSS API** | `@use "zeus-css/zeus" as *;` exposes every mixin, function, and token — **zero bytes of duplicate CSS emitted** — thanks to Sass `@forward`. |
| 🎨 **OKLCH-Native Theming** | Define your palette once in perceptually-uniform OKLCH; light/dark variants and states derive from it automatically. |
| 🧱 **Cascade-Layered by Design** | Everything ships inside `@layer reset, base, tokens, components, utilities` — your overrides win without a single `!important`. |
| 🤖 **AI-Agent Ready** | Ships with [`llms.txt`](./llms.txt), a generated [cheatsheet](./cheatsheet.md), and [`AGENTS.md`](./AGENTS.md) so Claude Code, Cursor, Copilot & friends write on-token Zeus CSS instead of inventing hex codes. |

<br>

## Quick Start

```bash
npm install zeus-css
```

**No customization needed?** Import the precompiled defaults from your app's single root entry point:

```ts
import 'zeus-css/dist/zeus.css';
```

<details>
<summary><strong>Where that import goes, per framework</strong></summary>
<br>

| Setup | Entry file | Import |
| --- | --- | --- |
| Next.js (App Router) | `app/layout.tsx` | `import 'zeus-css/dist/zeus.css';` |
| Next.js (Pages Router) | `pages/_app.tsx` | `import 'zeus-css/dist/zeus.css';` |
| React + Vite | `src/main.tsx` | `import 'zeus-css/dist/zeus.css';` |
| Create React App | `src/index.tsx` | `import 'zeus-css/dist/zeus.css';` |
| Vue 3 + Vite | `src/main.ts` | `import 'zeus-css/dist/zeus.css';` |
| Nuxt 3 | `nuxt.config.ts` | `css: ['zeus-css/dist/zeus.css']` |
| Astro | shared `Layout.astro` | `import 'zeus-css/dist/zeus.css';` in frontmatter |
| Plain HTML | `<head>` | `<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/zeus-css/dist/zeus.css">` |

> Always import from one shared/root entry point — never from an individual component. Next.js in particular will error on global CSS imported outside the root layout.

</details>

**Want your own palette, spacing, and breakpoints?** Scaffold a local theme instead:

```bash
npx zeus-css init
```

<details>
<summary><strong>What <code>init</code> creates — and what's safe to edit</strong></summary>
<br>

| File | Edit it? | Purpose |
| --- | :---: | --- |
| `zeus.config.scss` | ✅ | Feature toggles, breakpoints, layout-engine settings |
| `zeus.customize.scss` | ✅ | Design tokens — colors, typography, spacing, shadows |
| `zeus.scss` | ❌ generated | Bridge import for `.module.scss` files — emits no CSS |
| `zeus.theme.scss` | ❌ generated | Entry point you compile to get your themed stylesheet |
| `zeus-css-env.d.ts` | ❌ generated | TS ambient module so the CSS import type-checks |

Re-running `npx zeus-css init` (e.g. after an upgrade) only fills in files you're *missing* — it never touches ones you already have.

</details>

```scss
// zeus.customize.scss
$zeus-colors: (
  "primary": oklch(57.9% 0.232 259.6),
  "secondary": oklch(74.7% 0.174 60.6),
) !default;
```

```bash
sass --load-path=node_modules zeus.theme.scss zeus.theme.css
```

Then swap your import to point at the compiled output — **never import both the default stylesheet and your theme build**:

```ts
import './zeus.theme.css';
```

<br>

## The fluid engine, in one example

Zeus doesn't ask you to guess breakpoints for type scale — you declare the bounds once, and every token between them interpolates on its own.

```scss
@use "zeus-css/zeus" as *;

.card {
  padding: var(--space-md) var(--space-lg);
  background: var(--color-surface);
  border-radius: var(--border-radius-md);
  border: var(--border-line-sm) solid var(--color-border);

  // Structural change only — fluid scaling already handled the sizing
  @include screen-above(lg) {
    border-radius: var(--border-radius-lg);
  }
}
```

`--space-md`, `--text-h2`, `--border-radius-lg` — every token in the system is a live `clamp()` that scales continuously with the container, not a fixed value swapped at a breakpoint.

<br>

## What ships in the box

<table>
<tr><td valign="top" width="50%">

**Utility class families**
- Layout — `flex-*`, `grid-*`, `d-*`
- Spacing — `gap-*`, `p*()`, `m*()`
- Color — `bg-*`, `color-*`, `border-color-*`
- Typography — `text-*`
- Shadow — `shadow-*`
- Sizing — `w-*`, `h-*`, `min-w-*`, `max-w-*`, `box-*`
- Position — `absolute`, `sticky`, `inset-*`, `top-*`
- Animation — `animate-*`, `delay-*`, `duration-*`
- Compound — `.surface`, `.glass`, `.divider`, `.truncate`, `.line-clamp-*`

</td><td valign="top" width="50%">

**BEM components (`.z-*`)**
- `.z-card`, `.z-card__media`
- `.z-modal`, `.z-modal-backdrop`
- `.z-field`, `.z-input`, `.z-select`, `.z-checkbox`, `.z-radio`
- `.z-badge`, `.z-alert`, `.z-avatar` (+ group)
- `.z-table`, `.z-breadcrumb`, `.z-pagination`
- `btn-*` mixin family for fully custom buttons

**Shorthand mixin aliases**
- `p()` `m()` `g()` `r()` `t()` `sz()` — padding, margin, gap, radius, text, size

</td></tr>
</table>

<br>

## How it compares

|  | **Zeus CSS** | Utility-first frameworks | Component kits |
|---|:---:|:---:|:---:|
| Responsive scaling | Fluid `clamp()`, continuous | Breakpoint steps | Breakpoint steps |
| SCSS API surface | `@forward`, zero-emission | Usually none | Varies |
| Color model | OKLCH-native | Mostly HSL/HEX | Varies |
| Type-safe custom properties | `@property`-registered | Rare | Rare |
| Cascade isolation | Native `@layer` | Varies | Varies |
| AI-agent docs (`llms.txt`) | ✅ built-in | Varies | Varies |
| Needs PurgeCSS in prod | ✅ yes, like the rest of the category | ✅ yes | Usually no |

<br>

## ⚠️ Production builds need PurgeCSS

Zeus ships thousands of utility classes for maximum flexibility during development. Strip the unused ones before you ship:

```js
// postcss.config.js
module.exports = {
  plugins: [
    ['@fullhuman/postcss-purgecss', {
      content: [
        './pages/**/*.{js,jsx,ts,tsx}',
        './components/**/*.{js,jsx,ts,tsx}',
        './src/**/*.{js,jsx,ts,tsx}',
      ],
      defaultExtractor: (content) => content.match(/[\w-/:]+(?<!:)/g) || [],
    }],
  ],
};
```

<br>

## Docs

| Doc | What it's for |
|---|---|
| [`API.md`](./API.md) | The semver contract — what's public and stable |
| [`cheatsheet.md`](./cheatsheet.md) | Full, auto-generated mixin/class catalog |
| [`llms.txt`](./llms.txt) | Machine-readable reference for AI coding agents |
| [`AGENTS.md`](./AGENTS.md) | Rules for AI agents contributing to Zeus-powered codebases |

<br>

## Contributing

Issues and PRs are welcome at [zeus-css/zeus-css-framework](https://github.com/zeus-css/zeus-css-framework). This repository is published automatically from the framework's monorepo — please open issues here rather than against a fork.

## License

[MIT](./LICENSE) © [Stavros Kosmas Lazaris](https://zeuscss.com)

<br>

<div align="center">
<sub>⚡ built with an unreasonable amount of care for clamp() ⚡</sub>
</div>
