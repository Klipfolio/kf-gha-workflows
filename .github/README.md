# Klipfolio GitHub Workflows

Contains reusable GHA Workflows.  


## Available Workflows

### PR Lint
Ensures your PR title matches the [Conventional Commits spec](https://www.conventionalcommits.org/en/v1.0.0/). Uses GitHub Action [amannn/action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request). 

#### Usage
Create the following GHA workflow in your repository `.github/workflows/pr-lint.yml` and add it as a required check on your protected branches:
<!-- start usage -->
```yml
name: "PR Lint"

on:
  pull_request_target:
    types:
      - opened
      - edited
      - synchronize

jobs:
  main:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/pr-lint.yml@main
```
<!-- end usage -->
