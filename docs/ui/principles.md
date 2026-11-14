# Principles

Status: set. The interface rules in the [README](README.md) are part of this page.

## Character

- Softer, calmer and more modern than a typical admin tool. Quiet, rounded, generous spacing.
- Dark first, with a full light theme. Both are first-class, and a screen is checked in both.
- Warm stone neutrals, never blue-black. One accent, canopy green.
- Plain voice: sentence case, no exclamation marks, no emoji, no cheerleading.
- The Matumi idea: growth rings around a canopy core, a riverbank hardwood that holds firm in fast
  water. Quiet strength, not decoration.

## Ambition and ease of use

Every screen starts from its task, not from a default layout. Ambitious where it makes the task
clearer or more enjoyable, conventional where familiarity helps. The two can reinforce each other or
pull apart, and neither always wins. A stranger should be able to start the core task without
instructions.

Where the ambition lives:

- Components are predictable primitives. A button, an input and a menu behave as people expect, with
  standard keyboard and focus behaviour.
- Screen compositions and the app shell are where an app can be distinctive. Heartwood supports
  unusual layouts by keeping components independent of where they are placed.

## Density

Comfortable by default. A compact mode exists for tables and dense panels. Density is a setting of
the screen, not a different set of components.

## Motion

Motion explains a change (something arrived, moved or went away). It is short, never decorative, and
off when the person asks for reduced motion. Durations and easings are still to define in
[tokens](tokens.md).

## What we avoid

The list in the README under "It must not look generated", plus: modal chains, tooltips that carry
essential information (a tooltip explains, it never holds what the person needs to finish a task), colour as the only signal, and anything that looks interactive and is not.

## Writing

Use the user's words, not the system's. Say what happened and what to do next. Name a state with one
fixed word and use that word everywhere. Each product keeps its own list of state words in its own
design notes.
