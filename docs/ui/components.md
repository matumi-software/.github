# Components

Status: one component is built. The rest are listed and wait for the design pass.

Components are predictable primitives built on react-aria-components, styled with Tailwind and token
variables, and shown in the Storybook gallery. They carry no product wording.

## Inventory

| Group | Component | State |
|---|---|---|
| Actions | Button (primary, secondary, ghost; sm, md) | Built |
| Actions | Icon button (always with a tooltip and an accessible name), destructive button, segmented control | Planned |
| Inputs | Text input, search input, number field, select, combobox, checkbox, radio, switch, textarea | Planned |
| Display | Badge, status dot, tag, card, KPI card, callout, avatar, progress, skeleton | Planned |
| Display | Tooltip (name and one explanatory line, with the shortcut) | Planned, needed first because icon-only controls depend on it |
| Data | Table, list row, empty state, area chart | Planned |
| Navigation | Sidebar, nav button, tabs, breadcrumbs, page header | Planned |
| Feedback | Toast, dialog, confirm dialog, popover, menu | Planned |
| Theming | Theme switcher and colour vision mode switch | Planned |
| Shell | App shell parts, window chrome, tray window | Planned (see [shell](shell.md)) |
| Setup | The installer screens (see [installers](installers.md)) | Planned |

Some inventory items may merge or disappear. A badge, a status dot and a tag can be one component if
the design pass finds they are one idea. That follows the rule that a status appears once.

## An entry in this spec

Each component gets a section with:

1. Purpose, in one sentence, and when to use something else instead.
2. Anatomy: the parts, named.
3. Variants and sizes.
4. States: default, hover, pressed, focus, disabled, and error, loading or selected where they apply.
5. Behaviour: keyboard keys, and what it announces to a screen reader.
6. Content rules: label length, capitalisation, what it must not repeat.
7. The tokens it reads.

## Button, as built

- Variants: primary (the accent, at most one per view), secondary, ghost.
- Sizes: sm (32 px high) and md (40 px high).
- States: hover, pressed, keyboard focus ring, disabled.
- Built on react-aria's Button, so it handles press, keyboard and focus.
- Reads `accent`, `accent-hover`, `on-accent`, `bg-surface`, `bg-inset`, `border-control`, `accent-subtle`,
  `accent-text`, `focus-ring`, the spacing and radius tokens, and the sans font.
- Not yet built: a pending or loading state, an icon slot and a destructive variant. They wait for the
  status colours.
