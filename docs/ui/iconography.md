# Iconography

Status: direction set, details wait for the design pass.

## Sets

- `@matumi/icons` re-exports a chosen subset of lucide as the base set.
- Where lucide has no icon, or its icon does not fit, Heartwood draws a custom Matumi icon. Both live in
  the same package and are used the same way, so an app never knows which is which.
- A product's own mark is not part of this set. Marks belong to the product, and Heartwood has its
  own (see [branding](branding.md)).

## Rules

- One outline style, drawn on a 24 px grid and used at 16, 20 and 24 px. Round caps and joins.
- Custom icons use the same grid, stroke weight and optical size as the lucide set, so the two cannot
  be told apart in a row.
- An icon never stands alone unless its meaning is universal and it has an accessible name. When in
  doubt, a word beats an icon.
- No icon is added for decoration. If a card, button or heading looks the same without it, remove it.
- Colour comes from the text colour of the context. An icon takes a status colour only when it is the
  one indicator of that status (see [patterns](patterns.md)).

## Still to define

- The stroke weight. lucide uses 2 px at 24 px. The design pass may propose 1.5 px, and the choice must be
  made once for the whole set.
- The list of icons that need a custom drawing.
- A naming scheme and a process for adding an icon (see [contributing](contributing.md)).
