# Accessibility

Status: the contrast and test rules are set. The size rules are proposed.

## Rules

- Body text and supporting text reach 4.5:1 on every surface they sit on, in both themes. The tokens
  record which surfaces each text token is checked on.
- Controls, borders that define a control, and the focus ring reach 3:1. `border-default` is
  decorative and is never the only edge of a control.
- Every interactive element has a visible focus ring from `focus-ring`, shown for keyboard focus.
- Every component works with the keyboard alone, with the standard keys for its role. Behaviour comes
  from react-aria-components and is not reimplemented.
- Colour is never the only signal. A status is one element that carries a word, a glyph and a colour
  together, so it passes without colour and still appears once.
- Proposed: a target is at least 32 px high, and 40 px for a primary action.
- Motion respects the reduced-motion setting.

## Icon-only controls and tooltips

- An icon-only control has an accessible name (a label for screen readers) that is the same text as the
  first line of its tooltip. The tooltip is never the only place the name exists.
- The tooltip shows on hover and on keyboard focus, after a short delay, and without the delay when
  moving between neighbouring controls in the same group.
- It meets the WCAG 2.1 rule for content on hover or focus: it can be dismissed with Escape without moving
  focus, the pointer can move onto it without it vanishing, and it stays until dismissed, moved away from or
  no longer valid.
- It holds no information that is needed to complete a task, no links and no controls. Anything essential
  goes in the interface itself, because tooltips do not appear on touch and are easy to miss.
- It is short, in plain text, readable in both themes and in the colour vision mode, and it does not cover
  the control that opened it.
- Target size still applies: an icon-only control is not smaller than any other control.

## Colour vision mode

Every app has a colour vision mode, a setting that anyone can turn on. It is for people with colour
blindness (protanopia, deuteranopia, tritanopia, achromatopsia) and low colour contrast sensitivity.

- **It is a setting, not a theme.** It is a third choice beside light and dark, set with the
  `data-vision` attribute on the page, and every combination works: light or dark, with or without it.
- **What it changes:**
  - Status, chart and state colours switch to a second set chosen to stay apart under each type of
    colour vision, not only for typical vision.
  - Each status keeps its glyph and word, and the glyphs are made more different in shape. Chart series
    also differ by marker, dash or pattern, never by colour alone.
  - Contrast is raised a step on borders and the focus ring.
- **What it does not do:** it does not rely on the operating system. Windows colour filters change the
  whole screen after the app has drawn it, and apps cannot read them, so the app has its own setting.
  The app also follows `forced-colors` (Windows contrast themes) and `prefers-contrast: more`, which are
  separate and respected.
- **Where it lives:** in the shell's account and theme area, saved per person, and honoured by the
  setup screens too.
- **Products supply a second set.** A product that adds state colours, such as Koppel's, must also give
  a colour vision set for them. A product cannot ship an extension colour without one.
- **The second set is designed, not computed.** It comes from the design pass, and starts from a
  colour-blind safe palette. It must keep the brand: the accent stays canopy green where it is a brand
  colour and not a status.

### Testing it

- Chromium can emulate colour vision deficiencies, through the browser's own debugging protocol, with
  protanopia, deuteranopia, tritanopia and achromatopsia. The Playwright tests in the gallery run each
  status, chart and state story under each, with and without the mode, and keep screenshots for review.
- A palette check measures the distance between each pair of status colours after simulating each type of
  colour vision. The minimum distance is still to set, and a pair below it fails the build.
- Emulation approximates real vision. A person with colour blindness should review the real screens
  before release, where one can.

## How it is tested

- Each component has a vitest test with a vitest-axe check, in every variant.
- Storybook's accessibility check runs on every story and a violation fails it.
- Playwright tests in the gallery cover focus, keyboard and theme behaviour.
- A person checks the keyboard path and a screen reader on each new component before release.
  Automated checks find only part of the problems.
