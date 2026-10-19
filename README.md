# VeeLume CI

Reusable GitHub Actions workflows + project templates for VeeLume repos. Define once here, consume everywhere with a short caller.

## rust-ci

A reusable push/PR gate that mirrors `just check` + `just test` (fmt → clippy → optional frontend check → tests). Add a caller to any repo:

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push: { branches: [main] }
  pull_request:
jobs:
  ci:
    uses: VeeLume/ci/.github/workflows/rust-ci.yml@main
```

### Inputs

| Input | Default | Use |
|---|---|---|
| `os` | `ubuntu-latest` | runner; `windows-latest` only if tests must run on Windows |
| `toolchain` | `stable` | Rust toolchain |
| `frontend` | `false` | `true` for Tauri/JS repos → adds `pnpm install` + `pnpm check` |
| `clippy` | `true` | run `clippy -D warnings` |
| `windows-clippy` | `false` | `true` for repos with `cfg(windows)` code → parallel clippy-only job on `windows-latest` |
| `audit` | `false` | run `cargo-audit` |

Checks run on Linux because it is faster; release builds (Windows installers, Android) live in each repo's own publish workflow. On Linux, Tauri's system libraries are installed automatically when a `tauri.conf.json` is present. The Rust cache is saved only from the default branch; PRs restore it.

## templates/

Starter files to copy into a repo:

- `justfile` / `justfile.tauri` — common dev commands (`just --list`)
- `ci.yml` — the CI caller above
- `gitignore` — baseline ignores incl. common secret files; copy to `.gitignore` **before** the first commit
