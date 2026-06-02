# @man4ish/ui

Shared React component library for the [OmniBioAI](https://github.com/man4ish/omnibioai-studio) platform. Provides `Button`, `Badge`, `Card`, `Input`, `StatusDot`, and `Spinner` components built on unified design tokens with zero hardcoded colors.

---

## Overview

- **TypeScript-first** — full type definitions shipped with the package
- **Design-token driven** — all colors, spacing, and radii come from `@man4ish/design-tokens`; no hardcoded hex values anywhere in the library
- **Tree-shakeable** — built as ES module + CJS with Vite library mode; only import what you use
- **React 18+ peer dep** — works with any React 18+ project including the OmniBioAI Electron app and any downstream consumer

---

## Installation

This package is published to the GitHub Packages registry under `@man4ish`.

### 1. Authenticate with GitHub Packages

Create or update `.npmrc` in your project root:

```
@man4ish:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
```

Your token needs `read:packages` scope. Generate one at GitHub → Settings → Developer settings → Personal access tokens.

### 2. Install the package

```bash
npm install @man4ish/ui @man4ish/design-tokens
```

`@man4ish/design-tokens` is a required peer — it provides the CSS custom properties that all components reference.

### 3. Load design tokens

In your app entry point, import the token stylesheet once:

```ts
import '@man4ish/design-tokens/dist/tokens.css';
```

All components will pick up the correct colors, spacing, and radii automatically from there.

---

## Components

### `Button`

```tsx
import { Button } from '@man4ish/ui';

<Button variant="primary" onClick={handleRun}>
  Run analysis
</Button>

<Button variant="secondary" size="sm" disabled>
  Cancel
</Button>

<Button variant="danger" loading>
  Deleting...
</Button>
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `variant` | `'primary' \| 'secondary' \| 'ghost' \| 'danger'` | `'primary'` | Visual style |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Button size |
| `loading` | `boolean` | `false` | Shows spinner, disables interaction |
| `disabled` | `boolean` | `false` | Disables button |
| `onClick` | `() => void` | — | Click handler |

---

### `Badge`

```tsx
import { Badge } from '@man4ish/ui';

<Badge variant="success">Running</Badge>
<Badge variant="warning">Queued</Badge>
<Badge variant="danger">Failed</Badge>
<Badge variant="info">v1.0.0</Badge>
<Badge variant="neutral">Draft</Badge>
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `variant` | `'success' \| 'warning' \| 'danger' \| 'info' \| 'neutral'` | `'neutral'` | Semantic color |

---

### `Card`

```tsx
import { Card } from '@man4ish/ui';

<Card>
  <h3>Plugin output</h3>
  <p>Run completed in 4.2s</p>
</Card>

<Card elevated onClick={handleClick}>
  Clickable card
</Card>
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `elevated` | `boolean` | `false` | Adds shadow for visual prominence |
| `onClick` | `() => void` | — | Makes card interactive |
| `className` | `string` | — | Additional CSS classes |

---

### `Input`

```tsx
import { Input } from '@man4ish/ui';

<Input
  label="Sample ID"
  placeholder="Enter sample identifier"
  value={sampleId}
  onChange={e => setSampleId(e.target.value)}
/>

<Input
  label="VCF path"
  error="File not found"
  value={vcfPath}
  onChange={e => setVcfPath(e.target.value)}
/>
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `string` | — | Label above the input |
| `error` | `string` | — | Error message below; turns border red |
| `placeholder` | `string` | — | Placeholder text |
| `value` | `string` | — | Controlled value |
| `onChange` | `ChangeEventHandler` | — | Change handler |
| `disabled` | `boolean` | `false` | Disables input |

---

### `StatusDot`

Compact status indicator used in run lists, job monitors, and pipeline dashboards.

```tsx
import { StatusDot } from '@man4ish/ui';

<StatusDot status="running" />   // animated pulse
<StatusDot status="success" />
<StatusDot status="failed" />
<StatusDot status="queued" />
<StatusDot status="idle" />
```

| Prop | Type | Description |
|---|---|---|
| `status` | `'running' \| 'success' \| 'failed' \| 'queued' \| 'idle'` | Determines color and animation |

---

### `Spinner`

```tsx
import { Spinner } from '@man4ish/ui';

<Spinner />
<Spinner size="sm" />
<Spinner size="lg" />
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Spinner diameter |

---

## Importing

Everything is exported from the package root:

```ts
import { Button, Badge, Card, Input, StatusDot, Spinner } from '@man4ish/ui';
```

Tree-shaking is automatic — unused components are excluded from your bundle at build time.

---

## Local development

```bash
git clone https://github.com/man4ish/omnibioai-ui
cd omnibioai-ui
npm install

# Build once
npm run build

# Watch mode (rebuilds on every save)
npm run dev
```

Output lands in `dist/` as `index.js` (ES module) and `index.cjs` (CommonJS) with `index.d.ts` type declarations.

### Using in another local repo without publishing

```bash
# In this repo
npm run build
npm link

# In your consuming repo
npm link @man4ish/ui
```

---

## Repository layout

```
omnibioai-ui/
├── src/
│   ├── index.ts          ← package entry — re-exports all components
│   ├── Button/
│   ├── Badge/
│   ├── Card/
│   ├── Input/
│   ├── StatusDot/
│   └── Spinner/
├── vite.config.ts        ← library build config (ES + CJS, React external)
├── tsconfig.json
├── package.json          ← published as @man4ish/ui to GitHub Packages
└── .npmrc                ← scoped registry config
```

---

## Design token contract

Components reference CSS custom properties from `@man4ish/design-tokens`. No component contains a hardcoded color, spacing value, or radius — all visual decisions are delegated to the token layer. This means:

- Swapping themes (light/dark, brand variants) requires only a token override
- All OmniBioAI surfaces (Electron app, web, plugin UIs) stay visually consistent from a single source of truth
- No `!important` hacks or specificity wars when overriding in consuming apps

---

## Related packages

| Package | Purpose |
|---|---|
| [`@man4ish/design-tokens`](https://github.com/man4ish/omnibioai-design-tokens) | CSS custom properties — required peer dependency |
| [`omnibioai-studio`](https://github.com/man4ish/omnibioai-studio) | Electron + React app — primary consumer |
| [`omnibioai`](https://github.com/man4ish/omnibioai) | Django backend — 150+ plugin bioinformatics platform |

---

## License

MIT
