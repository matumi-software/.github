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

## Where it lives

Not decided. The installer is C# and WPF, a third language beside React and Rust, so by the repository
rules (a different language is a reason to split) it belongs in its own repository, shared by every
product, not in Heartwood. A product supplies its MSI, its mark, its name and its option text.

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
2. Decide the repository and name.
3. Build the tokens to XAML generator.
4. Prototype with Verteller, which has the most options, then Koppel, which tests elevation, the service
   and the driver.
5. Test in a virtual machine or a throwaway location, with the owner clicking any UAC prompt.
