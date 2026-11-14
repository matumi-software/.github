# Installers and setup branding

Status: direction decided on 2026-11-14. Still to do: the tokens to XAML step, a home for the code, and the
design pass for the screens.

## Decision

Every Matumi Windows installer is a WiX Burn bundle, one `.exe`, with a custom WPF managed bootstrapper
application, wrapping the product's MSI. This is the same approach as the Timebase installer, with the
Matumi look. It replaces the earlier proposals: Matumi Setup in a webview, Velopack, and a Rust launcher.

Why: of the options tried, the WPF window gave the best look, it is the only one that can show real
pages with choices as well as a splash, and the MSI underneath keeps rollback, repair, services, drivers
and firewall rules. Installers are built on Windows machines, so a Windows-only build is fine.

## What was tried (2026-11-14)

Spikes in scratch folders, installs only in throwaway locations, no real product touched.

| Option | Result |
|---|---|
| Themed WiX standard bundle (Koppel today) | Background image and light or dark switching work. Buttons and the progress bar stay standard Windows controls. How text looks over an image was not captured |
| Velopack Setup (Verteller) | Ran end to end: install, update in place, uninstall, data kept. A splash image and a progress bar, no pages. Per user only |
| Velopack MSI (per machine) | Hooks run elevated and can create a service and a firewall rule, but a failing hook is reported as success, a hook is cut off at 30 s, and the MSI uses standard dialogs. Not usable for service apps |
| Rust splash launcher around an MSI | Not finished. The spike was stopped |
| **WPF managed bootstrapper in a Burn bundle** | Built and run. Chosen by the owner as the best look |

The Velopack branch for Verteller (`feat/RVR/velopack-installer`) is parked and not merged.

## How it works

- **The file.** The product ships one `.exe`, the Burn bundle. It holds the WPF window and the MSI. A plain
  MSI is still built from the same source for managed deployment, where an administrator installs it with
  `msiexec` or Group Policy and sees no window.
- **The window.** A WPF application on .NET Framework 4.7.2, built on the `WixToolset.Mba.Core` package
  (4.0.6) and loaded by the bundle through WiX's managed bootstrapper host. It listens to the engine's
  events and draws everything itself.
- **The chain.** A .NET Framework 4.7.2 prerequisite, then the MSI. Windows 10 from version 1803 and
  Windows 11 include the framework, so the prerequisite only downloads it on very old Windows 10 builds.
- **The MSI stays the engine.** It installs the files, services, drivers, firewall rules and permissions
  and rolls back on failure. Koppel's MSI does not change.
- **Add or remove programs.** One entry, from the bundle. Uninstall, repair and modify run the same window.
- **Silent installs.** `Setup.exe /quiet` runs the bundle with no window and the same properties as today.
- **Versions.** WiX stays on the free 4.0.6 line. The paid Open Source Maintenance Fee applies to the official
  WiX 6 and 7 binaries (read in Timebase's build notes and in Velopack's documentation).

## Screens

The design pass designs the full set. Which of these a product shows depends on the product.

| Screen | Content |
|---|---|
| Welcome | The product's name and mark, one sentence, and the next step |
| Licence | Only where a licence must be accepted |
| Install location | Only where the choice is real |
| Options | The product's own choices, such as the graphics card for Verteller or services for Koppel |
| Prerequisites | What will be installed first and why |
| Progress | One progress indication and one line saying what is happening |
| Finish | What happened and the one next step, such as opening the app |
| Failure | What went wrong, what to do, and the log one click away |
| Upgrade | What changes when a newer version replaces an installed one |
| Maintenance | Repair, change and uninstall for an installed product |
| Uninstall | The same window, saying what stays, such as the user's data |

The interface rules apply. Ask only for decisions that change the result, with a plain default. Each
option is asked once: where an app has the same choice in its own Setup page, the installer or the app
shows it, not both. That is decided per product.

## What a product tells the window

A product gives the bootstrapper a `setup.json`. The contract is documented in Heartwood
(`src/installer/bootstrapper/README.md`). It is validated when the product builds, so a bad file fails the build
and never a person's install. Unknown fields are errors. These parts matter for the design:

| Part | What it does |
|---|---|
| `tagline`, `welcomeNote` | A short heading line, and an optional note under it that says what is installed |
| `options` | `bool`, `choice`, `folder`, `number` and `text`, each with a plain label, optional help, a default and the property it sets |
| `licence` | Optional, with an optional acceptance checkbox that the primary button waits for |
| `launch` | What the Finish screen offers to open: the target (or one read from the registry, so an existing install in another folder opens correctly), an optional label, and which kinds of run offer it (default: all but uninstall) |
| `finishNotes` | Ordered notes on the Finish screen, per kind of run, optionally only when a registry value or a variable meets a condition (for example a note about a failed optional step) |
| `restartNote` | Words for the Restart screen, such as why a driver needs a restart |
| `uninstallOption` | An optional checkbox under Uninstall, off by default and never shown on an upgrade, such as "Also delete my settings" |
| `uninstallNote` | What stays after an uninstall |

