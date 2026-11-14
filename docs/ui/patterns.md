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

- An empty state says what belongs here and offers the one action that fills it. No illustration is
  needed.
- Loading shows the shape of what is coming. A spinner is for waits the person caused.
- An error says what happened and what to do. It sits where the problem is, and it appears once.

## Confirmation

- Destructive actions name the thing and the consequence, and the button repeats the verb
  ("Delete device"), not "OK".
- Prefer undo to confirmation when the action can be reversed.

## Toasts

A toast reports the result of something the person did and is not visible on screen. It is never the
only place an error appears.
