# Cloud Native - Environment Variables

## Goal

Become familiar with using environment variables to provide settings to a .NET Core app.

## Prerequisites

- Visual Studio (min 2015)
- Internet Access
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)
- Powershell

## Load the Steeltoe Template for Cloud Foundry

1. From the Visual Studio homepage, choose `File` then `New` then `Project`. This will bring up the "New Project" window. 

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

1. Each time an instance of the app is deployed, these environment variables will be created and made available to the app.

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
  public class ValuesController : ControllerBase
	{
    ...
    private readonly IConfiguration _configuration;
  ```

1. Inject configuration in to the class constructor and fill the newly created variable.
  
  ```cs
  public ValuesController(IOptions<CloudFoundryApplicationOptions> appOptions,
											IOptions<CloudFoundryServicesOptions> serviceOptions,
											ILogger<ValuesController> logger,
											IConfiguration configuration)
		{
			...
			_configuration = configuration;

			...
		}
  ```

1. Locate the `[HttpGet]` endpoint, retrieve the environment variables value, and return its value when the endpoint is called.

  ```cs
  [HttpGet]
  public ActionResult<IEnumerable<string>> Get()
  {
    ...
    string myConnectionString = _configuration["CONNECTION_STRING"];

    return new string[] { myConnectionString };
  }
  ```

Alright! Now you're ready to publish and test the endpoint.