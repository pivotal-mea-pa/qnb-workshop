# Include Steeltoe Actuators and Create a Custom Health Check

## Goal

Assuming you have already pushed an app based on the Steeltoe Cloud Foundry templates, this lab will review what additions were made in the project to enable Actuators, and the benefits with its integration with App Manager.

## Prerequisites

- Visual Studio (min 2015)
- Internet Access
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)
- Powershell

## Add Steeltoe Management Actuators to the app

1. Open the `Program.cs` file by double clicking.

1. Add ??????????

1. Open the `Startup.cs` file by double clicking.

1. Confirm the Management package is implemented.

	```cs
	using Steeltoe.Management.CloudFoundry;
	```

1. In the `ConfigureServices` method, confirm the default actuator endpoints have been initialized.

	```cs
	public void ConfigureServices(IServiceCollection services)
	{
		services.AddCloudFoundryActuators(Configuration);

		...
	}
	```

1. In the `Configure` method, confirm the default actuator endpoints have been added.

	```cs
	public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
  {
		 app.UseCloudFoundryActuators();
		 
		 ...
	}
	```

1. Now say to yourself "wow that was easy", beause that all you need to get the default actuator endpoints going! To lean more about what is included in the default endpoints, have a look at the [Steeltoe management usage documentation](https://steeltoe.io/docs/steeltoe-management/#1-2-usage).

## Extend app health check to include a custom check

One very useful actuator is the /health endpoint. This helps BOSH determine an app's health and weather it needs to be recycled. Lets extend the default health check to include a custom check.

1. Create a new C# class in the app project, named `CustomHealthContributor.cs`. It can be located in the root of the project with the Project.cs and Statup.cs files.

1. If Visual Studio did not automatically open the new file, do so by by double clicking.

1. Add a reference to the HealthChecks package.

	```cs
	using Steeltoe.Common.HealthChecks;
	```

1. Extend the CustomHealthContributor class with the IHealthCheckContributor.

	```cs
	...
	public class CustomHealthContributor : IHealthContributor
	{
		...
	```

1. Within the class add an public Id property, and add the Health method with some very basic status reporting.

	```cs
	public string Id => "CustomHealthContributor";

	public HealthCheckResult Health()
	{
			var result = new HealthCheckResult {
					Status = HealthStatus.UP,
					Description = "This health check does not check anything, yet!"
			};

			result.Details.Add("status", HealthStatus.UP.ToString()); //a part of the middleware response
			return result;
	}
	```

1. Open the `Startup.cs` file, by double clicking.

1. Add a reference to the HealthChecks Steeltoe package.

	```cs
	using Steeltoe.Common.HealthChecks;
	```	

1. In the `ConfigureServices` method, register the new custom health check .

	```cs
	public void ConfigureServices(IServiceCollection services)
	{
		...

		services.AddSingleton<IHealthContributor, CustomHealthContributor>();
	}
	```

Along with the default Cloud Foundry health checks, we have added in the possibility to report an additional check, on some custom serivce or object. You can read more about what healthchecks are built in, in the [Steeltoe health documentation](https://steeltoe.io/docs/steeltoe-management/#1-2-3-health).

## Push the application to PASW, to see the Management actuators in action




### Whats included in /cloudfoundryapplication

## App Manager and Actuators
### Steeltoe icon
### Instance health check
### Dynamic log levels
### Request tracing