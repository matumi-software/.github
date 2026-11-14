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

## How it is tested

- Each component has a vitest test with a vitest-axe check, in every variant.
- Storybook's accessibility check runs on every story and a violation fails it.
- Playwright tests in the gallery cover focus, keyboard and theme behaviour.
- A person checks the keyboard path and a screen reader on each new component before release.
  Automated checks find only part of the problems.
