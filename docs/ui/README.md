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

## Interface rules

These apply to every screen, and a design that breaks one is not finished.

- **Say each thing once per screen.** A status, count, name or state appears in one place. If it
  appears twice, the second one tells the reader something new, and it is not a copy of the first.
  A device shown as connected does not also get a connected badge, a green dot and a line of text.
- **Every element earns its place.** If removing it loses no information and no action, remove it.
  That covers decorative badges, labels that restate the control, helper text that repeats the
  label, a heading that repeats the page title, and made-up stats.
- **It must not look generated.** No icon on every card, no uniform grids of identical cards, no
  gradients, glows or emoji, no oversized hero headings in a tool, no pills on everything.
  Hierarchy comes from spacing, weight and alignment, not from boxes and colour.
- **Write for an intelligent adult.** Plain, direct text. No cheerleading, no explaining the
  obvious, no filler.
- **Review it that way.** The checklist for any new screen: what is shown twice, and what could be
  removed?

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
- Storybook for the gallery
- Vitest, Testing Library and vitest-axe for tests
- A lint rule that rejects hard-coded colours, spacing and sizes, so only tokens are used

## Decisions

- Token source of truth: `@matumi/tokens` in Heartwood. It starts from `resources/brand/tokens.json`
  in this repo, imported once. The brand file stays the reference for logos and brand colours.
- Icons: lucide-react as the base, plus custom Matumi icons where lucide has no icon or the
  existing one does not fit. A custom icon has the same grid, stroke and sizes as the lucide set.
- Gallery: Storybook, themed with Heartwood's own branding. It lives in `src/apps/gallery`.
