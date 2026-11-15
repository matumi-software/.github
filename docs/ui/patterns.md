# Patterns

Status: the rules are set. Examples are added as screens are designed.

Each pattern follows the interface rules in the [README](README.md).

## One status, once

A status is shown in one place on a screen, by one element that carries a word, a glyph and a colour.
If the same fact is useful in a second place, the second place says something different, such as when
or why. A connected device does not get a badge, a green dot and a sentence.

## Forms

- Label above the field. The label says what the field is, and helper text adds only what the label
  cannot.
- Validate when the person leaves a field, and again on submit. Say what is wrong and how to fix it,
  next to the field.
- One primary action per form, and it says what it does ("Save changes", not "Submit").
- Settings that apply at once show it, and do not also have a save button.

## Tables and lists

- Columns are the facts the reader compares. A fact that is the same in every row is not a column.
- Sortable and selectable only where the reader can act on it.
- Row actions in one menu, not a row of icons.

## Empty, loading and error states

- A list with nothing in it is not a different screen. The page keeps its normal layout and its
  normal way to add: the list is empty, and the add form or editor is there. An info banner above it
  says what belongs here and what to do first. No custom "nothing yet" page, no illustration, and no
  extra step before the person can start.
- An empty list is not something that needs attention. It never goes in a "Needs you" tile or a
  count. Those hold only what is asking the person to act: a failure, a missing download, an invite
  waiting.
- A true empty state (the single message and action of an empty state component) is for a place with
  no way to add from it, such as a history that fills by itself.
- Loading shows the shape of what is coming. A spinner is for waits the person caused.
- An error says what happened and what to do. It sits where the problem is, and it appears once.

## Confirmation

- Destructive actions name the thing and the consequence, and the button repeats the verb
  ("Delete device"), not "OK".
- Prefer undo to confirmation when the action can be reversed.

## Toasts

A toast reports the result of something the person did and is not visible on screen. It is never the
only place an error appears.
