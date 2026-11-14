# Matumi UI specification

Status: outline. Each section below becomes its own file in this folder as it is written.

The UI is implemented in the Heartwood repo (`@matumi/ui`, `@matumi/shell`, `@matumi/tokens`,
`@matumi/icons`). This folder says what the UI should be. Heartwood's gallery shows what it is.
When the two disagree, fix the one that is wrong; do not let the spec describe a component that
does not exist.

## Direction

A softer, more modern look than the current apps. Same Matumi colours and type (see
`resources/brand`), applied through one shared component set so every app shares buttons, icons,
spacing and the app shell.

## Outline

| File | Covers |
|---|---|
| `principles.md` | Look and feel, density, tone, motion, dark first, what we avoid |
| `tokens.md` | Colour roles, type scale, spacing, radii, shadows, motion, light and dark |
| `iconography.md` | Icon set, grid, stroke, sizes, when to use an icon alone |
| `components.md` | One entry per component: purpose, anatomy, variants, states, accessibility |
| `shell.md` | App shell: sidebar, header, page layout, window chrome, tray windows |
| `patterns.md` | Forms, tables, empty states, errors, confirmations, toasts, loading |
| `accessibility.md` | Contrast, keyboard, focus, target size, screen reader rules, how it is tested |
| `contributing.md` | How to add or change a component, and how apps consume a release |

## Starting component inventory

Taken from what the existing dashboards need. This is a list to confirm, not a commitment.

| Group | Components |
|---|---|
| Actions | Button, icon button, delete button, segmented control |
| Inputs | Text input, search input, number field, select, switch |
| Display | Badge, status dot, tone dot, legend chip, card, KPI card, callout |
| Data | Data table, area chart |
| Navigation | Sidebar, nav button, panel tabs, page header |
| Feedback | Toast stack, confirm dialog, error boundary |
| Theming | Theme switcher |

## Proposed technical base

Chosen because it is accessible by default and already proven in a working dashboard.

- React and TypeScript, built with Vite
- Tailwind CSS 4 for styling, with tokens as CSS variables
- react-aria-components for behaviour and accessibility
- class-variance-authority, clsx and tailwind-merge for variants
- lucide-react as the starting icon set, with Heartwood's own icons added on top
- Vitest, Testing Library and vitest-axe for tests
- A lint rule that rejects hard-coded colours, spacing and sizes, so only tokens are used

## Open decisions

- Where the token source of truth lives: `resources/brand/tokens.json` here, or in
  `@matumi/tokens`.
- Gallery: Storybook or a custom app.
- Icons: keep lucide, or draw a Matumi set.
