# Repository structure

How every Matumi Software code repository is laid out, so that any repo feels familiar on first
open. This describes what the existing repos already do; it adds no new rules beyond naming them.

Scope: every repository that holds code. This `.github` repo and notes repos such as `app-ideas`
are exempt.

## On disk

Every repo is a bare clone under `D:\src\<org>\<repo>\`, with one worktree per branch in a
subfolder named after the branch. The repo root carries a `CLAUDE.md` with the worktree rules.
That file is the same in every repo, so it is not repeated here.

## Inside a worktree

```
<worktree>/
  README.md        what it is, install, quick start
  LICENSE
  CHANGELOG.md
  SECURITY.md      public repos
  CLAUDE.md        repo-specific rules and commands (the layout table lives here)
  .gitignore
  .gitattributes
  .editorconfig    optional, or in src/
  .github/         issue and PR templates, dependabot. No CI workflows (see below)
  docs/            architecture, build, operations, decisions, user docs
  resources/       brand/ and design-system/: logos, icons, social previews, sample data
  src/             all code, and the workspace root
  artifacts/       packaging output (installers, tarballs). Gitignored
```

## Rules

- The repo root holds only the files above. Nothing is built from outside `src/`.
- Workspace config sits in `src/`: `Cargo.toml` and `Cargo.lock` for Rust, `package.json` and
  `pnpm-workspace.yaml` for TypeScript. A repo that mixes both has both there.
- Build output (`target/`, `dist/`, `node_modules/`, `artifacts/`) is gitignored.
- Each unit of code is a folder directly under `src/`, or under `src/packages` and `src/apps` when
  a repo holds several of each. One crate or package per folder.
- Source images and brand files live in `resources/`, never in `src/`. Code that needs one copies
  it at build time.
- Apps with a browser or webview UI carry Playwright as a dev dependency, in `src/`, for AI-assisted
  design review and fault finding. It is dev only and never ships in a published package.
- Builds, tests and packaging run on local machines. No CI workflows and no hosted runners.
- Prose in docs, comments and commit messages uses no em dashes or en dashes.
- The repo's own `CLAUDE.md` carries what is specific to it: the layout table, the rules that
  differ, and the commands to build and check.

## Where a repo splits

One repo per product, plus shared repos. Inside a repo, split into packages, not repos. Code shared by
the products, such as the UI and the Windows installer, lives in Heartwood whatever its language. A new
repo is justified by a different licence, audience or release cadence, such as Skakel, which is a
product with its own users.

| Repo | Holds |
|---|---|
| `koppel`, `tweeling`, `verteller`, `strook` | One product each |
| `skakel` | The Rust SDK, used by Koppel and Tweeling |
| `wildwood` | Grove, Forester, Warden |
| `heartwood` | Shared libraries: the React UI, Rust crates when they arrive, and the C# installer used by every Windows app |
| `licensing` | Private. The licence check library (`matumi-licensing`) and the licence tools, shared by every app with a paid tier |
| `<product>-pro` | Private. The official build of an open source product, with its paid features and the licence check. Today `verteller-pro` |

## Open core and private repos

A product that is open source with a paid tier is two repos: the open one, which is the product, and a
private `<product>-pro` that wraps it. Both follow the layout above. These rules apply on top:

- **The open repo never names the private one.** It defines hooks (traits and extension points) and its own
  free answers, and builds and runs with no access to anything private. The private repo depends on the open
  repo by a pinned commit, never the other way round.
- **The free tier is real.** What a person gets without the private build is stated in the open repo's README
  and works with no account and no network call to us.
- **The edition is a licence, never a flag.** No setting, environment variable or file turns a paid feature
  on in a release build except a signed licence that verifies against a public key baked in when the build was
  made. A bad or missing licence leaves the person on the free tier, says why, and never stops the app.
- **Keys and tokens stay out of every repository.** A signing key lives in the owner's own data folder, or on
  the licence service. A test key may be used only for a release test and a build made with one is never
  shipped. The debug-only dev licence never compiles into a release build.
- **Official installers are built from the private repo.** The open repo's `build.ps1` builds the open
  binary. The private repo's installer uses the open repo's staging and Heartwood's shared module, so the
  packages are the ones in `docs/release-packages.md`, with the private binary inside.
- **Private repos use the same layout, brand and docs standard.** Each has a README with the product's mark,
  a `LICENSE` that says it is proprietary, a `CHANGELOG.md`, a `CLAUDE.md` with its layout and rules, and
  a `docs/` folder. The marks are the open product's, copied into `resources/brand`.
- **A private repo with a UI uses Heartwood** like any other app. `licensing` is the example: its studio page
  is built on `@heartwood/ui`.
- Non-commercial models and other third-party licence limits are enforced in the open core, and the private
  repo never adds a way round them.

## Heartwood

```
heartwood/<branch>/
  docs/
  resources/       brand and gallery branding
  src/
    package.json
    pnpm-workspace.yaml
    Cargo.toml     once the first shared crate arrives
    crates/        shared Rust crates
    installer/     C# and WPF: the setup window, its generated theme, the tokens to XAML generator
    packages/
      tokens/      colours, type, spacing, radii, motion as CSS variables
      icons/
      ui/          React components
      shell/       app shell, navigation, window chrome
    apps/
      gallery/     Storybook, browse every component; not published
```

- Packages publish as `@matumi/<name>`: `@matumi/tokens`, `@matumi/icons`, `@matumi/ui`,
  `@matumi/shell`. Each is versioned on its own.
- A later shared TypeScript library is a new folder under `packages/`, a Rust one under `crates/`.
- The installer is built as a versioned package that each product's build uses. A product supplies its
  MSI authoring, its mark, its name and its option text, and writes no C#.
- A product depends on a published version. It never copies the source into its own tree.
