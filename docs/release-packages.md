# Installers and release packages

Status: standard set on 2026-11-14. Every product follows it, finished or not, so that every Matumi app
installs, upgrades, uninstalls and releases the same way.

## What a product ships

| Package | File name | For |
|---|---|---|
| Windows installer | `<Product>-<version>-setup.exe` | People. The Burn bundle with the shared WPF window and the MSI inside (see `docs/ui/installers.md`) |
| Windows MSI | `<Product>-<version>.msi` | Managed deployment: Group Policy and `msiexec`, no window |
| Linux package | `<product>_<version>_amd64.deb` and `<product>-<version>-linux-x86_64.tar.gz` | Where the product runs on Linux |
| Container image | `matumisoftware/<product>:<version>` | Server products |
| macOS image | `<Product>-<version>-macos-universal.dmg` | Where a product runs on a Mac |
| Checksums | `SHA256SUMS` | Lists every file above |

- A product with variants adds `-<variant>` before the extension, for example `Tweeling-0.1.0-setup-twin.exe`.
- Every package carries `THIRD-PARTY-NOTICES.txt` and the product's licence text.
- The release notes are the product's `CHANGELOG.md` section for the version.
- The version is read from the workspace (`[workspace.package] version` in `src/Cargo.toml`) and is the same
  in the file name, the file properties, `--version`, and the Apps entry.
- Packages that do not apply are not built. A Windows-only product has no Linux package.

## Layout in every product repo

```
src/installer/
  windows/
    build.ps1        the one entry point
    Package.wxs      the MSI
    Bundle.wxs       the bundle, including the shared bootstrapper
    setup.json       the product's name, mark, options and finish step, for the bootstrapper
  linux/             build.sh, build-deb.sh, install.sh and service files, where the product runs on Linux
  macos/             build.sh and check.sh, where the product runs on a Mac
  notices/           about.toml and new-notices.ps1: the third-party notices
docs/installers.md   what the installer does, its properties, and the install test
artifacts/           gitignored: artifacts/installer, artifacts/linux, artifacts/macos
```

Source marks and icons live in `resources/` and are copied at build time. Nothing under `artifacts/` is
committed.

## The build

- `build.ps1` takes the same parameters in every repo: `-Version` (default: the workspace version), `-OutDir`
  (default: `artifacts/installer`), `-SkipBuild` (stage from existing builds), and `-Sign` (use the signing hook).
- It finds the shared bootstrapper through the `HEARTWOOD_INSTALLER_DIR` environment variable, the folder
  Heartwood's `src/installer/bootstrapper/build.ps1` writes. No repo path is hard-coded.
- The shared steps (reading the version, staging checks, notices, `wix build` for the MSI and the bundle,
  checksums, the signing hook) live in Heartwood as one PowerShell module. A product's `build.ps1` stages its
  own files and calls it.
- The build stops with an error, not a warning, when a guard fails: a missing file, a file that must not be
  in a package, a checksum that does not match.
- Everything builds on a local machine. There are no CI workflows and no hosted runners. WiX stays on the free
  4.0.6 line.

## What every installer does

- **One identity.** A fixed UpgradeCode per product that never changes. A newer version replaces an older one,
  the same version replaces itself so a rebuild installs over the last, and an older version is refused with a
  plain message.
- **Closes the running app** before replacing files, and does not start it again by itself. The Finish screen
  offers to open it.
- **Apps entry.** One entry: name, publisher Matumi Software, version, icon, and links for About, Help and Update.
- **User data stays.** Uninstall removes the program, shortcuts and registry values and leaves the user's data,
  and the Uninstall screen says what stays and how to delete it.
- **Silent installs.** `Setup.exe /quiet`, and the MSI by itself, accept the same documented properties for every
  choice. Exit codes 0 and 3010 are documented.
- **Failure.** A failed install rolls back, says what went wrong and what to do, and shows the log path.
- **Scope.** Decided by what the product installs: per user where it installs only for the person, per machine
  where it needs Program Files, a service, a driver or a firewall rule.
- **Signing.** One signing hook, one sign tool name, `matumi`. Until a certificate exists the packages are unsigned
  and the docs say so.

## Install test

Each product's `docs/installers.md` ends with the same checklist, ticked on a real machine or in a virtual machine:

- install, files in place, one Apps entry, shortcuts, the app starts
- an upgrade over the previous version keeps the data
- repair, then uninstall: the program is gone and the data stays
- a silent install and a silent uninstall
- a failing install rolls back
- per-machine products: the same, elevated, in a virtual machine, with the owner clicking any UAC prompt

## Releasing

Packages are built locally. The owner tags the version `v<version>`, uploads the packages, `SHA256SUMS` and the
release notes to the GitHub release, and publishes any container image. Nothing is published by a script.

## The products

| Product | Windows | Scope | Other packages |
|---|---|---|---|
| Verteller | Installer and MSI | Per user | Linux package later |
| Koppel | Installer and MSI | Per machine (service, driver, firewall rules) | `.deb`, tarball, container image |
| Tweeling | Installer and MSI, free and twin variants | Per machine (Program Files, virtual camera) | None |
| Strook | Installer and MSI | Per machine (service, data folder) | Linux package and container image to decide |
| Grove | Installer and MSI | Per user (the `grove` command and its PATH entry) | The Linux release with `install.sh` |
| Skakel | None. It is a library | | Release package: the crates, the virtual camera files, notices and checksums |

Forester and Warden are not started and get an installer when they exist.

## Products with a private build

A product that is open source with a paid tier (see "Open core and private repos" in `repo-structure.md`)
ships its official packages from the private `<product>-pro` repo, with the same file names, the same
installer and the same checks as any other product. Verteller is the first.

- The private repo has its own `src/installer/windows/build.ps1` with the same parameters. It builds the
  private binary, then runs the open repo's staging and Heartwood's shared module over it, so nothing about
  the installer is copied.
- The open repo's `build.ps1` still builds the open (Free) binary, for people who build from source.
- The licence's public key is baked into the binary when it is built. A release-test package, made with a test
  key from the licence tools, says so in its file name suffix (`-test`) and is never published. An official
  package is made with the production key only.
- The notices cover every crate in the private build, not only the open ones.
