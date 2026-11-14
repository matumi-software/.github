# Contributing to Heartwood

Status: the checks are set. Publishing and consuming are not decided yet.

## Adding or changing a component

1. Add or change its entry in [components](components.md) first. If it breaks an interface rule, fix
   the design, not the rule.
2. Build it in `src/packages/ui`, on react-aria-components, with tokens only.
3. Add a Storybook story for each variant and state.
4. Add a vitest test with an axe check for each variant, and a Playwright test in the gallery if it has
   keyboard, focus or theme behaviour worth pinning.
5. Run `pnpm build`, `pnpm typecheck`, `pnpm test`, `pnpm lint` and `pnpm e2e` from `src/`.
6. Check it in light and dark, and with the keyboard.
7. Open a pull request. PRs are rebase-merged.

## Adding an icon

Use a lucide icon when one fits. Draw a custom icon only when none does, on the same grid and stroke
(see [iconography](iconography.md)). Add it to `@matumi/icons` with a story.

## Versions and releases

- Each package is versioned on its own.
- Builds, tests and packaging run locally. There are no hosted runners.
- Not decided: where packages are published (the public npm registry, or a private feed), how a
  product's local build fetches them, and how changes are recorded in a changelog. Until this is
  decided, nothing is published.

## Licence

Contributions are under the repository's licence. See `LICENSE` and `TRADEMARKS.md` in Heartwood.
