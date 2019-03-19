# Zero-Downtime Deployment

**Previous:** [Smoke Tests](../smoke-tests)

When backend apps need to be upgraded, it would be disadvantageous for the server to be unavailable during this time. Users would be unable to access the app, which leads to less traffic and frustration for the website and the company. However there are ways around this, with the idea of zero downtime deployment.

In this section, we'll demonstrate a simple zero downtime deployment solution on our Concourse pipeline using the [cf-resource](https://github.com/concourse/cf-resource).

First, we'll show what happens without the solution. We'll ping our server every 5 seconds and do a `cf push` in the mean time.

Install the [watch](https://en.wikipedia.org/wiki/Watch_(Unix)) program (alternatives can be found on Windows, either through Cygwin, or just a Powershell loop):
```bash
brew install watch
```
Now run the following:
```bash
watch -n 5 "curl -s https://notes.dev.cfdev.sh/api/notes"
```
This will print out something like:
```bash
Every 5.0s: curl -s https://notes.dev.cfdev.sh/api/notes

[]
```
Now in a separate terminal window, run:
```bash
fly -t ci trigger-job --job=pipeline/build-deploy
```
This will trigger the `build-deploy` job which will deploy the app again. While this is occurring, monitor the output of `watch`. You'll see a `404` indicating the app is not accessible during this time:
```bash
Every 5.0s: curl -s https://notes.dev.cfdev.sh/api/notes

404 Not Found: Requested route ('notes.dev.cfdev.sh') does not exist.
```
The **cf resource** has an optional `current_app_name` parameter which, if set, will do a zero-downtime deployment. In `pipeline.yml` on the bottom of the `build-deploy` job, as part of the `params` sectioon, add:
```yaml
current_app_name: notes
```
Once the previous job is done, update the pipeline using `fly` and trigger the job again. As you monitor the output of `watch` this time, the app will continue to return the status code `200` with the empty array.

**Git Tag:** [zero-downtime-deployment](https://github.com/xtreme-steve-elliott/NotesApp/tree/zero-downtime-deployment)

**Resources:**  
[Blue-Green Deployment](https://docs.pivotal.io/pivotalcf/1-12/devguide/deploy-apps/blue-green.html)
