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
- An icon with a label, or an icon alone? See "Icon only" below.
- No icon is added for decoration. If a card, button or heading looks the same without it, remove it.
- Colour comes from the text colour of the context. An icon takes a status colour only when it is the
  one indicator of that status (see [patterns](patterns.md)).

## Icon only

Where an icon alone is clear, use it instead of an icon and a label. A label that repeats what the icon
already says is a repeat, and the interface rules say to remove it. An icon-only control always has a
tooltip, and it always has an accessible name.

Use an icon alone when all of these hold:

- The action is familiar from other software (close, search, copy, settings), or it is one of a row of
  actions that repeat, such as the actions on each row of a table.
- Space is limited, such as a toolbar, a table row or a collapsed sidebar.
- A wrong guess costs little and can be undone.

Keep the label (icon and label, or label alone) when:

- The action is destructive, irreversible or costly, or it is the primary action of the page.
- The icon is ambiguous, or means something different in different tools.
- It is a person's first contact with the feature, such as an empty state's single action.
- The same icon is used for two different actions in the app.

The tooltip is the explanation, in two lines at most:

1. The action's name, in the same words the control is announced with.
2. One plain sentence saying what it does or what will happen, and the shortcut where one exists.

Example: "Copy address" over "Copies the connection address to the clipboard", and the shortcut.
A tooltip never only repeats the name. If there is nothing to add, the control is simple enough that the
name is the whole tooltip, and that is allowed.

The tooltip rules, and how it behaves, are in [accessibility](accessibility.md).

## Still to define

- The stroke weight. lucide uses 2 px at 24 px. The design pass may propose 1.5 px, and the choice must be
  made once for the whole set.
- The list of icons that need a custom drawing.
- A naming scheme and a process for adding an icon (see [contributing](contributing.md)).
