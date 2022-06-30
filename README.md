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

### Legacy Build and Deploy Maven
[legacy_build_deploy_mvn_java](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/legacy_build_deploy_mvn_java.yml) builds and deploys our legacy internal Java components.
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
    uses: Klipfolio/kf-gha-workflows/.github/workflows/legacy_build_deploy_mvn_java.yml@main
    secrets: inherit
```
<!-- end usage -->

Example overriding default Java settings:
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
    uses: Klipfolio/kf-gha-workflows/.github/workflows/legacy_build_deploy_mvn_java.yml@main
    with:
      java-version: 11
      distribution: zulu
    secrets: inherit
```
<!-- end usage -->

### Legacy Docker Build and Deploy
[legacy_docker_build](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/legacy_docker_build.yml) builds and deploys a docker image of our legacy internal Java 8 components.
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

### Legacy VM Build and Deploy
[legacy_vm_deploy](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/legacy_vm_deploy.yml) builds and deploys a Rabbit VM AWS image.
#### Usage
Create the following GHA workflow in your repository `.github/workflows/ci.yml` and add it as a required check on your protected branches. **Note**: This is configured to run on every push event:
<!-- start usage -->
```yml
name: Continuous Integration

on:
  push

jobs:
  build_and_deploy:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/legacy_vm_deploy.yml@main
    secrets: inherit
```
<!-- end usage -->