Text in the notes can use bundle variables (`[VAR]`), environment variables (`%APPDATA%`, and `%NAME|fallback%` when a
variable may be unset) and, in a finish note, `{value}` for the value its condition read. The copy rules apply to all of
it: nothing repeated on a screen and the product name only in the header. The screenshot harness checks that.

The command line accepts any variable the bundle declares as overridable, for example `Setup.exe /quiet GPU=cpu`. Burn,
WiX and Windows built-ins are refused.

The primary button reads Install on the last page before Progress, Next before that, and Update, Repair, Change, Apply or
Uninstall for those runs. A product that has an uninstall option opens Maintenance with Uninstall preselected when it is
started from Apps, so the option is seen.

## Build

- One Apps entry. The bundle's MSI package sets `ARPSYSTEMCOMPONENT=1`, and the build checks the built bundle.
- The module validates the MSI (`wix msi validate`) with a list of suppressed rules and fails on any other warning.
- Products can add WiX extensions (4.0.6 only) and name their main exe so a shortcut can refer to it.
- WiX's temporary files go next to the output folder, never into the user's temp folder on C:.
- Burn passes `BURNMSIINSTALL=1` to an MSI it installs, so an MSI can tell a bundle run from a plain run.

## Layout

The window has one fixed layout, so nothing moves between screens and a person can click without looking.

- **One window size** for every screen of a product, not resizable. The design pass sets the size, and every
  product uses the same one.
- **Three fixed zones.** A header (the product's mark, its name and where you are), a content area, and a footer.
  Each keeps the same position and size on every screen.
- **Fixed button positions.** The footer holds every button, in the same places on every screen: Cancel on the
  left, and on the right Back, then the primary action at the far right. The primary button's label changes
  (Install, Next, Finish, Repair) and its position does not. A button that does not apply on a screen leaves its
  place empty, so the others never shift.
- **The content area never changes the window.** Long content, such as a long option list or a licence, scrolls
  inside the content area. The window does not grow.
- **The status line and the progress bar** sit in the same place on every screen that shows them.
- **Keyboard.** Tab goes through the content, then the footer. Enter presses the primary action and Escape
  asks to cancel. The focus ring is always visible.
- **Checked by a test.** The screenshot harness measures every button and zone on every screen and fails when a
  position or size differs from the one on the Welcome screen.

## Branding

Setup shows the Matumi brand as the backdrop and the product in front.

| Thing | Where it comes from |
|---|---|
| Window background | A Matumi splash: the rings pattern on the base colour, generated from the tokens |
| Product identity | The product's mark and name in the foreground, from the product repo. Never Heartwood's |
| Colours, type, spacing | Generated from `@matumi/tokens` into a XAML resource dictionary by a script, so the installer moves with the tokens. Light or dark follows the system. The colour vision mode applies too |
| Fonts | Instrument Sans and JetBrains Mono carried inside the application. Not verified |
| Icons | The product's app icon for the bundle, the shortcuts and Add or remove programs |
| Publisher | Matumi Software. Windows shows it on the prompts only for a signed program |
| Fallback MSI dialogs | Shown only when the plain MSI is run with its window. Bitmaps generated from the tokens |
| macOS | A disk image with a generated background and a clear drag to Applications |
| Linux | Package metadata, the desktop entry and the icon set. No graphical installer |

Every product follows the same package names, repository layout and build interface, set in
`docs/release-packages.md`.

## Where it lives

In Heartwood, under `src/installer`, beside the tokens it is generated from. It is shared code, and shared
code lives in Heartwood whatever its language. The repo then holds React, Rust and C#.

| Common, in Heartwood | Per product repo |
|---|---|
| The WPF bootstrapper application | The MSI authoring: files, services, drivers, firewall rules |
| The theme generated from the tokens, and the generator | The mark, the name and the app icon |
| The screens and their behaviour | The product's option text and which screens it shows |

A product writes no C#. It builds its MSI and hands the common bundle its mark, name and options.
How a product's build gets the common bundle (a local NuGet feed, for example) is not decided.

What would move it out of Heartwood: a different licence, a different release cadence, or a second
maintainer who works only on it.

## Risks and things not yet verified

- **Quiet uninstall hung.** During cleanup the bundle's own `/uninstall /quiet` run did not finish. It may
  be a result of stopping the spike mid-run, or a bug in the uninstall path with no window. Investigate
  before shipping.
- **Elevation.** The spike ran per user. Per-machine products such as Koppel need the bundle to elevate,
  and that was not tested.
- **Accessibility.** UI Automation, keyboard and screen reader behaviour of the WPF window are untested.
- **Styles drift.** WPF cannot use Heartwood components. The token generation step is what prevents drift,
  and it is not built.
- **Unsigned.** Windows warns before an unsigned program runs. Signing is separate from this choice.
- **Localisation and high contrast.** Not examined.

## Next steps

1. The design pass produces the screens with all options, in light, dark and colour vision mode.
2. Create `src/installer` in Heartwood and decide how products consume it.
3. Build the tokens to XAML generator.
4. Prototype with Verteller, which has the most options, then Koppel, which tests elevation, the service
   and the driver.
5. Test in a virtual machine or a throwaway location, with the owner clicking any UAC prompt.
