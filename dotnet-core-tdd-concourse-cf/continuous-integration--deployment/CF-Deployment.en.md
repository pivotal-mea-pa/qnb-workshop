**Previous:** [CI Setup](../CI-Setup)

In this section, we'll update the pipeline to deploy to our local instance of CF Dev. If you have not installed it, follow the instructions [here](../CF-Dev).

***

### Build

First off, add a new job to the pipeline, `build-deploy`:
```yaml
jobs:
  # ... (test job)
  - name: build-deploy
    plan:
      - get: repo
        trigger: true
        passed: [test]
      - task: build
        file: repo/ci/tasks/build.yml
        params:
          PROJECT_NAME: NotesApp
```
`build-deploy` fetches the version of the repository that has passed the `test` job and then runs the `build` task.

`ci/tasks/build.yml`:
```yaml
platform: linux

image_resource:
  type: docker-image
  source:
    repository: microsoft/dotnet
    tag: 2.2-sdk

inputs:
  - name: repo

outputs:
  - name: published

run:
  path: ./repo/ci/tasks/build.sh
```
This file will look very similiar to that of `test.yml`, except that there is an output of the `published` directory.

`ci/tasks/build.sh`:
```bash
#!/bin/bash

set -ex

pushd repo
    dotnet publish $PROJECT_NAME -o ../../published
popd
```
Again, very similar to `test.sh`, except that it's publishing the project, which compiles all the DLL files that comprise the app.

***

### Deploy

Update the `pipeline.yml` file with the following resource:
```yaml
resources:
    # ... (repo resource)
    - name: cf-deploy
      type: cf
      source:
        api: {{cf-api}}
        username: {{cf-username}}
        password: {{cf-password}}
        organization: {{cf-organization}}
        space: {{cf-space}}
        skip_cert_check: true
```
Refer to [CF Dev](../CF-Dev) for credential information for your CF Dev instance. Place the substitutions in your `credentials.yml`. Also note that `skip_cert_check` is set to `true`. This is because we're running CF Dev locally without a cert. For production, remove this line.

Add a task to the `build-deploy` job for deploying the app, using the `published` directory as the source:
```yaml
jobs:
  # ... (test job)
  - name: build-deploy
    plan:
      # ... (get repo task)
      # ... (build task)
      - put: cf-deploy
        params:
          manifest: repo/ci/manifest.yml
          path: published
```

`ci/manifest.yml`:
```yaml
applications:
  - name: notes
    buildpack: https://github.com/cloudfoundry/dotnet-core-buildpack.git#v2.2.7
```

***

Within `Program.cs`, add the following lines.

Before `.UseContentRoot(...)` add
```c#
.UseCloudFoundryHosting()
```
and after the `.AddJsonFile(...)` lines, add
```c#
.AddCloudFoundry()
.AddEnvironmentVariables();
```

These will allow the app to make use of the CloudFoundry when pushed.

***

Update the pipeline with `fly` from within the `ci` directory:
```bash
fly -t targetname set-pipeline -p pipeline -c pipeline.yml -l variables.yml
```
Then push up the changes to GitHub.

<details>
  <summary>Your pipeline should look like this:</summary>
  <a href="continuous-integration--deployment/pipeline-deploy.png" target="_blank">
    [[pipeline-deploy.png]]
  </a>
</details>

***

Use postman to make an api call to `https://notes.dev.cfdev.sh/api/notes`. Instead of an HTTP `200`, we get an HTTP `500`. That indicates the app was deployed but is encountering an issue. Get the recent logs with:
```bash
cf logs notes --recent
```

You should see:
```shell
MySql.Data.MySqlClient.MySqlException (0x80004005): Unable to connect to any of the specified MySQL hosts.
```
We need to update our hosted app, since it can't talk to our local mySQL database.

**Up Next:** [Connect to a DB on CF](../Connect-to-a-DB-on-CF)
