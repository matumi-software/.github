# App shell

Status: structure set. Layout details wait for the design pass.

`@matumi/shell` provides the frame every app sits in. It is not a fixed layout. It provides the parts
and the behaviour, and an app arranges them to fit its task.

## Parts

| Part | Job |
|---|---|
| Navigation | Moves between the app's areas. A sidebar by default, expanded or collapsed. An app may use another form when its task calls for it |
| Page header | Says where you are, with the one primary action for the page. It does not repeat the navigation label |
| Content area | Holds the page. Scrolls on its own |
| Window chrome | The title bar and controls of a desktop window, matching the theme |
| Tray window | A small popover from the system tray: one task, no navigation |
| Brand slot | Shows the product's own mark and name, never Heartwood's (see [branding](branding.md)) |
| Account and theme | Where they exist: the theme switch, the colour vision mode, and the account or connection area |

## Rules

- The shell never decides colour, type or spacing. It reads tokens.
- The navigation shows each area once. The current area is marked once.
- The shell works at a narrow width, because a desktop app can be resized small and the same UI can
  be served to a browser.
- State that belongs to the whole app (a lost connection, for example) appears once, in the shell, and
  not again on each page.

## Still to define

The sidebar widths, the collapse behaviour, the header height and the tray window size. These come from
the design pass.
