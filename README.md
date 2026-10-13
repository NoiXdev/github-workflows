# github-workflows

Repository containing our shared github workflows

## Releasing a Go product

`create_go_changelog_version_release.yaml` is the Go counterpart to the
compose one, and deliberately shorter: a Go binary takes its version from
the tag at build time (`-ldflags "-X main.version=..."`), so there is no
file holding a number that has to be kept in step. The workflow therefore
skips the version file entirely — bumping one nothing reads would add a
commit per release and nothing else. A private, unpublished
`web/package.json` is the usual trap here: its `version` field is
decoration, and this workflow leaves it alone.

`blueprints/release-go.yaml` triggers on CI *completing* on the default
branch rather than on a push or a tag. That is what makes "only the
default branch" and "only a green commit" true by construction instead of
by a check someone has to write and keep correct.

## Pinned actions

Every third-party action is pinned to a full commit SHA, with the version
it was at in a trailing comment. A moving tag — `@v4`, or worse `@stable`
— means whatever its owner points it at next runs here, and these
workflows cut releases and push to registries on behalf of every NoiX
product: a compromise of any one action would reach all of them at once.

Re-verified 2026-10-09 against `gh api repos/<owner>/<repo>/git/ref/tags/<tag>`
(annotated tags dereferenced to their commit), with the concrete version
read back from each repository's tag list.

**The pinned SHA is what the *referenced* tag pointed at, not the newest
release.** Several of these majors are well behind — `actions/checkout@v4`
against a v7 latest, `action-gh-release@v2` against v3 — so pinning to
"latest" would be a silent multi-major upgrade wearing the clothes of a
lockdown. Upgrading is its own decision, made deliberately.

The three `NoiXdev/github-workflows/actions/...@main` self-references are
deliberately still floating: a repository pinning itself turns every
change into a two-step dance, and the sensible answer is a release tag
for this repository — a decision for its owner, not one to invent here.

## Workflow Documentation

#### [Create Compose Changelog + Version + Release](./.github/workflows/create_compose_changelog_version_release.yaml)

A Simple Workflow that generates a changelog, version and creates a release designed for php composer projects
See Bluprint for how to use it [here](./blueprints/release.yaml)

#### [Create Docker Changelog + Version + Release](./.github/workflows/create_docker_changelog_version_release.yaml)

A dispatch-driven release for a product that ships a container image. It
computes the next version, writes it into the tree, builds and pushes the
image to Docker Hub, commits, tags, creates the release and announces it.
Supports `prerelease_id`, `dry_run` and multi-architecture builds through
`platforms`.

Docker Hub is addressed through `vars.DOCKERHUB_NAMESPACE` /
`vars.DOCKERHUB_IMAGE` and `secrets.DOCKERHUB_USERNAME` /
`secrets.DOCKERHUB_TOKEN`, as the per-repository release workflows it
generalises already do.

A product whose version lives somewhere other than a JSON manifest — a
`pyproject.toml`, a compose default, a shell script — points `version_script`
at an executable in its own repository, invoked as `./<script> <version>`.
How a version reaches those files is the product's business, not this
repository's; the same escape hatch the Tauri workflow has for its macOS
build.
See the blueprint for how to use it [here](./blueprints/release-docker.yaml).

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