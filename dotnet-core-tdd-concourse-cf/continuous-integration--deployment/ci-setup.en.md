# CI Setup

**Previous:** [Continuous Integration / Deployment](/workshop/#continuous-integration-deployment)

In this section, we'll make a Concourse pipeline to test our app. If you have not deployed Concourse locally yet, follow the instructions on [Deploy Concourse](../deploy-concourse).

First, make a `ci` directory at the root of our repository. Create a `pipeline.yml` file.

The pipeline is going to need access to the code, so add a resource:
```yaml
resources:
  - name: repo
    type: git
    source:
      uri: {{repo-url}}
      branch: master
```

We'll need something for the pipeline to actually do, so let's add a job:
```yaml
jobs:
  - name: test
    plan:
      # Fetch files from the repository.
      - get: repo
        trigger: true
      # In parallel
      - aggregate:
        # Run unit tests
        - task: test
          file: repo/ci/tasks/test.yml
          params:
            PROJECT_NAME: NotesApp.Tests
        # Run integration tests
        - task: integration-test
          file: repo/ci/tasks/test.yml
          params:
            PROJECT_NAME: NotesApp.IntegrationTests
```

Because we're passing in `PROJECT_NAME` to `test.yml`, we can re-use the same files for both types of tests. `test.yml` and `test.sh` are as follows:

`ci/tasks/test.yml`:
```yaml
platform: linux

image_resource:
  type: docker-image
  source:
    repository: microsoft/dotnet
    tag: 2.2-sdk

inputs:
  - name: repo

run:
  path: ./repo/ci/tasks/test.sh
```
We take in a single input, the repository, and execute our `test.sh` script.

`ci/tasks/test.sh`:
```bash
#!/bin/bash

set -ex

pushd repo
    pushd $PROJECT_NAME
        dotnet test
    popd
popd
```
If you're not familiar with `pushd` and `popd`, they behave similarly to `cd` except that they keep track of history of what was navigated to with `pushd` and can `popd` back to it later. Once we're in the correct directory, we simply need to run `dotnet test`.

***

Add a `variables.yml` file to hold the value of our variable, `{{repo-url}}`. It should look like:
```yaml
repo-url: https://github.com/xtreme-steve-elliott/NotesApp.git
```

The `variables.yml` file should not be tracked in source control once meaningful credentials are stored in it. At that point, open the `.gitignore` on the repository root, and add an entry to ignore the file:
```gitignore
ci/variables.yml
```
Push up the code since the pipeline will need to reference the `test.yml` and `test.sh` inside the `ci` directory.

Now, using the `fly` cli from the `ci` directory, deploy the pipeline:
```bash
fly -t targetname set-pipeline -p pipeline -c pipeline.yml -l variables.yml
```
Where `targetname` is the name of the target you setup in [Deploy Concourse](../deploy-concourse). This creates a pipeline called `pipeline` using the configuration at `ci/pipeline.yml` and loads variables from `ci/variables.yml` Unpause the pipeline to start it.

<details>
  <summary>Your pipeline should look like this:</summary>
  <a href="../CI-Setup/pipeline-test.png" target="_blank">
    ![pipeline-test.png](../CI-Setup/pipeline-test.png)
  </a>
</details>

***

**Git Tag:**  [ci-setup](https://github.com/xtreme-steve-elliott/NotesApp/tree/ci-setup)

**Up Next:** [CF Deployment](../cf-deployment)
