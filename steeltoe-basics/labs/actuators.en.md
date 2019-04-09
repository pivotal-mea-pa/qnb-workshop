# Include Steeltoe Actuators and Create a Custom Health Check

## Goal

Assuming you have already pushed an app using User Provided Services, this lab will review what additions are necessary in the project to enable Actuators and the automatic benefits in App Manager.

## Prerequisites

- Visual Studio Code
- .Net Core 2.2
- Internet Access
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)

## Add Steeltoe Management Actuators to the app

1. Add the `Steeltoe.Management.CloudFoundryCore` and `Steeltoe.Extensions.Logging.DynamicLogger` nuget packages to the project. 
```bash
$> dotnet add package Steeltoe.Management.CloudFoundry
$> dotnet add package Steeltoe.Extensions.Logging.DynamicLogger
```

1. Open the `Startup.cs` file by double clicking.

1. Include the Management and Logging libraries.
```cs
using Steeltoe.Management.CloudFoundry;
using Steeltoe.Extensions.Logging;
```

1. In the `ConfigureServices` method, add the cloud foundry actuators to the services collection.
```cs
public void ConfigureServices(IServiceCollection services) {
	services.AddCloudFoundryActuators(Configuration);

	...
}
```

1. In the `Configure` method, tell the application builder to use the cloud foundry actuators.
	```cs
	public void Configure(IApplicationBuilder app,
			IHostingEnvironment env,
			ILoggerFactory loggerFactory) {

		app.UseCloudFoundryActuators();
		app.UseMvc();
		 ...
	}
	```

1. In the project's `Program.cs` file we need to enable Dynamic Logging. This will allow us to dynamically configure logging levels via the Apps Manager.
```cs
WebHost.CreateDefaultBuilder(args)
...
    .ConfigureLogging((builderContext, loggingBuilder) => {
        loggingBuilder.AddDynamicConsole();
    })
```

1. In the project's `appsettings.json` file add the following section, this will set up an endpoint for App Manager to connect to.
```json
  "management": {
    "endpoints": {
      "path": "/cloudfoundryapplication"
    }
  }
```

1. Now say to yourself "wow that was easy", because thats all you need to do, to get the default actuators going! To lean more about what is included in the default endpoints, have a look at the [Steeltoe management usage documentation](https://steeltoe.io/docs/steeltoe-management/#1-2-usage).

## Extend app health check to include a custom check

Before we switch to App Manager and see all the wonderful benefits of actuators, let's customize things a little. One very useful actuator is the /health endpoint. This helps BOSH determine an app's health and weather it needs to be recycled. Lets extend the default health check to include a custom check.

1. Create a new C# class in the app project, named `CustomHealthContributor.cs`. It can be located in the root of the project with the Project.cs and Statup.cs files.

1. If Visual Studio did not automatically open the new file, do so by by double clicking.

1. Add a reference to the HealthChecks package.
	```cs
	using Steeltoe.Common.HealthChecks;
	```

1. Extend the CustomHealthContributor class with the IHealthCheckContributor.
	```cs
	...

	public class CustomHealthContributor : IHealthContributor {
		...

	```

1. Within the class add a public Id property and add the Health method with some very basic status reporting.
	```cs
	public string Id => "CustomHealthContributor";

	public HealthCheckResult Health(){
			var result = new HealthCheckResult {
					Status = HealthStatus.UP,
					Description = "This health check does not check anything, yet!"
			};

			result.Details.Add("status", HealthStatus.UP.ToString()); //a part of the middleware response
			return result;
	}
	```
1. Your `CustomHealthContributor` class should look like the following. Not the check isn't actually checking anything at the moment. It's just an example :).
	```cs
	public class CustomHealthContributor : IHealthContributor {
    public string Id => "CustomHealthContributor";

    public HealthCheckResult Health()
    {
        var result = new HealthCheckResult {
            // this is used as part of the aggregate, it is not directly part of the middleware response
            Status = HealthStatus.UP,
            Description = "This health check does not check anything"
        };
        result.Details.Add("status", HealthStatus.UP.ToString());
        return result;
    }
	}
	```

1. Open the `Startup.cs` file, by double clicking.

1. Add a reference to the HealthChecks Steeltoe package.
	```cs
	using Steeltoe.Common.HealthChecks;
	```	

1. In the `ConfigureServices` method, register the new custom health check as a singleton.
	```cs
	public void ConfigureServices(IServiceCollection services)
	{
		...

		services.AddSingleton<IHealthContributor, CustomHealthContributor>();
	}
	```

1. Push your application to Cloud Foundry (`cf push`) and validate that the Values endpoint (`/api/values`) still returns the options.

## Complete

Along with the default Cloud Foundry health checks, we have added in the possibility to report an additional check on some custom serivce or object. You can read more about what healthchecks are built in, in the [Steeltoe health documentation](https://steeltoe.io/docs/steeltoe-management/#1-2-3-health).

Next up, push the app to the foundation and explore all the cool things that get enabled in App Manager.
