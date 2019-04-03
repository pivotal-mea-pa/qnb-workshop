# Create a .NET Core Microservice for Linux

## Goal

Using the installed Visual Studio templates, create a new app based on Steeltoe that is ready for Cloud Foundry Linux Diego cell.

## Prerequisites

- Visual Studio Code
- .NET Core 2.2
- Internet Access

## Create a .NET Core Microservice

1. Open Visual Studio Code and open a new directory for your application. `File > Open...`

1. Open the Integrated Terminal provided in VS Code `Terminal > New Terminal` (Code blocks begining with `$> ` will indicate commands to be run in this terminal)

1. Initialize a new Web API project `$> dotnet new webapi`

## Explore the project

1. Open the `ValuesContoller` class, which can be found at the path `Controllers/ValuesController.cs`. This class creates a few RESTful endpoints for use. Note the route that the class establishes: `/api/values/` and `/api/values/5`. We will use these once the app is pushed to PASW.

1. Create a `manifest.yml` file at the root of the project. This file tells PAS or PASW how to containerize and deploy the app. It also provides details like how many instances to run, what services to bind, and establishes any custom environment variables.

  - name: is the name of the app that you use to reference it from the cf cli or in App Manager.
  - host: is part of the url to run the app. (defaults to the value of `name`)
  - instances: is the number of instances that PASW should deploy, of this app.
  - memory: is the amount of memory each app instance will be allowed.
  - disk_quota: is the amount of disk storage each app instance will be allowed.
  - buildpack: is how the app's container should be created and configured (runtime & system env vars).
  - stack: is what operating system to deploy the app instance to.
  - env: are environment variables the app should use during execution.

1. In the manifest, change the value of `name:` to be something authentic (like your name). Keep it alpha-numeric and use `-`(dash) as a space. Below is a minimal manifest file that you can use to build and deploy your application.

```
---
application:
-   name: <yourname>-steeltoe-app
    buildpack: https://github.com/cloudfoundry/dotnet-core-buildpack#v2.2.5
    instances: 1
    memory: 256M
```

1. Save the manifest file. The app is ready to be published and deployed to a PAS or PASW instance!

## Complete

Now you have a .NET Core microservice ready for deployment. In the following step you will create the artifact and deploy.
