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

### Deleted Branch Component Version
[branch_artifacts_dynamodb_delete](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/branch_artifacts_dynamodb_delete.yml) deletes branch component version items from our DynamoDB table.
#### Usage
Create the following GHA workflow in your repository `.github/workflows/branch_artifacts_dynamodb_delete.yml`. **Note**: This is configured to run on branch `delete` events:
<!-- start usage -->
```yml
name: On branch delete (DynamoDB cleanup)

on:
  delete

jobs:
  branch_delete:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/branch_artifacts_dynamodb_delete.yml@main
    with:
      branch: ${{ github.event.ref }}
      component: bb8
    secrets: inherit
```
<!-- end usage -->

### Update Branch Component Version
[branch_artifacts_dynamodb_update](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/branch_artifacts_dynamodb_update.yml) updates branch component version items in our DynamoDB table.
#### Usage
Create the following GHA workflow in your repository `.github/workflows/branch_artifacts_dynamodb_update.yml`. **Note**: This should be configured to run after a successful build/deploy of the branch. i.e. :
<!-- start usage -->
```yml
name: Notify Version update (DynamoDB)

jobs:
  build:
    runs-on: ubuntu-latest
    name: Build and Test
    outputs:
      branch: ${{ steps.step1.outputs.branch }}
      component: ${{ steps.step1.outputs.component }}
      version: ${{ steps.step1.outputs.version }}
    steps:
      - name: Setup variables
        id: step1
        run: |
          echo "::set-output name=branch::master"
          echo "::set-output name=component::bb8"
          echo "::set-output name=version::1.2.3"

  branch_update:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/branch_artifacts_dynamodb_update.yml@dynamodb-updates-worflow
    needs: build
    with:
      branch: ${{ needs.build.outputs.branch }}
      component: ${{ needs.build.outputs.component }}
      version: ${{ needs.build.outputs.version }}
    secrets: inherit
```
<!-- end usage -->

### PR Restricted Files
[pr-restricted-files](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/pr-restricted-files.yml) helps ensure your branch doesn't contain files that shouldn't be updated from a PR. A simple admin merge is required to override this check.

#### Usage
Create the following GHA workflow in your repository `.github/workflows/pr-restricted-files.yml` and add it as a required check on your main branch:
<!-- start usage -->
```yml
name: "PR Restricted Files"

on:
  pull_request_target:
    types:
      - opened
      - edited
      - synchronize

jobs:
  check:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/pr-restricted-files.yml@main
    with:
      file_paths_to_check: "package.json package-lock.json"

```
<!-- end usage -->

### PR Restricted Files
[ghcr_default_cleanup_schedule](https://github.com/Klipfolio/kf-gha-workflows/blob/main/.github/workflows/ghcr_default_cleanup_schedule.yml) used to cleanup packages associated to the repository calling this workflow

#### Usage
Create the following GHA workflow in your repository `.github/workflows/cleanup_ghcr.yml` which will use the default parameters that will cleanup all containers:
<!-- start usage -->
```yml
name: Delete old container images

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

jobs:
  artifact_update:
    uses: Klipfolio/kf-gha-workflows/.github/workflows/ghcr_default_cleanup_schedule.yml@main
    secrets: inherit

```
<!-- end usage -->

