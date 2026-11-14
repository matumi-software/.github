# Tokens

Status: the colour, spacing, radius and shadow tokens are set. The items under "Still to define" wait
for the design pass.

The source of truth is `@matumi/tokens` in Heartwood, imported once from `resources/brand/tokens.json`
in this repo. Heartwood generates `tokens.css`: light is the default, dark follows the system, and a
`data-theme` attribute on the page always wins. Every token is a CSS variable with the prefix
`--hw-`, for example `--hw-bg-page`.

Components use tokens only. Lint rejects a hard-coded colour or pixel size in a component.

## Colour

| Token | Light | Dark | Use |
|---|---|---|---|
| `bg-page` | `#FBF9F4` | `#161512` | Page background behind every panel. Warm char in dark, paper in light, never blue-black. |
| `bg-surface` | `#FFFFFF` | `#1E1D19` | Cards, panels and form sections. |
| `bg-inset` | `#F5F2EA` | `#131210` | Recessed areas: input wells, ID chips. |
| `bg-raised` | `#FFFFFF` | `#26241F` | Popovers, menus and dialogs that sit above a surface. |
| `text-primary` | `#1C1B18` | `#EDEAE0` | Body copy, headings on bg-page, bg-surface and bg-raised. |
| `text-secondary` | `#5C584D` | `#B5B0A2` | Supporting text: host names, descriptions, field help on bg-page and bg-surface. |
| `text-tertiary` | `#6E6A5E` | `#8A8578` | Captions, timestamps and placeholders on bg-page, bg-surface and bg-inset. Light value is one step darker than Matumi's stone-500 so it passes 4.5:1. |
| `border-default` | `#DDD8CA` | `#34322B` | Dividers and card edges. Decorative only, never the sole edge of a control. |
| `border-control` | `#8A8578` | `#7A766A` | Edges of inputs, switches and secondary buttons. Holds 3:1 on bg-surface, bg-page and bg-raised. |
| `focus-ring` | `#3A805C` | `#5FA983` | 2px solid keyboard focus ring with a 2px offset. Holds 3:1 on every surface in both themes. |
| `accent` | `#2A6B4E` | `#549572` | Canopy green, the accent of every Matumi Software product. Primary buttons, the selected tab, links and the one canopy element in every mark. One accent action per view. |
| `accent-hover` | `#20543D` | `#5FA983` | Hover and pressed state of accent fills. |
| `accent-subtle` | `#EAF2ED` | `#1D2B23` | Selected row and selected segment backgrounds. |
| `accent-text` | `#173F2E` | `#9CCAB1` | Accent text on accent-subtle and bg-surface. |
| `on-accent` | `#FFFFFF` | `#0E1F17` | Text and icons on an accent fill. |
| `river` | `#2F6F74` | `#5FA0A6` | The family second colour, for graphics and state. Never an accent, never a button. |

Dark is the first theme. Neutrals are warm, never blue-black. `accent` is the only accent. `river` is
for graphics and charts, never as an accent.

## Spacing

| Token | Value | Use |
|---|---|---|
| `space-1` | `4px` | Icon to label gap, badge padding block. |
| `space-2` | `8px` | Gap inside a control group, badge padding inline. |
| `space-3` | `12px` | Field label to input, device card row gap. |
| `space-4` | `16px` | Card padding, page gutter on phones. |
| `space-6` | `24px` | Gap between cards and form sections. |
| `space-8` | `32px` | Page gutter on desktop, gap between page regions. |

## Radius

| Token | Value | Use |
|---|---|---|
| `radius-sm` | `3px` | Badges, ID chips, checkboxes. |
| `radius-md` | `6px` | Buttons, inputs, segmented controls. |
| `radius-lg` | `10px` | Cards, dialogs, panels. |
| `radius-pill` | `999px` | Switch track and thumb, live-link dot. |

## Shadow

One shadow, for popovers, menus and dialogs. Nothing else casts one.

| Token | Light | Dark |
|---|---|---|
| `shadow-raised` | `0 8px 24px rgba(18, 24, 22, 0.12)` | `0 8px 24px rgba(0, 0, 0, 0.45)` |

## Type

- Instrument Sans for text and wordmarks (wordmarks 600, lowercase).
- JetBrains Mono for code, paths and identifiers.
- Sentence case everywhere.

## Still to define

| Token group | Why it is needed |
|---|---|
| Status colours: success, warning, danger, info | Fill, subtle background and text for each, in both themes. None exist yet, and badges, callouts and destructive buttons need them |
| Type scale | Sizes, weights and line heights for display, title, heading, body, small, caption and mono |
| Motion | Durations and easings, with a reduced-motion rule |
| Chart colours | A colour-blind safe categorical set from `accent` and `river`, plus sequential and diverging ramps |
| Extra elevation | Only if a design needs more than the one shadow |
