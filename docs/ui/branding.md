# Branding and the product layer

Status: layers set. Heartwood's own brand is still to design.

Most Matumi apps already have some branding, and Heartwood must work with it, not replace it. This page
says what belongs to whom.

## Three layers

| Layer | Owner | Holds |
|---|---|---|
| Matumi | `.github`, `resources/brand` | The Matumi rings mark, the family rules, the base colours and type, the organisation avatar |
| Heartwood | Heartwood repo | Tokens, components, shell and icons, plus Heartwood's own brand: its mark, wordmark and gallery identity |
| Product | Each product repo | The product mark and app icon, extra tokens the product needs, its list of state words, its copy rules, and components only that product uses |

A lower layer never overrides a higher one. A product adds tokens and never changes a Heartwood token.

## What exists today

Read from the repos on 2026-11-14. These vary a lot, and Heartwood must absorb each one.

| Product | Branding in its repo |
|---|---|
| Koppel | A full design system in `resources/design-system`: its own tokens, components, marks, and written rules. It has product state colours (shared, attached, pending, fault), fixed state words, and a pending stripe that means only "waiting for the server" |
| Skakel | Marks and assets in `resources/design-system/assets` |
| Tweeling | Tokens and assets in `resources/design-system` |
| Verteller | A mark and app icon, installer images in `resources/brand`, and a shell stylesheet in `resources/design-system` |
| Wildwood | Marks for Wildwood and Grove in `resources/design-system/assets` |
| Strook | No brand folder yet |

## Product marks

- Each product keeps its own mark and its own app icon, drawn to the family rules in
  `resources/brand/README.md`: a 64 grid, stroke 5, round caps, exactly one canopy element, and the app
  icon as the mark in paper on a canopy tile.
- The shell's brand slot shows the product's mark and name. An app never shows Heartwood's mark in its
  own interface.

## Product extensions

- A product may add tokens, such as Koppel's device state colours. They live in the product repo, are
  named with the product's name (for example `--koppel-state-attached`), and refer to Heartwood tokens
  where they can.
- A product token that repeats a Heartwood token is removed when the product adopts Heartwood. Where the
  values differ, the difference is settled once, in Heartwood, with the product's owner.
- Product state words and copy rules stay with the product. Heartwood supplies the components that show
  a state, not the words.
- A product component stays in the product until a second product needs it. Then it is proposed to
  Heartwood.

## Adopting Heartwood in an existing app

1. Replace the product's copies of base tokens and primitives with `@matumi/tokens` and `@matumi/ui`.
2. Keep the product mark, app icon, extension tokens, state words and product components.
3. Move the product's remaining design notes into its `resources/design-system`, trimmed to what is
   specific to it.
4. Check each screen against the interface rules, in light and dark.

## Heartwood's own brand

Heartwood is public, so it needs an identity of its own, separate from every product and from Matumi.

- A mark and a lowercase wordmark. Proposed: drawn to the same family rules as the other marks, so
  Heartwood sits with them. The name suggests the dense core of a tree.
- Used for the repository, the gallery header, the documentation and the social preview. Never inside
  a product.
- Until it is designed, the gallery uses a placeholder mark. The placeholder is not published in any
  package (see `TRADEMARKS.md` in Heartwood).
- It is designed in the same pass as the system, and recorded in Heartwood's `resources/brand`.

## Still to define

- The final Heartwood mark and wordmark.
- Whether the Matumi organisation mark appears on Heartwood's gallery, and where.
