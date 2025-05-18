# github-workflows

Repository containing our shared github workflows

## Workflow Documentation

#### [Create Compose Changelog + Version + Release](./.github/workflows/create_compose_changelog_version_release.yaml)

A Simple Workflow that generates a changelog, version and creates a release designed for php composer projects
See Bluprint for how to use it [here](./blueprints/release.yaml)

#### [Create NPM Changelog + Version + Release + Publish](./.github/workflows/create_npm_changelog_version_release_publish.yaml)

A Simple Workflow that generates a changelog, version and creates a release designed for npm projects
See Bluprint for how to use it [here](./blueprints/release-npm.yaml)

## Important notes

- All workflows have to be in the `.github/workflows` directory
- Workflows should be build in a way that they are actually reusable! Too specific workflows should be kept in their
  according repositories directly!