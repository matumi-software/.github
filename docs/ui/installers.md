# Installers and setup branding

Status: proposed. Nothing here is built. It needs a prototype before any product commits to it.

The setup experience is the first screen a person sees from a Matumi app. It follows the same system
as the app: the same tokens, the same components, the same interface rules, and the product's own mark
and name.

## Goals

- Look and feel like the app, not like a generic Windows wizard.
- Say each thing once, and ask only what the person must decide.
- Keep what installers do well: rollback, repair, clean uninstall, Add or remove programs, and silent
  installs for scripts and managed machines.
- One way to build it for every product, so a new app gets a good installer without new design work.

## What exists today

Read from the repos on 2026-11-14.

| Where | How |
|---|---|
| Koppel, Windows | A WiX bundle (`Setup.exe`) around an MSI. The bundle uses WiX's standard bootstrapper UI with a custom theme file. The MSI installs the service, the USB driver, firewall rules and folder permissions |
| Verteller, Windows | One per-user MSI, with the standard WiX dialogs, custom text and two generated bitmaps (banner and dialog image) |
| Koppel, Linux | `.deb` and tarball built with nfpm, plus Docker images. No graphical installer |
| ClaudeP (a separate project), macOS | A disk image and an app bundle, built by script |

One more installer, in a product outside Matumi, uses a managed (WPF) interface inside the MSI. It
looks far better than a standard wizard. From its project file, it is built with WixSharp on
.NET Framework 4.7.2, with its own copy of the interface code. This is the reference for what
"custom" can look like. No code from it is used here.

## Why the current options fall short

These limits are read from how each approach is built and from WiX's documented behaviour. If your own
experience differs, correct this list.

| Approach | Limits |
|---|---|
| Standard MSI dialogs with bitmaps | Fixed dialog layouts, static pictures, no live styling, no animation. Always looks like a wizard |
| WiX standard bootstrapper with a theme file | Better, but still a fixed set of pages and controls described in XML. No shared components, no custom flow |
| Managed UI (WPF or WinForms) in the MSI | Fully custom, but Windows only and tied to .NET Framework. A second UI codebase that cannot reuse the React components, so it drifts from the app |

## Proposal: Matumi Setup

A small Rust program with the setup screens built in React from Heartwood, that installs through the
product's MSI.

```
Setup.exe  (Rust host, a webview, the React setup UI, the MSI as a payload)
  checks the machine and what is already installed
  shows the screens
  asks for administrator rights only when the install needs them
  runs the MSI quietly, and reads its progress through the Windows Installer external UI interface
  shows progress, a clear failure with the log, and the finish screen
```

- **The MSI stays the engine.** It still installs the files, the service, the driver, the firewall rules
  and the permissions, and it still rolls back on failure. A person or a script can still install the MSI
  directly, with no Setup.exe.
- **The React UI is shared.** The screens use `@matumi/ui` and `@matumi/tokens` through a new
  `@matumi/setup` package, so Setup matches the app in light and dark, with the product's mark in the
  shell's brand slot.
- **The host is a Rust crate** in Heartwood (`src/crates/setup-host`), shared by every product. A product
  supplies its MSI, its mark and its text, and does not write a host.
- **Add or remove programs** points at Setup.exe for uninstall, repair and modify, as a WiX bundle does.
  The MSI's own entry is hidden, so the person sees one entry and the custom screens each time.
- **Silent installs** keep working: `Setup.exe /quiet` and a documented set of properties give the same
  result as installing the MSI.

## Screens

The screens are set per product by its task, under the same rules as the app. Typical content:

| Screen | Content |
|---|---|
| Welcome | The product's name and mark, one sentence about what it does, and the one decision most people make (where it installs, if that is a real choice). It does not list what the installer will do |
| Choices | Only decisions that change the result, such as a component or a start-up behaviour, each with a plain default. A choice that has one sensible answer is not asked |
| Progress | One progress indication, and one line saying what is happening. No second percentage |
| Finish | What happened and the one next step, such as opening the app. A failure says what went wrong and how to fix it, with the log one click away |

Uninstall, repair and modify use the same shell and the same rules.

## Branding

Setup shows the product's own brand, not Heartwood's. Heartwood supplies the structure, the product
supplies identity.

| Thing | Where it comes from |
|---|---|
| Setup window and screens | `@matumi/setup`, in the product's mark, name and tokens |
| Installer and shortcut icons, Add or remove programs icon | The product's app icon, drawn to the family rules |
| Publisher shown by Windows | Matumi Software. Real publisher text on the UAC and SmartScreen prompts needs a code signing certificate |
| Add or remove programs entry | Name, publisher, version, icon, links, written once by the host from the product's details |
| Fallback MSI dialogs | Shown only if the MSI is run directly. Generated bitmaps from the tokens and the product mark, by one script, never drawn by hand |
| macOS disk image | A background picture and icon layout generated from the tokens and the product mark, so the drag to Applications is branded and clear |
| Linux packages | The desktop entry, the icon set and the package description, from the product's details. There is no graphical installer to brand |
| Docker image page and repository | The product mark and a plain description |

Art for the fallback dialogs and the disk image is generated by a script from the tokens, so it moves
with the tokens. It is not edited by hand.

## Risks and limits

- **The unsigned warning does not go away.** A custom screen does not change what Windows shows before
  the program runs. Only code signing does. Today the installers are unsigned.
- **The webview.** Setup needs the Microsoft WebView2 runtime. It is built into Windows 11. On Windows 10
  it is usually present but not guaranteed. This is from my knowledge of Microsoft's distribution and needs
  checking. Setup must fall back to a plain native window with the same choices when it is missing, and
  must never fail to install because of it.
- **More to maintain.** Setup replaces what the WiX bundle does for free: the screen flow, the
  elevation handling and the error reporting. The host has to be small and well tested.
- **Chaining.** A WiX bundle can install several packages in order with rollback. Setup installs one MSI.
  Koppel's bundle chains the Visual C++ runtime only because of third-party driver files. The runtime can
  be carried with the product instead, as Verteller does.
- **Security.** An installer is a trusted program. It must verify the payload it runs, run elevated only
  for the MSI, and never run downloaded code without a check.
- **Accessibility.** The setup screens must meet the same rules as the app, and be tested with the
  keyboard and a screen reader.
- **Linux and macOS.** This proposal covers Windows. The other platforms keep native packages, branded as
  in the table above.

## Prototype and acceptance

1. Build the host and one screen flow for a product with no service or driver, such as Verteller or
   Tweeling, per user and with no administrator prompt.
2. It passes when:
   - an install, upgrade, repair and uninstall all work, and a failed install rolls back cleanly
   - `Setup.exe /quiet` and the plain MSI give the same result
   - Add or remove programs shows one entry with the right icon and details
   - it works with the keyboard and a screen reader, and in light and dark
   - it degrades without WebView2
3. Then Koppel, which tests the hard part: the service, the driver, the firewall rules, and a reboot
   that the driver may ask for.
4. Tests run in a virtual machine under `D:\testing`, per the testing rules. Installs wait for the
   owner's UAC click where an elevation prompt appears.

## Still to decide

- Whether this is the plan, or whether a themed WiX bundle is enough for products without a service.
- Whether the screens are authored in the setup package or taken from the product's app screens.
- Code signing: certificate, cost and who holds it.
- The macOS disk image layout and the Linux story.
