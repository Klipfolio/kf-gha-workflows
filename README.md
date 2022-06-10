# Klipfolio GitHub Workflows

Contains reusable GHA Workflows.  


## Available Workflows

### PR Lint
[PR Lint](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/pr-lint.yml) ensures your PR title matches the [Conventional Commits spec](https://www.conventionalcommits.org/en/v1.0.0/). Uses GitHub Action [amannn/action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request). 

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
  validate:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/pr-lint.yml@main
```
<!-- end usage -->

### Legacy Build and Deploy Maven (Java 8)
[legacy_build_deploy_mvn_java8](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/legacy_build_deploy_mvn_java8) builds and deploys our legacy internal Java 8 components.
#### Usage
Create the following GHA workflow in your repository `.github/workflows/ci.yml` and add it as a required check on your protected branches:
<!-- start usage -->
```yml
name: Continuous Integration

on:
  pull_request:
  push:
    branches:
      - master

jobs:
  build:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/legacy_build_deploy_mvn_java8.yml@main
    secrets: inherit
```
<!-- end usage -->

### Legacy Docker Build and Deploy
[legacy_docker_build](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/legacy_docker_build) builds and deploys a docker image of our legacy internal Java 8 components.
#### Usage
Create the following GHA workflow in your repository `.github/workflows/docker-build.yml` and add it as a required check on your protected branches:
<!-- start usage -->
```yml
name: Docker Build

on:
  pull_request:
  push:
    branches:
      - master

jobs:
  build_docker:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/legacy_docker_build.yml@main
    secrets: inherit
```
<!-- end usage -->
