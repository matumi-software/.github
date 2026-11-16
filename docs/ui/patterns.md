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
- A history that fills by itself, such as Activity, is the same: its normal page with no rows, and no
  banner, because there is nothing to tell the person to do.
- The empty state component is for a search with no matches, and for a failed load.
- Loading shows the shape of what is coming. A spinner is for waits the person caused.
- An error says what happened and what to do. It sits where the problem is, and it appears once.

## Confirmation

- Destructive actions name the thing and the consequence, and the button repeats the verb
  ("Delete device"), not "OK".
- Prefer undo to confirmation when the action can be reversed.

## Accounts and sign-in

Apps with accounts follow these rules. The shared pieces are `PeoplePage`, `AccountPage`,
`SetPasswordDialog` and `SignInPage` in Heartwood.

- **Codes are short-lived and shown on the host only.** An invitation or password reset code is
  one-time and lasts 1 minute. The countdown runs on the code, and when it runs out the person asks
  for a new one. The code appears only on the computer that runs the app, never on a remote screen:
  there Invite and Reset are off and a note says where to do them. The Invitation tile also gives the
  address the invited person opens, so the code does not arrive without a place to enter it.
- **A password is at least 10 characters, everywhere.** The sign-up with an invitation, a change, a
  reset and a first password all use the same minimum and the same sentence ("Use at least 10
  characters."). One app does not accept less on one screen than on another.
- **Forgot password? is quiet, and offered only where the app trusts the person without their
  current password.** That is the owner on the computer that runs the app, because anyone who can
  sit there can already do anything. It is a ghost button beside the change action on the account
  page, not a primary action, and it opens the same dialog without the current password. It is hidden
  on a remote screen and for a member, whose way back is a reset code from the owner. The help under
  Password says which applies to this person.
- **The signed-out form tells the owner the way in.** A short note under the form, such as "The
  owner can also sign in on studio-pc by running verteller open.", so the one person who can recover
  without a code is not left guessing. A member never needs it, and it takes no space of its own.
- **A change in flight shows on its row, not in a toast.** Removing a person or signing a device out
  waits for the app. The row says "Removing" or "Signing out" and its action is disabled. If the app
  refuses, the row says "Not removed: ..." or "Not signed out: ..." with the reason, until the next
  try. The row is where the person is looking, and it is gone or settled when the answer comes, so
  no toast is needed.

## Toasts

A toast reports the result of something the person did and is not visible on screen. It is never the
only place an error appears.
A change to a row in a list is not reported by a toast: the row shows it (see Accounts and sign-in).
