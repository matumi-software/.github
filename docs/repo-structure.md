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

One repo per product, plus shared repos. Inside a repo, split into packages, not repos. A new repo
is justified only by a different language, licence or audience, or release cadence.

| Repo | Holds |
|---|---|
| `koppel`, `tweeling`, `verteller`, `strook` | One product each |
| `skakel` | The Rust SDK, used by Koppel and Tweeling |
| `wildwood` | Grove, Forester, Warden |
| `heartwood` | Shared TypeScript libraries, starting with the UI |

## Heartwood

```
heartwood/<branch>/
  docs/
  resources/       brand and gallery branding
  src/
    package.json
    pnpm-workspace.yaml
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
- A later shared library is a new folder under `packages/`.
- A product depends on a published version. It never copies the source into its own tree.
