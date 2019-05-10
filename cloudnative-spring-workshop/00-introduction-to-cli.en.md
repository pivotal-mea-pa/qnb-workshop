# Introduction to CF CLI

- Change the working directory to be _cloudnative-spring-workshop/labs/samples_

Note the sub-directories present:

```bash
samples
├── coldfusion-example
├── dotnet-core-sample
├── go-sample
├── nodejs-sample
├── python-sample
```

If you want to be able to deploy these samples you must have `Go`, `Node`, `.Net Core`, and `Python` installed.

## How to target a foundation and login

*PWS*

* Open a Terminal (e.g., `cmd` or `bash` shell)

* Target a foundation and login

```bash
cf api https://api.run.pivotal.io --skip-ssl-validation
cf login
```
* Enter your account username and password, then select an org and space.

Alternatively, you can target explicitly:

```bash
cf target -o myorg -s myspace
```

## How to deploy an application

* Let's take a look at the CF CLI options
```bash
  cf help -a
```
* Let's see what buildpacks are available to us
```bash
  cf buildpacks
```
* Peruse the services you can provision and bind your applications to

```bash
  cf marketplace
```
* Time to deploy an app. How about Node.js?

```bash
  cd nodejs-sample
  cf push -c "node server.js"
```

If you are having trouble resolving artifacts, you are likely running in a [disconnected](https://docs.cloudfoundry.org/buildpacks/node/index.html#yarn_disconnected) environment, so follow these steps and try pushing the app again.

```bash
  yarn config set yarn-offline-mirror ./npm-packages-offline-cache
  cp ~/.yarnrc .
  rm -rf node_modules/ yarn.lock
  yarn install
```
* Next, let's try deploying a Python app.
```bash
  cd ../python-sample
  cf push my_pyapp
```
* Rinse and repeat for .Net Core and Go apps
```bash
  cd ../dotnet-core-sample
  cf push
  cd ../go-sample
  cf push awesome-go-app -m 64M --random-route
```
* And for our final trick, how about deploying a Cold Fusion app?
```bash
  cd.. coldfusion-example
```
* Grab this [file](https://storage.googleapis.com/cphillipson-workshops/devops-workshop/devops-workshop-cfmagic.zip) and unpack into `src/main/webapp` folder:
```bash
  unzip -q cfmagic.zip ./src/main/webapp
```
Then execute
```bash
  gradle war
  cf push
```
* Check what applications have been deployed so far
```bash
  cf apps
```
Take some time to visit each of the applications you've just deployed.

* Let's stop an app, then check that it has indeed been stopped

```bash
  cf stop cf-nodejs
  cf apps
```

## How to cleanup after yourself

* Finally, let's delete an app
```bash
  cf delete cf-nodejs
```
* Repeat `cf delete` for each app you deployed.

## Where to go for more help

[Getting Started with the CF CLI](https://docs.cloudfoundry.org/cf-cli/getting-started.html)

[Cloud Foundry Cheat Sheet](http://www.appservgrid.com/refcards/refcards/dzonerefcards/rc207-010d-cloud-foundry.pdf)
