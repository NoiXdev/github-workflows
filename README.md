# github-workflows

Repository containing our shared github workflows

## Workflow Documentation

#### [Create Compose Changelog + Version + Release](./.github/workflows/create_compose_changelog_version_release.yaml)

A Simple Workflow that generates a changelog, version and creates a release designed for php composer projects
See Bluprint for how to use it [here](./blueprints/release.yaml)

#### [Create NPM Changelog + Version + Release + Publish](./.github/workflows/create_npm_changelog_version_release_publish.yaml)

A Simple Workflow that generates a changelog, version and creates a release designed for npm projects
See Bluprint for how to use it [here](./blueprints/release-npm.yaml)

#### [Create Tauri Changelog + Version + Release](./.github/workflows/create_tauri_changelog_version_release.yaml)

A dispatch-driven release for Tauri desktop apps. Trigger it from the Actions tab and
pick `patch`, `minor` or `major`; it computes the next version, sets it across every
file (`package.json`, `tauri.conf.json`, `Cargo.toml` and the lockfiles via the
[`set-tauri-version`](./actions/set-tauri-version) composite action), generates the
changelog, commits + tags on `main`, and builds signed artifacts for macOS (universal),
Windows and Linux. Supports `prerelease` and `dry_run`. For a project with a special
macOS build (e.g. an embedded WidgetKit extension), pass `macos_build_script` pointing at
a repo script that builds/signs/notarizes and prints `DMG=<path>`.
See the blueprint for how to use it [here](./blueprints/release-tauri.yaml).

### Composite actions

- [`compute-version`](./actions/compute-version) — compute the next semver from a bump type + `package.json`, failing if the tag exists.
- [`set-tauri-version`](./actions/set-tauri-version) — set the version across `package.json`, `tauri.conf.json`, `Cargo.toml` and the lockfiles (no Rust toolchain needed).

## Important notes

- All workflows have to be in the `.github/workflows` directory
- Workflows should be build in a way that they are actually reusable! Too specific workflows should be kept in their
  according repositories directly!