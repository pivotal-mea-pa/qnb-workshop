# User Provided Services with Steeltoe

## Goal

User provided services are a great way for apps within a Space to share configurations. In this lab we will create a service that is available to all apps in a given Space via environment variables.

## Prerequisites

- Visual Studio (min 2015)
- Powershell

## Create a User Provided Service

1. Confirm the Org/Space you are currently targeting by opening a powershell window and run `cf target`. The cli should return the names of the Org and Space that will be the target of any commands issued. (If you've been logged out, it will let you know that as well)

1. Create a User Provided Service by running the provided command.

  ```bash
  cf create-user-provided-service 'app-connection-string' -p '{\"CONNECTION_STRING\":\"Server=FQDNServerAddress_cups;Database=myDataBase;User Id=myUsername;Password=myPassword;\"}'
  ```

1. Confirm the service is available for binding by listing all services in the space `cf services`. You should see the `app-connection-string` service listed.

## Bind the app to the new service

1. To have the new service bound to the app, we will add the direction to the manifest. Open the `manifest.yml` file by double clicking and add the following section.

  ```yml
  instances: 1
  memory: 512M
  disk_quota: 512M
  ...
  services:
    - app-connection-string
  ```

1. By having the service bound to the app, the environment variable `CONNECTION_STRING` will automatically be made available. To consume this variables value, we can use the Steeltoe Cloud Foundry Provider.

1. If you previoudly did the "Environment Variables" lab, we can remove some of those steps from the code.
  
  1. Remove the following lines from `Program.cs`.
    ```cs
    .ConfigureAppConfiguration((hostingContext, config) => {
      config.AddEnvironmentVariables(prefix: "MYAPP_");
    })
    ```
  
  1. Remove the configuration provisions from `Controllers\ValuesController.cs`.
    ```cs
    private readonly IConfiguration _configuration;
    ```
    ```cs
    public ValuesController(IOptions<CloudFoundryApplicationOptions> appOptions,
                        IOptions<CloudFoundryServicesOptions> serviceOptions,
                        ILogger<ValuesController> logger)
      {
        ...
      }
  ```

1. The Cloud Foundry Provider has already been provisioned in the `Controllers\ValuesController.cs` class. Notice the class variables `_appOptions` and `_serviceOptions`. Which makes consuming the value of the bound User Provided Service very easy!

1. Locate the `[HttpGet]` endpoint and replace the value of "myConnectionString" with a lookup using the `_serviceOptions` collection.

  ```cs
  [HttpGet]
  public ActionResult<IEnumerable<string>> Get()
  {
    ...
    string myConnectionString = _serviceOptions.Services["user-provided"]
                                  .First(q => q.Name.Equals("app-connection-string"))
                                  .Credentials["CONNECTION_STRING"].Value;

    return new string[] { myConnectionString };
  }
  ```

Simple! While it may not appear to be using environment variables, "behind the scenes" Steeltoe is aware of how Cloud Foundry makes services available to apps. So the "CloudFoundryServicesOptions" package did all the hard work of parsing values.

Consider creating the User Provided Service in each of your Org Spaces (aka environments). You might have Dev, Test, and Pre-Prod spaces. The app can be moved between spaces with no change, as along as each space has the same service name. But! the value could change between each Space depending on what tests needs to be run. In example, the Dev space is using a datastore with junk data, the Test space is using a datastore with limited access and clean data, and the Pre-Prod space has a similar datastore to Test but with even less access.

Build once and promote through environments! Thats cloud native.