# Cloud Native - Environment Variables

## Goal

Environment variables are a staple of cloud native best practices, but if you've been developing on a Windows desktop you may not be that familiar. This lab aims to introduce how a cloud native .NET app should hold very little configuration within, and instead rely on it's configuration to be provided in environment variables.

## Prerequisites

- Visual Studio (min 2015)
- Internet Access
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)
- Powershell

## Create a new Visual Studio Steeltoe project

1. From the Visual Studio homepage, choose `File > New > Project`. This will bring up the "New Project" window.

1. Choose `Installed` in the left panel and locate the search box at the top right. Type `steeltoe` in the search box. This will filter the installed project templates, to only those published by Steeltoe.

1. In the main panel locate the template named `Cloud Foundry w Steeltoe (.NET Core - Win)` and click.

1. Choose a name and location for the new project and click `ok` to create the project.

1. Be patient while the project is loaded and all needed packages are downloaded.

## Add environment variables in the app manifest

1. Open the `manifest.yml` file by double clicking. This file tells PASW how to containerize and deploy the app. It also provides details like how many instances to run, what services to bind, and establishes any custom environment variables.

1. Locate the `env` area to the manifest and add the `MYAPP_CONNECTION_STRING` variable. Leave the other environment variables as is.
  ```yml
  instances: 1
  memory: 512M
  disk_quota: 512M
  ...
  env:
    MYAPP_CONNECTION_STRING: 'Server=FQDNServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;'
  ```

1. Each time an instance of the app is deployed, these environment variables will be created and made available to the app. In the turn the app will use the provided values to do some work.

## Retrieve the environment variable values in the app

1. Locate the `Program.cs` file and double click it, to open.

1. Add a direction for all environment variables with the prefix "MYAPP_" to be loaded into the app's configuration collection (Microsoft.Extensions.Configuration).
  ```cs
  WebHost.CreateDefaultBuilder(args)
    ...

    .ConfigureAppConfiguration((hostingContext, config) => {
      config.AddEnvironmentVariables(prefix: "MYAPP_");
    });
  ```

1. Locate the `Controllers\ValuesController.cs` file and double click it, to open.

1. Add a class variable to hold the app's configuration.
  ```cs
  public class ValuesController : ControllerBase {
    ...
    private readonly IConfiguration _configuration;
  ```

1. Inject configuration in to the class constructor and fill the newly created variable.
  ```cs
  public ValuesController(IOptions<CloudFoundryApplicationOptions> appOptions,
    IOptions<CloudFoundryServicesOptions> serviceOptions,
    ILogger<ValuesController> logger,
    IConfiguration configuration) {
      ...
      _configuration = configuration;

      ...
    }
  ```

1. Locate the `[HttpGet]` endpoint, retrieve the environment variable and return its value when the endpoint is called.
  ```cs
  [HttpGet]
  public ActionResult<IEnumerable<string>> Get() {
    ...
    string myConnectionString = _configuration["CONNECTION_STRING"];

    return new string[] { myConnectionString };
  }
    ```
    
## Complete

Now instead of the app's connection string being saved in its web.config or being hard coded in the app, it's being provided at run time. This opens quite a few cloud native "doors" for us. We can build the app once and move it through different spaces (or environments), each providing a different backing data store (in the form of a connection string).

Alright! Now you're ready to publish and test the newly configured endpoint.