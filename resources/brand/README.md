# Matumi Software brand

The organisation uses the Matumi brand: growth rings around a canopy core, after the matumi, a
riverbank hardwood that holds firm in fast water. Canopy green on warm stone, dark first.
Wordmarks are always lowercase (matumi, wildwood, grove, koppel, skakel, tweeling); running text
keeps the capitals.

## The family

Every Matumi Software mark follows the same rules:

- Drawn on a 64 grid at stroke 5 with round caps.
- Exactly one canopy element; the rest is ink on light grounds and stone on dark grounds.
- The app icon is the whole mark in paper `#FBF9F4` on a canopy `#2A6B4E` tile with a 14/64 corner
  radius, the mark at 0.66 of the tile.
- Clear space is half the mark's width on every side. No mark is drawn below 16 px.

Each product keeps its own marks in its repo under `resources/design-system`: Wildwood and Grove
in `wildwood`, then `koppel`, `skakel` and `tweeling`. Koppel's K keeps its original heavy
construction, with only the circle in canopy.

## Files

- `assets/Org/matumi-software-avatar.png` (500x500, from the SVG next to it): the organisation
  avatar on GitHub and the company icon elsewhere. The mark in stone `#EDEAE0` and canopy `#549572`
  on char `#161512`. It is full bleed, and the mark stays well inside the inscribed circle so a
  round crop does not clip it.
- `assets/Logos/matumi-mark.svg`, `matumi-mark-dark.svg`, `matumi-mark-mono.svg`: the mark for light
  grounds, dark grounds and single colour.
- `assets/Logos/matumi-wordmark.svg` and `matumi-lockup-horizontal.svg`: "matumi" in Instrument Sans
  600. They load the font from the web, so convert the text to outlines before using them outside a
  browser.
- `tokens.json`: the colour, type, spacing, radius and shadow tokens every product starts from,
  with light and dark values and what each is for. A product adds its own tokens on top, such as
  Koppel's device states.

## Colour and type

| Token | Light | Dark | Use |
|---|---|---|---|
| canopy (`accent`) | `#2A6B4E` | `#549572` | Each mark's canopy element and the one accent |
| river | `#2F6F74` | `#5FA0A6` | A second colour in graphics and state, never an accent |
| page (`bg-page`) | `#FBF9F4` | `#161512` | Backgrounds, never blue-black |
| ink (`text-primary`) | `#1C1B18` | `#EDEAE0` | Text and the rest of each mark |

Instrument Sans for wordmarks (600) and text, JetBrains Mono for code, paths and identifiers.
Sentence case, no exclamation marks, no emoji.
