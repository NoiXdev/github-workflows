# github-workflows

Repository containing our shared github workflows

## Workflow Documentation

#### [Create Compose Changelog + Version + Release](./create_compose_changelog_version_release.yaml)

A Simple Workflow that generates a changelog, version and creates a release designed for php composer projects
See Bluprint for how to use it [here](./blueprints/release.yaml)

## Important notes

- All workflows have to be in the `.github/workflows` directory
- Workflows should be build in a way that they are actually reusable! Too specific workflows should be kept in their
  according repositories directly!